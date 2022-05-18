# Небезопасная передача sensitive-информации в Service

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
        <td>Способ обнаружения:<strong> DAST, SENSITIVE INFO</strong></td>
      </tr>
    </tbody>
</table>

## Описание

Приложение включает чувствительную информацию в неявный **Intent** для запуска **Service**. Это может привести к перехвату этой информации сторонними приложениями.

Межпроцессное взаимодействие (IPC) в Android осуществляться при помощи специальных объектов — **Intent**. Параметры обработчиков **Intent** задаются в основной файле манифеста приложения — ***AndroidManifest.xml*** либо, в случае с динамическими **BroadcastReceivers**, в коде приложения. В случае, если используется неявные **Intent** (не содержат имени конкретного компонента, вместо этого они в целом объявляют действие, которое требуется выполнить, что дает возможность компоненту из другого приложения обработать этот запрос, например, если требуется показать пользователю место на карте, то с помощью неявного объекта **Intent** можно запросить, чтобы это сделало другое приложение, в котором такая возможность предусмотрена), данные, содержащиеся в таких сообщениях, могут быть скомпрометированы. Кроме того, вредоносными приложениями могут использоваться механизмы делегирования управления процесса, такие как неявные вызовы компонентов приложений или объекты типа **PendingIntent**, для перехвата потока управления и фишинговых атак.

Опасность представляют объекты типов **Activity**, **Service**, **BroadcastReceiver** и **ContentProvider**, открытые для взаимодействия с другими приложениями и не относящиеся к системным Android-вызовам (таким как `android.intent.action.MAIN`). **BroadcastReceiver** по умолчанию открыт для взаимодействия с другими приложениями, в этом случае возможен перехват **Intent** с конфиденциальной информацией или перехват управления.

## Рекомендации

При использовании неявных Intent **нельзя** включать sensitive-информацию в параметры.

Риски при использовании **Service** и соответствующие им защитные меры различаются в зависимости от того, как используется этот **Service**. Для определения типа **Service**, который планируется создавать, необходимо воспользоваться таблицей и диаграммой, представленными ниже.

<figure markdown>
![](../../../rg/img/peredacha-sensitive-informacziya-vo-vnutrennij-service.png)
</figure>

<figure markdown>
![](../../../rg/img/4.png)
</figure>

Существуют различные реализации **Service**. Возможные комбинации реализаций и типов сервисов представлены в следующей таблице. OK — возможное сочетание. Прочерк «—» означает невозможность реализации.

<figure markdown>
![](../../../rg/img/sistema_stingrej_nebezopasnaya-peredacha-sensitive-informaczii-v-service-02.png)
</figure>

В примере будет рассмотрено правильное создание и использование внешнего сервиса. Важно помнить, что **нельзя** передавать конфиденциальную информацию при использовании внешнего публичного **Service**.


!!! note "Внимание!"
    В целях обеспечения безопасности приложения всегда используйте явный объект **Intent** при запуске **Service** и не объявляйте фильтры **Intent** для своих служб. Запуск служб с помощью неявных объектов **Intent** является рискованным с точки зрения безопасности, поскольку нельзя быть на абсолютно уверенным, какая служба отреагирует на такой объект **Intent**, а пользователь не может видеть, какая служба запускается. Начиная с Android 5.0 (уровень API 21) система вызывает исключение при вызове метода **bindService()** с помощью неявного объекта **Intent**.

**Создание и использование public Service**

**Public Service** — это **Service**, который может быть использован любым сторонним приложением. Необходимо понимать, что:

* **Public Service** может получить **Intent** из вредоносного приложения.
* Вредоносное приложение может получить **Intent**, отправленный в **Public Service** и/или считывать из него данные.

**Правила (создание Public Service):**

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

**PublicClientService.java**

    package com.appsec.android.service.publicservice;
    import android.app.IntentService;
    import android.content.Intent;
    import android.widget.Toast;
    public class PublicIntentService extends IntentService{
        /**
        * Когда наследуется класс IntentService, должен быть реализован конструктор по умолчанию, иначе возникнет ошибка
        */
        public PublicIntentService() {
            super("CreatingService");
        }
        @Override
        public void onCreate() {
            super.onCreate();
            
            Toast.makeText(this, this.getClass().getSimpleName() + " - onCreate()", Toast.LENGTH_SHORT).show();
        }
        
        @Override
        protected void onHandleIntent(Intent intent) {        
            // *** 2 *** Проводите проверку и безопасную обработку полученного Intent
            // См.п. "Безопасная обработка входных данных"
            String param = intent.getStringExtra("PARAM");
            Toast.makeText(this, String.format("Recieved parameter \"%s\"", param), Toast.LENGTH_LONG).show();
        }
        @Override
        public void onDestroy() {
            Toast.makeText(this, this.getClass().getSimpleName() + " - onDestroy()", Toast.LENGTH_SHORT).show();
        }
        
    }

**Правила (использование public Service):**

!!! note "Внимание!"
    Чтобы случайно не запустить **Service** другого приложения, всегда используйте явные объекты **Intent** для запуска собственных служб и не объявляйте для них фильтры **Intent**.

1. Используйте явный **Intent**.
2. Не включайте конфиденциальную информацию в отправляемые данные.

        package com.appsec.android.service.publicserviceuser;
        import android.app.Activity;
        import android.content.Intent;
        import android.os.Bundle;
        import android.view.View;
        public class PublicUserActivity extends Activity {
            // Информация о целевом сервисе
            private static final String TARGET_PACKAGE = "com.appsec.android.service.publicservice";
            private static final String TARGET_START_CLASS = "com.appsec.android.service.publicservice.PublicStartService";
            private static final String TARGET_INTENT_CLASS = "com.appsec.android.service.publicservice.PublicIntentService";
            @Override
            public void onCreate(Bundle savedInstanceState) {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.publicservice_activity);
            }
            
            public void onStopServiceClick(View v) {
                doStopService();
            }
                
            // Запуск IntentService
            public void onIntentServiceClick(View v) {      
                Intent intent = new Intent("com.appsec.android.service.publicservice.action.intentservice");
                // *** 1 *** Используйте явный Intent
                intent.setClassName(TARGET_PACKAGE, TARGET_INTENT_CLASS);
                // *** 2 *** Не включайте конфиденциальную информацию в отправляемые данные
                intent.putExtra("PARAM", "Not sensitive information");
                startService(intent);
            }
                
            @Override
            public void onStop(){
                super.onStop();
                doStopService();
            }
            
            private void doStopService() {            
                Intent intent = new Intent("com.appsec.android.service.publicservice.action.startservice");
                // *** 1 *** Используйте явный Intent
                intent.setClassName(TARGET_PACKAGE, TARGET_START_CLASS);
                stopService(intent);    	
            }
        }

## Ссылки

1. [https://developer.android.com/guide/components/intents-filters?hl=ru](https://developer.android.com/guide/components/intents-filters?hl=ru)

2. [https://github.com/OWASP/owasp-mstg/blob/master/Document/0x05h-Testing-Platform-Interaction.md](https://github.com/OWASP/owasp-mstg/blob/master/Document/0x05h-Testing-Platform-Interaction.md)

3. [https://developer.android.com/training/basics/intents/index.html](https://developer.android.com/training/basics/intents/index.html)