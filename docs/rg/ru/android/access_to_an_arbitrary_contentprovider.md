# Возможность получения доступа к произвольному ContentProvider

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
        <td>Способ обнаружения:<strong> IAST</strong></td>
      </tr>
    </tbody>
</table>

## Описание

Уязвимость позволяет получить доступ к внутренним **неэкспортируемым ContentProvider**.

Уязвимость присутствует в приложениях, которые используют **Intent** из недоверенного источника (например, полученные из стороннего приложения с помощью методов `getIntent`, `getParcelableExtra` или `onActivityResult`) для возврата данных с помощью метода `setResult`.

Например, вредоносное приложение может использовать такой код:

    Intent intent = new Intent();
    intent.setData(Uri.parse("content://com.victim.provider/secret_data.txt"));
    intent.setFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
    intent.setClassName("vuln.app.pkg", "vuln.app.pkg.SomeActivity");
    startActivityForResult(intent, 0);

Целевое уязвимое приложение (***SomeActivity.java***):

    super.onCreate(savedInstanceState);
    setResult(RESULT_OK, getIntent());
    finish();

В результате вредоносное приложение получит доступ к **ContentProvider** `com.victim.provider` уязвимого приложения.

## Рекомендации

Для устранения подобных проблем в приложении необходимо убедиться в соответствии нескольким правилам:

1. Реализовать private/in-house видимость у компонентов, которые принимают **Intent** и используют его в методе `setResult`. Например, объявление **Activity** внутренней — отсутствуют `intent-filter` или флаг `exported` выставлен в значение `false`.

        <?xml version="1.0" encoding="utf-8"?>
        <manifest xmlns:android="http://schemas.android.com/apk/res/android"
            package="com.swordfishsecurity.appsec.android.activity.privateactivity" >
        
            <application
                android:allowBackup="false"
                android:icon="@drawable/ic_launcher"
                android:label="@string/app_name" >
            
                <!-- Private activity -->
                <!-- *** 1 *** Не используйте taskAffinity -->
                <!-- *** 2 *** Не используйте launchMode -->
                <!-- *** 3 *** Явно указывайте атрибут exported="false" -->
                <activity
                    android:name=".PrivateActivity"
                    android:label="@string/app_name"
                    android:exported="false" />
                
                <!-- Public activity запускаемая по умолчанию -->
                <activity
                    android:name=".PrivateUserActivity"
                    android:label="@string/app_name"
                    android:exported="true" >
                    <intent-filter>
                        <action android:name="android.intent.action.MAIN" />
                        <category android:name="android.intent.category.LAUNCHER" />
                    </intent-filter>
                </activity>
            </application>
        </manifest>

2. Проводить валидацию **Intent** на предмет вредоносности:

    1. Такой **Intent** не должен направляться в private/in-house компоненты или компоненты внешних приложений.

            Intent intent = getIntent();
            Intent redirectIntent = (Intent) intent.getParcelableExtra(“redirect_intent”);
            ComponentName name = redirectIntent.resolveActivity(getPackageManager());
            // проверяем целевое имя пакета и класса
            if(name.getPackageName().equals(“safe_package”) && name.getClassName().equals(“safe_class”)) {
            startActivity(redirectIntent);
            }

    2. Если всё же предусмотрен запуск компонент внешних приложений, то нужно проводить валидацию/санитизацию **Permissions** передаваемых в «**to-be-redirected Intent**».    
        Пример валидации:

                Intent resultIntent = (Intent) intent.getParcelableExtra(“result_intent”);
                int flags = resultIntent.getFlags();
                if((flags & Intent.FLAG_GRANT_READ_URI_PERMISSION == 0) && (flags & Intent.FLAG_GRANT_WRITE_URI_PERMISSION == 0)) {
                setResult(RESULT_OK, resultIntent);
                }

        Пример для санитизации:

                resultIntent.removeFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
                resultIntent.removeFlags(Intent.FLAG_GRANT_WRITE_URI_PERMISSION);
                setResult(RESULT_OK, intent);

## Ссылки

1. [https://developer.android.com/guide/topics/manifest/activity-element#exported](https://developer.android.com/guide/topics/manifest/activity-element#exported)

2. [https://blog.oversecured.com/Android-Access-to-app-protected-components/](https://blog.oversecured.com/Android-Access-to-app-protected-components/)