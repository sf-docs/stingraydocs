# Обновление системы

  <h2><a id="Обновление_при_наличии_доступа_к_внешнему_репозиторию_docker-образов_GCP"></a>Обновление при наличии доступа к внешнему репозиторию docker-образов GCP</h2>
  <p>1. Остановите Stingray согласно инструкциям в разделе «<a data-xref="{title}" href="Ostanovka_Stingray.htm">Остановка Stingray</a>». <br />
    2. Обновите специальный docker-образ для подготовки конфигурационных файлов командой:</p>
  <pre class="language-bush"><code class="language-bush">docker pull eu.gcr.io/bishop-233115/stingray/wizard:release-x</code></pre>
  <p><strong></strong> <strong>Примечание:</strong> Версия релиза указывается в формате <code>release-x</code>, где <code>x</code> — это текущая версия. Пожалуйста, уточняйте эту информацию у вендора или на официальном сайте.<span style="font-size:11pt"><span style="line-height:107%"><span style="font-family:Calibri,sans-serif"></span></span></span></p>
  <p>3. Запустите docker-контейнер с параметром <code>update</code>.</p>
  <pre class="language-bush"><code class="language-bush">docker run -i -t -v /opt/stingray:/opt/docker-files eu.gcr.io/bishop-233115/stingray/wizard:release-x update</code></pre>
  <p><strong></strong><strong>Примечание:</strong> Версия релиза указывается в формате <code>release-x</code>, где <code>x</code> — это текущая версия. Пожалуйста, уточняйте эту информацию у вендора или на официальном сайте.<span style="font-size:11pt"><span style="line-height:107%"><span style="font-family:Calibri,sans-serif"></span></span></span></p>
  <p>4. После завершения копирования новых конфигурационных файлов необходимо выполнить команду обновления образов из директории с конфигурационными файлами (в примере <code>/opt/stingray</code>):</p>
  <pre class="language-bush"><code class="language-bush">docker-compose pull
docker pull eu.gcr.io/bishop-233115/stingray/android_api27:release-x
docker pull eu.gcr.io/bishop-233115/stingray/android_api30:release-x
docker pull eu.gcr.io/bishop-233115/stingray/ios:release-x
docker-compose up -d
docker exec stingray-maintenance django-adminmaintenance engines recreate
</code></pre>
  <p><strong>Примечание:</strong> Команда <code>recreate</code> пересоздает контейнеры в их ранее сохраненном состоянии, используя новые версии образов.</p>
  <p><strong></strong><strong>Примечание:</strong> Версия релиза указывается в формате <code>release-x</code>, где <code>x</code> — это текущая версия. Пожалуйста, уточняйте эту информацию у вендора или на официальном сайте.<span style="font-size:11pt"><span style="line-height:107%"><span style="font-family:Calibri,sans-serif"></span></span></span></p>
  <p><strong>Примечание:</strong> При скачивании нового образа старый образ не удаляется. Чтобы накопившиеся старые образы не занимали много места, рекомендуется их удалять, например, с помощью следующих команд:</p>
  <pre class="language-bush"><code class="language-bush">docker image prune</code></pre>
  <p>Эта команда удалит все docker образы без тегов (у которых тег &lt;none&gt;). Следует учитывать, что она не удалит образы с предыдущими версиями. Например, если была установлена версия Stingray 2.7, а вместо нее поставили новую версию 3.0, то старые образы не будут удалены, так как тег у старого образа будет 2.7, а не &lt;none&gt;.</p>
  <pre class="language-bush"><code class="language-bush">docker image prune -a</code></pre>
  <p>Эта команда удалит docker образы без тегов (у которых тег &lt;none&gt;) и docker образы, которые не используются ни одним контейнером. Но в случае, если, например, ещё ни один engine контейнер для какого-нибудь нового образа не создавался (а такое может быть, например, если версия для iOS ещё не использовалась), то эта команда удалит соответствующий образ. Далее, когда возникнет необходимость создать контейнер из этого образа, то это сделать уже не удастся, так как такого образа уже не будет.</p>
  <pre class="language-bush"><code class="language-bush">docker image rm image_id</code></pre>
  <p>Эта команда предназначена для индивидуального удаления образов.</p>
  <p>5. В случае возникновения ошибок возможна загрузка образов вручную:</p>
  <pre class="language-bush"><code class="language-bush">docker pull eu.gcr.io/bishop-233115/stingray/stingray:release-x
docker pull eu.gcr.io/bishop-233115/stingray/android_api27:release-x
docker pull eu.gcr.io/bishop-233115/stingray/android_api30:release-x
docker pull eu.gcr.io/bishop-233115/stingray/ios:release-x
docker pull eu.gcr.io/bishop-233115/stingray/stingray-ui:release-x
docker pull eu.gcr.io/bishop-233115/stingray/stingray-knowledgebase:release-x</code></pre>
  <p><strong>Примечание:</strong> Версия релиза указывается в формате <code>release-x</code>, где <code>x</code> — это текущая версия. Пожалуйста, уточняйте эту информацию у вендора или на официальном сайте.<span style="font-size:11pt"><span style="line-height:107%"><span style="font-family:Calibri,sans-serif"></span></span></span></p>
  <p>После загрузки образов запустите систему согласно инструкциям в предыдущем пункте данного раздела.</p>
  <p>6. Если осуществляется переход с версии Stingray 2.х на версию Stingray 3.х, для корректной работы вновь установленной версии необходимо однократное выполнение команды:</p>
  <pre class="language-bush"><code class="language-bush">docker exec stingray-maintenance django-admin maintenance engines fill_id</code></pre>
  <p>Эта команда обеспечивает корректное взаимодействие всех компонентов системы после обновления версии. Повторное выполнение этой команды не имеет смысла, но при этом Stingray продолжит корректно функционировать.</p>
  <h2>Обновление при отсутствии доступа к внешнему репозиторию docker-образов GCP</h2>
  <p>1. Остановите Stingray согласно инструкциям в разделе «<a data-xref="{title}" href="Ostanovka_Stingray.htm">Остановка Stingray</a>».</p>
  <p>2. При отсутствии доступа к внешнему репозиторию docker-образов, образы поставляются в виде выгруженных tar-архивов. Для доступа к данным архивам необходимо запросить их у поставщика продукта.</p>
  <p>3. После того, как архивы загружены и перенесены на сервер Stingray необходимо их импортировать в docker. Для этого выполните следующую команду для всех полученных архивов:</p>
  <pre class="language-bush"><code class="language-bush">docker load -i &lt;archive_name&gt;.tar</code></pre>
  <p>4. Запустите специальный конфигуратор (Wizard) с параметром <code>update</code>.</p>
  <pre class="language-bush"><code class="language-bush">docker run -i -t -v /opt/stingray-docker-compose:/opt/docker-files eu.gcr.io/bishop-233115/stingray/wizard:release-x update</code></pre>
  <p><strong></strong><strong>Примечание:</strong> Версия релиза указывается в формате <code>release-x</code>, где <code>x</code> — это текущая версия. Пожалуйста, уточняйте эту информацию у вендора или на официальном сайте.<span style="font-size:11pt"><span style="line-height:107%"><span style="font-family:Calibri,sans-serif"></span></span></span></p>
  <p>5. После загрузки образов запустите систему согласно инструкциям в разделе в в пунктах 4 и 6 раздела «<a href="#Обновление_при_наличии_доступа_к_внешнему_репозиторию_docker-образов_GCP">Обновление при наличии доступа к внешнему репозиторию docker-образов GCP</a>».</p>
</body>
</html>