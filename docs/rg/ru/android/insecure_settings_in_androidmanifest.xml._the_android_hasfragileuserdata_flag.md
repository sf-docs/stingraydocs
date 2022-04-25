# Небезопасные настройки в AndroidManifest.xml. Флаг android:hasFragileUserData

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

Атрибут `android:hasFragileUserData` определяет, показывать ли пользователю запрос на сохранение данных приложения, когда он его удаляет. Значение по умолчанию — `false`. Работает с `android>=10` (версия `sdk: compileSdkVersion>=29`).

Приложение, собранное без явно определенного флага `android:hasFragileUserData` в ***AndroidManifest.xml***, не дает понять, обрабатывает ли оно важные пользовательские данные или нет. Если флаг выставлен со значением `true`, но приложение хранит важные данные, есть риск оставить их на устройстве после удаления. Для этого пользователь должен включить тумблер `Keep app data` при удалении приложения.

<figure markdown>
![](../../img/image13.png)
</figure>

В результате данные как во внутреннем хранилище (`/data/data/<package_name>`), так и во внешнем (в общем случае `/storage/sdcard0/Android/data/<package_name>/`) не будут удалены.

Удобно использовать флаг `android:hasFragileUserData`, если необходимо временно удалить приложение с устройства, а впоследствии установить его обратно и получить такое же состояние.

Однако злоумышленник, установивший переподписанное apk с таким же именем пакета, будет иметь доступ к этим данным.

## Рекомендации

Рекомендуется явно указать данное значение, чтобы однозначно определить, обрабатывает ли приложение важные пользовательские данные или нет.

**Пример безопасного кода (файл AndroidManifest.xml)**

    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
        package="com.appsec.android.activity.privateactivity" >
    
        <application
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:hasFragileUserData="false" >        
        </application>
    </manifest>

## Ссылки

1. [https://developer.android.com/guide/topics/manifest/application-element](https://developer.android.com/guide/topics/manifest/application-element)

2. [https://gist.github.com/agnostic-apollo/58def38fdb1c90563454974108ae52df](https://gist.github.com/agnostic-apollo/58def38fdb1c90563454974108ae52df)

3. [https://www.xda-developers.com/android-10-manifest-flag-developers-retain-app-data-before-uninstalling/](https://www.xda-developers.com/android-10-manifest-flag-developers-retain-app-data-before-uninstalling/)