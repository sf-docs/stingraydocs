# Остановка Stingray

  <p>Для остановки приложения в папке, указанной при генерации конфигурации (в примере <code>/opt/stingray</code>) выполните команду:</p>
  <pre class="language-bush"><code class="language-bush">docker exec stingray-maintenance django-admin maintenance engines preserve
docker-compose stop<span style="font-size: 1rem; word-spacing: normal;"></span></code></pre>
  <p><strong>Примечание:</strong> Команда <code>preserve</code> сохраняет состояние контейнеров для дальнейшего их восстановления в ранее сохраненном состоянии.</p>
</body>
</html>