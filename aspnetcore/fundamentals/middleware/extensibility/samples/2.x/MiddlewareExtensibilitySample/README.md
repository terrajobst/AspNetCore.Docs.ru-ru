# <a name="aspnet-core-middleware-extensibility-sample"></a>Пример расширяемости ПО промежуточного слоя ASP.NET Core

Пример демонстрирует сценарии, описанные в статье [Фабричный метод активации ПО промежуточного слоя в ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/middleware-extensibility).

В примере приложения показана активация ПО промежуточного слоя следующими способами:

* Стандартный. Дополнительные сведения о стандартной активации ПО промежуточного слоя см. [здесь](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/).
* Реализация [IMiddleware](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddleware). ПО промежуточного слоя активируется классом [IMiddlewareFactory](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) по умолчанию.

Реализации ПО промежуточного слоя функционируют идентичным образом и записывают значение, предоставленное в параметре строки запроса (`key`). ПО промежуточного слоя использует внедренный контекст базы данных (ограниченная служба) для записи значения строки запроса в базу данных, выполняющуюся в памяти.
