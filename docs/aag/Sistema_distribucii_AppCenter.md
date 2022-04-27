# Система дистрибуции AppCenter

  <h3>Параметры запуска</h3>
  <p>Параметры запуска зависят от расположения файла apk, отправляемого на анализ. Так же, существуют обязательные параметры, которые необходимо указывать при любом виде запуска:</p>
  <ul class="Disc">
    <li><code>url </code>— сетевой адрес Stingray (путь до корня без последнего «/»), при использовании cloud версии — <a href="https://saas.stingray-mobile.ru">https://saas.stingray-mobile.ru</a>; </li>
    <li><code>profile_id</code> — id профиля, для которого проводится анализ;</li>
    <li><code>testcase_id</code> — id того тест кейса, который будет воспроизведен во время анализа; возможен запуск нескольких тест кейсов, для этого их id перечисляются через пробел. Это необязательный параметр, если он не задан, то будет запущено сканирование в ручном режиме и через 20 секунд после запуска остановлено, а данные отправлены на анализ;</li>
    <li><code>token </code>— CI/CD токен для доступа, более подробная информация приведена в разделе <a data-xref="{title}" href="Integracii.htm">Интеграции</a>;</li>
    <li><code>distribution_system</code> — способ загрузки приложения, возможные опции: <code>file</code>, <code>appcenter</code>;</li>
    <li><code>company_id </code>— идентификатор компании, в рамках которой будет осуществлено сканирование;</li>
    <li><code>architecture_id</code> — опциональный параметр. Определяет идентификатор архитектуры операционной системы, на которой будет произведено сканирование;</li>
    <li><code>nowait </code>— опциональный параметр, определяющий необходимость ожидания завершения сканирования. Если данный флаг установлен — скрипт не будет дожидаться завершения сканирования, а выйдет сразу же после запуска. Если флаг не стоит — скрипт будет ожидать завершения процесса анализа и формировать отчет;</li>
    <li><code>summary_report_json_file_name</code> — опциональный параметр. Определяет имя JSON файла, в который выгружается информация по сканированию в формате JSON. При отсутствии параметра информация сохраняться в JSON не будет;</li>
    <li><code>pdf_report_file_name</code> — опциональный параметр. Определяет имя PDF файла в который выгружается информация по сканированию в формате PDF. При отсутствии параметра PDF-отчет сохраняться не будет.</li>
  </ul>
  <p>Для загрузки приложения из системы дистрибуции AppCenter при запуске необходимо указать параметр <code>distribution_system appcenter</code>. Так же необходимо указать обязательные параметры:</p>
  <ul class="Disc">
    <li><code>appcenter_token</code> — API токен для доступа. Как его получить можно узнать <a href="https://docs.microsoft.com/en-us/appcenter/api-docs/">здесь</a>;</li>
    <li><code>appcenter_owner_name</code> — владелец приложения, как узнать имя владельца можно <a href="https://intercom.help/appcenter/en/articles/1764707-how-to-find-the-app-name-and-owner-name-from-your-app-url">здесь</a> или в <a href="https://docs.microsoft.com/en-us/appcenter/api-docs/#find-your-app-center-app-name-and-owner-name">официальной документации</a>;</li>
    <li><code>appcenter_app_name</code> — имя приложения в системе AppCenter. Как его узнать можно по <a href="https://docs.microsoft.com/en-us/appcenter/api-docs/#find-your-app-center-app-name-and-owner-name">ссылке</a>;</li>
    <li><code>appcenter_release_id</code> или <code>appcenter_app_version</code>:
      <ul>
        <li><code>appcenter_release_id</code> — идентификатор загружаемого релиза в системе AppCenter для конкретного приложения. Возможно выставить значение <code>latest </code>— тогда будет загружен последний доступный релиз приложения. <a href="https://openapi.appcenter.ms/#/distribute/releases_getLatestByUser">Официальная документация</a>;</li>
        <li><code>appcenter_app_version </code>— при указании данного параметра будет найдена и скачана конкретная версия приложения по коду его версии (указанной в Android Manifest) (поле <code>version </code>в <a href="https://openapi.appcenter.ms/#/distribute/releases_list">документации</a>).</li>
      </ul>
    </li>
  </ul>
  <h3>Примеры запуска</h3>
  <h4>AppCenter по id релиза</h4>
  <p>Для запуска анализа приложения по известному имени, владельцу и ID релиза необходимо выполнить следующую команду:</p>
  <pre class="language-bush"><code class="language-bush">mdast_cli --distribution_system appcenter --appcenter_token 18bc81146d374ba4b1182ed65e0b3aaa 
--appcenter_owner_name test_org_or_user --appcenter_app_name demo_app --appcenter_release_id 710 
--url &quot;https://saas.stingray-mobile.ru&quot; --profile_id 2 --testcase_id 3 --company_id 1 --architecture_id 1
--token &quot;eyJ0eXA4OiJKA1QiLbJhcGciO5JIU4I1NiJ1...&quot;</code></pre>
  <p>В результате у владельца (пользователя или организации <code>test_org_or_user</code>) будет найдено приложение <code>demo_app</code> с <code>ID</code> релиза <code>710</code>. Данная версия релиза будет загружена и передана на анализ безопасности в Stingray.</p>
  <p>Для загрузки релиза с последней версией необходимо указать параметр <code>appcenter_release_id latest</code>. Тогда команда будет выглядеть следующим образом:</p>
  <pre class="language-bush"><code class="language-bush">mdast_cli --distribution_system appcenter --appcenter_token 18bc81146d374ba4b1182ed65e0b3aaa 
--appcenter_owner_name &quot;test_org_or_user&quot; --appcenter_app_name &quot;demo_app&quot; --appcenter_release_id latest 
--url &quot;https://saas.stingray-mobile.ru&quot; --profile_id 2 --testcase_id 3 --company_id 1 --architecture_id 1
--token &quot;eyJ0eXA4OiJKA1QiLbJhcGciO5JIU4I1NiJ1...&quot;</code></pre>
  <p>и будет загружен последний доступный релиз для данного приложения.</p>
  <h4>AppCenter по версии приложения</h4>
  <p>Для запуска анализа приложения по известному имени, владельцу и версии приложения (version_code в Android Manifest) необходимо выполнить следующую команду:</p>
  <pre class="language-bush"><code class="language-bush">mdast_cli --distribution_system appcenter --appcenter_token 18bc81146d374ba4b1182ed65e0b3aaa 
--appcenter_owner_name &quot;test_org_or_user&quot; --appcenter_app_name &quot;demo_app&quot; --appcenter_app_version 31337 
--url &quot;https://saas.stingray-mobile.ru&quot; --profile_id 2 --testcase_id 3 --company_id 1 --architecture_id 1 
--token &quot;eyJ0eXA4OiJKA1QiLbJhcGciO5JIU4I1NiJ1...&quot;</code></pre>
  <p>В результате у владельца (пользователя или организации <code>test_org_or_user</code>) будет найдено приложение <code>demo_app</code> и найден релиз, в котором была указана версия приложения <code>31337</code>. Данная версия релиза будет загружена и передана на анализ безопасности в Stingray.</p>
  <p> </p>
</body>
</html>