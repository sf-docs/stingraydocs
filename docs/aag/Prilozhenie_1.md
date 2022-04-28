# Приложение 1. Список модулей для сбора информации

  <p>Система включает в себя девятнадцать модулей для сканирования и сбора информации о приложении. Каждый из модулей отвечает за сбор «своей» информации. Список всех модулей представлен на вкладке <strong>Модули</strong> на странице <strong>Профиль</strong>. Из этого списка можно выбрать и настроить те модули, которые будут включены при сканировании приложения с этим профилем.</p>
  <p>Модули собирают следующую информацию:</p>
  <ul>
    <li><strong>Базы данных </strong>— список баз данных, с которыми работает приложение. Указываются путь к базе данных, ее тип и размер.</li>
    <li><strong>Файлы приложения</strong> — список файлов приложения. Указываются путь к файлу, его расширение, тип, владелец, ID владельца, группа, ID группы, права, время создания и модификации.</li>
    <li><strong>Дамп памяти приложения</strong> — указаны имя файла с дампом памяти приложения,<br />
      его тип и размер.</li>
    <li><strong>Информация о ключах </strong>— модуль для анализа безопасности хранилищ, в которых сохраняются ключи шифрования</li>
    <li><strong>Записи системного журнала</strong> — указываются уровень, время, приложение, тэг,<br />
      ID процесса, ID потока, сообщение.</li>
    <li><strong>Отслеживание Activity</strong> — это все различные экраны приложения, которые были<br />
      запущены во время сканирования. Для каждого экрана (Activity) приведены его имя, метод и параметры запуска.</li>
    <li><strong>Отслеживание Broadcast Receiver</strong> — Broadcast Receiver по умолчанию открыт для взаимодействия с другими приложениями. Указываются время, метод, зарегистрированные интенты (Intents), полученный интент (Intent), разрешения, приоритет, компонент и дополнительные параметры.</li>
    <li><strong>Отслеживание Broadcast</strong> — при использовании Broadcast Receiver риски и соответствующие им защитные меры различаются в зависимости от типа Broadcast. Указываются время, имя и метод объектов.</li>
    <li><strong>Отслеживание Intent</strong> — межпроцессное взаимодействие в Android осуществляется при помощи специальных объектов — Intent. Для каждого объекта приведены время, имя, метод и параметры.</li>
    <li><strong>Отслеживание Service</strong> — объекты типа Service, открытые для взаимодействия с другими приложениями и не относящиеся к системным Android-вызовам. Указываются время, имя и метод объекта.</li>
    <li><strong>Работа с SharedPreferences</strong> — постоянное хранилище на платформе Android, используемое приложениями для хранения, например, своих настроек. Указываются время, метод, ключ и его старое и новое значение.</li>
    <li><strong>Сетевая активность</strong> — все данные, переданные по сети — адрес, хост, протокол, время, метод, порт, а также содержание запроса и ответа.</li>
    <li><strong>Содержимое SharedPreferences</strong> — указываются ключ и его значение.</li>
    <li><strong>Работа с SQLite</strong> — это легковесная база данных в Android. Указываются время, метод и запрос.</li>
    <li><strong>Используемые файлы</strong> — список файлов, не являющихся внутренними файлами приложения, с которыми оно взаимодействует. Указываются путь к файлу, его расширение, владелец, группа, права, тип операции (чтение/запись), количество обращений, время создания и модификации.</li>
    <li><a name="_Hlk60065543"><strong>SAST</strong> — </a>модуль для статического анализа приложения.</li>
    <li><strong>Поиск чувствительной информации</strong> — модуль для поиска ранее найденных чувствительных значениях в результатах других модулей. То есть, если была найдена чувствительная информация «session_id=111222333444555», то будет произведен поиск значения «111222333444555» во всех ранее собранных данных, но уже без привязки к имени параметра «session_id».</li>
    <li><strong>Интеграция с AppScreener</strong> — модуль интеграции с системой статического анализа <a href="https://rt-solar.ru/products/solar_appscreener/capabilities/">Solar AppScreener</a>. </li>
  </ul>
  <ul>
    <li><strong>Интеграция с Oversecured</strong> — модуль интеграции c системой статического анализа <a href="https://oversecured.com/">Oversecured</a>.</li>
  </ul>
  <p> </p>
</body>
</html>