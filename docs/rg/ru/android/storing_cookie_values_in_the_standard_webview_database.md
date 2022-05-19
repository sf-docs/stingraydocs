# Хранение значений Cookies в стандартной базе WebView

<table class='noborder'>
    <colgroup>
      <col/>
      <col/>
    </colgroup>
    <tbody>
      <tr>
        <td rowspan="2"><img src="../../../img/defekt_vysokij.png"/></td>
        <td>Критичность:<strong> ВЫСОКИЙ</strong></td>
      </tr>
      <tr>
        <td>Способ обнаружения:<strong> DAST, SENSITIVE INFO</strong></td>
      </tr>
    </tbody>
</table>

## Описание

При использовании **WebView** со стандартными настройками (или неправильно сконфигурированного), все значения Cookie, которыми оперирует **WebView** будут сохранены в директорию приложения в специальной базе Cookie.db. При этом значения в данной базе никак не шифруются и доступны в открытом виде.

Несмотря на то, что данный файл находится во внутренней директории приложения, хранить сессионные идентификаторы и другие чувствительные данные, относящиеся к процессу аутентификации в открытом виде не рекомендуется. Получить данный файл можно несколькими способами, начиная от backup приложения, до эксплуатации других уязвимостей, позволяющих читать произвольные файлы в директории приложения.

## Рекомендации

Для предотвращения хранения значений Cookie необходимо правильно настраивать **WebView** при создании. В случае, если до этого значения были сохранены их необходимо удалить.

Необходимо иметь ввиду, что на разных версиях Android SDK эта операция совершается по-разному. Ниже представлен пример кода, который реализует корректное очищение Cookie для всех версий Android: 

    webView.clearCache(true);
        webView.clearHistory();
        WebSettings webSettings = webView.getSettings();
        webSettings.setSaveFormData(false);
        
        // Not needed for API level 18 or greater (deprecated)
        webSettings.setSavePassword(false); 
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP_MR1) {
            CookieManager.getInstance().removeAllCookies(null);
            CookieManager.getInstance().flush();
        } else {
            CookieSyncManager cookieSyncMngr = CookieSyncManager.createInstance(this);
            cookieSyncMngr.startSync();
            CookieManager cookieManager = CookieManager.getInstance();
            cookieManager.removeAllCookie();
            cookieManager.removeSessionCookie();
            cookieSyncMngr.stopSync();
            cookieSyncMngr.sync();
        }

## Ссылки

1. [https://developer.android.com/reference/android/webkit/CookieManager#removeAllCookies(android.webkit.ValueCallback<java.lang.Boolean>)](https://developer.android.com/reference/android/webkit/CookieManager#removeAllCookies(android.webkit.ValueCallback<java.lang.Boolean>))

2. [https://www.owasp.org/index.php/Mobile_Top_10_2016-M2-Insecure_Data_Storage](https://www.owasp.org/index.php/Mobile_Top_10_2016-M2-Insecure_Data_Storage)