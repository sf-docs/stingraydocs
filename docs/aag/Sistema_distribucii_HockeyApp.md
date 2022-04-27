# Система дистрибуции HockeyApp

  <h3>Параметры запуска</h3>
  <p>Параметры запуска зависят от расположения файла apk, отправляемого на анализ.</p>
  <p>Так же, существуют обязательные параметры, которые необходимо указывать при любом виде запуска:</p>
  <ul class="Disc">
    <li><code>url</code> — сетевой адрес Stingray (путь до корня без последнего «/»), при использовании cloud версии — <a href="https://saas.stingray-mobile.ru">https://saas.stingray-mobile.ru</a>;</li>
    <li><code>profile_id</code> — id профиля, для которого проводится анализ;</li>
    <li><code>testcase_id</code> — id того тест кейса, который будет воспроизведен во время анализа; возможен запуск нескольких тест кейсов, для этого их id перечисляются через пробел. Это необязательный параметр, если он не задан, то будет запущено сканирование в ручном режиме и через 20 секунд после запуска остановлено, а данные отправлены на анализ;</li>
    <li><code>token </code>— CI/CD токен для доступа, более подробная информация приведена в разделе <a data-xref="{title}" href="Integracii.htm">Интеграции</a>;</li>
    <li><code>distribution_system</code> — способ загрузки приложения, возможные опции: <code>file</code>, <code>hockeyapp</code>. Более подробно про них описано ниже в соответствующих разделах;</li>
    <li><code>company_id </code>— идентификатор компании, в рамках которой будет осуществлено сканирование;</li>
    <li><code>architecture_id</code> — опциональный параметр. Определяет идентификатор архитектуры операционной системы, на которой будет произведено сканирование;</li>
    <li><code>nowait </code>— опциональный параметр, определяющий необходимость ожидания завершения сканирования. Если данный флаг установлен — скрипт не будет дожидаться завершения сканирования, а выйдет сразу же после запуска. Если флаг не стоит — скрипт будет ожидать завершения процесса анализа и формировать отчет;</li>
    <li><code>summary_report_json_file_name</code> — опциональный параметр. Определяет имя JSON файла, в который выгружается информация по сканированию в формате JSON. При отсутствии параметра информация сохраняться в JSON не будет;</li>
    <li><code>pdf_report_file_name</code> — опциональный параметр. Определяет имя PDF файла в который выгружается информация по сканированию в формате PDF. При отсутствии параметра PDF-отчет сохраняться не будет.</li>
  </ul>
  <h3>Примеры запуска</h3>
  <h4>HockeyApp по bundle_identifier и указанной версии</h4>
  <p>Для запуска анализа приложения из системы HockeyApp:</p>
  <pre class="language-bush"><code class="language-bush">mdast_cli --distribution_system hockeyapp --hockey_token 18bc81146d374ba4b1182ed65e0b3aaa --bundle_id com.appsec.demo 
--hockey_version 31337 --url &quot;https://saas.stingray-mobile.ru&quot; --profile_id 2 --testcase_id 3 --company_id 1 
--architecture_id 1 --token &quot;eyJ0eXA4OiJKA1QiLbJhcGciO5JIU4I1NiJ1...&quot;</code></pre>
  <p>В результате в системе HockeyApp будет найдено приложение с идентификатором пакета <code>com.appsec.demo</code> и версией <code>31337</code>. Он будет скачан и для него будет проведен автоматизированный анализ с профилем с<code> id 2</code> и будет запущен тест кейс с <code>id 3</code>.</p>
  <h4>HockeyApp по public_identifier и с последней доступной версией</h4>
  <p>Для запуска анализа последней версии приложения из системы HockeyApp по его публичному идентификатору:</p>
  <pre class="language-bush"><code class="language-bush">mdast_cli --distribution_system hockeyapp --hockey_token 18bc81146d374ba4b1182ed65e0b3aaa 
--public_id &quot;1234567890abcdef1234567890abcdef&quot; --url &quot;https://saas.stingray-mobile.ru&quot; --profile_id 2 
--testcase_id 3 --company_id 1 --architecture_id 1 --token &quot;eyJ0eXA4OiJKA1QiLbJhcGciO5JIU4I1NiJ1...&quot;</code></pre>
  <p>В результате в системе HockeyApp будет найдено приложение с уникальным публичным идентификатором <code>1234567890abcdef1234567890abcdef</code> и последней доступной версией. Файл приложения будет скачен и для него будет проведен автоматизированный анализ с профилем с<code> id 2</code> и будет запущен тест кейс с <code>id 3</code>.</p>
  <p> </p>
</body>
</html>