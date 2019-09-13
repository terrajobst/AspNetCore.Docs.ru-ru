# <a name="blazor-webassembly-sample-app"></a>Пример приложения блазор

В этом примере показано использование сценариев Блазор, описанных в документации по Блазор.

## <a name="call-web-api-example"></a>Пример вызова веб-API

Для примера веб-API требуется работающий веб-API на основе примера приложения для <a href="https://docs.microsoft.com/aspnet/core/tutorials/first-web-api">учебника: Создание веб-API с помощью ASP.NET Core</a> MVC. Пример приложения выполняет запросы к веб-API по адресу `https://localhost:10000/api/todo`. Если используется другой адрес веб-API, обновите `ServiceEndpoint` значение константы в `@functions` блоке компонента Razor.</p>

Пример приложения выполняет запрос на <a href="https://docs.microsoft.com/aspnet/core/security/cors">общий доступ к ресурсам в разных источниках (CORS)</a> от `http://localhost:5000` или `https://localhost:5001` к веб-API. Учетные данные (файлы cookie и заголовки авторизации) разрешены. Добавьте следующую конфигурацию по промежуточного слоя CORS в `Startup.Configure` метод веб-API перед вызовом: `UseMvc`</p>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization)
    .AllowCredentials());
```

Настройте домены и порты `WithOrigins` в соответствии с требованиями приложения блазор.

Веб-API настроен для CORS, чтобы разрешить авторизацию файлов cookie, заголовков и запросов из клиентского кода, но веб-API, созданный с помощью руководства, не авторизует запросы. Рекомендации по реализации см. в <a href="https://docs.microsoft.com/aspnet/core/security/">статье ASP.NET Core безопасность и идентификация</a> .
