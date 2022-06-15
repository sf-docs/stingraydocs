# Интеграция с Google Play

Интеграция с Google Play осуществляется с помощью скрипта [mdast_scan.py](https://github.com/Dynamic-Mobile-Security/mdast-cli).

## Сбор необходимых параметров

!!! note "Примечание"
    Поскольку передача учетных данных осуществляется в незащищенном виде, для интеграции с Google Play следует использовать специально выделенный для этих целей сервисный Google-аккаунт. Двухфакторная аутентификация для аккаунта должна быть отключена. 

При предварительном запуске скрипта обязательна передача следующих параметров:

* `google_play_package_name` — имя скачиваемого пакета. Чтобы узнать имя пакета приложения, можно открыть его страницу в Google Play — имя пакета является частью URL (параметр `id`);

    <figure markdown>
    ![](../ag/img/32.png)
    </figure>

* `google_play_email` — электронная почта аккаунта Google;
* `google_play_password` — пароль аккаунта Google;
* `distribution_system` — для Google Play указываем значение `google_play`.

!!! note "Примечание"
    Если на этом этапе необходимо скачать приложение, добавьте параметр `--google_play_download_with_creds`.

Результатом будут следующие сообщения:

    20/04/2022 15:03:36 - INFO Google Play - Google Play integration, trying to login
    20/04/2022 15:03:36 - INFO Google Play - Logging in with email and password, you should copy token after
    20/04/2022 15:03:38 - INFO Google Play - gsfId: 36**********2297818, authSubToken: Jg**********RjqFD_pGqcTFjodc_mBULuauit8o1uB4-AKFaFKHr6wb9serwzgwLBIRvA.
    20/04/2022 15:03:38 - INFO Google Play - You should copy these parameters and use them for next scans instead of email and password:
    20/04/2022 15:03:38 - INFO Google Play - "--google_play_gsfid 36**********2297818 --google_play_auth_token Jgju_**********FD_pGqcTFjodc_mBULuauit8o1uB4-AKFaFKHr6wb9serwzgwLBIRvA."

В последней строке указаны два параметра `--google_play_gsfid` и `--google_play_auth_token`, которые необходимо скопировать. В дальнейшем они будут использоваться для скачивания приложения и запуска сканирования. Использование этих параметров вместо электронной почты и пароля позволит избежать дальнейших проверок безопасности в браузере.

## Пример запуска скрипта

После получения необходимых параметров можно, запустив скрипт, скачать приложение и запустить его ручное сканирование.

    python mdast_cli/mdast_scan.py \
      --profile_id 1337 \
      --architecture_id 1 \
      --distribution_system google_play \
      --url "https://saas.mobile.appsec.world" \
      --company_id 1 \
      --token 5d5f6c98*********487a68ee20d4562d9f \
      --google_play_package_name com.instagram.android \
      --google_play_gsfid 432******************43 \
      --google_play_auth_token JAgw_2h*************************************8KRaYQ.
      --google_play_file_name best_apk_d0wnl04d3r

В результате приложение будет скачано в папку `downloaded_apps` под именем `best_apk_d0wnl04d3r.apk`, а также запустится ручное сканирование.

!!! note "Примечание"
    Более подробная информация о параметрах скрипта приведена в разделе «[Системы CI/CD](./sistemy_ci_cd.md)».