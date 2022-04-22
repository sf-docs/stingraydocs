# Хранение sensitive-информации в общедоступной защищённой базе данных

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
        <td>Способ обнаружения:<strong> DAST, API</strong></td>
      </tr>
    </tbody>
</table>

## Описание

Приложение хранит чувствительную информацию в общедоступной защищенной базе данных. В общем случае это не является уязвимостью, но необходимо удостовериться, что для шифрования базы данных [используется надежный пароль]().

Найденные чувствительные данные используется системой для поиска их использования или [хранения в собранных данных]().

## Рекомендации

Для защиты от перехвата данных в runtime необходимо использовать защитные меры по обнаружению инструментации приложения и обнаружению root-доступа. Одним из хороших способов является использование библиотек [DetectFrida](https://github.com/darvincisec/DetectFrida) и [DetectMagiskHide](https://github.com/darvincisec/DetectMagiskHide). Данные библиотеки реализуют проверки в нативном коде, что существенно усложняет их анализ и модификацию.

## Ссылки

1.  [https://github.com/sqlcipher/android-database-sqlcipher](https://github.com/sqlcipher/android-database-sqlcipher)

2.  [https://github.com/darvincisec/DetectMagiskHide](https://github.com/darvincisec/DetectMagiskHide)

3. [https://github.com/darvincisec/DetectFrida](https://github.com/darvincisec/DetectFrida)

4. [https://darvincitech.wordpress.com/2019/12/23/detect-frida-for-android/](https://darvincitech.wordpress.com/2019/12/23/detect-frida-for-android/)

5. [https://darvincitech.wordpress.com/2019/11/04/detecting-magisk-hide/](https://darvincitech.wordpress.com/2019/11/04/detecting-magisk-hide/)

6. [https://github.com/OWASP/owasp-stg/blob/master/Document/0x05d-Testing-Data-Storage.md#sqlite-databases-encrypted](https://github.com/OWASP/owasp-stg/blob/master/Document/0x05d-Testing-Data-Storage.md#sqlite-databases-encrypted)