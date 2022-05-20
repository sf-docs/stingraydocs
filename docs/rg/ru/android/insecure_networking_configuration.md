# Небезопасная конфигурация сетевого взаимодействия

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
        <td>Способ обнаружения:<strong> SAST, NETWORK</strong></td>
      </tr>
    </tbody>
</table>

## Описание

Приложение использует небезопасно настроенную конфигурацию сетевого взаимодействия. С выходом Android 6.0 Marshmallow, Google представил атрибут манифеста `android: usesCleartextTraffic`, как средство защиты от случайного использования протокола http. Android 7.0 Nougat расширил этот атрибут, представив функцию настройки безопасности сетевого взаимодействия Android, которая позволяет разработчикам более четко определять параметры соединений. Конфигурация сетевого взаимодействия — это XML-файл, в котором настраиваются параметры сетевой безопасности для приложения Android. Данная настройка задается специальным атрибутом в ***AndroidManifest.xml*** — `android:networkSecurityConfig`.

**Пример подключения:**

    <?xml version="1.0" encoding="utf-8"?>
    <manifest ... >
    <application android:networkSecurityConfig="@xml/network_security_config"
    ... >
    ...
    </application>
    </manifest>

## Рекомендации

Функция Network Security Config позволяет настраивать параметры сетевого взаимодействия в декларативном файле конфигурации без изменения кода приложения. Эти параметры можно настроить для определенных доменов и для конкретного приложения. Ключевые возможности, которые предоставляет такой подход:

* **Custom trust anchors:** Настройка доверия центрам сертификации (CA), которым будет доверять приложение при сетевом взаимодействии. Например, доверие определенным самоподписанным сертификатам или ограничение набора общедоступных центров сертификации, которым доверяет приложение (добавленные пользователем в доверенные или те CA, которым доверяет система)
* **Debug-only overrides:** Настройка соединений специально для Debug-версии приложения, что позволяет разграничивать среды разработки и Production.
* **Cleartext traffic:** Настройки для разрешения или запрета соединений по HTTP.
* **Certificate pinning:** Настройка для реализации SSL-Pinning.

В файле конфигурации можно применять настройки для приложения в блоке `base-config`, только для определенных доменов и поддоменов в блоке `domain-config` и отдельно для debug-сброки приложения в блоке `debug-overrides`.

Рассмотрим примеры настроек, которые доступны для настройки:

**Custom trust anchors: Настройка доверия центрам сертификации (CA)**

Конфигурация сетевой безопасности Android дает разработчикам несколько вариантов для выбора центров сертификации, которым будет доверять приложение. По умолчанию в Android 7+ (Nougat, Oreo и Pie), приложение будет доверять только сертификатам, которые отмечены, как системные (system):

    <network-security-config>
    <base-config>
        <trust-anchors>
        <certificates src="system"/>
        </trust-anchors>
    </base-config>
    </network-security-config>

В Android 6 (Marshmallow) и ниже приложение также будет доверять установленным пользователем сертификатам (user). При такой настройке, если пользователь установит свой сертификат, то используя его, можно будет осуществить атаку MiTM (человек посередине):

    <network-security-config>
    <base-config>
        <trust-anchors>
        <certificates src="system"/>
        <certificates src="user"/>
        </trust-anchors>
    </base-config>
    </network-security-config>

Также есть возможность включения доверия к сертификатам, расположенным в ассетах приложения:

    <network-security-config>
    <base-config>
        <trust-anchors>
        <certificates src="@raw/my_custom_ca"/>
        </trust-anchors>
    </base-config>
    </network-security-config>

Наиболее безопасным способом является первый вариант, при котором приложение доверяет только системным сертификатам.

**Настройка взаимодействия по протоколу HTTP**

Благодаря разбиению на отдельные конфигурации для всего приложения, отдельных доменов и debug-версии возможно контролировать разрешениями для использования http на каждом из уровней. Рассмотрим подробнее — в первом примере передача данных по HTTP запрещена на уровне всего приложения (это наиболее безопасный вариант):

    <network-security-config>
    <base-config cleartextTrafficPermitted="false" />
    ...
    </network-security-config>

