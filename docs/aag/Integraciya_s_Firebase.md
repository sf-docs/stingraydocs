# Интеграция с Firebase

  <h3>Сбор необходимых параметров</h3>
  <p>Система Stingray предлагает возможность интеграции с популярным набором сервисов <a href="https://firebase.google.com/">Firebase</a> от компании Google, который используется на всех этапах жизненного цикла разработки программного обеспечения для мобильных устройств.</p>
  <p><span><span>Интеграция с Firebase производится с помощью скрипта <a href="https://github.com/Dynamic-Mobile-Security/mdast-cli"><em><strong>mdast_scan.py</strong></em></a>. <span><span>Для запуска скрипта в режиме интеграции с Firebase необходимо указать параметр </span><code data-renderer-mark="true">--distribution_system firebase</code><span>.</span></span></span></span></p>
  <p><span>Чтобы скачать приложение с Firebase и просканировать его, необходимо собрать информацию о значениях cookies для Google SSO аутентификации, а также о значениях параметров проекта в Firebase. </span></p>
  <table style="width: 100%;border-width: 1px;border-style: none;border-color: #000000;border-left-width: 1px;border-left-style: none;border-left-color: #000000;border-top-width: 1px;border-top-style: none;border-top-color: #000000;border-right-width: 1px;border-right-style: none;border-right-color: #000000;border-bottom-width: 1px;border-bottom-style: none;border-bottom-color: #000000">
    <colgroup>
      <col style="width: 100px;" />
      <col />
    </colgroup>
    <tbody>
      <tr style="height: 100px;">
        <td style="text-align: center;border-width: 3px;border-style: solid;border-color: transparent;border-left-width: 3px;border-left-style: solid;border-left-color: transparent;border-top-width: 3px;border-top-style: solid;border-top-color: transparent;border-right-width: 3px;border-right-style: solid;border-right-color: transparent;border-bottom-width: 3px;border-bottom-style: solid;border-bottom-color: transparent;background-color: #006699"><span style="font-size:2rem;"><span style="font-weight:bold;"><span style="color:#ffffff;font-size:2rem">!</span></span></span></td>
        <td style="border-width: 3px;border-style: solid;border-color: transparent;border-left-width: 3px;border-left-style: solid;border-left-color: transparent;border-top-width: 3px;border-top-style: solid;border-top-color: transparent;border-right-width: 3px;border-right-style: solid;border-right-color: transparent;border-bottom-width: 3px;border-bottom-style: solid;border-bottom-color: transparent;background-color: rgba(0, 102, 153, 0.20)"><strong style="color: #006699"><span>Не используйте личные учетные записи, так как параметры, используемые в процессе аутентификации в Firebase, привязаны к вашему аккаунту. Мы рекомендуем создать специальную техническую учетную запись для интеграции.</span></strong></td>
      </tr>
    </tbody>
  </table>
  <p>В браузере авторизуйтесь в Firebase с использованием Google SSO и соберите информацию о неоходимых cookies. <span>Например, при использовании браузера Google Chrome или Microsoft Edge откройте DevTools, нажав F12 или использовав специальную комбинацию клавиш, которая применяется для этого в вашем браузере</span>, <span>перейдите на вкладку <strong>Application</strong>, а затем, выбрав в меню слева пункт <strong>Cookies</strong>,</span> скопируйте значения следующих cookies: <strong>SID</strong>, <strong>SSID</strong>, <strong>APISID</strong>, <strong>SAPISID</strong> и <strong>HSID</strong>. Данная информация впоследствии будет использоваться в качестве параметров запуска скрипта <em><strong>mdast_scan.py</strong></em>.</p>
  <p style="text-align: center"><img height="1134" src="https://user-images.githubusercontent.com/46852358/149788352-d453dd78-547f-4989-8132-b94a6f020a81.png" width="95%" /></p>
  <p>Далее необходимо получить следующие параметры проекта: <strong>project_id</strong>, <strong>app_id</strong>, <strong>app_code</strong> и <strong>api_key</strong>. Для этого перейдите на страницу проекта, адрес которой имеет следующий вид:</p>
  <p><code>https://console.firebase.google.com/u/0/project/<strong>{project_id}</strong>/overview</code></p>
  <p> </p>
  <p style="text-align: center"><img height="1530" src="https://user-images.githubusercontent.com/46852358/149789837-2787cb52-355d-4ef0-9440-89053764db78.png" width="95%" /></p>
  <p>Слева в меню в разделе <strong>Release &amp; Monitor</strong> выберите пункт <strong>App Distribution</strong>. Предварительно открыв DevTools (F12), выберите необходимый релиз из списка и нажмите кнопку <strong>Download</strong>.</p>
  <p style="text-align: center"><img height="1530" src="https://user-images.githubusercontent.com/46852358/149791304-2658f1be-9ee0-422e-94ce-59f1ba1858df.png" style="cursor: nesw-resize;" width="95%" /></p>
  <p><span>В окне DevTools на вкладке <strong>Network</strong> отследите запрос, содержащий URL следующего вида:</span></p>
  <p><code>https://firebaseappdistribution-pa.clients6.google.com/v1/projects/<strong>{project_id}</strong>/apps/<strong>{app_id}</strong>/releases/<strong>{app_code}</strong>:getLatestBinary?alt=json&amp;key=<strong>{api_key}</strong> </code></p>
  <p> </p>
  <p style="text-align: center"><img height="770" src="https://user-images.githubusercontent.com/46852358/149792212-512d33ab-2323-45b6-a25c-6a8d817cde1f.png" width="95%" /></p>
  <p style="text-align: left">Из данного URL получаем остальные необходимые параметры.</p>
  <p>В результате все необходимые параметры для запуска скрипта <em><strong>mdast_scan.py </strong></em>собраны. <span>В зависимости от того, какое приложение скачивается, необходимо с помощью параметра <code data-renderer-mark="true">firebase_app_extension</code> указать расширение файла: для Android приложения — </span><code data-renderer-mark="true">apk</code><span> , а для iOS — </span><code data-renderer-mark="true">ipa</code><span> .</span></p>
  <p>Приведем полный список собранных параметров:</p>
  <ul class="Disc">
    <li><strong><code>firebase_SID_cookie</code></strong> — SID;</li>
    <li><strong><code>firebase_HSID_cookie</code></strong> — HSID;</li>
    <li><strong><code>firebase_SSID_cookie</code></strong> — SSID;</li>
    <li><strong><code>firebase_APISID_cookie</code></strong> — APISID;</li>
    <li><strong><code>firebase_SAPISID_cookie</code></strong> — SAPISID;</li>
    <li><strong><code>firebase_project_id</code></strong> — {project id};</li>
    <li><strong><code>firebase_app_id</code></strong> — {application id};</li>
    <li><strong><code>firebase_app_code</code></strong> — {application code};</li>
    <li><strong><code>firebase_api_key</code></strong> — {api key}.</li>
    <li><strong><code>firebase_app_extension</code></strong> — расширение файла: apk для Android или ipa для iOS.</li>
  </ul>
  <p><span>Также возможно задать название скачиваемого файла, для этого надо задать опциональный параметр  </span><code data-renderer-mark="true">firebase_file_name</code>.</p>
  <h3>Пример запуска скрипта</h3>
  <p>Чтобы запустить сканирование приложения, скачанного с Firebase, необходимо выполнить следующую команду:</p>
  <pre class="language-bash"><code class="language-bash">python mdast_cli/mdast_scan.py \
    --profile_id 468 \
    --architecture_id 2 \
    --distribution_system firebase \
    --firebase_project_id 2834204**** \
    --firebase_app_id 1:283***3642:android:8b0a0***56ac40c1a43 \
    --firebase_app_code 2b***sltr0 \
    --firebase_api_key AIzaSyDov*****qKdbj-geRWyzMTrg \
    --firebase_SID_cookie FgiA*****ZiQakQ-_C-5ZaEHvbDMFGkrgriAByQ9P9fv7LfRrYJ5suXgrCwIQBoOjA.  \
    --firebase_HSID_cookie AsiL****OjPI \
    --firebase_SSID_cookie A****dwcZk1Z-1pE \
    --firebase_APISID_cookie Z-FmS1aPB****djK/AjmG0h2Hc-GG9g2Ac \
    --firebase_SAPISID_cookie XYR2tnf****0zOt/AEvVZ8JVEuCnE6pxm \
    --url &quot;https://saas.mobile.appsec.world&quot; \
    --company_id 1 \ 
    --token 2fac9652a2fbe4****9f44af59c3381772f \
    --firebase_file_name your_app_file_name  \
    --firebase_file_extension apk</code>
</pre>
</body>
</html>