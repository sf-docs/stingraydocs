# Требования к инфраструктуре

## Требования к аппаратному обеспечению

Серверные компоненты Stingray должны быть установлены на выделенном сервере, предназначенном исключительно для эксплуатации серверных компонент Stingray.

Для работы системы обязательно наличие поддержки виртуализации, а именно необходим процессор с поддержкой технологии виртуализации Intel Virtualization Technology (VT, VT-x, vmx) или AMD Virtualization (AMD-V, SVM).

### Минимальные технические характеристики серверного оборудования

* 6-ядерный CPU 2 ГГц; 
* Оперативная память: 24 Гб. 
* Свободное дисковое пространство 100 GB (+ пространство для размещения прикладных систем и баз данных).

### Рекомендуемые технические характеристики серверного оборудования

* 8-ядерный CPU 3 ГГц. 
* Оперативная память: 32 Гб. 
* Свободное дисковое пространство 500 GB (+ пространство для размещения прикладных систем и баз данных).

Для развертывания платформы Stingray на базе клиентской инфраструктуры требуется следующая минимальная аппаратная или виртуальная конфигурация оборудования:
  
Наименование|Кол-во|CPU|RAM, Гб|HDD, Гб
-|:-:|-|:-:|:-:
Stingray (минимальная конфигурация)<b></b>

        <td style="background-color: rgba(0, 105, 230, 0.20);text-align: center;border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        1

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        6-ядерный процессор (2 ГГц) с поддержкой технологии виртуализации Intel Virtualization Technology (VT, VT-x, vmx) или AMD Virtualization (AMD-V, SVM)

        <td style="background-color: rgba(0, 105, 230, 0.20);text-align: center;border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        24

        <td style="background-color: rgba(0, 105, 230, 0.20);text-align: center;border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        200

      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        Stingray (рекомендуемая конфигурация)<b></b>

        <td style="background-color: rgba(0, 105, 230, 0.20);text-align: center;border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        1

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        8-ядерный процессор (3 ГГц) с поддержкой технологии виртуализации Intel Virtualization Technology (VT, VT-x, vmx) или AMD Virtualization (AMD-V, SVM)

        <td style="background-color: rgba(0, 105, 230, 0.20);text-align: center;border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        32

        <td style="background-color: rgba(0, 105, 230, 0.20);text-align: center;border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        500

      </tr>
    </tbody>
  </table>
Минимальная конфигурация рассчитана из следующего количества сканирующих агентов:

* Два сканирующих агента Android.
* Два сканирующих агента iOS.

При увеличении количества сканирующих агентов необходимо пересмотреть конфигурацию оборудования из расчета 1 ядро и 4 Гб ОЗУ на каждого дополнительного сканирующего агента при их параллельной работе. Также можно исходить из таблицы ниже.
  <table
    <colgroup>
      <col style="width: 667px;" />
      <col style="width: 328px;" />
      <col style="width: 191px;" />
    </colgroup>
    <tbody>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        Количество сканирующих агентов

        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        Ядер процессора

        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        RAM, Гб

      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        2 Android, 2 iOS

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
        6

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
        24

      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        4 Android, 4 iOS<b></b>

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
        8

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
        24

      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        6 Android, 6 iOS

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
        12

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
        32

      </tr>
    </tbody>
  </table>
## Архитектура и ОС
Система поддерживает следующие типы ОС и ПО для полноценного функционирования.
  <table class="thickhdrevenrows" style="width: 100%;">
    <tbody>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
        Операционная система

        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
        Архитектура

        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
        Платформа

      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
        Linux

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
        64-bit

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
        Ubuntu Server 18.04.6 x64

      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
        Linux<b></b>

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
        64-bit

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
        Centos / RHEL 7 and higher

      </tr>
    </tbody>
  </table>
## Сетевой доступ
Установленные для эксплуатации Stingray технические средства должны быть совместимы между собой и поддерживать сетевой протокол TCP/IP. Для первоначальной настройки и установки платформы Stingray и сопутствующих пакетов желателен доступ к следующим ресурсам:
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
        №

        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        Адрес источника

        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        Адрес приемника

        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
        Тип<br />
              подключения

        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
        Порты

        <td style="background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
        Назначение

      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
        1

        <td style="text-align: left;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        Сетевой адрес Stingray

        <td style="text-align: left;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        <a href="https://download.docker.com">https://download.docker.com</a>

        <td style="text-align: center;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
        Внешний

        <td style="text-align: center;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
        80, 443

        <td style="text-align: left;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
         Установка docker

      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
        2

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        Сетевой адрес Stingray

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        <a href="http://archive.ubuntu.com">http://archive.ubuntu.com</a>
        <a href="">http://security.ubuntu.com</a>

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
        Внешний

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
        80, 443

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
        Установка сопутствующих пакетов

      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
        3

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        Сетевой адрес Stingray

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        <a href="https://eu.gcr.io/">https://eu.gcr.io/</a>

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
        Внешний

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
        80, 443

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
        Авторизация в хранилище docker и загрузка docker-образов

      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
        4

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        Сетевой адрес Stingray

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        <a href="">https://storage.googleapis.com</a>

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
        Внешний

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
        80, 443

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
        Авторизация в хранилище docker и загрузка docker-образов

      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
        5

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        CI/CD система, в которой осуществляется процесс сборки приложения

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        Сетевой адрес Stingray

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
        Внутренний

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
        80, 443

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
        Загрузка артефакта сборки (мобильного приложения) для анализа в Stingray

      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
        6

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        Сетевой адрес системы Stingray

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        Backend мобильного приложения

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
        Внутренний

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
        80, 443

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
        Сетевая доступность backend для корректной работы мобильного приложения

      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
        7

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        Рабочее место пользователя Stingray

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        Сетевой адрес Stingray

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
        Внутренний

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
        80, 443

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
        Работа пользователей с графическим интерфейсом системы

      </tr>
      <tr>
        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center;font-size: 0.9rem">
        8

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        Рабочее место администратора Stingray

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);font-size: 0.9rem">
        Сетевой адрес Stingray

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
        Внутренний

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;text-align: center;font-size: 0.9rem">
        80, 443, 22

        <td style="background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: #FFFFFF;border-left-width: 3px;border-left-color: #FFFFFF;border-top-width: 3px;border-top-color: #FFFFFF;border-right-width: 3px;border-right-color: #FFFFFF;border-bottom-width: 3px;border-bottom-color: #FFFFFF;font-size: 0.9rem">
        Администрирование системы Stingray

      </tr>
    </tbody>
  </table>
 
</body>
</html>