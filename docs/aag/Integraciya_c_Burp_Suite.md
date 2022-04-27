# Интеграция c Burp Suite

  <p>Stingray обеспечивает возможность передачи данных в различные инструменты анализа защищенности приложений, включая очень популярный и универсальный — <a href="https://portswigger.net/burp" target="_blank">Burp Suite</a>. Такая интеграция призвана облегчить более тщательный анализ результатов сканирования. </p>
  <h2><a id="Получение_результатов_работы_модуля_Сетевая_активность"></a>Получение результатов работы модуля Сетевая активность</h2>
  <p>Чтобы передать собранные в результате сканирования данные в Burp Suite, необходимо перейти на страницу <strong>Результаты сканирований</strong>.</p>
  <p style="text-align: center"><img height="980" src="assets/images/image54.png" width="95%" /></p>
  <p>Нажав на строку соответствующего сканирования, перейти на страницу <strong>Результат сканирования</strong>.</p>
  <p style="text-align: center"><img height="617" src="assets/images/image55.png" width="95%" /></p>
  <p>Перейти на вкладку <strong>Собранные данные</strong> и, используя расположенное в левом верхнем углу раскрывающееся меню, выбрать модуль <strong>Сетевая активность</strong>.</p>
  <p style="text-align: center"><img height="843" src="assets/images/image64.png" width="80%" /></p>
  <p>Нажмите кнопку <strong>Загрузить данные модуля</strong>, чтобы скачать zip-архив с результатами сканирования выбранного модуля.</p>
  <p style="text-align: center"><img height="839" src="assets/images/image65.png" width="80%" /></p>
  <p>Скачанный архив содержит три файла в различных форматах.</p>
  <h2>Импорт данных в Burp Suite</h2>
  <p>Для импорта данных в Burp Suite необходимо использовать специальный плагин <strong><a href="https://github.com/Dynamic-Mobile-Security/burp-har-importer">burp-har-loader</a></strong>.</p>
  <p>Скачайте плагин и запустите Burp Suite.</p>
  <p>Перейдите на страницу <strong>Extender</strong>.</p>
  <p style="text-align: center"><img height="807" src="assets/images/image57.png" style="border-width: 1px; border-style: solid; border-color: rgb(0, 0, 0);" width="95%" /></p>
  <p>Нажав кнопку <strong>Add</strong>, откройте окно добавления плагина в Burp Suite.</p>
  <p>В поле <strong>Extension file (.jar)</strong> укажите файл плагина.</p>
  <p>Нажмите кнопку <strong>Next</strong>, чтобы перейти в следующее диалоговое окно.</p>
  <p style="text-align: center"><img height="807" src="assets/images/image58.png" style="border-width: 1px; border-color: rgb(0, 0, 0); border-style: solid;" width="95%" /></p>
  <p>Убедившись, что в следующем окне отображается необходимый плагин, нажмите кнопку <strong>Close</strong>.</p>
  <p style="text-align: center"><img height="411" src="assets/images/image61.png" style="border-width: 1px; border-style: solid; border-color: rgb(0, 0, 0);" width="60%" /></p>
  <p> В результате успешного добавления плагина в строке меню Burp Suite появится новый пункт <strong>HAR Import Sitemap</strong>.</p>
  <p style="text-align: center"><img height="437" src="assets/images/image83.png" width="80%" /></p>
  <p>Нажав пункт меню <strong>HAR Import Sitemap</strong>, переходим на страницу плагина. Реализована возможность импорта данных в двух форматах: csv и har. Процесс экспорта данных из Stingray подробно описан выше в разделе <a href="#Получение_результатов_работы_модуля_Сетевая_активность">Получение результатов работы модуля Сетевая активность</a>.</p>
  <p>Выполнив импорт в удобном формате, можно перейти на вкладку <strong>Target</strong>, где теперь импортированы в Sitemap все запросы, которые были собраны во время работы приложения. Таким образом, можно запустить автоматическое сканирование или проанализировать вручную.</p>
  <p style="text-align: center"><img height="990" src="assets/images/image63.png" style="border-width: 1px; border-style: solid; border-color: rgb(0, 0, 0);" width="95%" /></p>
  <p> </p>
  <p> </p>
</body>
</html>