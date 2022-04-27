# Требования к инфраструктуре

  <h2>Требования к аппаратному обеспечению</h2>
  <p>Серверные компоненты Stingray должны быть установлены на выделенном сервере, предназначенном исключительно для эксплуатации серверных компонент Stingray.<br />
    Для работы системы обязательно наличие поддержки виртуализации, а именно необходим процессор с поддержкой технологии виртуализации Intel Virtualization Technology (VT, VT-x, vmx) или AMD Virtualization (AMD-V, SVM).</p>
  <h3><strong>Минимальные технические характеристики серверного оборудования</strong></h3>
  <ul class="Disc">
    <li>4-ядерный CPU 2 ГГц; </li>
    <li>Оперативная память: 16 Гб. </li>
    <li>Свободное дисковое пространство 100 GB (+ пространство для размещения прикладных систем и баз данных).</li>
  </ul>
  <h3><strong>Рекомендуемые технические характеристики серверного оборудования</strong></h3>
  <ul class="Disc">
    <li>8-ядерный CPU 3 ГГц. </li>
    <li>Оперативная память: 32 Гб. </li>
    <li>Свободное дисковое пространство 500 GB (+ пространство для размещения прикладных систем и баз данных).</li>
  </ul>
  <p>Для развертывания платформы Stingray на базе клиентской инфраструктуры требуется следующая минимальная аппаратная или виртуальная конфигурация оборудования:</p>
  <table class="thickhdrevenrows" style="width: 100%;font-size: 0.9rem">
    <colgroup>
      <col style="width: 372px;" />
      <col style="width: 119px;" />
      <col style="width: 453px;" />
      <col style="width: 107px;" />
      <col style="width: 96px;" />
    </colgroup>
    <tbody>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>Наименование<span></span></strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.30);text-align: center;border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>Кол-во<span></span></strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>CPU</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.30);text-align: center;border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>RAM, Гб</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.30);text-align: center;border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>HDD, Гб</strong></p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Stingray (минимальная конфигурация)<b></b></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);text-align: center;border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>1</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>4-ядерный процессор (2 ГГц) с поддержкой технологии виртуализации Intel Virtualization Technology (VT, VT-x, vmx) или AMD Virtualization (AMD-V, SVM)</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);text-align: center;border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>16</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);text-align: center;border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>200</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Stingray (рекомендуемая конфигурация)<b></b></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);text-align: center;border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>1</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>8-ядерный процессор (3 ГГц) с поддержкой технологии виртуализации Intel Virtualization Technology (VT, VT-x, vmx) или AMD Virtualization (AMD-V, SVM)</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);text-align: center;border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>32</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);text-align: center;border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>500</p>
        </td>
      </tr>
    </tbody>
  </table>
  <p>Минимальная конфигурация рассчитана из следующего количества сканирующих агентов:</p>
  <ul class="Disc">
    <li>Два сканирующих агента Android.</li>
    <li>Два сканирующих агента iOS.</li>
  </ul>
  <p>При увеличении количества сканирующих агентов необходимо пересмотреть конфигурацию оборудования из расчета 1 ядро и 4 Гб ОЗУ на каждого дополнительного сканирующего агента при их параллельной работе. Также можно исходить из таблицы ниже.</p>
  <table class="thickhdrevenrows" style="width: 100%;font-size: 0.9rem">
    <colgroup>
      <col style="width: 667px;" />
      <col style="width: 328px;" />
      <col style="width: 191px;" />
    </colgroup>
    <tbody>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>Количество сканирующих агентов</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>Ядер процессора</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>RAM, Гб</strong></p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>2 Android, 2 iOS</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
          <p>4</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
          <p>16</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>4 Android, 4 iOS<b></b></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
          <p>8</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
          <p>24</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>6 Android, 6 iOS<span></span></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
          <p>12</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
          <p>32</p>
        </td>
      </tr>
    </tbody>
  </table>
  <h2>Архитектура и ОС</h2>
  <p>Система поддерживает следующие типы ОС и ПО для полноценного функционирования.</p>
  <table class="thickhdrevenrows" style="width: 100%;">
    <tbody>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p><strong>Операционная система</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p><strong>Архитектура</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p><strong>Платформа</strong></p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Linux</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
          <p>64-bit</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Ubuntu Server 18.04.6 x64</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Linux<b></b></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
          <p>64-bit</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Centos / RHEL 7 and higher</p>
        </td>
      </tr>
    </tbody>
  </table>
  <h2>Сетевой доступ</h2>
  <p>Установленные для эксплуатации Stingray технические средства должны быть совместимы между собой и поддерживать сетевой протокол TCP/IP. Для первоначальной настройки и установки платформы Stingray и сопутствующих пакетов желателен доступ к следующим ресурсам:</p>
  <table class="thickhdrevenrows" style="width: 100%;text-align: left">
    <colgroup>
      <col style="width: 42px;" />
      <col style="width: 338px;" />
      <col style="width: 318px;" />
      <col />
      <col />
      <col />
    </colgroup>
    <thead>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>№<span></span></strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>Адрес источника<span></span></strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><strong>Адрес приемника<span></span></strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p><strong>Тип<br />
              подключения<span></span></strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p><strong>Порты</strong></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p><strong>Назначение<span></span></strong></p>
        </td>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
          <p>1</p>
        </td>
        <td style="text-align: left;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Сетевой адрес Stingray</p>
        </td>
        <td style="text-align: left;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><a href="https://download.docker.com"><span>https://download.docker.com</span></a></p>
        </td>
        <td style="text-align: center;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Внешний</p>
        </td>
        <td style="text-align: center;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>80, 443</p>
        </td>
        <td style="text-align: left;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p> Установка docker</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
          <p>2</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Сетевой адрес Stingray</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><a href="http://archive.ubuntu.com">http://archive.ubuntu.com</a></p>
          <p><a href="">http://security.ubuntu.com</a></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
          <p>Внешний</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
          <p>80, 443</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Установка сопутствующих пакетов</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
          <p>3</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Сетевой адрес Stingray</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><a href="https://eu.gcr.io/">https://eu.gcr.io/</a></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
          <p>Внешний</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
          <p>80, 443</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Авторизация в хранилище docker и загрузка docker-образов</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
          <p>4</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Сетевой адрес Stingray</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p><a href="">https://storage.googleapis.com</a></p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
          <p>Внешний</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
          <p>80, 443</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Авторизация в хранилище docker и загрузка docker-образов</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
          <p>5</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>CI/CD система, в которой осуществляется процесс сборки приложения</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Сетевой адрес Stingray</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
          <p>Внутренний</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
          <p>80, 443</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Загрузка артефакта сборки (мобильного приложения) для анализа в Stingray</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
          <p>6</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Сетевой адрес системы Stingray</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Backend мобильного приложения</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
          <p>Внутренний</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
          <p>80, 443</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Сетевая доступность backend для корректной работы мобильного приложения</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
          <p>7</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Рабочее место пользователя Stingray</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Сетевой адрес Stingray</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
          <p>Внутренний</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
          <p>80, 443</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Работа пользователей с графическим интерфейсом системы</p>
        </td>
      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
          <p>8</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Рабочее место администратора Stingray</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
          <p>Сетевой адрес Stingray</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
          <p>Внутренний</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
          <p>80, 443, 22</p>
        </td>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
          <p>Администрирование системы Stingray</p>
        </td>
      </tr>
    </tbody>
  </table>
  <p> </p>
</body>
</html>