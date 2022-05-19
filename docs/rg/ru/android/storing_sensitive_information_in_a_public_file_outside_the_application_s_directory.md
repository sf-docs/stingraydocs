# Хранение sensitive-информации в общедоступном файле вне директории приложения

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
        <td>Способ обнаружения:<strong> DAST, FILES</strong></td>
      </tr>
    </tbody>
</table>

## Описание

Приложение хранит чувствительную информацию в общедоступном файле вне директории приложения.

Для того, чтобы понять, какие именно данные необходимо защищать, прежде всего необходимо определить, какие данные обрабатывает и хранит приложение и какая часть из этой информации считается конфиденциальной. Как правило, в таких случаях полагаются на законодательство и здравый смысл. Нет смысла защищать шифрованием абсолютно всю информацию, которое хранит приложение, это может повлиять на скорость и стабильность работы. Вместо этого следует однозначно определить, что именно является конфиденциальными данными для вашего приложения или компании, и сосредоточить свое внимание именно на этих данных.

Принято считать, что необходимо хранить как можно меньше конфиденциальных данных в локальном хранилище (внутреннем или внешнем). Однако в большинстве случаев, хранения такой информации не удастся избежать. Например, не стоит заставлять пользователя вводить сложный пароль при каждом запуске приложения с точки зрения удобства использования. Большинство приложений должны локально кэшировать какой-либо токен аутентификации. Персонально идентифицируемая информация (PII) и другие типы конфиденциальных данных также могут быть сохранены, если этого требует конкретный сценарий.

Приложение может хранить данные в нескольких местах, например, на устройстве или на внешней SD-карте. Наиболее широко используемые способы хранения данных на устройстве:

* Shared Preferences.
* Базы данных SQLite.
* Базы данных Realm.
* Внутренняя память.
* Внешнее хранилище.

Не стоит забывать, что использование любого из этих способов не гарантирует безопасность хранимых данных. Если дополнительно перед этим не использовать шифрование или хэширование, то эти данные смогут быть доступны злоумышленнику.

!!! note "Внимание!"
    Очень часто ошибочно считается, что данные, которые хранятся во внутренней директории приложения уже защищены при помощи механизма песочницы и злоумышленник до них не доберется. Существует большое количество способов, начиная от простого локального или облачного backup приложения, заканчивая физическим доступом к устройству и эксплуатации различных уязвимостей. **Информация, размещенная в открытом виде внутри директории приложения не защищена!**

Размещение же информации на внешнем носителе или в общих директориях делает ее доступной для всех. И в таких файлах точно не стоит хранить конфиденциальную информацию.

## Рекомендации

Способ хранения конфиденциальной информации зависит от её типа. В случае с необходимостью хранения ключей шифрования — лучше всего использовать системное хранилище, AndroidKeyStore, но, к сожалению, это не всегда возможно, так что необходимо исходить из максимально допустимой защиты в зависимости от версии Android:

* На Android API<18 ключи шифрования должны храниться внутри директории приложения в BKS.
* На Android API>=18 RSA ключи должны храниться в AndroidKeyStore, AES ключи в BKS.
* На Android API>=23 RSA и AES ключи должны храниться в AndroidKeyStore.

Не стоит забывать, что при использовании BKS во внутренней директории приложения необходимо дополнительно защищать его и хранящиеся в нем ключи с помощью надежного пароля. Как один из вариантов, сгенерированный пароль должен быть проверен в базе наиболее популярных паролей и должен соответствовать минимальным требованиям:

* Длина пароля не меньше 20 символов.
* Обязательно содержание хотя бы одной прописной буквы.
* Обязательно содержание хотя бы одной заглавной буквы.
* Обязательно содержание хотя бы одной цифры.
* Обязательно содержание хотя бы одного спец символа.

Для обеспечения конфиденциальности и проверки целостности данных Android оснащена множеством криптографических функций. Методы, с помощью которых приложения Android могут безопасно осуществлять шифрование и дешифрование (для обеспечения конфиденциальности), а также аутентификацию сообщений (MAC) и цифровые подписи (для проверки целостности).

