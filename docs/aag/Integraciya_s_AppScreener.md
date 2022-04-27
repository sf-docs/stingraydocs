# Интеграция с AppScreener

  <h2>Создание токена AppScreener</h2>
  <p>Для интеграции с AppScreener  понадобится токен, порядок создания которого приведен ниже.</p>
  <p>В пользовательском интерфейсе AppScreener перейдите в профиль пользователя (<strong>Profile</strong>).</p>
  <p style="text-align: center"><img height="774" src="assets/images/image69.png" style="border-width: 1px; border-style: solid; border-color: rgb(0, 0, 0);" width="95%" /></p>
  <p>Откройте вкладку <strong>Token</strong>. Введите пароль Администратора AppScreener в поле <strong>Enter your password</strong>, укажите время действия токена в минутах в поле <strong>Token validity time (min)</strong> и нажмите кнопку <strong>Get Active Token</strong>.</p>
  <p>Скопируйте сгенерированный токен, нажав иконку <img height="22" src="assets/images/image70.png" width="20" /> .</p>
  <p style="text-align: center"><img height="774" src="assets/images/image72.png" style="border-width: 1px; border-style: solid; border-color: rgb(0, 0, 0); cursor: nwse-resize;" width="95%" /></p>
  <h2><a id="Настройка_интеграции_с_AppScreener"></a>Настройка интеграции с AppScreener</h2>
  <p>Для настройки интеграции с AppScreener выберите в раскрывающемся меню вкладки <strong>Интеграции</strong> параметр <strong>AppScreener</strong> и переведите селектор, расположенный ниже, в положение «включено». </p>
  <p>Вставьте токен для интеграции с инструментом в поле <strong>Токен</strong>.</p>
  <p><strong>URL</strong> задается при установке системы и не может быть изменен <strong>Пользователем</strong> или <strong>Администратором</strong> системы.</p>
  <p>Нажмите кнопку <strong>Тест</strong>, чтобы проверить соединение с инструментом. В случае успешного соединения в верхней части пользовательского интерфейса появится сообщение о том, что введенный токен валиден, в противном случае отобразится предупреждение об ошибке.</p>
  <p>После нажатия кнопки <strong>Сохранить</strong> в нижней части страницы отобразится таблица, позволяющая задать соответствия между проектами Stingray и AppScreener. Для настройки используйте раскрывающиеся меню.</p>
  <p style="text-align: center"> <img height="849" src="assets/images/image34.png" style="border-width: 1px; border-style: solid; border-color: rgb(0, 0, 0); cursor: nesw-resize;" width="95%" /></p>
  <p>Для завершения настройки интеграции необходимо перейти в профиль выбранного проекта и на вкладке <strong>Модули</strong> активировать соответствующий модуль, см. раздел <a href="../ug/profile.htm" title="Профиль">Профиль</a> Руководства пользователя.</p>
  <p style="text-align: center"><img height="1176" src="assets/images/image35.png" style="cursor: nesw-resize; border-width: 1px; border-style: solid; border-color: rgb(0, 0, 0);" width="95%" /></p>
  <p style="text-align: left"><strong>Примечание:</strong> Если в ходе настройки интеграции с инструментом не были заданы соответствия проектов Stingray и AppScreener, то при первом запуске сканирования в AppScreener будет создан проект, название которого будет сформировано по следующей маске:</p>
  <p style="text-align: center"><span style="text-align:center;"><em>Название компании_название проекта</em></span></p>
  <p>Все последующие сканирования в выбранном проекте Stingray будут выполняться в созданном проекте AppScreener при условии, что в профиле проекта Stingray включена соответствующая интеграция.</p>
  <p> </p>
</body>
</html>