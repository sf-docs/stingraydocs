# Системы CI/CD

  <p>Для встраивания в процесс CI/CD предусмотрен специальный скрипт, в процессе выполнения которого собранное приложение отправляется в систему для анализа. На выходе формируется JSON файл с подробными результатами.</p>
  <h3>Варианты установки</h3>
  <h4><strong>DockerHub</strong></h4>
  <p>Можно установить пакет, используя docker image:</p>
  <pre class="language-bush"><code class="language-bush">docker pull mobilesecurity/mdast_cli:release-x</code></pre>
  <p><strong>Примечание:</strong> Версия релиза указывается в формате <code>release-x</code>, где <code>x</code> — <span style="font-size:11pt"><span style="line-height:107%">это текущая версия. Пожалуйста, уточняйте эту информацию у вендора или на официальном сайте.</span></span></p>
  <h4><strong>Пакетный менеджер pip</strong></h4>
  <p>Возможно установить пакет, используя pip:</p>
  <pre class="language-bush"><code class="language-bush">pip install mdast_cli</code></pre>
  <p>При таком способе возможно запускать сканирование без указания интерпретатора python при помощи команды mdast_cli, пример:</p>
  <pre class="language-bush"><code class="language-bush">mdast_cli -h</code></pre>
  <p>Во всех примерах ниже будет использован именно такой подход.</p>
  <h4><strong>Исходный код</strong></h4>
  <p>Также поддерживается запуск при помощи загрузки исходных файлов и запуска непосредственно основного скрипта:</p>
  <pre class="language-bush"><code class="language-bush">python3 mdast_cli/mdast_scan.py -h</code></pre>
  <p>При таком способе запуска необходимо дополнительно установить пакеты, указанные в файле requirements.txt.</p>
  <h3>Варианты запуска</h3>
  <p>На данный момент поддерживается несколько вариантов запуска:</p>
  <ul class="Disc">
    <li>анализ приложения, apk-файл которого расположенного локально;</li>
    <li>анализ приложения из системы <a href="https://hockeyapp.net/">HockeyApp</a>;</li>
    <li>анализ приложений из системы <a href="https://appcenter.ms/">AppCenter</a>;</li>
    <li>анализ приложений из системы <a href="https://help.sonatype.com/repomanager3">Nexus Repository 3.x.</a> </li>
  </ul>
  <h3>Параметры запуска</h3>
  <p>Параметры запуска зависят от расположения файла apk, отправляемого на анализ. Так же, существуют обязательные параметры, которые необходимо указывать при любом виде запуска:</p>
  <ul class="Disc">
    <li><code>url</code> — сетевой адрес Stingray (путь до корня без последнего «/»), при использовании cloud версии — <a href="https://saas.stingray-mobile.ru">https://saas.stingray-mobile.ru</a>;</li>
    <li><code>profile_id</code> — id профиля, для которого проводится анализ;</li>
    <li><code>testcase_id</code> — id того тест кейса, который будет воспроизведен во время анализа; возможен запуск нескольких тест кейсов, для этого их id перечисляются через пробел. Это необязательный параметр, если он не задан, то будет запущено сканирование в ручном режиме и через 20 секунд после запуска остановлено, а данные отправлены на анализ;</li>
    <li><code>token </code>— CI/CD токен для доступа, более подробная информация приведена в разделе <a data-xref="{title}" href="Integracii.htm">Интеграции</a>;</li>
    <li><code>distribution_system</code> — способ загрузки приложения, возможные опции: <code>file</code>, <code>appcenter</code>, <code>hockeyapp</code>. Более подробно про них описано ниже в соответствующих разделах;</li>
    <li><code>company_id</code> — идентификатор компании, в рамках которой будет осуществлено сканирование;</li>
    <li><code>architecture_id</code> — опциональный параметр. Определяет идентификатор архитектуры операционной системы, на которой будет произведено сканирование;</li>
    <li><code>nowait </code>— опциональный параметр, определяющий необходимость ожидания завершения сканирования. Если данный флаг установлен — скрипт не будет дожидаться завершения сканирования, а выйдет сразу же после запуска. Если флаг не стоит — скрипт будет ожидать завершения процесса анализа и формировать отчет;</li>
    <li><code>summary_report_json_file_name </code>— опциональный параметр. Определяет имя JSON файла, в который выгружается информация по сканированию в формате JSON. При отсутствии параметра информация сохраняться в JSON не будет;</li>
    <li><code>pdf_report_file_name</code> — опциональный параметр. Определяет имя PDF файла в который выгружается информация по сканированию в формате PDF. При отсутствии параметра PDF-отчет сохраняться не будет.</li>
  </ul>
  <h3>Локальный запуск</h3>
  <p>Данный вид запуска подразумевает, что apk файл приложения для анализа располагается локально, рядом (на одной системе) со скриптом. Для выбора этого способа при запуске необходимо указать параметр <code>distribution_system file</code>. В этом случае обязательным параметром необходимо указать путь к файлу <code>file_path</code>.</p>
  <p><strong>Для запуска анализа локального файла</strong></p>
  <pre class="language-bush"><code class="language-bush">mdast_cli --distribution_system file \
--file_path &quot;/files/demo/apk/demo.apk&quot; \
--url &quot;https://saas.stingray-mobile.ru&quot; \
--profile_id 1 \
--testcase_id 4 \
--company_id 1 
- architecture_id 1 \
--token &quot;token_value&quot; </code></pre>
  <p>В результате будет запущен автоматизированный анализ приложения <code>demo.apk</code> с профилем с <code>id 1</code> и будет запущен тест кейс с <code>id 4</code>.</p>
  <p><strong>Запуск без ожидания завершения сканирования</strong></p>
  <pre class="language-bush"><code class="language-bush">mdast_cli --distribution_system file \
--file_path &quot;/files/demo/apk/demo.apk&quot; \ 
--url &quot;https://saas.stingray-mobile.ru&quot; \
--profile_id 1 \
--testcase_id 4 \
--company_id 1 \ 
— architecture_id 1 \
--token &quot;token_value&quot; \
–nowait</code></pre>
  <p>В результате будет запущен автоматизированный анализ приложения <code>demo.apk</code> с профилем с <code>id 1</code> и будет запущен тест кейс с <code>id 4</code> и скрипт завершится сразу после запуска сканирования и не будет дожидаться окончания и генерировать отчет.</p>
  <p><strong>Генерация Summary отчета в формате JSON</strong></p>
  <pre class="language-bush"><code class="language-bush">mdast_cli --distribution_system file \
--file_path &quot;/files/demo/apk/demo.apk&quot; \ 
--url &quot;https://saas.stingray-mobile.ru&quot; \
--profile_id 1 \
--testcase_id 4 \
--company_id 1 \
- architecture_id 1 \
--token &quot;token_value&quot; \
--summary_report_json_file_name json-scan-repot.json</code></pre>
  <p>В результате будет запущен автоматизированный анализ приложения <code>demo.apk</code> с профилем с <code>id 1</code> и будет запущен тест кейс с <code>id 4</code> и по завершении сканирования будет выгружен JSON отчет с суммарным количеством дефектов и краткой статистикой по сканированию.</p>
  <p> </p>
</body>
</html>