В некоторых исключительных случаях необходимо соединение по HTTP с определенными домнами. Как раз для этого случая присутствует возможность отдельно указать для каких доменов что разрешено:

    <network-security-config>
    <domain-config cleartextTrafficPermitted="true">
    <domain includeSubdomains="true">insecure.example.com</domain>
    <domain includeSubdomains="true">insecure.cdn.example2.com</domain>
    </domain-config>
    <base-config cleartextTrafficPermitted="false" />
    </network-security-config>

При такой настройке для всех сетевых соединений будет разрешен доступ только по HTTPS, за исключением двух (**insecure.example.com** и **insecure.cdn.example2.com**), включая их поддомены (за включение которых отвечает настройка **includeSubdomains**), для которых разрешено общение по HTTP.

**Настройки Certificate Pinning**

Network Security Config позволяет достаточно просто подключить механизм Certificate Pinning в приложение. Но стоит учитывать определенные нюансы. Рассмотрим конфигурацию, которая с первого взгляда выглядит, как правильно настроенная, и разберем, как ее можно немного улучшить:

    <network-security-config>
    <domain-config>
    <domain includeSubdomains="true">example.com</domain>
    <pin-set>
    <pin digest="SHA-256">7HIpactkIAq2Y49orFOOQKurWxmmSFZhBCoQYcRhJ3Y=</pin>
    </pin-set>
    </domain-config>
    </network-security-config>

Этот пример имеет два небольших недостатка:

1. Для отпечатка сертификата (pin-set) не установлен срок действия.
2. Нет резервного сертификата.

Если срок действия вашего сертификата подойдет к концу и у в настройках не указан срок действия, приложение перестанет подключаться к серверу и будет выдавать ошибку. Но, если установлен срок действия, и он подойдет к концу, приложение перейдет на использование доверенных центров сертификации, установленных в системе. И вместо того, чтобы получить неработоспособное приложение, вы получите отсутствие SSL-Pinning в течении некоторого времени, пока не обновите сертификат в приложении.

Чтобы этого избежать, если вы знаете сертификат, который будет изменен на вашем сервере после истечения срока текущего, можно сразу указать его в настройках “резервных сертификатов“.

Вот пример наиболее корректного использования функционала Certificate Pinning:

    <network-security-config>
    <domain-config>
    <domain includeSubdomains="true">example.com</domain>
    <pin-set expiration="2021-01-01">
    <pin digest="SHA-256">7HIpactkIAq2Y49orFOOQKurWxmmSFZhBCoQYcRhJ3Y=</pin>
    <!-- backup pin -->
    <pin digest="SHA-256">fwza0LRMXouZHRC8Ei+4PyuldPDcf3UKgO/04cDM1oE=</pin>
    </pin-set>
    </domain-config>
    </network-security-config>

Несмотря на всё удобство использования Network Security Config, некоторые проверки придется выполнять самостоятельно в коде приложения. Например, все равно нужно будет определить, выполняет ли ваше приложение проверку имени хоста, поскольку Network Security Config не защитит от проблем такого типа.

!!! note "Примечание"
    Так же, перед имплементацией необходимо убедиться, что сторонние библиотеки поддерживают Network Security Config. В противном случае, эти средства защиты могут вызвать проблемы в вашем приложении. Кроме того, Network Security Config не поддерживается сетевыми соединениями более низкого уровня, такими как веб-сокеты.

## Ссылки

1. [https://developer.android.com/guide/topics/manifest/application-element#usesCleartextTraffic](https://developer.android.com/guide/topics/manifest/application-element#usesCleartextTraffic)

2. [https://developer.android.com/training/articles/security-config](https://developer.android.com/training/articles/security-config)

3. [https://www.nowsecure.com/blog/2018/08/15/a-security-analysts-guide-to-network-security-configuration-in-android-p/](https://www.nowsecure.com/blog/2018/08/15/a-security-analysts-guide-to-network-security-configuration-in-android-p/)