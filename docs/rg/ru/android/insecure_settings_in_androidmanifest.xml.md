# Небезопасные настройки в AndroidManifest.xml

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
        <td>Способ обнаружения:<strong> SAST, APK</strong></td>
      </tr>
    </tbody>
</table>

## Описание

Приложение Android, собранное с включенным [режимом отладки](https://developer.android.com/guide/topics/manifest/application-element#debug) (флаг `android:debuggable = True` в `AndroidManifest.xml`) может позволить злоумышленнику получить доступ к конфиденциальной информации, контролировать поток выполнения приложения и также получить возможность выполнение кода в контексте приложения.

**Пример уязвимого кода (файл AndroidManifest.xml)**

    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
            package="com.appsec.android.activity.privateactivity" >
            <application
                    <!-- *** Включенние отладочного режима *** -->
                android:debuggable="true"
                android:icon="@drawable/ic_launcher"
                android:label="@string/app_name" >
                <activity
                        android:name=".PrivateActivity"
                        android:label="@string/app_name"
                        android:exported="false" />
            </application>
    </manifest>

## Рекомендации

При сборке релизной версии приложения убедитесь, что отключена возможность отладки приложения. Отключить возможность отладки можно удалив атрибут `android:debuggable` из тега **<application\>** в файле манифеста или установив для атрибута `android:debuggable` значение `false` в файле манифеста.

**Пример безопасного кода**

    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.appsec.android.activity.privateactivity" >
    <application
    <!-- *** Выключенние отладочного режима *** -->
    android:debuggable="false"
    android:icon="@drawable/ic_launcher"
    android:label="@string/app_name" >
    <activity
    android:name=".PrivateActivity"
    android:label="@string/app_name"
    android:exported="false" />
    </application>
    </manifest>
Также возможно установить настройку отладочного режима для различных вариантов сборки через конфигурацию в файле `build.gradle`:

    android {
        defaultConfig {
            ...
            ...
        }
        buildTypes {
            release {
                // *** Отключение отладочного режима для релизной сборки приложения *** //
                debuggable false
                minifyEnabled true
                proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            }
            debug {
                applicationIdSuffix ".debug"
                // *** Включение отладочного режима для целей разработки *** //
                debuggable true
            }

## Ссылки

1. [https://developer.android.com/studio/build/build-variants#build-types](https://developer.android.com/studio/build/build-variants#build-types)

2. [https://developer.android.com/studio/publish/preparing#publishing-configure](https://developer.android.com/studio/publish/preparing#publishing-configure)

3. [https://resources.infosecinstitute.com/android-hacking-security-part-6-exploiting-debuggable-android-applications/](https://resources.infosecinstitute.com/android-hacking-security-part-6-exploiting-debuggable-android-applications/)

4. [https://securitygrind.com/how-to-exploit-a-debuggable-android-application/](https://securitygrind.com/how-to-exploit-a-debuggable-android-application/)

5. [https://github.com/OWASP/owasp-mstg/blob/master/Document/0x05i-Testing-Code-Quality-and-Build-Settings.md](https://github.com/OWASP/owasp-mstg/blob/master/Document/0x05i-Testing-Code-Quality-and-Build-Settings.md)