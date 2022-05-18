# Небезопасные настройки в AndroidManifest.xml. Флаг android:requestLegacyExternalStorage

<table class='noborder'>
    <colgroup>
      <col/>
      <col/>
    </colgroup>
    <tbody>
      <tr>
        <td rowspan="2"><img src="../../../img/defekt_nizkij.png"/></td>
        <td>Критичность:<strong> НИЗКИЙ</strong></td>
      </tr>
      <tr>
        <td>Способ обнаружения:<strong> SAST, APK</strong></td>
      </tr>
    </tbody>
</table>

## Описание

Приложение Android, собранное с атрибутом `android:requestLegacyExternalStorage=true` в ***AndroidManifest.xml***, предоставляет доступ к каталогам и различным типам мультимедийных файлов, хранящихся во внешнем хранилище. Этот флаг используется в старой модели доступа к файлам, которая не поддерживается в новых версиях Android.

1. Если в AndroidManifest присутствует атрибут `android:requestLegacyExternalStorage=true` и `targetSDK >=30`, то он игнорируется системой, так как, начиная с Android 11, поддерживается только [scoped-хранилище данных](https://developer.android.com/about/versions/11/privacy/storage). 
2. Если `targetSDK = 29` — дефолтное значение атрибута **false** (если он не представлен в манифесте).
3. Если `targetSDK >= 28` — дефолтное значение атрибута **true** (если он не представлен в манифесте).

**Пример уязвимой конфигурации (файл AndroidManifest.xml):**

    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
        package="com.appsec.android.activity.privateactivity" >
    
        <application
            android:icon="@drawable/ic_launcher"
            android:requestLegacyExternalStorage="true"
            android:label="@string/app_name" >
            
            <activity
                android:name=".PrivateActivity"
                android:label="@string/app_name"
                android:exported="false" />
                
        </application>
    </manifest>

## Рекомендации

Рекомендуется не устанавливать атрибут `android:requestLegacyExternalStorage` и использовать только scoped-хранилище, чтобы гарантировать лучшую защиту приложений и пользовательских данных на внешнем хранилище.

## Ссылки

1. [https://developer.android.com/training/data-storage/use-cases](https://developer.android.com/training/data-storage/use-cases)

2. [https://commonsware.com/blog/2019/06/07/death-external-storage-end-saga.html](https://commonsware.com/blog/2019/06/07/death-external-storage-end-saga.html)

3. [https://medium.com/mindful-engineering/scoped-storage-in-android-d52460630d6a](https://medium.com/mindful-engineering/scoped-storage-in-android-d52460630d6a)

4. [https://blog.mindorks.com/understanding-the-scoped-storage-in-android](https://blog.mindorks.com/understanding-the-scoped-storage-in-android)