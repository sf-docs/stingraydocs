# Хранение sensitive-информации в общедоступной незащищённой базе данных

<table class='noborder'>
    <colgroup>
      <col/>
      <col/>
    </colgroup>
    <tbody>
      <tr>
        <td rowspan="2"><img src="../../../img/defekt_kritichnyj.png"/></td>
        <td>Критичность:<strong> КРИТИЧНЫЙ</strong></td>
      </tr>
      <tr>
        <td>Способ обнаружения:<strong> DAST, API</strong></td>
      </tr>
    </tbody>
</table>

## Описание

Приложение хранит чувствительную информацию в общедоступной незащищенной базе данных, что может повлечь компрометацию данных.

Для того, чтобы понять, какие именно данные необходимо защищать, прежде всего необходимо определить, какие данные обрабатывает и хранит приложение и какая часть из этой информации считается конфиденциальной. Как правило, в таких случаях полагаются на законодательство и здравый смысл. Нет смысла защищать шифрованием абсолютно всю информацию, которое хранит приложение, это может повлиять на скорость и стабильность работы. Вместо этого следует однозначно определить, что именно является конфиденциальными данными для вашего приложения или компании, и сосредоточить свое внимание именно на этих данных.

## Рекомендации

В случае необходимости хранения чувствительной информации в базе данных нужно дополнительно шифровать итоговую базу данных или данные, которые в ней хранятся. В качестве примера с шифрованием базы данных можно воспользоваться библиотекой [sqlcipher](https://github.com/sqlcipher/sqlcipher).

**Пример использования SQLCipher (Java)**

    package com.demo.sqlcipher;
    import java.io.File;
    import net.sqlcipher.database.SQLiteDatabase;
    import android.app.Activity;
    import android.os.Bundle;
    public class HelloSQLCipherActivity extends Activity {
        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.main);
            InitializeSQLCipher();
        }
        private void InitializeSQLCipher() {
            SQLiteDatabase.loadLibs(this);
            File databaseFile = getDatabasePath("demo.db");
            databaseFile.mkdirs();
            databaseFile.delete();
            SQLiteDatabase database = SQLiteDatabase.openOrCreateDatabase(databaseFile, "test123", null);
            database.execSQL("create table t1(a, b)");
            database.execSQL("insert into t1(a, b) values(?, ?)", new Object[]{"one for the money",
                                                                            "two for the show"});
        }
    }

**Пример использования SQLCipher (Kotlin)**

    package com.demo.sqlcipher
    import android.os.Bundle
    import androidx.appcompat.app.AppCompatActivity
    import net.sqlcipher.database.SQLiteDatabase
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)
            SQLiteDatabase.loadLibs(this)
            val databaseFile = getDatabasePath("demo.db")
            if(databaseFile.exists()) databaseFile.delete()
            databaseFile.mkdirs()
            databaseFile.delete()
            val database = SQLiteDatabase.openOrCreateDatabase(databaseFile, "test123", null)
            database.execSQL("create table t1(a, b)")
            database.execSQL("insert into t1(a, b) values(?, ?)",
                arrayOf("one for the money", "two for the show")
            )
        }
    }

В данном примере использован «захардкоженный» пароль для базы данных **«test123»**. В реальном приложении не стоит использовать настолько ненадежный пароль и хранить его в исходном коде или в открытом виде.

В качестве способа без хранения пароля — вычисление его “на лету“ с использованием пароля пользователя при помощи процедуры усиления ключа.

**Правила:**

1. Явно определяйте режим шифрования и дополнения блоков.
2. Используйте криптостойкие технологии шифрования, включающие алгоритм, режим блочного шифрования и режим дополнения блоков.
3. В процессе генерации ключа на основе пароля используйте «соль» (salt).
4. В процессе генерации ключа на основе пароля используйте достаточное количество итераций хеширования.
5. Используйте ключ с длиной, которая обеспечит криптостойкость шифрования.
 
        package com.appsec.android.cryptsymmetricpasswordbasedkey;
        
        import android.os.Build
        import javax.crypto.SecretKeyFactory
        import javax.crypto.spec.PBEKeySpec
        @Deprecated("Use Argon2 instead")
        internal object Pbkdf2Factory {
            private const val DEFAULT_ITERATIONS = 10_000
            private val systemAlgorithm by lazy {
                if (Build.VERSION.SDK_INT < Build.VERSION_CODES.O) {
                    "PBKDF2withHmacSHA1"
                } else {
                    "PBKDF2withHmacSHA256"
                }
            }
            fun createKey(
                passphraseOrPin: CharArray,
                salt: ByteArray,
                algorithm: String = systemAlgorithm,
                iterations: Int = DEFAULT_ITERATIONS
            ): Pbkdf2Key {
                @Suppress("MagicNumber")
                val keySpec = PBEKeySpec(passphraseOrPin, salt, iterations, 256)
                val secretKey = SecretKeyFactory.getInstance(algorithm).generateSecret(keySpec)
                return Pbkdf2Key(
                    secretKey.algorithm,
                    iterations,
                    salt,
                    secretKey.encoded
                )
            }
        }

В дальнейшем получившееся значение ключа можно использовать в качестве пароля для шифрования базы данных и нет необходимости в его хранении, так как каждый раз при вводе пароля он буде вычисляться на лету и передаваться в функцию открытия БД.

!!! note "Внимание!"
    При таком подходе при изменении секрета (пароля пользователя) данные необходимо перешифровать с новым секретом, если есть необходимость в их сохранении. Также если пароль пользователя представляет из себя пин-код, то лучше использовать подход KEK(Key Encryption Key) + DEK(Data Encryption Key), при котором создается ключ для шифрования данных и он дополнительно шифруется на пароле пользователя. При таком подходе в случае изменения секрета необходимо будет только перешифровать ключ и не трогать зашифрованные данные пользователя.

При подключении библиотеки SQLCipher не забудьте добавить правила в Proguard для коррекной работы приложения.

Правила для proguard:

    -keep,includedescriptorclasses class net.sqlcipher.** { *; }
    -keep,includedescriptorclasses interface net.sqlcipher.** { *; }

## Ссылки

1. [https://github.com/sqlcipher/android-database-sqlcipher](https://github.com/sqlcipher/android-database-sqlcipher)

2. [https://github.com/OWASP/owasp-mstg/blob/master/Document/0x05d-Testing-Data-Storage.md#sqlite-databases-encrypted](https://github.com/OWASP/owasp-mstg/blob/master/Document/0x05d-Testing-Data-Storage.md#sqlite-databases-encrypted)

3. [https://cwe.mitre.org/data/definitions/521.html](https://cwe.mitre.org/data/definitions/521.html)

4. [https://www.techopedia.com/definition/5660/data-encryption-key-dek](https://www.techopedia.com/definition/5660/data-encryption-key-dek)