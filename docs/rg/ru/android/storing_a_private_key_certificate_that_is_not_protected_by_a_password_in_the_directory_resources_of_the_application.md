# Хранение приватного ключа/сертификата, не защищенного паролем в директории/ресурсах приложения

<table class='noborder'>
    <colgroup>
      <col/>
      <col/>
    </colgroup>
    <tbody>
      <tr>
        <td rowspan="2"><img src="../../../img/defekt_vysokij.png"/></td>
        <td>Критичность:<strong> ВЫСОКИЙ</strong></td>
      </tr>
      <tr>
        <td>Способ обнаружения:<strong> DAST, SENSITIVE INFO</strong></td>
      </tr>
    </tbody>
</table>

## Описание

Хранение приватного ключа, не защищенного паролем (то есть, в открытом виде), в любом месте на файловой системе устройства является серьезной проблемой. Приватные ключи используются для шифрования или расшифровки данных (в зависимости от типа алгоритма, симметричный или асимметричный) и не должны быть доступны никому.

Более серьезной уязвимостью является использование одного ключа, поставляемого с приложением (в ресурсах или получаемого с сервера) для шифрования данных пользователей. Получив значение такого ключа из одного экземпляра приложения, можно расшифровать данные другого пользователя.

## Рекомендации

Для хранения ключей рекомендуемым способом является использование KeyChain.

KeyStore предоставляет несколько основных функций, которые существенно упрощают работу с криптографическими ключами:

* Случайная генерация ключей.

* Надежное хранение ключей.

Все, что вам нужно сделать, это:

* Сгенерировать случайный ключ при первом запуске приложения.

* Если вы хотите зашифровать данные, получите ключ из KeyStore, зашифруйте с его помощью данные, а затем сохраните зашифрованные данные.

* Если вы хотите расшифровать данные, получите ключ из KeyStore, а затем используйте его для расшифровки данных.

>Способ использования Android KeyStore отличается в зависимости от версии (до и после Android 6).

### Android 6 и выше

Для уровня API 23 и выше реализация будет проще, поскольку генерация AES ключей происходит средствами системы. Пример можно найти в документации API [KeyGenParameterSpec](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html).

**Генерация ключа:**

    private static final String AndroidKeyStore = "AndroidKeyStore";
    private static final String AES_MODE = "AES/GCM/NoPadding";
    keyStore = KeyStore.getInstance(AndroidKeyStore);
    keyStore.load(null);
    if (!keyStore.containsAlias(KEY_ALIAS)) {
        KeyGenerator keyGenerator = KeyGenerator.getInstance(KeyProperties.KEY_ALGORITHM_AES, AndroidKeyStore);
        keyGenerator.init(
                new KeyGenParameterSpec.Builder(KEY_ALIAS,
                        KeyProperties.PURPOSE_ENCRYPT | KeyProperties.PURPOSE_DECRYPT)
                        .setBlockModes(KeyProperties.BLOCK_MODE_GCM)
                        .setEncryptionPaddings(KeyProperties.ENCRYPTION_PADDING_NONE)
                        .setRandomizedEncryptionRequired(false) 
                        .build());
        keyGenerator.generateKey();
    }
    
**Получение ключа:**

    private java.security.Key getSecretKey(Context context) throws Exception {
    return keyStore.getKey(XEALTH_KEY_ALIAS, null);
    }

**Шифрование данных:**

    Cipher c = Cipher.getInstance(AES_MODE);
    c.init(Cipher.ENCRYPT_MODE, getSecretKey(context), new GCMParameterSpec(128, FIXED_IV.getBytes()));
    byte[] encodedBytes = c.doFinal(input);
    String encryptedBase64Encoded = Base64.encodeToString(encodedBytes, Base64.DEFAULT);
    return encryptedBase64Encoded;

**Расшифровка данных:**

    Cipher c = Cipher.getInstance(AES_MODE);
    c.init(Cipher.DECRYPT_MODE, getSecretKey(context), new GCMParameterSpec(128, FIXED_IV.getBytes()));
    byte[] decodedBytes = c.doFinal(encrypted);
    return decodedBytes;

**Вектор инициализации**

