# Хранение чувствительной информации в Binary Cookies

<table class='noborder'>
    <colgroup>
      <col/>
      <col/>
    </colgroup>
    <tbody>
      <tr>
        <td rowspan="2"><img src="../../../img/defekt_info.png"/></td>
        <td>Критичность:<strong> ИНФО</strong></td>
      </tr>
      <tr>
        <td>Способ обнаружения:<strong> DAST, ФАЙЛЫ ПРИЛОЖЕНИЯ</strong></td>
      </tr>
    </tbody>
</table>

## Описание

Приложение не отключает хранение значений Cookies при работе с **WebView**.

При использовании **WebView** (это касается как UIWebView, так и WKWebView) необходимо помнить, что значения Cookies, полученные при посещении сайта, сохраняются локально в файле специального формата `Cookies.binarycookies`. При этом в данных значениях могут быть и чувствительные данные, к примеру, сессионные идентификаторы или токены для доступа к сервису.

## Рекомендации

При правильном использовании необходимо ограничивать хранение Cookie временем, в которое они используются. Обычно при закрытии **WebView** эти данные больше не нужны. А лучше всего отключить использование Cookie вообще.

Как пример, можно отключить хранение:

    import UIKit
    import WebKit
    class ViewController: UIViewController, WKUIDelegate {
        var webView: WKWebView!
        override func loadView()
            let webConfiguration = WKWebViewConfiguration()
            webView = WKWebView(frame: .zero, configuration: webConfiguration)
            webView.uiDelegate = self
            view = webView
        }
        override func viewDidLoad() {
            super.viewDidLoad()
            let myURL = URL(string: "http://example.com")
            var myRequest = URLRequest(url: myURL!)
            myRequest.httpShouldHandleCookies = false
            webView.load(myRequest)
        }
    }

Для удаления значений Cookie (например, при логауте из приложения или его закрытии/открытии):

    let storage = HTTPCookieStorage.shared
            if let cookies = storage.cookies{
                for cookie in cookies {
                    storage.deleteCookie(cookie)
                }
        }

## Ссылки

1. [https://book.hacktricks.xyz/mobile-apps-pentesting/ios-pentesting#cookies](https://book.hacktricks.xyz/mobile-apps-pentesting/ios-pentesting#cookies)
2. [https://developer.apple.com/forums/thread/81874](https://developer.apple.com/forums/thread/81874)