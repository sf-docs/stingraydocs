# Установка Stingray

  <table style="width: 100%;border-width: 1px;border-style: none;border-color: #000000;border-left-width: 1px;border-left-style: none;border-left-color: #000000;border-top-width: 1px;border-top-style: none;border-top-color: #000000;border-right-width: 1px;border-right-style: none;border-right-color: #000000;border-bottom-width: 1px;border-bottom-style: none;border-bottom-color: #000000">
    <colgroup>
      <col style="width: 100px;" />
      <col />
    </colgroup>
    <tbody>
      <tr style="height: 100px;">
        <td style="text-align: center;border-width: 3px;border-style: solid;border-color: transparent;border-left-width: 3px;border-left-style: solid;border-left-color: transparent;border-top-width: 3px;border-top-style: solid;border-top-color: transparent;border-right-width: 3px;border-right-style: solid;border-right-color: transparent;border-bottom-width: 3px;border-bottom-style: solid;border-bottom-color: transparent;background-color: #0069E6"><span style="color:#003366;"><span style="font-size:2rem;"><span style="font-weight:bold;"><span style="font-size: 2rem;color: #FFFFFF">!</span></span></span></span></td>
        <td style="border-width: 3px;border-style: solid;border-color: transparent;border-left-width: 3px;border-left-style: solid;border-left-color: transparent;border-top-width: 3px;border-top-style: solid;border-top-color: transparent;border-right-width: 3px;border-right-style: solid;border-right-color: transparent;border-bottom-width: 3px;border-bottom-style: solid;border-bottom-color: transparent;background-color: rgba(0, 105, 230, 0.30);color: #003366"><strong style="color: #006699"></strong><span style="font-weight:bold;">Все действия, описанные в данном разделе, необходимо производить от пользователя </span><strong><span lang="EN-US">root</span><span style="font-weight:bold;">.</span></strong></td>
      </tr>
    </tbody>
  </table>
  <h2><span style="font-size: 24px; font-weight: bold;">Подготовка инфраструктуры</span></h2>
  <p>1. Необходимо убедиться, что CPU имеет поддержку технологии аппаратной виртуализации (Intel Virtualization Technology (VT, VT-x, vmx) или AMD Virtualization (AMD-V, SVM)), выполнив команды:</p>
  <h3><strong>Для Ubuntu</strong></h3>
  <pre class="language-bush"><code class="language-bush">sudo apt-get install cpu-checker
kvm-ok</code></pre>
  <p>2. Установить требуемые пакеты:</p>
  <h3><strong>Ubuntu Server 16:</strong></h3>
  <pre class="language-bush"><code class="language-bush">sudo apt-get install qemu-kvm libvirt-bin ubuntu-vm-builder bridge-utils</code></pre>
  <h3><strong>Ubuntu Server 18/20:</strong></h3>
  <pre class="language-bush"><code class="language-bush">sudo apt install qemu-kvm libvirt-clients libvirt-daemon-system bridge-utils virt-manager</code></pre>
  <h3><strong>RHEL/CentOS:</strong></h3>
  <pre class="language-bush"><code class="language-bush">sudo yum install qemu-kvm libvirt libvirt-python libguestfs-tools virt-install</code></pre>
  <p>3. Установите docker и docker-compose, если это не было сделано заранее. Рекомендации по установке можно найти на официальном сайте:</p>
  <ul class="Disc">
    <li><a href="https://docs.docker.com/install/linux/docker-ce/ubuntu/">https://docs.docker.com/install/linux/docker-ce/ubuntu/</a></li>
    <li><a href="https://docs.docker.com/compose/install/">https://docs.docker.com/compose/install/</a></li>
  </ul>
  <p>4. Создайте группы и пользователей.</p>
  <pre class="language-bash"><code class="language-bash">groupadd --system --gid 171 kvm
groupadd --gid 1717 emulator
useradd --uid 1717 --gid emulator --groups kvm emulator</code></pre>
  <p>Если группа kvm уже существует, то вместо первой строки выполнить следующее:</p>
  <pre class="language-bash"><code class="language-bash">groupmod --gid 171 kvm
