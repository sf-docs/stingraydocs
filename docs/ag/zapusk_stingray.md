# Запуск Stingray

!!! note "Примечание"
    В этом разделе описан процесс первого запуска версии системы Stingray. Процесс запуска Stingray после обновления версии описан в разделе «[Обновление системы](./obnovlenie_sistemy.md)». Процесс запуска Stingray после перезагрузки сервера без обновления версии Stingray описан в разделе «[Перезагрузка сервера без обновления Stingray](./perezagruzka_servera_bez_obnovleniya_stingray.md)».

Для запуска приложения в папке, указанной при генерации конфигурации (в примере ***/opt/stingray***) выполните команды:

    docker-compose pull
    docker pull cr.yandex/crp8idtsajke3lbauqel/stingray/android_api27:release-x
    docker pull cr.yandex/crp8idtsajke3lbauqel/stingray/android_api30:release-x
    docker pull cr.yandex/crp8idtsajke3lbauqel/stingray/ios:release-x
    docker-compose up -d

!!! note "Примечание"
    Версия релиза указывается в формате `release-x`, где `x` — это текущая версия (например, 2022.6.1). Пожалуйста, уточняйте эту информацию у вендора или на официальном сайте.

При первом запуске системы, в случае если команда docker-compose не может загрузить образ из репозитория, необходимо вручную загрузить контейнеры:

    docker pull cr.yandex/crp8idtsajke3lbauqel/stingray/stingray:release-x
    docker pull cr.yandex/crp8idtsajke3lbauqel/stingray/android_api27:release-x
    docker pull cr.yandex/crp8idtsajke3lbauqel/stingray/android_api30:release-x
    docker pull cr.yandex/crp8idtsajke3lbauqel/stingray/ios:release-x
    docker pull cr.yandex/crp8idtsajke3lbauqel/stingray/stingray-ui:release-x
    docker pull cr.yandex/crp8idtsajke3lbauqel/stingray/stingray-knowledgebase:release-x

!!! note "Примечание"
    Версия релиза указывается в формате `release-x`, где `x` — это текущая версия (например, 2022.6.1). Пожалуйста, уточняйте эту информацию у вендора или на официальном сайте.