Вектор инициализации — это криптографическая функция, которая отвечает за случайность первого блока шифрования. Нужно помнить, что IV, который используется при шифровании, должен быть тем же самым, который используется при расшифровке. По умолчанию Android заставляет каждый раз использовать случайный IV, но можно отключить его, вызвав `setRandomizedEncryptionRequired ()` при генерации ключа.

Благодаря безопасности, обеспечиваемой Android KeyStore, случайный IV является излишним, поэтому вместо этого возможно использовать фиксированный IV. Если есть необходимость использовать случайные IV, можно вызвать метод `getIV()` при шифровании данных и использовать тот же IV при их расшифровке.

### Ниже Android 6

Для версий Android API ниже 23 (Android 6), требуется немного больше работы. [KeyGenParameterSpec](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html) доступен только в API 23, поэтому хранилище KeyStore не может само генерировать случайные ключи AES. Вместо этого необходимо самим сгенерировать ключи используя API [KeyPairGeneratorSpec](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html).

Как следует из названия, KeyPairGeneratorSpec генерирует пары открытого и закрытого ключей. Шифрование с открытым ключом предназначено в основном для подписи и аутентификации и не подходит для шифрования больших блоков данных, но может сочетаться с блочным шифром, таким как AES.

То есть это принцип KEK+DEK (Key Encryption Key + Data Encryption Key). Шифруем данные на одном ключе, который в свою очередь шифруем на другом ключе, который хранится в KeyStore. У такого подхода есть свои плюсы, например при изменении ключа шифрования достаточно перешифровать AES ключ и не трогать пользовательские данные (не перешифровывать их). Примерный алгоритм действий выглядит следующим образом:

**Генерация ключей:**

* Сгенерировать пару ключей RSA.
* Сгенерировать случайный ключ AES.
* Зашифровать ключ AES с помощью открытого ключа RSA.
* Сохранить зашифрованный ключ в Shared Preferences.

**Шифрование и хранение данных:**

* Получить зашифрованный ключ AES из Shared Preferences.
* Расшифровать ключ, используя закрытый ключ RSA.
* Зашифровать данные с помощью ключа AES.
 
**Получение и расшифровка данных:**

* Получить зашифрованный ключ AES из Shared Preferences.
* Расшифровать ключ, используя закрытый ключ RSA.
* Расшифровать данные с помощью ключа AES.

**Генерация RSA ключей**

    private static final String     AndroidKeyStore = "AndroidKeyStore";
    keyStore = KeyStore.getInstance(AndroidKeyStore);
    keyStore.load(null);
    // Generate the RSA key pairs
    if (!keyStore.containsAlias(KEY_ALIAS)) {
        // Generate a key pair for encryption
        Calendar start = Calendar.getInstance();
        Calendar end = Calendar.getInstance();
        end.add(Calendar.YEAR, 30);
        KeyPairGeneratorSpec spec = new      KeyPairGeneratorSpec.Builder(context)
                .setAlias(KEY_ALIAS)
                .setSubject(new X500Principal("CN=" + KEY_ALIAS))
                .setSerialNumber(BigInteger.TEN)
                .setStartDate(start.getTime())
                .setEndDate(end.getTime())
                .build();
        KeyPairGenerator kpg = KeyPairGenerator.getInstance(KeyProperties.KEY_ALGORITHM_RSA, AndroidKeyStore);
        kpg.initialize(spec);
        kpg.generateKeyPair();
    }

