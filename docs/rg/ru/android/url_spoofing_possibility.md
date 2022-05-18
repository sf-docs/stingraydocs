# Возможность подмены URL

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

Уязвимость позволяет злоумышленнику контролировать URL в экземпляре класса `java.net.URL`. В зависимости от способов применения URL эта уязвимость может использоваться в различных векторах атак.

Уязвимость присутствует, если экземпляр класса `java.net.URL` создаётся на основе строки, полученной из недоверенного источника.

Например, уязвимое приложение может использовать такой код:


    if("https".equals(uri.getScheme()) && "vuln.app.pkg".equals(uri.getHost())) {
    String path = uri.getPath();
    if("/login".equals(path)) {
        String urlStr = uri.getQueryParameter("url");
        if(urlStr != null) {
        try {
            URL url = new URL(urlStr);
            /* .. */
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
        }
        finish();
    }
    }

## Рекомендации

1. Проводить валидацию URL:

    1. Разрешать доступ только к ресурсам компании, т. е. поддерживать «белый список» URL-адресов и сверять с ним создаваемый экземпляр класса URL.

    2. Разрешать доступ только к определённым origin, т. е проверять схему и домен URL.

2. Сформированный URL также нужно проверять перед непосредственным использованием.

        List<String> whiteHosts = Arrays.asList("stackoverflow.com",  "stackexchange.com", "google.com");
        public boolean isValid(String url) {
        String host = Uri.parse(url).getHost();
        if(whiteHosts.contains(host) {
            return true;
        }
        return false;
        }

## Ссылки

1. [Input Validation - OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html) 

2. [UrlQuerySanitizer  |  Android Developers](https://developer.android.com/reference/android/net/UrlQuerySanitizer) 