# Установка Stingray
!!! note "Примечание"
    Все действия, описанные в данном разделе, необходимо производить от пользователя root.

## Подготовка инфраструктуры

1. Необходимо убедиться, что CPU имеет поддержку технологии аппаратной виртуализации (Intel Virtualization Technology (VT, VT-x, vmx) или AMD Virtualization (AMD-V, SVM)), выполнив команды:

    === "Ubuntu"
            sudo apt-get install cpu-checker
            kvm-ok

2. Установить требуемые пакеты:

    === "Ubuntu Server 16"
            sudo apt-get install qemu-kvm libvirt-bin ubuntu-vm-builder bridge-utils
    === "Ubuntu Server 18/20"
            sudo apt install qemu-kvm libvirt-clients libvirt-daemon-system bridge-utils virt-manager
    === "RHEL/CentOS"
            sudo yum install qemu-kvm libvirt libvirt-python libguestfs-tools virt-install

3. Установите docker и docker-compose, если это не было сделано заранее. Рекомендации по установке можно найти на официальном сайте:

    * [https://docs.docker.com/install/linux/docker-ce/ubuntu/](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
    * [https://docs.docker.com/compose/install/](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

4. Создайте группы и пользователей.

        groupadd --system --gid 171 kvm
        groupadd --gid 1717 emulator
        useradd --uid 1717 --gid emulator --groups kvm emulator

    Если группа kvm уже существует, то вместо первой строки выполнить следующее:

        groupmod --gid 171 kvm
        chgrp kvm /dev/kvm

### Установка при наличии доступа к внешнему репозиторию docker-образов YCR

1. Авторизуйте docker на доступ к репозиторию YCR docker-образов:

        cat stingray-numeric_id.json | docker login --username json_key --password-stdin cr.yandex

    Ключ `stingray-numeric_id.json` для подключения к Yandex Container Registry (YCR) docker-образам компании Stingray Technologies предоставляется при покупке лицензии Stingray.

2. Загрузите специальный docker-образ для подготовки конфигурационных файлов командой:

        docker pull cr.yandex/crp8idtsajke3lbauqel/stingray/wizard:release-x

    !!! note "Примечание"
        Версия релиза указывается в формате `release-x`, где `x` — это текущая версия (например, 2022.6.1). Пожалуйста, уточняйте эту информацию у вендора или на официальном сайте.

### Установка без наличия доступа к внешнему репозиторию docker-образов YCR

1. При отсутствии доступа к внешнему репозиторию docker-образов, образы поставляются в виде выгруженных tar-архивов. Для доступа к данным архивам необходимо запросить их у поставщика продукта.

2. После того, как архивы загружены и перенесены на сервер Stingray необходимо их импортировать в docker. Для этого выполните следующую команду для всех полученных архивов:

        docker load -i <archive_name>.tar

## Настройка системы

1. Создайте директорию, где будут располагаться конфигурационные файлы Stingray, к примеру директорию `/opt/stingray`.

2. При необходимости использования HTTPS соединения создайте директорию, где будут располагаться файл полной цепочки сертификатов (например, `/opt/certs`) и закрытого ключа, скопируйте файлы сертификата и закрытого ключа и назовите их в соответствии с требованиями:

    * ***fullchain.pem*** — полная цепочка сертификатов;
    * ***privkey.pem*** — приватный ключ.

3. Запустите docker-контейнер для подготовки конфигурации.

    Пример запуска контейнера с двумя подключенными volumes для файлов конфигурации и с сертификатами (при доступе по HTTPS):

        docker run -i -t -v /opt/stingray:/opt/docker-files -v /opt/certs:/opt/nginx/certs cr.yandex/crp8idtsajke3lbauqel/stingray/wizard:release-x

    !!! note "Примечание"
        Версия релиза указывается в формате `release-x`, где `x` — это текущая версия (например, 2022.6.1). Пожалуйста, уточняйте эту информацию у вендора или на официальном сайте.

    Пример запуска контейнера с одним volume для файлов конфигурации (при доступе по HTTP):

        docker run -i -t -v /opt/stingray:/opt/docker-files cr.yandex/crp8idtsajke3lbauqel/stingray/wizard:release-x

    !!! note "Примечание"
        Версия релиза указывается в формате `release-x`, где `x` — это текущая версия (например, 2022.6.1). Пожалуйста, уточняйте эту информацию у вендора или на официальном сайте.

После запуска контейнера в интерактивном режиме необходимо заполнить ряд параметров.

Параметр|Описание|Значение по умолчанию
-|-|-
STINGRAY_DOMAIN|Домен, на котором будет располагаться система для корректной настройки маршрутизации и обращения UI к нужному серверу|saas.stingray-mobile.ru
IP_EXTERNAL|IP сервера, на который устанавливается Stingray|0.0.0.0
USE_SSL|Параметр, определяющий, будет ли проходить соединение через протокол http или https. При указании «1» приложение конфигурируется для использования 443 порта и проводит настройку для соединения по HTTPS (копируются из второго volume с сертификатами)|0
POSTGRES_USER|Пользователь, с которым будет запущена база данных Postgres|stingray
POSTGRES_PASSWORD|Пароль пользователя, с которым будет запущена база данных Postgres|P@ssw0rd
RABBITMQ_DEFAULT_USER|Пользователь, с которым будет запущен брокер RabbitMQ|stingray
RABBITMQ_DEFAULT_PASS|Пароль пользователя, с которым будет запущен брокер RabbitMQ|P@ssw0rd
STINGRAY_DEBUG|Запустить сервер с расширенным выводом ошибок|0
STINGRAY_DOCKER_LOGIN|Флаг, определяющий сетевую доступность от сервера Stingray до внешнего хранилища docker-образов. Данная функциональность необходима для динамического создания сканирующих агентов и загрузки последней актуальной версии при обновлении системы. При выставлении значения «0» — необходимо при обновлении системы вручную загрузить последнюю версию образа|0
STINGRAY_LANGUAGE_CODE|Язык системы по умолчанию. Влияет на язык swagger и язык по умолчанию для вновь создаваемых языков|ru
STINGRAY_TIME_ZONE|Временная зона для корректного отображения времени|Europe/Moscow
STINGRAY_ACCESS_TOKEN_LIFETIME|Время жизни access_token в минутах|60
STINGRAY_REFRESH_TOKEN_LIFETIME|Время жизни refresh_token в минутах|1440
STINGRAY_CI_TOKEN_LIFETIME|Время жизни токена для интеграции в CI/CD|525600
STINGRAY_COMPANY_NAME|Название компании|Company Name
STINGRAY_COMPANY_DESCRIPTION|Описание компании|Company Description
STINGRAY_SUPERUSER_USERNAME|Имя пользователя с ролью Супер администратора|admin
STINGRAY_SUPERUSER_PASSWORD|Пароль пользователя с ролью Супер администратора|admin
STINGRAY_CREATE_SAMPLES|Флаг, определяющий, нужно ли создавать сущности по умолчанию, которые задаются далее (проекты/профили, пользователей, сканирующие агенты/компания)|1
STINGRAY_ADMIN_USERNAME|Имя пользователя с ролью Администратора компании|company_admin
STINGRAY_ADMIN_PASSWORD|Пароль пользователя с ролью Администратора компании|123
STINGRAY_ADMIN_FIRSTNAME|Имя Администратора|FirstName
STINGRAY_ADMIN_LASTNAME|Фамилия Администратора|LastName
STINGRAY_ENGINE_NAME_ANDROID|Имя агента для Android Engine|stingray-engine
STINGRAY_ENGINE_NAME_IOS|Имя агента для iOS Engine|stingray-engine-ios
STINGRAY_PROJECT_NAME_ANDROID|Имя проекта, создаваемого по умолчанию для Android проекта|Project Name Android
STINGRAY_PROJECT_DESCRIPTION_ANDROID|Описание проекта, создаваемого по умолчанию для Android проекта|Project Description Android
STINGRAY_PROJECT_NAME_IOS|Имя проекта, создаваемого по умолчанию для iOS проекта|Project Name iOS
STINGRAY_PROJECT_DESCRIPTION_IOS|Описание проекта, создаваемого по умолчанию для iOS проекта|Project Description iOS
STINGRAY_PROFILE_NAME_ANDROID|Имя профиля, создаваемого по умолчанию для Android проекта|Profile Name Android
STINGRAY_PROFILE_DESCRIPTION_ANDROID|Описание профиля, создаваемого по умолчанию для Android проекта|Profile Description Android
STINGRAY_PROFILE_NAME_IOS|Имя профиля, создаваемого по умолчанию для iOS проекта|Profile Name iOS
STINGRAY_PROFILE_DESCRIPTION_IOS|Описание профиля, создаваемого по умолчанию для iOS проекта|Profile Description iOS
STINGRAY_AUDIT_USE|Включение или выключение аудита событий в системе|1
STINGRAY_AUDIT_MAX_LENGTH|Максимальное количество записей в одном файле. После  превышения заданного количества к концу файла добавляется постфикс (.1, .2 и  т. д.), а новая информация записывается в стандартный файл без постфикса|1000
STINGRAY_AUDIT_FILE_COUNT|Количество файлов, которое будет храниться в системе. При превышении количества файлов старые удаляются|10

В результате выполнения в директории `/opt/stingray` будут созданы все необходимые файлы для запуска.

### Список контейнеров

Имя контейнера|Описание
-|-
stingray-nginx|Входная точка для обращений к backend и UI. Выполняет функции reverse-proxy
stingray-backend|Backend приложения, отвечает за основную логику обработки пользовательских запросов и выдачу результатов
stingray-rabbitmq|Менеджер очередей сканирования, управляет очередью сканирования
stingray-postgres|База данных
stingray-ui|Пользовательский интерфейс
stingray-redis|Redis для промежуточного хранения оперативной информации
engine-android|Сканирующий модуль для Android-проектов. Название контейнера может быть произвольным
engine-ios|Сканирующий модуль для iOS-проектов. Название контейнера может быть произвольным
stingray-knowledgebase|Контейнер, содержащий в себе информацию по устранению уязвимостей и документацию
stingray-maintenance|Осуществляет управление контейнерами - проверку статусов, перезагрузку, запуск и остановку 