Для того, чтобы выбрать подходящий в заданных условиях метод шифрования и тип ключа, можно воспользоваться следующими схемами:

<figure markdown>
![](../../img/6.png)
</figure>

<figure markdown>
![](../../img/7.png)
</figure>

**Шифрование/дешифрование ключом на основе пароля**

Для примера рассмотрим шифрование/дешифрование ключом на основе пароля пользователя. В этом случае нет необходимости в хранении ключа шифрования, так как он генерируется “на лету“ с использованием пароля, который ввел пользователь:

Правила:

1. Явно определяйте режим шифрования и дополнения блоков.
2. Используйте криптостойкие технологии шифрования, включающие алгоритм, режим блочного шифрования и режим дополнения блоков.
3. В процессе генерации ключа на основе пароля используйте «соль» (salt).
4. В процессе генерации ключа на основе пароля используйте достаточное количество итераций хеширования.
5. Используйте ключ с длиной, которая обеспечит криптостойкость шифрования.

        package com.appsec.android.cryptsymmetricpasswordbasedkey;
        
        import java.security.InvalidAlgorithmParameterException;
        import java.security.InvalidKeyException;
        import java.security.NoSuchAlgorithmException;
        import java.security.SecureRandom;
        import java.security.spec.InvalidKeySpecException;
        import java.util.Arrays;
        
        import javax.crypto.BadPaddingException;
        import javax.crypto.Cipher;
        import javax.crypto.IllegalBlockSizeException;
        import javax.crypto.NoSuchPaddingException;
        import javax.crypto.SecretKey;
        import javax.crypto.SecretKeyFactory;
        import javax.crypto.spec.IvParameterSpec;
        import javax.crypto.spec.PBEKeySpec;
        
        public final class AesCryptoPBEKey {
        
            // *** 1 *** Явно определяйте режим шифрования и дополнения блоков.
            // *** 2 *** Используйте криптостойкие технологии шифрования, включающие алгоритм, режим блочного шифрования и режим дополнения блоков
            // Параметры передаваемые в метод getInstance класса Cipher: алгоритм шифрования, режим блочного шифрования, режим дополнения блоков
            // в этом примере следующие значения: алгоритм шифрования=AES, режим блочного шифрования=CBC, режим дополнения блоков=PKCS7Padding
            private static final String TRANSFORMATION = "AES/CBC/PKCS7Padding";
        
            // Строка, используемая для получения экземпляра класса, который будет генерировать ключ
            private static final String KEY_GENERATOR_MODE = "PBEWITHSHA256AND128BITAES-CBC-BC";
        
            // *** 3 *** В процессе генерации ключа на основе пароля используйте "соль" (salt)
            // Длина строки "соли" в байтах
            public static final int SALT_LENGTH_BYTES = 20;
        
            // *** 4 *** В процессе генерации ключа на основе пароля используйте достаточное количество итераций хеширования
            // Указание числа повторений смешиваний, используемых при генерации ключей с помощью PBE
            private static final int KEY_GEN_ITERATION_COUNT = 1024;
        
            // *** 5 *** Используйте ключ с длиной, которая обеспечит криптостойкость шифрования
            // Длина ключа в битах
            private static final int KEY_LENGTH_BITS = 128;
        
            private byte[] mIV = null;
            private byte[] mSalt = null;
        
            public byte[] getIV() {
                return mIV;
            }
        
            public byte[] getSalt() {
                return mSalt;
            }
        
            AesCryptoPBEKey(final byte[] iv, final byte[] salt) {
                mIV = iv;
                mSalt = salt;
            }
        
            AesCryptoPBEKey() {
                mIV = null;
                initSalt();
            }
        
            private void initSalt() {
                mSalt = new byte[SALT_LENGTH_BYTES];
                SecureRandom sr = new SecureRandom();
                sr.nextBytes(mSalt);
            }
        
            public final byte[] encrypt(final byte[] plain, final char[] password) {
                byte[] encrypted = null;
        
                try {
                    // *** 1 *** Явно определяйте режим шифрования и дополнения блоков.
                    // *** 2 *** Используйте криптостойкие технологии шифрования, включающие алгоритм, режим блочного шифрования и режим дополнения блоков
                    Cipher cipher = Cipher.getInstance(TRANSFORMATION);
        
                    // *** 3 *** В процессе генерации ключа на основе пароля используйте "соль" (salt)
                    SecretKey secretKey = generateKey(password, mSalt);
                    cipher.init(Cipher.ENCRYPT_MODE, secretKey);
                    mIV = cipher.getIV();
        
                    encrypted = cipher.doFinal(plain);
                } catch (NoSuchAlgorithmException e) {
                } catch (NoSuchPaddingException e) {
                } catch (InvalidKeyException e) {
                } catch (IllegalBlockSizeException e) {
                } catch (BadPaddingException e) {
                } finally {
                }
        
                return encrypted;
            }
        
            public final byte[] decrypt(final byte[] encrypted, final char[] password) {
                byte[] plain = null;
        
                try {
                    // *** 1 *** Явно определяйте режим шифрования и дополнения блоков.
                    // *** 2 *** Используйте криптостойкие технологии шифрования, включающие алгоритм, режим блочного шифрования и режим дополнения блоков
                    Cipher cipher = Cipher.getInstance(TRANSFORMATION);
        
                    // *** 3 *** В процессе генерации ключа на основе пароля используйте "соль" (salt)
                    SecretKey secretKey = generateKey(password, mSalt);
                    IvParameterSpec ivParameterSpec = new IvParameterSpec(mIV);
                    cipher.init(Cipher.DECRYPT_MODE, secretKey, ivParameterSpec);
        
                    plain = cipher.doFinal(encrypted);
                } catch (NoSuchAlgorithmException e) {
                } catch (NoSuchPaddingException e) {
                } catch (InvalidKeyException e) {
                } catch (InvalidAlgorithmParameterException e) {
                } catch (IllegalBlockSizeException e) {
                } catch (BadPaddingException e) {
                } finally {
                }
        
                return plain;
            }
        
            private static final SecretKey generateKey(final char[] password, final byte[] salt) {
                SecretKey secretKey = null;
                PBEKeySpec keySpec = null;
        
                try {
                    // *** 2 *** Используйте криптостойкие технологии шифрования, включающие алгоритм, режим блочного шифрования и режим дополнения блоков
                    // Получение экземпляра класса для генерации ключа
                    // В этом примере используется класс KeyFactory, который применяет алгоритм SHA256 для генерации AES-CBC 128-битного ключа
                    SecretKeyFactory secretKeyFactory = SecretKeyFactory.getInstance(KEY_GENERATOR_MODE);
        
                    // *** 3 *** В процессе генерации ключа на основе пароля используйте "соль" (salt)
                    // *** 4 *** В процессе генерации ключа на основе пароля используйте достаточное количество итераций хеширования
                    // *** 5 *** Используйте ключ с длиной, которая обеспечит криптостойкость шифрования
                    keySpec = new PBEKeySpec(password, salt, KEY_GEN_ITERATION_COUNT, KEY_LENGTH_BITS);
                    // Очистка пароля - требуется для усложнения процедуры отладки и невключения пароля в дамп памяти
                    Arrays.fill(password, '?');
                    // Генерация ключа
                    secretKey = secretKeyFactory.generateSecret(keySpec);
                } catch (NoSuchAlgorithmException e) {
                } catch (InvalidKeySpecException e) {
                } finally {
                    keySpec.clearPassword();
                }
        
                return secretKey;
            }
        }

## Ссылки

1. [https://mobile-security.gitbook.io/mobile-security-testing-guide/android-testing-guide/0x05d-testing-data-storage](https://mobile-security.gitbook.io/mobile-security-testing-guide/android-testing-guide/0x05d-testing-data-storage)

2. [https://cwe.mitre.org/data/definitions/200.html](https://cwe.mitre.org/data/definitions/200.html)

3. [https://cwe.mitre.org/data/definitions/311.html](https://cwe.mitre.org/data/definitions/311.html)

4. [https://cwe.mitre.org/data/definitions/312.html](https://cwe.mitre.org/data/definitions/312.html)