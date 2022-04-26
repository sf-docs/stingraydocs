# Включенное кэширование сетевых запросов

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

При использовании стандартной библиотеки для осуществления сетевого взаимодействия в iOS все сетевые запросы кэшируются в файловой системе устройства. Эти файлы кэша могут обладать интересными сведениями, включая запросы на аутентификацию, содержащие все учетные данные пользователя.

Несмотря на то, что данные файлы находятся во внутренней директории приложения, хранить сессионные идентификаторы и другие чувствительные данные, относящиеся к процессу аутентификации, в открытом виде не рекомендуется. 

## Рекомендации

Для отключения кэширования сетевых запросов необходимо воспользоваться одним из предложенных способов в зависимости от реализации:

1. Удалить общий кэш в любое время (например, при запуске приложения):

        URLCache.shared.removeAllCachedResponses()

2. Отключить кэш на глобальном уровне:

        let theURLCache = URLCache(memoryCapacity: 0, diskCapacity: 0, diskPath: nil)
        URLCache.shared = theURLCache

3. Если используется объект NSURLConnection с делегатом, можно отключить кэш с помощью следующего метода:

        func connection(_ connection: NSURLConnection, willCacheResponse cachedResponse: CachedURLResponse) -> CachedURLResponse?
            {
                return nil
            }

4. Создать URL-запрос, который не будет использовать кэш:

        var request = NSMutableURLRequest(url: theUrl, cachePolicy: .reloadIgnoringLocalCacheData, timeoutInterval: urlTimeoutTime)

5. Также объект NSURLRequest имеет атрибут cachePolicy, который определяет работу с кэшем:

    1. UseProtocolCachePolicy — значение по умолчанию, кэширование зависит от HTTP заголовков.
    2. ReloadIgnoringLocalCacheData  — кэш не используется.

6. Ну и один из самых простых вариантов — при открытии/закрытии приложения просто очищать эту базу сетевых запросов или удалять файл.

## Ссылки

1. [https://codewithchris.com/preventing-nsurlconnection-cache-issues/](https://codewithchris.com/preventing-nsurlconnection-cache-issues/)
2. [https://developer.apple.com/forums/thread/45794](https://developer.apple.com/forums/thread/45794)
3. [https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/URLLoadingSystem/Concepts/CachePolicies.html](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/URLLoadingSystem/Concepts/CachePolicies.html)