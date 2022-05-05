# Остановка Stingray

Для остановки приложения в папке, указанной при генерации конфигурации (в примере ***/opt/stingray***) выполните команду:

    docker exec stingray-maintenance django-admin maintenance engines preserve
    docker-compose stop

!!! note "Примечание"
    Команда `preserve` сохраняет состояние контейнеров для дальнейшего их восстановления в ранее сохраненном состоянии.