**Процедуры шифрования и дешифрования RSA:**

    private static final String RSA_MODE =  "RSA/ECB/PKCS1Padding";
    private byte[] rsaEncrypt(byte[] secret) throws Exception{
        KeyStore.PrivateKeyEntry privateKeyEntry = (KeyStore.PrivateKeyEntry) keyStore.getEntry(KEY_ALIAS, null);
        // Encrypt the text
        Cipher inputCipher = Cipher.getInstance(RSA_MODE, "AndroidOpenSSL");
        inputCipher.init(Cipher.ENCRYPT_MODE, privateKeyEntry.getCertificate().getPublicKey());
        ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
        CipherOutputStream cipherOutputStream = new CipherOutputStream(outputStream, inputCipher);
        cipherOutputStream.write(secret);
        cipherOutputStream.close();
        byte[] vals = outputStream.toByteArray();
        return vals;
    }
    private  byte[]  rsaDecrypt(byte[] encrypted) throws Exception {
        KeyStore.PrivateKeyEntry privateKeyEntry = (KeyStore.PrivateKeyEntry)keyStore.getEntry(KEY_ALIAS, null);
        Cipher output = Cipher.getInstance(RSA_MODE, "AndroidOpenSSL");
        output.init(Cipher.DECRYPT_MODE, privateKeyEntry.getPrivateKey());
        CipherInputStream cipherInputStream = new CipherInputStream(
                new ByteArrayInputStream(encrypted), output);
        ArrayList values = new ArrayList<>();
        int nextByte;
        while ((nextByte = cipherInputStream.read()) != -1) {
            values.add((byte)nextByte);
        }
        byte[] bytes = new byte[values.size()];
        for(int i = 0; i < bytes.length; i++) {
            bytes[i] = values.get(i).byteValue();
        }
        return bytes;
    }

**Генерация и сохранение AES ключа:**

    SharedPreferences pref = context.getSharedPreferences(SHARED_PREFENCE_NAME, Context.MODE_PRIVATE);
    String enryptedKeyB64 = pref.getString(ENCRYPTED_KEY, null);
    if (enryptedKeyB64 == null) {
    byte[] key = new byte[16];
    SecureRandom secureRandom = new SecureRandom();
    secureRandom.nextBytes(key);
    byte[] encryptedKey = rsaEncrypt(key);
    enryptedKeyB64 = Base64.encodeToString(encryptedKey, Base64.DEFAULT);
    SharedPreferences.Editor edit = pref.edit();
    edit.putString(ENCRYPTED_KEY, enryptedKeyB64);
    edit.commit();
    }

**Шифрование и расшифровка данных:**

    private static final String AES_MODE = "AES/ECB/PKCS7Padding";
    private Key getSecretKey(Context context) throws Exception{
        SharedPreferences pref = context.getSharedPreferences(SHARED_PREFENCE_NAME, Context.MODE_PRIVATE);
        String enryptedKeyB64 = pref.getString(ENCRYPTED_KEY, null);          
        // need to check null, omitted here
        byte[] encryptedKey = Base64.decode(enryptedKeyB64, Base64.DEFAULT);
        byte[] key = rsaDecrypt(encryptedKey);
        return new SecretKeySpec(key, "AES");
    }
    public String encrypt(Context context, byte[] input) {
        Cipher c = Cipher.getInstance(AES_MODE, "BC");
        c.init(Cipher.ENCRYPT_MODE, getSecretKey(context));
        byte[] encodedBytes = c.doFinal(input);
        String encryptedBase64Encoded =  Base64.encodeToString(encodedBytes, Base64.DEFAULT);
        return encryptedBase64Encoded;
    }
    public byte[] decrypt(Context context, byte[] encrypted) {
        Cipher c = Cipher.getInstance(AES_MODE, "BC");
        c.init(Cipher.DECRYPT_MODE, getSecretKey(context));
        byte[] decodedBytes = c.doFinal(encrypted);
        return decodedBytes;
    }

## Ссылки

1. [https://doridori.github.io/android-security-the-forgetful-keystore/#sthash.cxj8r3G6.dpbs](https://doridori.github.io/android-security-the-forgetful-keystore/#sthash.cxj8r3G6.dpbs)

2. [http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)

3. [http://developer.android.com/reference/android/security/KeyPairGeneratorSpec.html](http://developer.android.com/reference/android/security/KeyPairGeneratorSpec.html)

4. [https://medium.com/@ericfu/securely-storing-secrets-in-an-android-application-501f030ae5a3#:~:text=With%20these%2C%20storing%20secrets%20becomes,the%20encrypted%20data%20in%20Preferences.](https://medium.com/@ericfu/securely-storing-secrets-in-an-android-application-501f030ae5a3#:~:text=With%20these%2C%20storing%20secrets%20becomes,the%20encrypted%20data%20in%20Preferences.)

5. [https://www.owasp.org/index.php/Mobile_Top_10_2016-M2-Insecure_Data_Storage](https://www.owasp.org/index.php/Mobile_Top_10_2016-M2-Insecure_Data_Storage)