# Передача sensitive информации в HTTP-запросе

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
        <td>Способ обнаружения:<strong> DAST, NETWORKING</strong></td>
      </tr>
    </tbody>
</table>

## Описание

Использование HTTP вместо HTTPS позволяет реализовать атаку «человек посередине». Это может привести к полной утрате конфиденциальности передаваемых данных. Следует учитывать, что все данные, передаваемые по протоколу HTTP, передаются полностью в plain text и абсолютно никак не защищены. Любой, кто находится в одной сети с Вами может получить эти данные, а при определенном опыте и подменить.

Использование протокола HTTPS, основанного на HTTP и SSL / TLS, позволяет защитить передаваемые данные от несанкционированного доступа и изменения. Рекомендуется использовать HTTPS для всех случаев передачи ценной информации между клиентом и сервером, в частности, для страницы логина и всех страниц, требующих аутентификации.

## Рекомендации

В идеале необходимо полностью отказаться от использования не зашифрованного трафика в приложении. В случае если это сделать проблематично или есть необходимость использовать какие-то сторонние сервисы по протоколу HTTP обратите отдельное внимание на проверку и валидацию полученных данных и никогда не передавайте по такому протоколу конфиденциальную информацию.

В случае, если необходимо выбрать, как будет осуществляться передача данных, можно руководствоваться следующей схемой:

<figure markdown>
![](../../img/5.png)
</figure>

Сравнение HTTP и HTTPS:

<figure markdown>
![](../../img/sistema_stingrej_vklyuchenie_chuvstvitelnoj_informaczii_v_https_zapros_sravnenie-http-i-https.png)
</figure>

Android использует **java.net.HttpURLConnection/javax.net.ssl.HttpsURLConnection** в качестве API для организации канала связи с помощью протоколов HTTP/HTTPS. Поддержка Apache HttpClient прекращена начиная с Android 6.0 (API 23).

!!! note "Внимание!"
    Для организации канала по HTTPS не стоит использовать класс SSLSocket, т.к. он, в отличие от HttpsURLConnection, по умолчанию не проверяет соответствие имени сервера и имени хоста, указанного в сертификате. Кроме того, делая такую реализацию разработчики часто допускают ошибки, которые приводят к дефектам безопасности в канале связи.

**Использование HTTPS с SSL-пиннингом**

Приложение может дополнительно защитить себя от мошеннических сертификатов с помощью технологии, известной как SSL-pinning. Она предотвращает компрометацию сертификата доверенного удостоверяющего центра в системном хранилище, что делает практически невозможным нарушение безопасности канала передачи данных приложения.

Правила:

* Сверяйте сертификат сервера с сохранённым в приложении.
* Схема URI должна быть https://.
* В передаваемые данные можно включать чувствительную информацию.
* Полученным данным можно доверять, т.к. они получены от подлинного сервера.
* Обрабатывайте ошибки SSL надлежащим образом.

**Пример корректной реализации SSL-Pinning**

