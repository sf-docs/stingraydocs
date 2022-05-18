# Доступное на запись хранилище ключей

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
        <td>Способ обнаружения:<strong> DAST, КЛЮЧЕВАЯ ИНФОРМАЦИЯ</strong></td>
      </tr>
    </tbody>
  </table>

## Описание

Приложение использует доступное на запись хранилище ключей. Это может привести к подмене ключевой информации. Ключ шифрования не должен храниться в общедоступном месте.

При использовании криптографических операций на устройстве необходимо обеспечить максимальную безопасность основного секрета в таких операциях — ключа шифрования. При использовании ассиметричного шифрования — необходимо сохранить в секрете приватный ключ, в то время как в случае использования симметричных алгоритмов следует защищать ключ, который используется и для шифрования, и для расшифрования sensitive-информации. Существует несколько основных способов хранения ключей в зависимости от версии операционной системы в хранилище AndroidKeyStore или в директории приложении в BKS. Наиболее безопасным вариантом, безусловно является хранение ключей в AndroidKeyStore. Но, если необходимо хранить ключи в BKS нужно не забывать о том, что и само хранилище, и все ключи в нем должны быть защищены надежным паролем.

Компрометация ключевой информации, которая используется в приложении, может привести к катастрофическим последствиям, в зависимости от использования данной информации в приложении, начиная от расшифровки файлов, трафика, заканчивая компрометацией закрытого ключа, использующегося для подписи приложения.

## Рекомендации

Ключи шифрования не должны храниться в общедоступном месте, даже если это директория приложения на SD-карте.

Существует несколько основных способов хранения ключей в зависимости от версии операционной системы:

* На Android API<18 ключи шифрования должны храниться внутри директории приложения в BKS.
* На Android API>=18 RSA ключи должны храниться в AndroidKeyStore, AES ключи в BKS.
* На Android API>=23 RSA и AES ключи должны храниться в AndroidKeyStore.

Не стоит забывать, что при использовании BKS во внутренней директории приложения необходимо дополнительно защищать его и хранящиеся в нем ключи с помощью надежного пароля. Как один из вариантов, сгенерированный пароль должен быть проверен в базе наиболее популярных паролей и должен соответствовать минимальным требованиям:

* Длина пароля не меньше 20 символов.
* Обязательно содержание хотя бы одной прописной буквы.
* Обязательно содержание хотя бы одной строчной буквы.
* Обязательно содержание хотя бы одной цифры.
* Обязательно содержание хотя бы одного спец символа.

