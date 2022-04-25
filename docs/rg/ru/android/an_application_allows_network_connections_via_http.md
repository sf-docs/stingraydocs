# Приложение разрешает сетевые соединения по протоколу HTTP

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
        <td>Способ обнаружения:<strong> SAST, MANIFEST</strong></td>
      </tr>
    </tbody>
</table>

## Описание

В настройках AndroidManifest выставлен атрибут `android:usesCleartextTraffic=true`, разрешающий приложению взаимодействие с любыми серверами по незащищенному протоколу http. Данная настройка зависит от нескольких параметров и её значение по умолчанию также зависит от targetSDK, указанный в манифесте приложения:

Если в AndroidManifest присутствует атрибут `android:networkSecurityConfig` — то значение `android:usesCleartextTraffic` не учитывается, так как все настройки сети определяются внутри файла сетевой конфигурации.

1. Если targetSDK =< 27 — дефолтное значение атрибута true (если он не представлен в манифесте).
2. Если targetSDK >= 28 — дефолтное значение атрибута false (если он не представлен в манифесте).

**Пример уязвимой конфигурации:**

    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.appsec.android.activity.privateactivity" >
    <application
    <!-- *** Включенние отладочного режима *** -->
    android:debuggable="true"
    android:icon="@drawable/ic_launcher"
    android:usesCleartextTraffic="true"
    android:label="@string/app_name" >
    <activity
    android:name=".PrivateActivity"
    android:label="@string/app_name"
    android:exported="false" />
    </application>
    </manifest>

## Рекомендации

Рекомендуется явно отключить возможность передачи данных по незащищенному протоколу http, для этого необходимо выставить атрибуту `android:usesCleartextTraffic` значение `false`.

**Пример безопасной конфигрурации:**

    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.appsec.android.activity.privateactivity" >
    <application
    <!-- *** Включенние отладочного режима *** -->
    android:debuggable="true"
    android:icon="@drawable/ic_launcher"
    android:usesCleartextTraffic="false"
    android:label="@string/app_name" >
    <activity
    android:name=".PrivateActivity"
    android:label="@string/app_name"
    android:exported="false" />
    </application>
    </manifest>

## Ссылки

1. [https://developer.android.com/guide/topics/manifest/application-element#usesCleartextTraffic](https://developer.android.com/guide/topics/manifest/application-element#usesCleartextTraffic)

2. [https://imstudio.medium.com/android-8-cleartext-http-traffic-not-permitted-73c1c9e3b803](https://imstudio.medium.com/android-8-cleartext-http-traffic-not-permitted-73c1c9e3b803)