# Система дистрибуции AppCenter

## Параметры запуска

Параметры запуска зависят от расположения файла apk, отправляемого на анализ. Также существуют обязательные параметры, которые необходимо указывать при любом виде запуска:

* `url` — сетевой адрес Stingray (путь до корня без последнего «/»), при использовании cloud версии — [https://saas.stingray-mobile.ru](https://saas.stingray-mobile.ru); 
* `profile_id` — id профиля, для которого проводится анализ;
* `testcase_id` — id того тест-кейса, который будет воспроизведен во время анализа; возможен запуск нескольких тест-кейсов, для этого их id перечисляются через пробел. Это необязательный параметр, если он не задан, то будет запущено сканирование в ручном режиме и через 20 секунд после запуска остановлено, а данные отправлены на анализ;
* `token` — CI/CD токен для доступа, более подробная информация приведена в разделе «[Интеграции](./integracii.md)» Руководства пользователя;
* `distribution_system` — способ загрузки приложения, возможные опции: `file`, `google_play`, `appstore`, `firebase`, `appcenter`, `nexus`;
* `company_id` — идентификатор компании, в рамках которой будет осуществлено сканирование;
* `architecture_id` — опциональный параметр. Определяет идентификатор архитектуры операционной системы, на которой будет произведено сканирование;
* `nowait` — опциональный параметр, определяющий необходимость ожидания завершения сканирования. Если данный флаг установлен — скрипт не будет дожидаться завершения сканирования, а выйдет сразу же после запуска. Если флаг не установлен — скрипт будет ожидать завершения процесса анализа и формировать отчет;
* `summary_report_json_file_name` — опциональный параметр. Определяет имя JSON файла, в который выгружается информация по сканированию в формате JSON. При отсутствии параметра информация сохраняться в JSON не будет;
* `pdf_report_file_name` — опциональный параметр. Определяет имя PDF файла в который выгружается информация по сканированию в формате PDF. При отсутствии параметра PDF-отчет сохраняться не будет.

Для загрузки приложения из системы дистрибуции AppCenter при запуске необходимо указать параметр `distribution_system appcenter`. Также необходимо указать обязательные параметры:

* `appcenter_token` — API токен для доступа. Как его получить, можно узнать [здесь](https://docs.microsoft.com/en-us/appcenter/api-docs/);
* `appcenter_owner_name` — владелец приложения. Как узнать имя владельца, можно прочитать [здесь](https://intercom.help/appcenter/en/articles/1764707-how-to-find-the-app-name-and-owner-name-from-your-app-url) или в [официальной документации](https://docs.microsoft.com/en-us/appcenter/api-docs/#find-your-app-center-app-name-and-owner-name);
* `appcenter_app_name` — имя приложения в системе AppCenter. Как его узнать, можно прочитать [здесь](https://docs.microsoft.com/en-us/appcenter/api-docs/#find-your-app-center-app-name-and-owner-name);
    * `appcenter_release_id` или `appcenter_app_version`:
    * `appcenter_release_id` — идентификатор загружаемого релиза в системе AppCenter для конкретного приложения. Возможно выставить значение `latest` — тогда будет загружен последний доступный релиз приложения ([официальная документация](https://openapi.appcenter.ms/#/distribute/releases_getLatestByUser));
    * `appcenter_app_version` — при указании данного параметра будет найдена и скачана конкретная версия приложения по коду его версии (указанной в Android Manifest) (поле `version` в [документации](https://openapi.appcenter.ms/#/distribute/releases_list)).

## Примеры запуска

### AppCenter по id релиза

Для запуска анализа приложения по известному имени, владельцу и ID релиза необходимо выполнить следующую команду:

    mdast_cli --distribution_system appcenter --appcenter_token 18bc81146d374ba4b1182ed65e0b3aaa 
      --appcenter_owner_name test_org_or_user --appcenter_app_name demo_app --appcenter_release_id 710 
      --url "https://saas.stingray-mobile.ru" --profile_id 2 --testcase_id 3 --company_id 1 --architecture_id 1
      --token "eyJ0eXA4OiJKA1QiLbJhcGciO5JIU4I1NiJ1..."

В результате у владельца (пользователя или организации `test_org_or_user`) будет найдено приложение `demo_app` с `ID` релиза `710`. Данная версия релиза будет загружена и передана на анализ безопасности в Stingray.

Для загрузки релиза с последней версией необходимо указать параметр `appcenter_release_id latest`. Тогда команда будет выглядеть следующим образом:

    mdast_cli --distribution_system appcenter --appcenter_token 18bc81146d374ba4b1182ed65e0b3aaa 
      --appcenter_owner_name "test_org_or_user" --appcenter_app_name "demo_app" --appcenter_release_id latest 
      --url "https://saas.stingray-mobile.ru" --profile_id 2 --testcase_id 3 --company_id 1 --architecture_id 1
      --token "eyJ0eXA4OiJKA1QiLbJhcGciO5JIU4I1NiJ1..."

и будет загружен последний доступный релиз для данного приложения.

### AppCenter по версии приложения

Для запуска анализа приложения по известному имени, владельцу и версии приложения (`version_code` в Android Manifest) необходимо выполнить следующую команду:

    mdast_cli --distribution_system appcenter --appcenter_token 18bc81146d374ba4b1182ed65e0b3aaa 
      --appcenter_owner_name "test_org_or_user" --appcenter_app_name "demo_app" --appcenter_app_version 31337 
      --url "https://saas.stingray-mobile.ru" --profile_id 2 --testcase_id 3 --company_id 1 --architecture_id 1 
      --token "eyJ0eXA4OiJKA1QiLbJhcGciO5JIU4I1NiJ1..."

В результате у владельца (пользователя или организации `test_org_or_user`) будет найдено приложение `demo_app` и найден релиз, в котором была указана версия приложения `31337`. Данная версия релиза будет загружена и передана на анализ безопасности в Stingray.