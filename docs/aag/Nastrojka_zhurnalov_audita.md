# Настройка журналов аудита

  <h2><a name="_Toc80613864">Журналы аудита</a></h2>
  <p>Журналы аудита располагаются в директории, в которой расположены конфигурационные файлы, созданные после запуска Wizard (см. раздел <a href="Ustanovka_Stingray.htm" title="Установка Stingray">Установка Stingray</a> Руководства по установке, настройке и интеграции), в общем случае (по умолчанию):</p>
  <pre class="language-java"><code class="language-java"><span style="font-size:0.8rem;">&lt;path_to_config_directory&gt;/logs/audit.log</span></code></pre>
  <div>
    <h2><a name="_Toc80613865">Управление журналами аудита</a></h2>
  </div>
  <p>Управление журналами аудита производится во время первоначальной установки системы и включает в себя следующие пункты:</p>
  <ul class="Disc">
    <li>Настройка включения аудита.</li>
    <li>Настройка количества записей в одном файле лога.</li>
    <li>Настройка количества хранимых файлов лога.</li>
  </ul>
  <h3><a name="_Toc80613866">События аудита</a></h3>
  <p>События аудита логируются при каждом изменении базы данных в случае любого изменения:</p>
  <pre class="language-java"><code class="language-java"><span style="font-size:0.8rem">2021-02-07 21:54:38.364420 Process ID: 39 Event ID: 7, Event Name: Update Record,
Host: http://localhost:4200, User: admin, Args: {&#39;table&#39;: &#39;User&#39;, &#39;id&#39;: 1,
&#39;fields&#39;: {&#39;language&#39;: {&#39;before&#39;: &#39;en&#39;, &#39;after&#39;: &#39;ru&#39;}}}</span></code></pre>
  <p>Параметры, представленные в каждой записи:</p>
  <ul>
    <li>Время события — время произошедшего события, в формате YYYY-MM-DD h:m:s (год-месяц-день часы:минуты:секунды).</li>
    <li>Идентификатор процесса — идентификатор процесса, от которого произошло событие, внутри системы.</li>
    <li>Идентификатор события — цифровой идентификатор события.</li>
    <li>Имя события — описание события в человеко-читаемом формате.</li>
    <li>Хост — имя хоста, от которого пришел запрос.</li>
    <li>Имя пользователя — пользователь, от имени которого пришел запрос.</li>
    <li>Аргументы запроса — переданные аргументы запроса, определяющие состояние до и после обновления/изменения/удаления и несущие другой информативный характер.</li>
  </ul>
  <p>Перечень всех событий и их описание:</p>
  <table class="thickhdrevenrows" style="width: 100%;">
    <colgroup>
      <col style="width: 45px;" />
      <col style="width: 309px;" />
      <col style="width: 835px;" />
    </colgroup>
    <thead>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p><strong>ID</strong></p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p><strong>Название<span></span></strong></p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p><strong>Описание<span></span></strong></p>
        </td>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>1</p>
        </td>
        <td style="text-align: left;font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Login Success</p>
        </td>
        <td style="text-align: left;font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Событие успешного входа в систему</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>2</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Login Fail</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Неуспешная попытка входа в систему</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>3</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Logout</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Событие выхода из системы</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>4</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Bad Request</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Событие, определяющее неправильное обращение к системе. В информации события указывается код ошибки, которую вернул сервер. Включает в себя события попытки доступа неавторизованного пользователя к ресурсам (код ошибки 401), попытки доступа к ресурсам, для которых у пользователя нет прав (код ошибки 403), а также неверно сформированные запросы (код ошибки 400) и т. д.</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>5</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Server Error</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Событие об ошибке сервера с информацией о причине возникновения ошибки</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>6</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>New Record</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Создание нового элемента в системе. Список возможных элементов, к которым применяется аудит, указан в таблице ниже</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>7</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Update Record</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Изменение элемента в системе. Логируется старое и новое значение элемента. Список возможных элементов, к которым применяется аудит, указан в таблице ниже</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>8</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Delete Record</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Удаление элемента в системе. Список возможных элементов, к которым применяется аудит, указан в таблице ниже</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>9</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>New Related Record</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Событие создания новой связанной сущности. В системе присутствует иерархия сущностей и при явном добавлении одного элемента может создаваться несколько дочерних.<br />
            Для разделения событий создания основных и вложенных сущностей используются различные типы событий</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>10</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Delete Related Record</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Событие удаления связанной сущности. В системе присутствует иерархия сущностей и при явном удалении одного элемента может удаляться несколько дочерних. Для разделения событий удаления основных и вложенных сущностей используются различные типы событий</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>11</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Change Password</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Событие смены пароля пользователей</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>12</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Dast Action</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Событие совершения операций со сканированиями (запуск, остановка, завершение сканирования)</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>13</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Testcase Action</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Событие совершения операций с автотестами (запуск, остановка, завершение записи автотеста)</p>
        </td>
      </tr>
    </tbody>
  </table>
  <p>Список элементов, к которым применяются события создания/удаления/модификации:</p>
  <table class="thickhdrevenrows" style="width: 100%;text-align: left">
    <colgroup>
      <col style="width: 41px;" />
      <col style="width: 314px;" />
      <col style="width: 834px;" />
    </colgroup>
    <thead>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p><strong>№</strong></p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p><strong>Модель в БД</strong></p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.30);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p><strong>Описание</strong></p>
        </td>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="text-align: center;font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>1</p>
        </td>
        <td style="text-align: left;font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>stingray_user</p>
        </td>
        <td style="text-align: left;font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Пользователи системы</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>2</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>stingray_project</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Проекты</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>3</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>stingray_profile</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Профили</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>4</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>stingray_dast</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Сканирования</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>5</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>stingray_dastIssue</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Уязвимости, выявленные в ходе сканирования</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>
            <w:sdt docparttype="Watermarks" id="1331103090" sdtdocpart="t"></w:sdt>6
          </p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>stingray_DastResult</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Собранные в результате проведения сканирования данные</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>7</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>stingray_ProjectIssue</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Сущность, связывающая уязвимости между сканированиями</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>8</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>stingray_TestCase</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Автотесты, записываемые внутри системы</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>9</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>stingray_Rule</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Правила поиска уязвимостей</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>10</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>stingray_RuleModule</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Связка правил поиска уязвимостей с модулями для поиска</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>11</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>stingray_RuleExpression</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Связка правил поиска уязвимостей со строками поиска</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>12</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>stingray_Expression</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Связка правил поиска уязвимостей с возможными местами поиска</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>13</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>stingray_Injection</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Строки поиска для определения чувствительной информации</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>14</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>stingray_Settings</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Настройки системы</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>15</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>stingray_RequirementGroup</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Стандарты и категории внутри стандартов</p>
        </td>
      </tr>
      <tr>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255);text-align: center">
          <p>16</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>stingray_Requirement</p>
        </td>
        <td style="font-size: 0.9rem;background-color: rgba(0, 105, 230, 0.20);border-width: 3px;border-color: rgb(255, 255, 255)">
          <p>Требования внутри стандартов</p>
        </td>
      </tr>
    </tbody>
  </table>
  <p> </p>
</body>
</html>