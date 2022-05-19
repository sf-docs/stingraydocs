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

iOS предоставляет приложениям возможность выводить информацию в системный журнал. Приложения могут отправлять информацию в журнал, используя следующие средства:

* NSLog макрос
* printf семейство функции
* NSAssert семейство функции
* Макросы

В процессе разработки приложения в финальную версию могут попасть участки кода с логированием чувствительной информации (пароли, личные данные, ключи шифрования)

## Рекомендации

### Изменение поведения макросов логирования в зависимости от типа сборки приложения

Пример кода:

    #ifdef DEBUG
    #   define NSLog (...) NSLog(__VA_ARGS__)
    #else
    #   define NSLog (...)
    #endif

### Применение фреймоврка OSLogPrivacy для разграничения вывода в зависимости от чувствительности денных

Пример кода:

    @frozen struct OSLogPrivacy
    ... 
    Logger().info("User bank account number: \(accountNumber, privacy: .private)")

## Ссылки

1. [Poor Logging Practice | OWASP](https://www.owasp.org/index.php/Poor_Logging_Practice)
2. [CWE-215: Insertion of Sensitive Information Into Debugging Code](https://cwe.mitre.org/data/definitions/215.html)
3. [OSLogPrivacy](https://developer.apple.com/documentation/os/oslogprivacy)