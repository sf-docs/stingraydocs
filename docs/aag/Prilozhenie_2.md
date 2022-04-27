# Приложение 2. Список обнаруживаемых уязвимостей

  <p>Все уязвимости, обнаруживаемые во время сканирований, относятся к какому-либо из требований стандартов информационной безопасности, на соответствие которым проверяется приложение. На каждую выявленную уязвимость в системе заводится дефект. С требованием соотносятся определенные типы дефектов, при нахождении которых в приложении требование будет считаться не выполненным. Работа с выявленными уязвимостями в системе проводится на странице Результаты сканирования на вкладке <strong>Дефекты</strong>, см. подробности в разделе <a href="../ug/rezultaty_skanirovanij.htm">Результаты сканирований</a> Руководства пользователя.</p>
  <p>Различным дефектам система присваивает различные уровни критичности:</p>
  <p>«<img src="../assets/images/image.png" />» — критичный;<br />
    «<img height="17" src="../assets/images/image1.png" width="15" />» — высокий;<br />
    «<img src="../assets/images/image2.png" />» — средний;<br />
    «<img height="18" src="../assets/images/image3.png" width="15" />» — низкий;<br />
    «<img src="../assets/images/image4.png" />» — инфо.</p>
  <p>Полный список обнаруживаемых системой уязвимостей и уровень критичности соответствующих им дефектов выглядит так:</p>
  <h2>Android</h2>
  <p>Небезопасное хранение ключевой информации:</p>
  <p>    <img height="17" src="../assets/images/image1.png" width="15" /> Доступное на запись хранилище ключей<br />
        <img height="17" src="../assets/images/image1.png" width="15" /> Доступное на запись хранилище ключей со слабым паролем<br />
        <img src="../assets/images/image2.png" /> Доступное на чтение файловое хранилище ключей<br />
        <img src="../assets/images/image.png" /> Доступное на чтение хранилище ключей со слабым паролем, содержащее закрытые ключи<br />
        <img height="18" src="../assets/images/image3.png" width="15" /> Доступное на чтение хранилище ключей со слабым паролем, содержащее открытые ключи<br />
        <img height="18" src="../assets/images/image.png" width="16" /> Доступное на чтение хранилище ключей с приватными ключами, защищёнными слабым паролем<br />
        <img height="18" src="../assets/images/image3.png" width="15" /> Использование файлового хранилища ключей<br />
        <img src="../assets/images/image2.png" /> Хранилище ключей со слабым паролем, содержащее закрытые ключи<br />
        <img height="18" src="../assets/images/image3.png" width="15" /> Хранилище ключей со слабым паролем, содержащее открытые ключи<br />
        <img height="17" src="../assets/images/image1.png" width="15" /> Хранилище ключей с приватными ключами, защищёнными слабым паролем</p>
  <p>Передача sensitive-информации в Activity:</p>
  <p>    <img height="18" src="../assets/images/image.png" width="16" /> Небезопасная передача sensitive-информации в Activity<br />
        <img height="17" src="../assets/images/image1.png" width="15" /> Небезопасная передача sensitive-информации во внешнюю Activity<br />
        <img src="../assets/images/image4.png" /> Передача sensitive-информации во внутреннюю Activity</p>
  <p>Передача sensitive-информации в Service:</p>
  <p>    <img height="18" src="../assets/images/image.png" width="16" /> Небезопасная передача sensitive-информации в Service<br />
        <img height="17" src="../assets/images/image1.png" width="15" /> Небезопасная передача sensitive-информации во внешний Service<br />
        <img src="../assets/images/image4.png" /> Небезопасная передача sensitive-информации во внутренний Service</p>
  <p>Передача sensitive-информации по сети:</p>
  <p>    <img height="17" src="../assets/images/image1.png" width="15" /> Включение sensitive-информации в параметры GET-запроса<br />
        <img src="../assets/images/image4.png" /> Включение чувствительной информации в HTTPS запрос<br />
        <img height="18" src="../assets/images/image.png" width="16" /> Передача sensitive информации в HTTP-запросе<br />
        <img height="18" src="../assets/images/image.png" width="16" /> Получение sensitive информации в HTTP-ответе<br />
        <img src="../assets/images/image4.png" /> Получение чувствительной информации в HTTPS-ответе</p>
  <p>Хранение sensitive-информации:</p>
  <p>    <img src="../assets/images/image2.png" /> Хранение sensitive-информации в памяти<br />
        <img height="18" src="../assets/images/image.png" width="16" /> Хранение sensitive-информации в общедоступном файле вне директории приложения<br />
        <img height="18" src="../assets/images/image.png" width="16" /> Хранение sensitive-информации в общедоступном файле внутри директории приложения<br />
        <img src="../assets/images/image4.png" /> Хранение sensitive-информации в приватном файле вне директории приложения<br />
        <img src="../assets/images/image4.png" /> Хранение sensitive-информации в приватном файле внутри директории приложения<br />
        <img height="18" src="../assets/images/image3.png" width="15" /> Хранение sensitive-информации в общедоступной защищённой базе данных<br />
        <img src="../assets/images/image4.png" /> Хранение sensitive-информации в защищённой базе данных<br />
        <img height="18" src="../assets/images/image.png" width="16" /> Хранение sensitive-информации в общедоступной незащищённой базе данных<br />
        <img height="18" src="../assets/images/image.png" width="16" /> Хранение sensitive-информации в исходном коде приложения<br />
        <img src="../assets/images/image2.png" /> Хранение или использование ранее найденной sensitive-информации<br />
        <img src="../assets/images/image2.png" /> Хранение sensitive-информации в кэше клавиатуры</p>
  <p><img height="17" src="../assets/images/image1.png" width="15" /> Вывод sensitive-информации в системный лог<br />
    <img height="18" src="../assets/images/image3.png" width="15" /> Небезопасный алгоритм подписи<br />
    <img height="18" src="../assets/images/image3.png" width="15" /> Недостаточная длина ключа подписи<br />
    <img height="18" src="../assets/images/image.png" width="16" /> Передача sensitive-информации в BroadcastReceiver<br />
    <img src="../assets/images/image4.png" /> Передача sensitive-информации в параметрах SQL-запроса<br />
    <img src="../assets/images/image2.png" /> Возможность создания резервной копии приложения<br />
    <img height="18" src="../assets/images/image3.png" width="15" /> Приложение не обфусцировано<br />
    <img src="../assets/images/image2.png" /> Слабый пароль шифрования базы данных<br />
    <img src="../assets/images/image4.png" /> Перехват пароля шифрования базы данных<br />
    <img height="17" src="../assets/images/image1.png" width="15" /> Приложение разрешает сетевые соединения по протоколу HTTP<br />
    <img height="17" src="../assets/images/image1.png" style="cursor: nwse-resize;" width="15" /> Небезопасная конфигурация сетевого взаимодействия<br />
    <img height="18" src="../assets/images/image.png" width="16" /> Потенциальное выполнение произвольного кода в контексте приложения<br />
    <img height="17" src="../assets/images/image1.png" style="cursor: nwse-resize;" width="15" /> Хранение значений Cookies в стандартной базе WebView<br />
    <img height="17" src="../assets/images/image1.png" style="cursor: nwse-resize;" width="15" /> Хранение приватного ключа/сертификата не защищенного паролем в директории/ресурсах приложения<br />
    <img src="../assets/images/image4.png" /> Хранение сертификата/ключа в директории/ресурсах приложения<br />
    <img height="18" src="../assets/images/image3.png" width="15" /> Хранение приватного ключа/сертификата защищенного паролем в директории/ресурсах приложения<br />
    <img src="../assets/images/image4.png" /> Хранение сертификата/ключа в директории/ресурсах приложения<br />
    <img height="17" src="../assets/images/image1.png" style="cursor: nwse-resize;" width="15" /> Небезопасные настройки в AndroidManifest.xml<br />
    <img height="18" src="../assets/images/image3.png" width="15" /> Небезопасные настройки в AndroidManifest.xml. Флаг android:hasFragileUserData<br />
    <img height="18" src="../assets/images/image3.png" width="15" /> Небезопасные настройки в AndroidManifest.xml. Флаг android:requestLegacyExternalStorage
  </p>
  <h2>iOS</h2>
  <p>Хранение ключей/сертификатов:</p>
  <p>    <img src="../assets/images/image4.png" /> Хранение сертификата/ключа в директории/ресурсах приложения<br />
        <img height="18" src="../assets/images/image3.png" width="15" /> Хранение приватного ключа/сертификата защищенного паролем в директории/ресурсах приложения<br />
        <img src="../assets/images/image4.png" /> Хранение публичного ключа/сертификата в директории/ресурсах приложения<br />
        <img height="18" src="../assets/images/image.png" width="16" /> Хранение приватного ключа/сертификата не защищенного паролем в директории/ресурсах приложения</p>
  <p>Небезопасное хранение ключевой информации:</p>
  <p>    <img height="17" src="../assets/images/image1.png" style="cursor: nwse-resize;" width="15" /> Доступное на запись хранилище ключей<br />
        <img height="18" src="../assets/images/image.png" width="16" /> Доступное на запись хранилище ключей со слабым паролем<br />
        <img src="../assets/images/image2.png" /> Доступное на чтение файловое хранилище ключей<br />
        <img height="17" src="../assets/images/image1.png" style="cursor: nwse-resize;" width="15" /> Доступное на чтение хранилище ключей со слабым паролем, содержащее закрытые ключи<br />
        <img height="18" src="../assets/images/image3.png" width="15" /> Доступное на чтение хранилище ключей со слабым паролем, содержащее открытые ключи<br />
        <img height="18" src="../assets/images/image.png" width="16" /> Доступное на чтение хранилище ключей с приватными ключами, защищёнными слабым паролем<br />
        <img height="18" src="../assets/images/image3.png" width="15" /> Использование файлового хранилища ключей<br />
        <img src="../assets/images/image2.png" /> Хранилище ключей со слабым паролем, содержащее закрытые ключи<br />
        <img src="../assets/images/image4.png" /> Хранилище ключей со слабым паролем, содержащее открытые ключи<br />
        <img height="17" src="../assets/images/image1.png" style="cursor: nwse-resize;" width="15" /> Хранилище ключей с приватными ключами, защищёнными слабым паролем</p>
  <p><img height="17" src="../assets/images/image1.png" style="cursor: nwse-resize;" width="15" /> Небезопасная конфигурация App Transport Security<br />
    <img src="../assets/images/image2.png" /> Приложение не использует функции защиты от переполнений<br />
    <img src="../assets/images/image4.png" /> Наличие скриптов сборки в собранном пакете приложения<br />
    <img src="../assets/images/image4.png" /> Наличие Podfile в собранном пакете приложения<br />
    <img src="../assets/images/image2.png" /> Хранение sensitive-информации в кэше клавиатуры<br />
    <img height="17" src="../assets/images/image1.png" style="cursor: nwse-resize;" width="15" /> Вывод sensitive-информации в системный лог<br />
    <img height="18" src="../assets/images/image.png" width="16" /> Хранение sensitive-информации в исходном коде приложения<br />
    <img src="../assets/images/image2.png" /> Хранение или использование ранее найденной чувствительной информации
  </p>
  <p> </p>
</body>
</html>