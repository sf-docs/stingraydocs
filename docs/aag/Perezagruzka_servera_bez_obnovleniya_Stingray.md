# Перезагрузка сервера без обновления Stingray

  <p>В случае, если необходимо перезагрузить сервер, на котором установлен Stingray, например, для обновления операционной системы, без обновления версии Stingray, перед перезагрузкой сервера в папке, указанной при генерации конфигурации (в примере <code>/opt/stingray</code>), выполните команду:</p>
  <pre class="language-bush"><code class="language-bush">docker exec stingray-maintenance django-admin maintenance engines preserve</code></pre>
  <p>После перезагрузки сервера там же выполните команду:</p>
  <pre class="language-bush"><code class="language-bush">docker exec stingray-maintenance django-admin maintenance engines restore</code></pre>
</body>
</html>