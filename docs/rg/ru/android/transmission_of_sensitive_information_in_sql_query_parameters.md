# Передача sensitive-информации в параметрах SQL-запроса

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
        <td>Способ обнаружения:<strong> DAST, SQL</strong></td>
      </tr>
    </tbody>
</table>

## Описание

Приложение использует чувствительную информацию при запросах к базе данных. Перехват SQL-запросов не является уязвимостью, в случае, если используются меры по определению инструментации приложения при помощи таких инструментов, как Frida или Xposed, осуществляется проверка на root-доступ и база данных, в которой хранится чувствительная информация [зашифрована](./storing_sensitive_information_in_a_protected_database.md) с использованием [стойкого пароля](./weak_database_encryption_password.md).

Перехваченные данные используется системой для поиска перехваченного [значения в собранных данных](./storage_or_use_of_previously_found_sensitive_information.md).

## Рекомендации

Для защиты от перехвата пароля в runtime необходимо использовать защитные меры по обнаружению инструментации приложения и обнаружению root-доступа. Одним из хороших способов является использование библиотек [DetectFrida](https://github.com/darvincisec/DetectFrida) и [DetectMagiskHide](https://github.com/darvincisec/DetectMagiskHide). Данные библиотеки реализуют проверки в нативном коде, что существенно усложняет их анализ и модификацию.

## Ссылки

1. [https://github.com/sqlcipher/android-database-sqlcipher](https://github.com/sqlcipher/android-database-sqlcipher)

2. [https://github.com/darvincisec/DetectMagiskHide](https://github.com/darvincisec/DetectMagiskHide)

3. [https://github.com/darvincisec/DetectFrida](https://github.com/darvincisec/DetectFrida)

4. [https://darvincitech.wordpress.com/2019/12/23/detect-frida-for-android/](https://darvincitech.wordpress.com/2019/12/23/detect-frida-for-android/)

5. [https://darvincitech.wordpress.com/2019/11/04/detecting-magisk-hide/](https://darvincitech.wordpress.com/2019/11/04/detecting-magisk-hide/)

6. [https://github.com/OWASP/owasp-mstg/blob/master/Document/0x05d-Testing-Data-Storage.md#sqlite-databases-encrypted](https://github.com/OWASP/owasp-mstg/blob/master/Document/0x05d-Testing-Data-Storage.md#sqlite-databases-encrypted)