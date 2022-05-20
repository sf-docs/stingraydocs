# Перезагрузка сервера без обновления Stingray

В случае, если необходимо перезагрузить сервер, на котором установлен Stingray, например, для обновления операционной системы, без обновления версии Stingray, перед перезагрузкой сервера в папке, указанной при генерации конфигурации (в примере `/opt/stingray`), выполните команду:

    docker exec stingray-maintenance django-admin maintenance engines preserve

После перезагрузки сервера там же выполните команду:

    docker exec stingray-maintenance django-admin maintenance engines restore

.