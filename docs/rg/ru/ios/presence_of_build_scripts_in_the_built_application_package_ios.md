# Наличие скриптов сборки в собранном пакете приложения

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
        <td>Способ обнаружения:<strong> DAST, SENSITIVE INFO, FILES</strong></td>
      </tr>
    </tbody>
</table>

## Описание

В пакете приложения присутствуют файлы скриптов, которые используются в процессе сборки приложения. Наличие таких файлов может помочь злоумышленнику в определении особенностей процесса сборки, а также потенциально раскрыть информацию о внутренних репозиториях (если используются внутренние компоненты).

## Рекомендации

Рекомендуется исключить из итоговой сборки файлы, которые не требуются для работы приложения и необходимы только во время сборки.

1. Если отсутствует файл пользовательских настроек для сборки необходимо его создать.

    <figure markdown>
    ![](../../img/image.png)
    </figure>

2. Добавить ключ настройки **EXCLUDED_SOURCE_FILE_NAMES** если он отсуствует.

    <figure markdown>
    ![](../../img/image2.png)
    </figure>

3. Добавить значение настройки какие файлы и папки необходимо исключить из конечной сборки приложения.

    <figure markdown>
    ![](../../img/image3.png)
    </figure>