**Пример генерации защищенного хранилища BKS с паролем и ключом, также защищенным паролем:**

    keytool -importcert -v -trustcacerts -file "C:\Users\Indra\Documents\myapp.com.cer"
    -alias IntermediateCA -keystore "C:\Users\Indra\Documents\appKeyStore.bks"
    -provider org.bouncycastle.jce.provider.BouncyCastleProvider
    -providerpath "C:\Users\Indra\Downloads\bcprov-jdk15on-154.jar"
    -storetype BKS -storepass StorePass123
    keytool -list -keystore "C:\Users\Indra\Documents\appKeyStore.bks"
    -provider org.bouncycastle.jce.provider.BouncyCastleProvider
    -providerpath "C:\Users\Indra\Downloads\bcprov-jdk15on-154.jar"
    -storetype BKS -storepass "StorePass123"
    ------------------------------------------------------------------------------------
    openssl pkcs12 -export -in "/home/myapp/myapp_cert_2016/ssl_certificate.crt"
    -inkey "/home/myapp/myapp_cert_2016/domainname.key"
    -certfile "/home/myapp/myapp_cert_2016/ssl_certificate.crt"
    -out testkeystore.p12
    Export password : exportpass123
    keytool -importkeystore -srckeystore "C:\Users\Indra\myapp\testkeystore.p12"
    -srcstoretype pkcs12 -destkeystore ""C:\Users\Indra\myapp\wso2carbon.jks"
    -deststoretype JKS
    Destination keystore password : exportpass123
    ----------------------------------------------------- Final JKS Keystore generation 
    # openssl pkcs12 -export -in "/home/myapp/myapp_cert_2016/ssl_certificate.crt"
    -inkey "/home/myapp/myapp_cert_2016/domainname.key" -certfile "/home/myapp/myapp_cert_2016/ssl_certificate.crt"
    -out myapp_cert.p12
    Export Password : StorePass123
    keytool -importkeystore -srckeystore "C:\Users\Indra\myapp\myapp_cert.p12" -srcstoretype pkcs12
    -destkeystore "C:\Users\Indra\myapp\myapp_keystore.jks" -deststoretype JKS
 
    Import Password : StorePass123
 
    ----------------------------------------------------- Final BKS Keystore generation 
    keytool -importkeystore -srckeystore "C:\Users\Indra\myapp\myapp_keystore.jks -deststoretype JKS"
    -destkeystore "C:\Users\Indra\myapp\myapp_keystore.bks" -srcstoretype JKS -deststoretype BKS
    -srcstorepass StorePass123 -deststorepass StorePass123 -provider org.bouncycastle.jce.provider.BouncyCastleProvider
    -providerpath "C:\Users\Indra\Downloads\bcprov-jdk15on-154.jar"
 
    On error or exception steps to be taken
    - Comment above line and add the new line in java.security file in jre/lib/security
    #security.provider.7=com.sun.security.sasl.Provider
    security.provider.7=org.bouncycastle.jce.provider.BouncyCastleProvider
    - You need to install the Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy

Для генерации и использования AndroidKeyStore возможно использовать вспомогательные библиотеки или же возможно реализовать генерацию и получение ключа самостоятельно. Для примера ниже приведены основные части кода, которые могут потребоваться для реализации шифрования с хранением ключа в AndroidKeyStore.

**Добавление нового ключа в KeyStore**

    public void createNewKeys(View view) {
        String alias = aliasText.getText().toString();
        try {
            // Create new key if needed
            if (!keyStore.containsAlias(alias)) {
                Calendar start = Calendar.getInstance();
                Calendar end = Calendar.getInstance();
                end.add(Calendar.YEAR, 1);
                KeyPairGeneratorSpec spec = new KeyPairGeneratorSpec.Builder(this)
                        .setAlias(alias)
                        .setSubject(new X500Principal("CN=Sample Name, O=Android Authority"))
                        .setSerialNumber(BigInteger.ONE)
                        .setStartDate(start.getTime())
                        .setEndDate(end.getTime())
                        .build();
                KeyPairGenerator generator = KeyPairGenerator.getInstance("RSA", "AndroidKeyStore");
                generator.initialize(spec);
                KeyPair keyPair = generator.generateKeyPair();
            }
        } catch (Exception e) {
            Toast.makeText(this, "Exception " + e.getMessage() + " occured", Toast.LENGTH_LONG).show();
            Log.e(TAG, Log.getStackTraceString(e));
        }
        refreshKeys();
    }

**Удаление ключа из KeyStore**

    public void deleteKey(final String alias) {
        AlertDialog alertDialog =new AlertDialog.Builder(this)
                .setTitle("Delete Key")
                .setMessage("Do you want to delete the key \"" + alias + "\" from the keystore?")
                .setPositiveButton("Yes", new DialogInterface.OnClickListener() {
                    public void onClick(DialogInterface dialog, int which) {
                        try {
                            keyStore.deleteEntry(alias);
                            refreshKeys();
                        } catch (KeyStoreException e) {
                            Toast.makeText(MainActivity.this,
                                    "Exception " + e.getMessage() + " occured",
                                    Toast.LENGTH_LONG).show();
                            Log.e(TAG, Log.getStackTraceString(e));
                        }
                        dialog.dismiss();
                    }
                })
                .setNegativeButton("No", new DialogInterface.OnClickListener() {
                    public void onClick(DialogInterface dialog, int which) {
                        dialog.dismiss();
                    }
                })
                .create();
        alertDialog.show();
    }

