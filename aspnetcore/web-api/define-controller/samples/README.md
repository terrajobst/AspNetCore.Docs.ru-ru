# <a name="aspnet-core-web-api-controller-sample"></a><span data-ttu-id="f9c7a-101">Пример контроллера веб-API ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f9c7a-101">ASP.NET Core Web API Controller Sample</span></span>

<span data-ttu-id="f9c7a-102">Этот пример приложения состоит из следующих проектов:</span><span class="sxs-lookup"><span data-stu-id="f9c7a-102">This sample app consists of the following projects:</span></span>

- <span data-ttu-id="f9c7a-103">**WebApiSample.Api**: проект ASP.NET Core 2.1, предназначенный для .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="f9c7a-103">**WebApiSample.Api**: An ASP.NET Core 2.1 project targeting .NET Core 2.1.</span></span>
- <span data-ttu-id="f9c7a-104">**WebApiSample.Api.Pre21**: проект ASP.NET Core 2.0, предназначенный для .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="f9c7a-104">**WebApiSample.Api.Pre21**: An ASP.NET Core 2.0 project targeting .NET Core 2.0.</span></span>
- <span data-ttu-id="f9c7a-105">**WebApiSample.DataAccess**: библиотека классов .NET Standard 2.0, которая служит уровнем доступа к данным для двух проектов веб-API.</span><span class="sxs-lookup"><span data-stu-id="f9c7a-105">**WebApiSample.DataAccess**: A .NET Standard 2.0 class library serving as a data access tier for the 2 Web API projects.</span></span>

<span data-ttu-id="f9c7a-106">В этом примере показаны различные способы создания контроллера веб-API:</span><span class="sxs-lookup"><span data-stu-id="f9c7a-106">This sample illustrates variations of Web API controller creation:</span></span>

- [<span data-ttu-id="f9c7a-107">Создание класса, производного от ControllerBase</span><span class="sxs-lookup"><span data-stu-id="f9c7a-107">Derive class from ControllerBase</span></span>](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [<span data-ttu-id="f9c7a-108">Аннотирование класса атрибутом ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="f9c7a-108">Annotate class with ApiControllerAttribute</span></span>](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
