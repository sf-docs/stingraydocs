# Запуск Stingray

  <p><strong>Примечание:</strong> В этом разделе описан процесс первого запуска версии системы Stingray. Процесс запуска Stingray после обновления версии описан в разделе «<a href="Obnovlenie_sistemy.htm">Обновление</a><a href="Obnovlenie_sistemy.htm"> системы</a>». Процесс запуска Stingray после перезагрузки сервера без обновления версии Stingray описан в разделе «<a href="Perezagruzka_servera_bez_obnovleniya_Stingray.htm">Перезагрузка</a><a href="Perezagruzka_servera_bez_obnovleniya_Stingray.htm"> сервера без обновления </a><a href="Perezagruzka_servera_bez_obnovleniya_Stingray.htm">Stingray</a>».</p>
  <p>Для запуска приложения в папке, указанной при генерации конфигурации (в примере <code>/opt/stingray</code>) выполните команды:</p>
  <pre class="language-bush"><code class="language-bush">docker-compose pull
docker pull eu.gcr.io/bishop-233115/stingray/android_api27:release-x
docker pull eu.gcr.io/bishop-233115/stingray/android_api30:release-x
docker pull eu.gcr.io/bishop-233115/stingray/ios:release-x
docker-compose up -d<span style="color:#ff0000;">
</span></code></pre>
  <p><strong></strong><strong>Примечание:</strong> Версия релиза указывается в формате <code>release-x</code>, где <code>x</code> — это текущая версия. Пожалуйста, уточняйте эту информацию у вендора или на официальном сайте.<span style="font-size:11pt"><span style="line-height:107%"><span style="font-family:Calibri,sans-serif"></span></span></span></p>
  <p>При первом запуске системы, в случае если команда docker-compose не может загрузить образ из репозитория, необходимо вручную загрузить контейнеры:</p>
  <pre class="language-bush"><code class="language-bush">docker pull eu.gcr.io/bishop-233115/stingray/stingray:release-x
docker pull eu.gcr.io/bishop-233115/stingray/android_api27:release-x
docker pull eu.gcr.io/bishop-233115/stingray/android_api30:release-x
docker pull eu.gcr.io/bishop-233115/stingray/ios:release-x
docker pull eu.gcr.io/bishop-233115/stingray/stingray-ui:release-x
docker pull eu.gcr.io/bishop-233115/stingray/stingray-knowledgebase:release-x</code></pre>
  <p><strong></strong><strong>Примечание:</strong> Версия релиза указывается в формате <code>release-x</code>, где <code>x</code> — это текущая версия. Пожалуйста, уточняйте эту информацию у вендора или на официальном сайте.<span style="font-size:11pt"><span style="line-height:107%"><span style="font-family:Calibri,sans-serif"></span></span></span></p>
  <p> </p>
</body>
</html>