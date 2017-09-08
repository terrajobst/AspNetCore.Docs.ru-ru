# <a name="aspnet-core-response-caching-sample-aspnet-core-1x"></a>Кэширование образец ответа ASP.NET Core (ASP.NET Core 1.x)

В этом примере описывается использование ASP.NET Core [по промежуточного слоя кэширования ответа](xref:performance/caching/middleware) с ASP.NET Core 1.x. Для образца ASP.NET Core 2.x. в разделе [пример кэширования ASP.NET Core ответа (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples/2.x).

Приложение отправляет `Hello World!` сообщений и текущим временем вместе с `Cache-Control` заголовок, чтобы настроить поведение кэширования. Приложение также отправляет `Vary` заголовок, чтобы настроить кэш для использования в ответе, только если `Accept-Encoding` заголовок последующих запросов совпадает с подписью из исходного запроса.

При выполнении этого образца, ответ обслуживается из кэша при возможных и хранимые на срок до 10 секунд.