**Применения ключа для шифрования**

    public void encryptString(String alias) {
        try {
            KeyStore.PrivateKeyEntry privateKeyEntry = (KeyStore.PrivateKeyEntry)keyStore.getEntry(alias, null);
            RSAPublicKey publicKey = (RSAPublicKey) privateKeyEntry.getCertificate().getPublicKey();
            // Encrypt the text
            String initialText = startText.getText().toString();
            if(initialText.isEmpty()) {
                Toast.makeText(this, "Enter text in the 'Initial Text' widget", Toast.LENGTH_LONG).show();
                return;
            }
            Cipher input = Cipher.getInstance("RSA/CBC/PKCS7Padding", "AndroidOpenSSL");
            input.init(Cipher.ENCRYPT_MODE, publicKey);
            ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
            CipherOutputStream cipherOutputStream = new CipherOutputStream(
                    outputStream, input);
            cipherOutputStream.write(initialText.getBytes("UTF-8"));
            cipherOutputStream.close();
            byte [] vals = outputStream.toByteArray();
            encryptedText.setText(Base64.encodeToString(vals, Base64.DEFAULT));
        } catch (Exception e) {
            Toast.makeText(this, "Exception " + e.getMessage() + " occured", Toast.LENGTH_LONG).show();
            Log.e(TAG, Log.getStackTraceString(e));
        }
    }

**Применения ключа для расшифровки**

    public void decryptString(String alias) {
        try {
            KeyStore.PrivateKeyEntry privateKeyEntry = (KeyStore.PrivateKeyEntry)keyStore.getEntry(alias, null);
            RSAPrivateKey privateKey = (RSAPrivateKey) privateKeyEntry.getPrivateKey();
            Cipher output = Cipher.getInstance("RSA/CBC/PKCS7Padding", "AndroidOpenSSL");
            output.init(Cipher.DECRYPT_MODE, privateKey);
            String cipherText = encryptedText.getText().toString();
            CipherInputStream cipherInputStream = new CipherInputStream(
                    new ByteArrayInputStream(Base64.decode(cipherText, Base64.DEFAULT)), output);
            ArrayList values = new ArrayList<>();
            int nextByte;
            while ((nextByte = cipherInputStream.read()) != -1) {
                values.add((byte)nextByte);
            }
            byte[] bytes = new byte[values.size()];
            for(int i = 0; i < bytes.length; i++) {
                bytes[i] = values.get(i).byteValue();
            }
            String finalText = new String(bytes, 0, bytes.length, "UTF-8");
            decryptedText.setText(finalText);
        } catch (Exception e) {
            Toast.makeText(this, "Exception " + e.getMessage() + " occured", Toast.LENGTH_LONG).show();
            Log.e(TAG, Log.getStackTraceString(e));
        }
    }

## Ссылки

1. [https://developer.android.com/training/articles/keystore#kotlin](https://developer.android.com/training/articles/keystore#kotlin)

2. [https://www.androidauthority.com/use-android-keystore-store-passwords-sensitive-information-623779/](https://www.androidauthority.com/use-android-keystore-store-passwords-sensitive-information-623779/)

3. [https://developer.android.com/guide/topics/security/cryptography](https://developer.android.com/guide/topics/security/cryptography)

4. [https://developer.android.com/reference/androidx/security/crypto/EncryptedFile](https://developer.android.com/reference/androidx/security/crypto/EncryptedFile)

5. [https://security.stackexchange.com/questions/128003/does-the-use-of-a-smartphones-secure-element-really-offer-security-benefits-to](https://security.stackexchange.com/questions/128003/does-the-use-of-a-smartphones-secure-element-really-offer-security-benefits-to)

6. [https://github.com/Q42/Qlassified-Android](https://github.com/Q42/Qlassified-Android)