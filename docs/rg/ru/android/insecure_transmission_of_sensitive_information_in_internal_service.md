# Небезопасная передача sensitive-информации во внутренний Service

<table class='noborder'>
    <colgroup>
      <col/>
      <col/>
    </colgroup>
    <tbody>
      <tr>
        <td rowspan="2"><img src="../../../img/defekt_info.png"/></td>
        <td>Критичность:<strong> ИНФО</strong></td>
      </tr>
      <tr>
        <td>Способ обнаружения:<strong> DAST, SENSITIVE INFO</strong></td>
      </tr>
    </tbody>
</table>

## Описание

Приложение включает чувствительную информацию в **Intent** для запуска внутреннего **Service**. В общем случае это не является уязвимостью, но при наличии root-доступа такую информацию можно перехватить.

Межпроцессное взаимодействие (IPC) в Android осуществляться при помощи специальных объектов — **Intent**. Параметры обработчиков **Intent** задаются в основной файле манифеста приложения — ***AndroidManifest.xml*** либо, в случае с динамическими **BroadcastReceivers**, в коде приложения. В случае, если используется неявные **Intent** (не содержат имени конкретного компонента, вместо этого они в целом объявляют действие, которое требуется выполнить, что дает возможность компоненту из другого приложения обработать этот запрос, например, если требуется показать пользователю место на карте, то с помощью неявного объекта **Intent** можно запросить, чтобы это сделало другое приложение, в котором такая возможность предусмотрена), данные, содержащиеся в таких сообщениях, могут быть скомпрометированы. Кроме того, вредоносными приложениями могут использоваться механизмы делегирования управления процесса, такие как неявные вызовы компонентов приложений или объекты типа **PendingIntent**, для перехвата потока управления и фишинговых атак.

Опасность представляют объекты типов **Activity**, **Service**, **BroadcastReceiver** и **ContentProvider**, открытые для взаимодействия с другими приложениями и не относящиеся к системным Android-вызовам (таким как `android.intent.action.MAIN`).  **BroadcastReceiver** по умолчанию открыт для взаимодействия с другими приложениями, в этом случае возможен перехват **Intent** с конфиденциальной информацией или перехват управления.

## Рекомендации

При обращении к внешним публичным **Service** в **Intent нельзя** включать конфиденциальную информацию.

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

В примере будет рассмотрено правильное создание и использование внутреннего сервиса. Важно помнить, что нельзя передавать конфиденциальную информацию при использовании внешнего публичного **Service**.

!!! note "Внимание!"
    В целях обеспечения безопасности приложения всегда используйте явный объект **Intent** при запуске **Service** и не объявляйте фильтры **Intent** для своих служб. Запуск служб с помощью неявных объектов **Intent** является рискованным с точки зрения безопасности, поскольку нельзя быть на абсолютно уверенным, какая служба отреагирует на такой объект **Intent**, а пользователь не может видеть, какая служба запускается. Начиная с Android 5.0 (уровень API 21) система вызывает исключение при вызове метода **bindService()** с помощью неявного объекта **Intent**.

**Создание и использование Private Service**

**Private Service** не может использоваться из других приложений и, поэтому, является наиболее защищённым. Для использования **Private Service** используется явный **Intent** (с указанием имени класса), поэтому нет необходимости беспокоиться о возможности непреднамеренной отправки данных в стороннее приложение.

**Правила (создание Private Service):**

1. Явно указывайте атрибут `exported="false"`.
2. Проводите проверку и безопасную обработку полученного **Intent**, несмотря на то, что он был получен из того же самого приложения.
3. В **Intent** результата можно включать конфиденциальную информацию, т.к. его отправка и получение происходит внутри приложения.

