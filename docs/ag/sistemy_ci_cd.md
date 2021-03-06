# Системы CI/CD

Для встраивания в процесс CI/CD предусмотрен специальный [скрипт](https://github.com/Dynamic-Mobile-Security/mdast-cli), в процессе выполнения которого собранное приложение отправляется в систему для анализа. На выходе формируется JSON файл с подробными результатами.

## Варианты установки

### DockerHub

Можно установить пакет, используя docker image:

    docker pull mobilesecurity/mdast_cli:release-version

!!! note "Примечание"
    Версия релиза указывается в формате `release-version`, где `release-version` — это текущая версия. Пожалуйста, уточняйте эту информацию у вендора, на официальном сайте или на [странице в Docker Hub](https://hub.docker.com/repository/docker/mobilesecurity/mdast_cli).

### Пакетный менеджер pip

Возможно установить пакет, используя pip:

    pip install mdast_cli

При таком способе возможно запускать сканирование без указания интерпретатора python при помощи команды mdast_cli, пример:

    mdast_cli -h

Во всех примерах ниже будет использован именно такой подход.

### Исходный код

Также поддерживается запуск при помощи загрузки исходных файлов и запуска непосредственно основного скрипта:

    python3 mdast_cli/mdast_scan.py -h

При таком способе запуска необходимо дополнительно установить пакеты, указанные в файле `requirements.txt`.

## Варианты запуска

На данный момент поддерживается несколько вариантов запуска:

* анализ приложения, файл которого расположенного локально;
* анализ приложений из [Google Play](https://play.google.com/store/apps);
* анализ приложений из [Appstore](https://www.apple.com/app-store/);
* анализ приложений из [Firebase](https://firebase.google.com/);
* анализ приложений из системы [AppCenter](https://appcenter.ms/);
* анализ приложений из системы [Nexus Repository 3.x](https://help.sonatype.com/repomanager3).

## Параметры запуска

Параметры запуска зависят от расположения файла приложения, отправляемого на анализ. Так же, существуют обязательные параметры, которые необходимо указывать при любом виде запуска:

* `url` — сетевой адрес Stingray (путь до корня без последнего «/»), при использовании cloud версии — [https://saas.stingray-mobile.ru](https://saas.stingray-mobile.ru);
* `profile_id` — id профиля, для которого проводится анализ;
* `testcase_id` — id того тест-кейса, который будет воспроизведен во время анализа; возможен запуск нескольких тест-кейсов, для этого их id перечисляются через пробел. Это необязательный параметр, если он не задан, то будет запущено сканирование в ручном режиме и через 30 секунд после запуска остановлено, а данные отправлены на анализ;
* `token` — CI/CD токен для доступа, более подробная информация приведена в разделе «[Интеграции](./integracii.md)» Руководства пользователя;
* `distribution_system` — способ загрузки приложения, возможные опции: `file`, `google_play`, `appstore`, `firebase`, `appcenter`, `nexus`. Более подробно про них описано ниже в соответствующих разделах;
* `company_id` — идентификатор компании, в рамках которой будет осуществлено сканирование;
* `architecture_id` — опциональный параметр. Определяет идентификатор архитектуры операционной системы, на которой будет произведено сканирование;
* `nowait` — опциональный параметр, определяющий необходимость ожидания завершения сканирования. Если данный флаг установлен — скрипт не будет дожидаться завершения сканирования, а выйдет сразу же после запуска. Если флаг не установлен — скрипт будет ожидать завершения процесса анализа и формировать отчет;
* `summary_report_json_file_name` — опциональный параметр. Определяет имя JSON файла, в который выгружается информация по сканированию в формате JSON. При отсутствии параметра информация сохраняться в JSON не будет;
* `pdf_report_file_name` — опциональный параметр. Определяет имя PDF файла в который выгружается информация по сканированию в формате PDF. При отсутствии параметра PDF-отчет сохраняться не будет.

## Локальный запуск

Данный вид запуска подразумевает, что файл приложения для анализа располагается локально, рядом (на одной системе) со скриптом. Для выбора этого способа при запуске необходимо указать параметр `distribution_system file`. В этом случае обязательным параметром необходимо указать путь к файлу `file_path`.

### Для запуска анализа локального файла

    mdast_cli --distribution_system file \
      --file_path "/files/demo/apk/demo.apk" \
      --url "https://saas.stingray-mobile.ru" \
      --profile_id 1 \
      --testcase_id 4 \
      --company_id 1 
      --architecture_id 1 \
      --token "token_value" 

В результате будет запущен автоматизированный анализ приложения `demo.apk` с профилем с `id 1` и будет запущен тест-кейс с `id 4`.

### Запуск без ожидания завершения сканирования

    mdast_cli --distribution_system file \
      --file_path "/files/demo/apk/demo.apk" \ 
      --url "https://saas.stingray-mobile.ru" \
      --profile_id 1 \
      --testcase_id 4 \
      --company_id 1 \ 
      --architecture_id 1 \
      --token "token_value" \
      --nowait

В результате будет запущен автоматизированный анализ приложения `demo.apk` с профилем `id 1` (тест-кейс `id 4`). Сразу после этого скрипт завершится и отчет генерироваться не будет.

### Генерация Summary отчета в формате JSON

    mdast_cli --distribution_system file \
      --file_path "/files/demo/apk/demo.apk" \ 
      --url "https://saas.stingray-mobile.ru" \
      --profile_id 1 \
      --testcase_id 4 \
      --company_id 1 \
      --architecture_id 1 \
      --token "token_value" \
      --summary_report_json_file_name json-scan-repot.json

В результате будет запущен автоматизированный анализ приложения `demo.apk` с профилем `id 1` (тест-кейс `id 4`) и по завершении сканирования будет выгружен JSON-отчет с суммарным количеством дефектов и краткой статистикой по сканированию.