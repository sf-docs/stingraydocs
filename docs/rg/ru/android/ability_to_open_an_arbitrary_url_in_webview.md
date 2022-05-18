# Возможность открытия произвольного URL в WebView

<table class='noborder'>
    <colgroup>
      <col/>
      <col/>
    </colgroup>
    <tbody>
      <tr>
        <td rowspan="2"><img src="../../../img/defekt_srednij.png"/></td>
        <td>Критичность:<strong> СРЕДНИЙ</strong></td>
      </tr>
      <tr>
        <td>Способ обнаружения:<strong> IAST</strong></td>
      </tr>
    </tbody>
</table>

## Описание

Уязвимость позволяет открывать произвольные URL в **WebView** приложения. В зависимости от настроек **WebView** эта уязвимость может использоваться в различных векторах атак.

Уязвимость присутствует в приложениях, которые используют данные из недоверенного источника  для формирования URL, используемого впоследствии при вызове метода `WebView.loadUrl`.

Например, уязвимое приложение может использовать такой код:

    WebView webView = findViewById(R.id.webview);
    setupWebView(webView);
    webView.loadUrl(getIntent().getStringExtra("url"));

Или такой:

    String url = uri.getQueryParameter("url");
    if(url != null) {
    webView.loadUrl(url);
    }

## Рекомендации

1. Не использовать динамически формируемые URL для **WebView**.

        webView.loadUrl("https://url.to.your.contents/");

2. Проводить валидацию URL:

    1. Разрешать доступ только к ресурсам компании, т. е. поддерживать white-list URL-адресов и сверять с ним URL, передаваемый в метод `loadUrl`.

    2. Разрешать доступ только к определённым origin, т. е. проверять схему и домен URL.

            List<String> whiteHosts = Arrays.asList("stackoverflow.com",  "stackexchange.com", "google.com");
            public boolean isValid(String url) {
            String host = Uri.parse(url).getHost();
            if(whiteHosts.contains(host) {
                return true;
            }
            return false;
            }

3. Отключить потенциально опасные настройки доступа к ресурсам приложения из **WebView**:

`WebSettings.setAllowContentAccess(false)`<br>

`WebSettings.setAllowFileAccess(false)`<br>

`WebSettings.setAllowFileAccessFromFileURLs(false)`<br>

`WebSettings.setAllowUniversalAccessFromFileURLs(false)`<br>

`WebSettings.setGeolocationEnabled(false)`<br>

## Ссылки

1. [Android security checklist: WebView](https://blog.oversecured.com/Android-security-checklist-webview/)

2. [UrlQuerySanitizer  |  Android Developers](https://developer.android.com/reference/android/net/UrlQuerySanitizer)