**AndroidManifest.xml**

    <!--?xml version="1.0" encoding="utf-8"?-->
    <manifest xmlns:android="http://schemas.android.com/apk/res/android" package="com.appsec.android.service.privateservice"></manifest>
    <application android:icon="@drawable/ic_launcher" android:label="@string/app_name" android:allowbackup="false">
    <activity android:name=".PrivateUserActivity" android:label="@string/app_name" android:exported="true">
    <intent-filter>
    <action android:name="android.intent.action.MAIN"></action>
    <category android:name="android.intent.category.LAUNCHER"></category>
    </intent-filter>
    </activity></application>
    <!-- Private Service производный от класса Service -->
    <!-- *** 1 *** Явно указывайте атрибут exported="false" -->
    <service android:name=".PrivateStartService" android:exported="false"></service>

**PrivateStartService.java**

    package com.appsec.android.service.privateservice;
    
    import android.app.Service;
    import android.content.Intent;
    import android.os.IBinder;
    import android.widget.Toast;
    
    public class PrivateStartService extends Service {
        
        @Override
        public void onCreate() {
            Toast.makeText(this, "PrivateStartService - onCreate()", Toast.LENGTH_SHORT).show();
        }
    
        @Override
        public int onStartCommand(Intent intent, int flags, int startId) {
            // *** 2 *** Проводите проверку и безопасную обработку полученного Intent, несмотря на то, что он был получен из того же самого приложения
            // См.п. "Безопасная обработка входных данных"
            String param = intent.getStringExtra("PARAM");
            Toast.makeText(this,
                    String.format("PrivateStartService\nПолученные параметры: \"%s\"", param),
                    Toast.LENGTH_LONG).show();
    
            return Service.START_NOT_STICKY;
        }
    
        @Override
        public void onDestroy() {
            Toast.makeText(this, "PrivateStartService - onDestroy()", Toast.LENGTH_SHORT).show();
        }
    
        @Override
        public IBinder onBind(Intent intent) {
            return null;
        }
    }

**Правила (использование Private Service):**

!!! note "Внимание!"
    Чтобы случайно не запустить **Service** другого приложения, всегда используйте явные объекты **Intent** для запуска собственных служб и не объявляйте для них фильтры **Intent**.

1. Используйте явный **Intent** с указанием имени класса **Service** внутри приложения.
2. В передаваемые данные можно включать конфиденциальную информацию, т.к. их отправка и получение происходит внутри приложения.
3. Проводите проверку и безопасную обработку полученных данных результата, несмотря на то, что они были получены из того же самого приложения.

**PrivateUserActivity.java**

    package com.appsec.android.service.privateservice;
    
    import android.app.Activity;
    import android.content.Intent;
    import android.os.Bundle;
    import android.view.View;
    
    public class PrivateUserActivity extends Activity {
    
        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.privateservice_activity);
        }
    
        // StartService
        
        public void onStartServiceClick(View v) {
            // *** 1 *** Используйте явный Intent с указанием имени класса Service внутри приложения
            Intent intent = new Intent(this, PrivateStartService.class);
    
            // *** 2 *** В передаваемые данные можно включать конфиденциальную информацию, т.к. их отправка и получение происходит внутри приложения
            intent.putExtra("PARAM", "Чувствительная ифнормация");
    
            startService(intent);
        }
    
        public void onStopServiceClick(View v) {
            doStopService();
        }
    
        @Override
        public void onStop() {
            super.onStop();
            doStopService();
        }
    
        private void doStopService() {
            // *** 1 *** Используйте явный Intent с указанием имени класса Service внутри приложения
            Intent intent = new Intent(this, PrivateStartService.class);
            stopService(intent);
        }
    }

## Ссылки

1. [https://developer.android.com/guide/components/intents-filters?hl=ru](https://developer.android.com/guide/components/intents-filters?hl=ru)

2. [https://github.com/OWASP/owasp-mstg/blob/master/Document/0x05h-Testing-Platform-Interaction.md](https://github.com/OWASP/owasp-mstg/blob/master/Document/0x05h-Testing-Platform-Interaction.md)

3. [https://developer.android.com/training/basics/intents/index.html](https://developer.android.com/training/basics/intents/index.html)