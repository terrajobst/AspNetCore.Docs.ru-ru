# <a name="aspnet-core-response-caching-sample-aspnet-core-2x"></a>Кэширование образец ответа ASP.NET Core (ASP.NET Core 2.x)

В этом примере описывается использование ASP.NET Core [по промежуточного слоя кэширования ответа](xref:performance/caching/middleware) с ASP.NET Core 2.x. Пример 1.x ASP.NET Core см. в разделе [пример кэширования ASP.NET Core ответа (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples/1.x).

Приложение отправляет `Hello World!` сообщений и текущим временем вместе с `Cache-Control` заголовок, чтобы настроить поведение кэширования. Приложение также отправляет `Vary` заголовок, чтобы настроить кэш для использования в ответе, только если `Accept-Encoding` заголовок последующих запросов совпадает с подписью из исходного запроса.

При выполнении этого образца, ответ обслуживается из кэша при возможных и хранимые на срок до 10 секунд.