**PrivateCertificateHttpsGet.java**

    package com.appsec.android.https.privatecertificate;
    
    import java.io.BufferedInputStream;
    import java.io.ByteArrayOutputStream;
    import java.io.IOException;
    import java.net.HttpURLConnection;
    import java.net.URL;
    import java.security.KeyStore;
    import java.security.SecureRandom;
    
    import javax.net.ssl.HostnameVerifier;
    import javax.net.ssl.HttpsURLConnection;
    import javax.net.ssl.SSLContext;
    import javax.net.ssl.SSLException;
    import javax.net.ssl.SSLSession;
    import javax.net.ssl.TrustManagerFactory;
    
    import android.content.Context;
    import android.os.AsyncTask;
    
    public abstract class PrivateCertificateHttpsGet extends AsyncTask {
    
        private Context mContext;
    
        public PrivateCertificateHttpsGet(Context context) {
            mContext = context;
        }
    
        @Override
        protected Object doInBackground(String... params) {
            TrustManagerFactory trustManager;
            BufferedInputStream inputStream = null;
            ByteArrayOutputStream responseArray = null;
            byte[] buff = new byte[1024];
            int length;
    
            try {
                URL url = new URL(params[0]);
                // *** 1 *** Сверяйте сертификат сервера с сохранённым в приложении
                // Настраиваем keystore для установки соединений таким образом, чтобы он включал только сертификат из ресурсов приложения
                KeyStore ks = KeyStoreUtil.getEmptyKeyStore();
                KeyStoreUtil.loadX509Certificate(ks,
                        mContext.getResources().getAssets().open("cacert.crt"));
    
                // *** 2 *** Схема URI должна быть https://
                // *** 3 *** В передаваемые данные можно включать чувствительную информацию
                trustManager = TrustManagerFactory.getInstance(TrustManagerFactory.getDefaultAlgorithm());
                trustManager.init(ks);
                SSLContext sslCon = SSLContext.getInstance("TLS");
                sslCon.init(null, trustManager.getTrustManagers(), new SecureRandom());
    
                HttpURLConnection con = (HttpURLConnection)url.openConnection();
                HttpsURLConnection response = (HttpsURLConnection)con;
                response.setDefaultSSLSocketFactory(sslCon.getSocketFactory());
    
                response.setSSLSocketFactory(sslCon.getSocketFactory());
                checkResponse(response);
    
                // *** 4 *** Полученным данным можно доверять, т.к. они получены от подлинного сервера
                inputStream = new BufferedInputStream(response.getInputStream());
                responseArray = new ByteArrayOutputStream();
                while ((length = inputStream.read(buff)) != -1) {
                    if (length > 0) {
                        responseArray.write(buff, 0, length);
                    }
                }
                return responseArray.toByteArray();
            } catch(SSLException e) {
                // *** 5 *** Обрабатывайте ошибки SSL надлежащим образом
                // Пропускаем, т.к. это пример
                return e;
            } catch(Exception e) {
                return e;
            } finally {
                if (inputStream != null) {
                    try {
                        inputStream.close();
                    } catch (Exception e) {
                        // Пропускаем, т.к. это пример
                    }
                }
                if (responseArray != null) {
                    try {
                        responseArray.close();
                    } catch (Exception e) {
                        // Пропускаем, т.к. это пример
                    }
                }
            }
        }
    
        private void checkResponse(HttpURLConnection response) throws IOException {
            int statusCode = response.getResponseCode();
            if (HttpURLConnection.HTTP_OK != statusCode) {
                throw new IOException("HttpStatus: " + statusCode);
            }
        }
    }

**KeyStoreUtil.java**

    package com.appsec.android.https.privatecertificate;
    
    import java.io.IOException;
    import java.io.InputStream;
    import java.security.KeyStore;
    import java.security.KeyStoreException;
    import java.security.NoSuchAlgorithmException;
    import java.security.cert.Certificate;
    import java.security.cert.CertificateException;
    import java.security.cert.CertificateFactory;
    import java.security.cert.X509Certificate;
    import java.util.Enumeration;
    
    public class KeyStoreUtil {
        public static KeyStore getEmptyKeyStore() throws KeyStoreException,
                NoSuchAlgorithmException, CertificateException, IOException {
            KeyStore ks = KeyStore.getInstance("BKS");
            ks.load(null);
            return ks;
        }
    
        public static void loadAndroidCAStore(KeyStore ks)
                throws KeyStoreException, NoSuchAlgorithmException,
                CertificateException, IOException {
            KeyStore aks = KeyStore.getInstance("AndroidCAStore");
            aks.load(null);
            Enumeration aliases = aks.aliases();
            while (aliases.hasMoreElements()) {
                String alias = aliases.nextElement();
                Certificate cert = aks.getCertificate(alias);
                ks.setCertificateEntry(alias, cert);
            }
        }
        
        public static void loadX509Certificate(KeyStore ks, InputStream is)
                throws CertificateException, KeyStoreException {
            try {
                CertificateFactory factory = CertificateFactory.getInstance("X509");
                X509Certificate x509 = (X509Certificate)factory.generateCertificate(is);
                String alias = x509.getSubjectDN().getName();
                ks.setCertificateEntry(alias, x509);
            } finally {
                try { is.close(); } catch (IOException e) { /* Пропускаем, т.к. это пример*/ }
            }
        }
    }

