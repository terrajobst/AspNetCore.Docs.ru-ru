# <a name="aspnet-core-web-api-controller-sample"></a>Пример контроллера веб-API ASP.NET Core

Этот пример приложения состоит из следующих проектов:

- **WebApiSample.Api**: проект ASP.NET Core 2.1, предназначенный для .NET Core 2.1.
- **WebApiSample.Api.Pre21**: проект ASP.NET Core 2.0, предназначенный для .NET Core 2.0.
- **WebApiSample.DataAccess**: библиотека классов .NET Standard 2.0, которая служит уровнем доступа к данным для двух проектов веб-API.

В этом примере показаны различные способы создания контроллера веб-API:

- [Создание класса, производного от ControllerBase](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [Аннотирование класса атрибутом ApiControllerAttribute](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
