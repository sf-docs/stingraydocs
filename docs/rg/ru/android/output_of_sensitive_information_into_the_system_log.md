# Вывод sensitive-информации в системный лог

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

Android предоставляет приложениям возможность выводить информацию в системный журнал. Приложения могут отправлять информацию в журнал, используя класс `android.util.Log`.

До Android 4.0 любое приложение с разрешением **READ_LOGS** могло получать доступ ко всему системному логу (включая системные логи и логи других приложений). После Android 4.1 спецификация разрешения **READ_LOGS** была изменена, и приложение может получить доступ только к своим данным. Однако, подключив устройство Android к ПК, можно получить вывод системного журнала из других приложений.

Поэтому важно, чтобы приложения не отправляли конфиденциальную информацию для вывода в системный журнал.

Класс `android.util.Log` предоставляет ряд возможностей для вывода информации:

* Log.d (Debug).

* Log.e (Error).

* Log.i (Info).

* Log.v (Verbose).

* Log.w (Warn).

Так же возможно использование аналогичных по функциональности библиотек (Одной из популярных является [Timber](https://github.com/JakeWharton/timber)). 

**Пример уязвимого кода**

    Log.d("authorize", "Login Success! access_token="
        + getAccessToken() + " expires="
        + getAccessExpires());

## Рекомендации

Перед публикацией приложения необходимо убедиться, что в системный журнал не попадает конфиденциальная информация. Также если приложение использует сторонние библиотеки, необходимо удостовериться, что библиотека так же не отправляет конфиденциальную информацию и сконфигурирована соответствующим образом (подключена релизная версия библиотеки или выставлены правильные атрибуты).

Одним из распространенных решений является объявление и использование пользовательского класса логирования, для автоматического включения / выключения вывода информации в системный журнал в зависимости от типа сборки (release / debug).

    if (BuildConfig.DEBUG) {
            ...
            serverEditText.setText("http://test.test");
            loginEditText.setText("user_test");
            passwordEditText.setText("12345");
            ...
        }

Так же хорошей практикой является использование ProGuard для удаления определенных вызовов логирования. Для исключения из релизной сборки логирования из библиотек **Timber** и **android.util.Log**.

**Пример настроек Proguard**

    -assumenosideeffects class android.util.Log {
        public static boolean isLoggable(java.lang.String, int);
        public static int v(...);
        public static int i(...);
        public static int w(...);
        public static int d(...);
        public static int e(...);
    }
    -assumenosideeffects class timber.log.Timber* {
        public static *** v(...);
        public static *** d(...);
        public static *** i(...);
        public static *** e(...);
        public static *** w(...);
    }

**Включение использования Proguard для релизной сборки приложения**

    buildTypes {
        releaseSomeBuildType {
            ...
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'your-proguard-file.pro'
        }
    } 

## Ссылки

1.  [https://www.owasp.org/index.php/Poor_Logging_Practice](https://www.owasp.org/index.php/Poor_Logging_Practice)

2.  [https://cwe.mitre.org/data/definitions/778.html](https://cwe.mitre.org/data/definitions/778.html)

3.  [https://source.android.com/setup/contribute/code-style#log-sparingly](https://source.android.com/setup/contribute/code-style#log-sparingly)