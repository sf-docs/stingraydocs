# Интеграция с Google Play

  <p>Интеграция с Google Play осуществляется с помощью скрипта <em><strong><a href="https://github.com/Dynamic-Mobile-Security/mdast-cli">mdast_scan.py</a></strong></em>.</p>
  <h3>Сбор необходимых параметров</h3>
  <p><strong>Примечание: </strong>Поскольку передача учетных данных осуществляется в незащищенном виде, для интеграции с Google Play следует использовать специально выделенный для этих целей <span>сервисный</span> Google-аккаунт. Двухфакторная аутентификация для аккаунта должна быть отключена. </p>
  <p>При предварительном запуске скрипта обязательна передача следующих параметров:</p>
  <ul class="Disc">
    <li><code>google_play_package_name</code> — имя скачиваемого пакета. Чтобы узнать имя пакета приложения, можно открыть его страницу в Google Play — имя пакета является частью URL (параметр <code>id</code>).</li>
  </ul>
  <p style="text-align: center"><img src="../assets/images/image97.png" /></p>
  <ul class="Disc">
    <li><code>google_play_email</code> — электронная почта аккаунта Google.</li>
    <li><code>google_play_password</code> — пароль аккаунта Google.</li>
    <li><code>distribution_system</code> — для Google Play указываем значение <code>google_play</code>.</li>
  </ul>
  <p>Таким образом, команда предварительного запуска скрипта может выглядеть следующим образом:</p>
  <pre class="language-bush"><code class="language-bush">python mdast_cli/mdast_scan.py \
        --profile_id 1337 \
        --architecture_id 1 \
        --distribution_system google_play \
        --url &quot;https://saas.mobile.appsec.world&quot; \
        --company_id 1 \
        --token 5d5f6c98*********487a68ee20d4562d9f \
        --google_play_package_name com.instagram.android \
        --google_play_email download*******ly@gmail.com \
        --google_play_password Password</code></pre>
  <p>Результатом предварительного запуска будут следующие сообщения: </p>
  <pre class="language-bush"><code class="language-bush">20/04/2022 15:00:30 - INFO Google Play - Google Play integration, trying to login
20/04/2022 15:00:30 - INFO Google Play - Logging in with email and password, you should copy token after
20/04/2022 15:00:30 - ERROR Google Play - Security check is needed, try to visit <strong>https://accounts.google.com/b/0/DisplayUnlockCaptcha</strong> to unlock.
20/04/2022 15:00:30 - ERROR Google Play - server says: &quot;NeedsBrowser&quot;</code></pre>
  <p>Необходимо перейти по ссылке и пройти проверку безопасности в браузере.</p>
  <p><strong>Примечание:</strong> Возможно, такая проверка не потребуется.</p>
  <p style="text-align: center"><img height="621" src="../assets/images/image99.png" width="95%" /></p>
  <p>Пройдя проверку безопасности, запустите скрипт с этими же параметрами повторно. </p>
  <p><strong>Примечание:</strong> Если на этом этапе необходимо скачать приложение, добавьте параметр <code>--google_play_download_with_creds</code>.</p>
  <p>Результатом будут следующие сообщения:</p>
  <pre class="language-bush"><code class="language-bush">20/04/2022 15:03:36 - INFO Google Play - Google Play integration, trying to login
20/04/2022 15:03:36 - INFO Google Play - Logging in with email and password, you should copy token after
20/04/2022 15:03:38 - INFO Google Play - gsfId: 3664438267732297818, authSubToken: Jgju_Toz4YVtRjqFD_pGqcTFjodc_mBULuauit8o1uB4-AKFaFKHr6wb9serwzgwLBIRvA.
20/04/2022 15:03:38 - INFO Google Play - You should copy these parameters and use them for next scans instead of email and password:
20/04/2022 15:03:38 - INFO Google Play - &quot;--google_play_gsfid 36**********2297818 --google_play_auth_token Jgju_**********FD_pGqcTFjodc_mBULuauit8o1uB4-AKFaFKHr6wb9serwzgwLBIRvA.&quot;</code></pre>
  <p>В последней строке указаны два параметра <code>--</code><code>google_play_gsfid</code> и <code>--google_play_auth_token</code>, которые необходимо скопировать. В дальнейшем они будут использоваться для скачивания приложения и запуска сканирования. Использование этих параметров вместо электронной почты и пароля позволит избежать дальнейших проверок безопасности в браузере.</p>
  <h3>Пример запуска скрипта</h3>
  <p>После получения необходимых параметров можно, запустив скрипт, скачать приложение и запустить его ручное сканирование.</p>
  <pre class="language-bush"><code class="language-bush">python mdast_cli/mdast_scan.py \
    --profile_id 1337 \
    --architecture_id 1 \
    --distribution_system google_play \
    --url &quot;https://saas.mobile.appsec.world&quot; \
    --company_id 1 \
    --token 5d5f6c98*********487a68ee20d4562d9f \
    --google_play_package_name com.instagram.android \
    --google_play_gsfid 432******************43 \
    --google_play_auth_token JAgw_2h*************************************8KRaYQ.
    --google_play_file_name best_apk_d0wnl04d3r</code></pre>
  <p>В результате приложение будет скачано в папку <code>downloaded_apps</code> под именем <code>best_apk_d0wnl04d3r.apk</code>, а также запустится ручное сканирование.</p>
  <p><strong>Примечание:</strong> Более подробная информация о параметрах скрипта приведена в разделе <a href="../aag/sistemy_ci_cd.htm">Системы CI/CD</a>.</p>
  <p> </p>
</body>
</html>