**PrivateCertificateHttpsActivity.java**

    package com.appsec.android.https.privatecertificate;
    
    import android.app.Activity;
    import android.graphics.Bitmap;
    import android.graphics.BitmapFactory;
    import android.os.AsyncTask;
    import android.os.Bundle;
    import android.view.View;
    import android.widget.EditText;
    import android.widget.ImageView;
    import android.widget.TextView;
    
    public class PrivateCertificateHttpsActivity extends Activity {
    
        private EditText mUrlBox;
        private TextView mMsgBox;
        private ImageView mImgBox;
        private AsyncTask mAsyncTask ;
    
        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            
            mUrlBox = (EditText)findViewById(R.id.urlbox);
            mMsgBox = (TextView)findViewById(R.id.msgbox);
            mImgBox = (ImageView)findViewById(R.id.imageview);
        }
        
        @Override
        protected void onPause() {
            if (mAsyncTask != null) mAsyncTask.cancel(true);
            super.onPause();
        }
        
        public void onClick(View view) {
            String url = mUrlBox.getText().toString();
            mMsgBox.setText(url);
            mImgBox.setImageBitmap(null);
            
            if (mAsyncTask != null) mAsyncTask.cancel(true);
            
            mAsyncTask = new PrivateCertificateHttpsGet(this) {
                @Override
                protected void onPostExecute(Object result) {
                    if (result instanceof Exception) {
                        Exception e = (Exception)result;
                        mMsgBox.append("\nException occurs\n" + e.toString());
                    } else {
                        byte[] data = (byte[])result;
                        Bitmap bmp = BitmapFactory.decodeByteArray(data, 0, data.length);
                        mImgBox.setImageBitmap(bmp);
                    }
                }
            }.execute(url);
        }
    }

**Использование современного подхода — Network Security Configuration**

Платформа Android предоставляет новый простой инструмент для настройки сети — Network Security Configuration (NSC). Он доступен с Android 7.0. С помощью NSC можно производить настройку сетевых соединений, в том числе и SSL-Pinning, с использованием файлов XML. Чтобы включить конфигурацию, необходимо связать файл конфигурации с манифестом приложения. Для того, что это сделать, используйте атрибут `networkSecurityConfig` в теге `application`.

1. Создать файл с конфигурацией

        res/xml/network_security_config.xml

2. Добавить атрибут `android:networkSecurityConfig` с указанием расположения файла в ***AndroidManifest.xml***:

        <?xml version="1.0" encoding="utf-8"?>
        <manifest
        xmlns:android="http://schemas.android.com/apk/res/android"
        package="co.netguru.demoapp">
        <application
        android:networkSecurityConfig="@xml/network_security_config">
        ...
        </application>
        </manifest>

3. Настройте файл конфигурации и добавьте отпечатки сертификатов

        <?xml version="1.0" encoding="utf-8"?>
        <network-security-config>
        <domain-config>
        <domain includeSubdomains="true">example.com</domain>
        <pin-set>
        <pin digest="SHA-256">ZC3lTYTDBJQVf1P2V7+fibTqbIsWNR/X7CWNVW+CEEA=</pin>
        <pin digest="SHA-256">GUAL5bejH7czkXcAeJ0vCiRxwMnVBsDlBMBsFtfLF8A=</pin>
        </pin-set>
        </domain-config>
        </network-security-config>

Этот метод чрезвычайно прост в реализации. Однако имейте в виду, что он доступен только для API уровня 24 или выше.

## Ссылки

1. [http://thedifference.ru/chem-otlichaetsya-http-ot-https/](http://thedifference.ru/chem-otlichaetsya-http-ot-https/)

2. [https://habrahabr.ru/post/252507/](https://habrahabr.ru/post/252507/)

3. [https://www.owasp.org/index.php/Transport_Layer_Protection_Cheat_Sheet](https://www.owasp.org/index.php/Transport_Layer_Protection_Cheat_Sheet)

4. [http://mashable.com/2011/05/31/https-web-security/](http://mashable.com/2011/05/31/https-web-security/)

5. [https://medium.com/@appmattus/android-security-ssl-pinning-1db8acb6621e](https://medium.com/@appmattus/android-security-ssl-pinning-1db8acb6621e)

6. [https://developer.android.com/training/articles/security-ssl](https://developer.android.com/training/articles/security-ssl)

7. [https://www.netguru.com/codestories/3-ways-how-to-implement-certificate-pinning-on-android](https://www.netguru.com/codestories/3-ways-how-to-implement-certificate-pinning-on-android)

8. [https://developer.android.com/training/articles/security-config](https://developer.android.com/training/articles/security-config)