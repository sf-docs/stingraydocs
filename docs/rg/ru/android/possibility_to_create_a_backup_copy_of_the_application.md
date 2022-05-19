# Возможность создания резервной копии приложения

<table class='noborder'>
    <colgroup>
      <col/>
      <col/>
    </colgroup>
    <tbody>
      <tr>
        <td rowspan="2"><img src="../../../img/defekt_srednij.png"/></td>
        <td>Критичность:<strong> СРЕДНИЙ</strong></td>
      </tr>
      <tr>
        <td>Способ обнаружения:<strong> DAST, APK</strong></td>
      </tr>
    </tbody>
</table>

## Описание

Приложение Android, собранное с включенной опцией [создания резервной копии](https://developer.android.com/guide/topics/manifest/application-element#allowbackup) (флаг `android:allowBackup = True` в ***AndroidManifest.xml***), может позволить злоумышленнику, имея физический доступ к устройству, создать backup приложения со всеми данными, хранящимися во внутренней директории приложения. Помимо доступа к информации возможно изменить информацию, содержащуюся в файлах и восстановить настройки из измененного backup. Такой вариант эксплуатации, в ряде случаев, может повлечь за собой компрометацию пользовательских данных.

Стоит иметь ввиду, что по умолчанию данная опция включена.

**Пример уязвимого кода (файл AndroidManifest.xml):**

    <?xml version=»1.0″ encoding=»utf-8″?>
    <manifest xmlns:android=»http://schemas.android.com/apk/res/android»
    package=»com.appsec.android.activity.privateactivity» >
    <application
    <!— *** Включенние возможности создания резервной копии *** —>
    android:allowBackup=»true»
    android:icon=»@drawable/ic_launcher»
    android:label=»@string/app_name» >
    <activity
    android:name=».PrivateActivity»
    android:label=»@string/app_name»
    android:exported=»false» />
    </application>
    </manifest>

**Пример команды, которая создаст резервную копию приложения:**

    adb backup -f com.appsec.android

Далее при помощи скриптов или утилиты [android-backup-extractor](https://github.com/nelenkov/android-backup-extractor) необходимо преобразовать из формата Android Backup в обычный архив и получить доступ к данным. Либо, можно воспользоваться альтернативной командой:

    dd if=mybackup.ab bs=24 skip=1| openssl zlib -d > mybackup.tar
    # Или воспользоваться abe
    abe unpack mybackup.ab mybackup.tar

При необходимости, можно изменить содержимое файлов, той же утилитой [android-backup-extractor](https://github.com/nelenkov/android-backup-extractor) сформировать архив типа Android Backup и восстановить на устройстве:

    abe pack mybackup.tar mybackup.adb restore mybackup.ab

## Рекомендации

При сборке релизной версии приложения убедитесь, что отключена возможность создания резервной копии. Отключить эту возможность можно установив для атрибута `android:allowBackup` значение `false` в файле манифеста.

**Пример безопасного кода:**

    <?xml version=»1.0″ encoding=»utf-8″?>
    <manifest xmlns:android=»http://schemas.android.com/apk/res/android»
    package=»com.appsec.android.activity.privateactivity» >
    <application
    <!— *** Выключенние возможности создания резервной копии *** —>
    android:allowBackup=»false»
    android:icon=»@drawable/ic_launcher»
    android:label=»@string/app_name» >
    <activity
    android:name=».PrivateActivity»
    android:label=»@string/app_name»
    android:exported=»false» />
    </application>
    </manifest>

## Ссылки

1. [https://developer.android.com/guide/topics/manifest/application-element#allowbackup](https://developer.android.com/guide/topics/manifest/application-element#allowbackup)

2. [https://github.com/nelenkov/android-backup-extractor](https://github.com/nelenkov/android-backup-extractor)

3. [https://securitygrind.com/exploiting-android-backup/](https://securitygrind.com/exploiting-android-backup/)

4. [https://github.com/OWASP/owasp-stg/blob/master/Document/0x05d-Testing-Data-Storage.md#local](https://github.com/OWASP/owasp-stg/blob/master/Document/0x05d-Testing-Data-Storage.md#local)

5. [https://resources.infosecinstitute.com/topic/android-hacking-security-part-15-hacking-android-apps-using-backup-techniques/](https://resources.infosecinstitute.com/topic/android-hacking-security-part-15-hacking-android-apps-using-backup-techniques/)