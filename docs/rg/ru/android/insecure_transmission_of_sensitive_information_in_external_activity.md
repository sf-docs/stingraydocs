# Небезопасная передача sensitive-информации во внешнюю Activity

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

Приложение включает чувствительную информацию в **Intent** для запуска внешней **Activity**. Это может привести к перехвату этой информации сторонними приложениями.

Межпроцессное взаимодействие (IPC) в Android осуществляться при помощи специальных объектов — **Intent**. Параметры обработчиков **Intent** задаются в основной файле манифеста приложения — ***AndroidManifest.xml*** либо, в случае с динамическими **BroadcastReceivers**, в коде приложения. В случае, если используется неявные **Intent** (не содержат имени конкретного компонента, вместо этого они в целом объявляют действие, которое требуется выполнить, что дает возможность компоненту из другого приложения обработать этот запрос, например, если требуется показать пользователю место на карте, то с помощью неявного объекта **Intent** можно запросить, чтобы это сделало другое приложение, в котором такая возможность предусмотрена), данные, содержащиеся в таких сообщениях, могут быть скомпрометированы. Кроме того, вредоносными приложениями могут использоваться механизмы делегирования управления процесса, такие как неявные вызовы компонентов приложений или объекты типа **PendingIntent**, для перехвата потока управления и фишинговых атак.

Опасность представляют объекты типов **Activity**, **Service**, **BroadcastReceiver** и **ContentProvider**, открытые для взаимодействия с другими приложениями и не относящиеся к системным Android-вызовам (таким как `android.intent.action.MAIN`). **BroadcastReceiver** по умолчанию открыт для взаимодействия с другими приложениями, в этом случае возможен перехват **Intent** с конфиденциальной информацией или перехват управления.

## Рекомендации

При вызове внешних (не относящихся к разрабатываемому приложению) **Activity нельзя** включать sensitive-информацию в параметры. Это может привести к компрометации такой информации.

Риски при использовании **Activity** и соответствующие им защитные меры различаются в зависимости от того, как используется эта **Activity**. Выделяются 4 типа **Activity** в зависимости от способов использования. Для определения типа **Activity**, которую планируется создавать, необходимо воспользоваться таблицей и диаграммой, представленными ниже.

<figure markdown>
![](../../../rg/img/sistema_stingrej_nebezopasnaya-peredacha-sensitive-informaczii-v-activity.png)
</figure>

<figure markdown>
![](../../../rg/img/2.png)
</figure>

**Создание и использование Public Activity**

В качестве примера будет разобран процесс создания и использования **Public Activity**.

**Public Activity** — это **Activity**, которая может быть использована любым сторонним приложением. Необходимо понимать, что:

* **Public Activity** может получить **Intent** из вредоносного приложения.
* Вредоносное приложение может получить **Intent**, отправленный в **Public Activity** и/или считывать из него данные.

**Правила (создание Public Activity):**

1. Явно указывайте атрибут **exported="true"**.
2. Проводите проверку и безопасную обработку полученного **Intent**.
3. Не включайте в **Intent** результата чувствительную информацию.

**AndroidManifest.xml**

    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.appsec.android.activity.publicactivity" >
    <application
    android:allowBackup="false"
    android:icon="@drawable/ic_launcher"
    android:label="@string/app_name" >
    <!-- Public Activity -->
    <!-- *** 1 *** Явно указывайте атрибут exported="true" -->
    <activity
    android:name=".PublicActivity"
    android:label="@string/app_name"
    android:exported="true">
    <!-- Обьявление intent фильтра для получения неявных Intent'ов с определённым Action -->
    <intent-filter>
    <action android:name="com.appsec.android.activity.MY_ACTION" />
    <category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
    </activity>
    </application>
    </manifest>

**PublicActivity.java**

    package com.appsec.android.activity.publicactivity;
    import android.app.Activity;
    import android.content.Intent;
    import android.os.Bundle;
    import android.view.View;
    import android.widget.Toast;
    public class PublicActivity extends Activity {
      @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.main);
            
            // *** 2 *** Проводите проверку и безопасную обработку полученного Intent
            // Т.к. это Public Activity, то возможно что отправителем Intent'a является вредоносное приложение
            // См.п. "Безопасная обработка входных данных"
        
        String param = getIntent().getStringExtra("PARAM");
          Toast.makeText(this, String.format("Received param: \"%s\"", param), Toast.LENGTH_LONG).show();
      }
      public void onReturnResultClick(View view) {
        
        // *** 3 *** Не включайте в Intent результата чувствительную информацию
        // Т.к. это Public Activity, то возможно что получателем Intent'a будет вредоносное приложение
        Intent intent = new Intent();
        intent.putExtra("RESULT", "Not Sensitive Info");
        setResult(RESULT_OK, intent);
        finish();
      }
    }

**Правила (использование Public Activity):**

1. Не включайте конфиденциальную информацию в **Intent**, используемый для запуска **Activity**.
2. Проводите проверку и безопасную обработку полученных данных результата.

**PublicUserActivity.java**

    package com.appsec.android.activity.publicuser;
    import android.app.Activity;
    import android.content.ActivityNotFoundException;
    import android.content.Intent;
    import android.os.Bundle;
    import android.view.View;
    import android.widget.Toast;
    public class PublicUserActivity extends Activity {
        private static final int REQUEST_CODE = 1;
        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.main);
        }
        
        public void onUseActivityClick(View view) {
          
          try {
            // *** 1 *** Не включайте конфиденциальную информацию в Intent, используемый для запуска Activity
              Intent intent = new Intent("org.jssec.android.activity.MY_ACTION");
              intent.putExtra("PARAM", "Not Sensitive Info");
              startActivityForResult(intent, REQUEST_CODE);
          } catch (ActivityNotFoundException e) {
              Toast.makeText(this, "Target activity not found.", Toast.LENGTH_LONG).show();
          }
        }
        @Override
        public void onActivityResult(int requestCode, int resultCode, Intent data) {
            super.onActivityResult(requestCode, resultCode, data);
            // *** 2 *** Проводите проверку и безопасную обработку полученных данных результата
            // См.п. "Безопасная обработка входных данных"
            
        if (resultCode != RESULT_OK) return;
        switch (requestCode) {
        case REQUEST_CODE:
          String result = data.getStringExtra("RESULT");
              Toast.makeText(this, String.format("Received result: \"%s\"", result), Toast.LENGTH_LONG).show();
          break;
        }
      }
    }

## Ссылки

1. [https://developer.android.com/guide/components/intents-filters?hl=ru](https://developer.android.com/guide/components/intents-filters?hl=ru)

2. [https://github.com/OWASP/owasp-mstg/blob/master/Document/0x05h-Testing-Platform-Interaction.md](https://github.com/OWASP/owasp-mstg/blob/master/Document/0x05h-Testing-Platform-Interaction.md)

3. [https://developer.android.com/training/basics/intents/index.html](https://developer.android.com/training/basics/intents/index.html)