chgrp kvm /dev/kvm</code></pre>
  <h3>Установка при наличии доступа к внешнему репозиторию docker-образов GCP</h3>
  <p>1. Авторизуйте docker на доступ к репозиторию GCP docker-образов:</p>
  <pre class="language-bush"><code class="language-bush">cat stingray-numeric_id.json | sudo docker login -u _json_key --password-stdin https://eu.gcr.io</code></pre>
  <p>Ключ <code>stingray-numeric_id.json</code> для подключения к GCP docker-образам компании Stingray Technologies предоставляется при покупке лицензии Stingray.</p>
  <p>2. Загрузите специальный docker-образ для подготовки конфигурационных файлов командой:</p>
  <pre class="language-bush"><code class="language-bush">docker pull eu.gcr.io/bishop-233115/stingray/wizard:release-x</code></pre>
  <p><strong>Примечание:</strong> Версия релиза указывается в формате <code>release-x</code>, где <code>x</code> — это текущая версия. Пожалуйста, уточняйте эту информацию у вендора или на официальном сайте.</p>
  <h3>Установка без наличия доступа к внешнему репозиторию docker-образов GCP</h3>
  <p>1. При отсутствии доступа к внешнему репозиторию docker-образов, образы поставляются в виде выгруженных tar-архивов. Для доступа к данным архивам необходимо запросить их у поставщика продукта.</p>
  <p>2. После того, как архивы загружены и перенесены на сервер Stingray необходимо их импортировать в docker. Для этого выполните следующую команду для всех полученных архивов:</p>
  <pre class="language-bush"><code class="language-bush">docker load -i &lt;archive_name&gt;.tar</code></pre>
  <h2>Настройка системы</h2>
  <p>1. Создайте директорию, где будут располагаться конфигурационные файлы Stingray, к примеру директорию <code>/opt/stingray</code>.</p>
  <p>2. При необходимости использования HTTPS соединения создайте директорию, где будут располагаться файл полной цепочки сертификатов (например, <code>/opt/certs</code>) и закрытого ключа, скопируйте файлы сертификата и закрытого ключа и назовите их в соответствии с требованиями:</p>
  <ul class="Disc">
    <li>fullchain.pem — полная цепочка сертификатов;</li>
    <li>privkey.pem — приватный ключ.</li>
  </ul>
  <p><br />
    3. Запустите docker-контейнер для подготовки конфигурации.</p>
  <p>Пример запуска контейнера с двумя подключенными volumes для файлов конфигурации и с сертификатами (при доступе по https):</p>
  <pre class="language-bush"><code class="language-bush">docker run -i -t -v /opt/stingray:/opt/docker-files -v /opt/certs:/opt/nginx/certs eu.gcr.io/bishop-233115/stingray/wizard:release-x</code></pre>
  <p><strong></strong><strong>Примечание:</strong> Версия релиза указывается в формате <code>release-x</code>, где <code>x</code> — это текущая версия. Пожалуйста, уточняйте эту информацию у вендора или на официальном сайте.<span style="font-size:11pt"><span style="line-height:107%"><span style="font-family:Calibri,sans-serif"></span></span></span></p>
  <p>Пример запуска контейнера с одним volume для файлов конфигурации (при доступе по http):</p>
  <pre class="language-bush"><code class="language-bush">docker run -i -t -v /opt/stingray:/opt/docker-files eu.gcr.io/bishop-233115/stingray/wizard:release-x</code></pre>
  <p><strong></strong><strong>Примечание:</strong> Версия релиза указывается в формате <code>release-x</code>, где <code>x</code> — это текущая версия. Пожалуйста, уточняйте эту информацию у вендора или на официальном сайте.<span style="font-size:11pt"><span style="line-height:107%"><span style="font-family:Calibri,sans-serif"></span></span></span></p>
  <p>После запуска контейнера в интерактивном режиме необходимо заполнить ряд параметров.</p>
  <table class="thickhdrevenrows" style="width: 100%;border-width: 1px;border-color: #FFFFFF;border-left-width: 1px;border-left-color: #FFFFFF;border-top-width: 1px;border-top-color: #FFFFFF;border-right-width: 1px;border-right-color: #FFFFFF;border-bottom-width: 1px;border-bottom-color: #FFFFFF">
    <colgroup>
      <col style="width: 311px;" />
      <col style="width: 677px;" />
      <col />
    </colgroup>
    <thead>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>Параметр</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>Описание</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p><strong>Значение<br />
              по умолчанию</strong></p>
        </td>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="text-align: left;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_DOMAIN</strong></p>
        </td>
        <td style="text-align: left;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Домен, на котором будет располагаться система для корректной настройки маршрутизации и обращения UI к нужному серверу</p>
        </td>
        <td style="text-align: left;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>saas.stingray-mobile.ru</p>
        </td>
      </tr>
      <tr>
        <td style="text-align: left;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem"><strong>IP_EXTERNAL</strong></td>
        <td style="text-align: left;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">IP сервера, на который устанавливается Stingray</td>
        <td style="text-align: left;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">0.0.0.0</td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>USE_SSL</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Параметр, определяющий, будет ли проходить соединение через протокол http или https. При указании «1» приложение конфигурируется для использования 443 порта и проводит настройку для соединения по https (копируются из второго volume с сертификатами)</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>0</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>POSTGRES_USER</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Пользователь, с которым будет запущена база данных Postgres</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>stingray</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>POSTGRES_PASSWORD</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Пароль пользователя, с которым будет запущена база данных Postgres</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>P@ssw0rd</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>RABBITMQ_DEFAULT_USER</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Пользователь, с которым будет запущен брокер RabbitMQ</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>stingray</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>RABBITM</strong><strong>Q_DEFAULT_</strong><strong>PASS</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Пароль пользователя, с которым будет запущен брокер RabbitMQ</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>P@ssw0rd</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_DEBUG</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Запустить сервер с расширенным выводом ошибок</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>0</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_DOCKER_LOGIN</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Флаг, определяющий сетевую доступность от сервера Stingray до внешнего хранилища docker-образов. Данная функциональность необходима для динамического создания сканирующих агентов и загрузки последней актуальной версии при обновлении системы. При выставлении значения «0» — необходимо при обновлении системы вручную загрузить последнюю версию образа</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>0</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_LANGUAGE_CODE</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Язык системы по умолчанию. Влияет на язык swagger и язык по умолчанию для вновь создаваемых языков</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>ru</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_TIME_ZONE</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Временная зона для корректного отображения времени</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Europe/Moscow</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_ACCESS_TOKEN_LIFETIME</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Время жизни access_token в минутах</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>60</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_REFRESH_TOKEN_LIFETIME</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Время жизни refresh_token в минутах</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>1440</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_CI_TOKEN_LIFETIME</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Время жизни токена для интеграции в CI/CD</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>525600</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_COMPANY_NAME</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Название компании</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Company Name</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_COMPANY_DESCRIPTION</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Описание компании</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Company Description</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_SUPERUSER_USERNAME</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Имя пользователя с ролью Супер администратора</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>admin</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_SUPERUSER_PASSWORD</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Пароль пользователя с ролью Супер администратора</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>admin</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_CREATE_SAMPLES</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Флаг, определяющий, нужно ли создавать сущности по умолчанию, которые задаются далее (проекты/профили, пользователей, сканирующие агенты/компания)</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>1</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_ADMIN_USERNAME</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Имя пользователя с ролью Администратора компании</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>company_admin</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_ADMIN_PASSWORD</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Пароль пользователя с ролью Администратора компании</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>123</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_ADMIN_FIRSTNAME</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Имя Администратора</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>FirstName</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_ADMIN_LASTNAME</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Фамилия Администратора</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>LastName</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_ENGINE_NAME_ANDROID</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Имя агента для Android Engine</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>stingray-engine</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_ENGINE_NAME_IOS</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Имя агента для iOS Engine</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>stingray-engine-ios</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_PROJECT_NAME_ANDROID</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Имя проекта, создаваемого по умолчанию для Android проекта</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Project Name Android</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_PROJECT_DESCRIPTION_ANDROID</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Описание проекта, создаваемого по умолчанию для Android проекта</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Project Description Android</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_PROJECT_NAME_I</strong><strong>OS</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Имя проекта, создаваемого по умолчанию для iOS проекта</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Project Name iOS</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_PROJECT_DESCRIPTION_IOS</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Описание проекта, создаваемого по умолчанию для iOS<br />
            проекта</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Project Description iOS</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_PROFILE_NAME_ANDROID</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Имя профиля, создаваемого по умолчанию для Android проекта</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Profile Name Android</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_PROFILE_DESCRIPTION_ANDROID</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Описание профиля, создаваемого по умолчанию для Android проекта</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Profile Description Android</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_PROFILE_NAME_IOS</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Имя профиля, создаваемого по умолчанию для iOS проекта</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Profile Name iOS</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_PROFILE_DESCRIPTION_IOS</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Описание профиля, создаваемого по умолчанию для iOS проекта</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Profile Description iOS</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_AUDIT_USE</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Включение или выключение аудита событий в системе</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>1</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_AUDIT_MAX_LENGTH</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Максимальное количество записей в одном файле. После  превышения заданного количества к концу файла добавляется постфикс (.1, .2 и  т. д.), а новая информация записывается в стандартный файл без постфикса</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>1000</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>STINGRAY_AUDIT_FILE_COUNT</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Количество файлов, которое будет храниться в системе. При превышении количества файлов старые удаляются</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>10</p>
        </td>
      </tr>
    </tbody>
  </table>
  <p><br />
    В результате выполнения в директории <code>/opt/stingray</code> будут созданы все необходимые файлы для запуска.</p>
  <h3>Список контейнеров</h3>
  <table class="thickhdrevenrows" style="width: 100%;">
    <thead>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF">
          <p><strong>Имя контейнера<span></span></strong></p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF">
          <p><strong>Описание<span></span></strong></p>
        </td>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="text-align: left;font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF">
          <p><strong>stingray-nginx</strong></p>
        </td>
        <td style="text-align: left;font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF">
          <p>Входная точка для обращений к backend и UI. Выполняет функции reverse-proxy</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF">
          <p><strong>stingray-backend</strong></p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF">
          <p>Backend приложения, отвечает за основную логику обработки пользовательских запросов и выдачу результатов</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF">
          <p><strong>stingray-rabbitmq</strong></p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF">
          <p>Менеджер очередей сканирования, управляет очередью сканирования</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF">
          <p><strong>stingray-postgres</strong></p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF">
          <p>База данных</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF">
          <p><strong>stingray-ui</strong></p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF">
          <p>Пользовательский интерфейс</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF">
          <p><strong>stingray-redis</strong></p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF">
          <p>Redis для промежуточного хранения оперативной информации</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF">
          <p><strong>engine-android</strong></p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF">
          <p>Сканирующий модуль для Android-проектов. Название контейнера может быть произвольным</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF">
          <p><strong>engine-ios</strong></p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF">
          <p>Сканирующий модуль для iOS-проектов. Название контейнера может быть произвольным</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF">
          <p><strong>stingray-knowledgebase</strong></p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF">
          <p>Контейнер, содержащий в себе информацию по устранению уязвимостей и документацию</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF"><strong>stingray-<span style="font-size:10.5pt"><span style="font-family:&quot;Segoe UI&quot;,sans-serif"><span style="color:#172b4d">maintenance</span></span></span></strong></td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF">
          Осуществляет управление контейнерами - проверку статусов, перезагрузку, запуск и остановку
        </td>
      </tr>
    </tbody>
  </table>
  <p> </p>
</body>
</html>