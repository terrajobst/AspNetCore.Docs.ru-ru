---
title: Интеграционные тесты в ASP.NET Core
author: guardrex
description: Узнайте, как с помощью интеграционных тестов можно проверить работу компонентов приложения на уровне инфраструктуры, включая базу данных, файловую систему и сеть.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
uid: test/integration-tests
ms.openlocfilehash: e18c5704c9d4db9669d8f831f1b556d1723a0fc1
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894170"
---
# <a name="integration-tests-in-aspnet-core"></a>Интеграционные тесты в ASP.NET Core

По [Люк Лэтем](https://github.com/guardrex) и [Стив Смит](https://ardalis.com/)

Интеграционные тесты убедиться, что компоненты приложения правильно работают на уровне, который включает в себя приложения вспомогательной инфраструктурой, например базы данных, файловой системы и сети. ASP.NET Core поддерживает интеграционные тесты, используя платформу модульного тестирования с веб-сервер тестирования и тестирования в памяти сервера.

В этом разделе предполагается, что основные модульных тестов. Если не знакомы с основными понятиями тестирования, см. в разделе [модульное тестирование в .NET Core и .NET Standard](/dotnet/core/testing/) раздел и его связанное содержимое.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

Пример приложения — это приложение Razor Pages, а также знание основных понятий страниц Razor. Если не знакомы с Razor Pages, см. в разделах:

* [Введение в Razor Pages](xref:razor-pages/index)
* [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Модульные тесты страниц Razor](xref:test/razor-pages-tests)

## <a name="introduction-to-integration-tests"></a>Общие сведения о интеграционные тесты

Интеграционные тесты оценивают компоненты приложения на более широкую чем [модульные тесты](/dotnet/core/testing/). Модульные тесты используются для проверки изолированного программных компонентов, таких как отдельные методы. Интеграционные тесты убедитесь, что два или более компонентов приложения совместно для создания ожидаемым результатом, возможно, включающий все компоненты, необходимые для полной обработки запроса.

Эти широкие тесты используются для тестирования приложения инфраструктуры и всей платформы, часто входят следующие компоненты:

* База данных
* Файловая система
* Сетевые устройства
* Конвейер обработки запросов и ответов

Модульные тесты с целью использования компонентов, известный как *fakes* или *макеты объектов*, вместо компонентов инфраструктуры.

В отличие от модульных тестов тестов интеграции.

* Используйте фактические компоненты, которые использует приложение в рабочей среде.
* Требуются дополнительные кода и обработки данных.
* Выполняться дольше.

Таким образом ограничьте использование интеграционные тесты для наиболее важных сценариев инфраструктуры. Если поведение может быть проверен с использованием модульного теста или интеграционного теста, выберите модульного теста.

> [!TIP]
> Не создавайте тесты интеграции для каждой возможные перестановки доступа данных и файла с базами данных и файловые системы. Независимо от того, сколько помещает через приложения взаимодействуют с базами данных и файловые системы, фокусом набор чтения, записи, обновления и удаления интеграции тестов обычно поддерживают адекватного тестирования базы данных и компоненты файловой системы. Модульные тесты используются для стандартных тестов метод логики, которые взаимодействуют с этими компонентами. В модульных тестах использование инфраструктуры fakes/макеты приводят к более быстрого выполнения теста.

> [!NOTE]
> В обсуждении интеграционных тестов, часто называют протестированного проекта *тестируемой системы*, или «SUT» для краткости.

## <a name="aspnet-core-integration-tests"></a>Тесты интеграции ASP.NET Core

Интеграционные тесты в ASP.NET Core необходимо следующее:

* Тестовый проект позволяет содержать и выполнения тестов. Тестовый проект содержит ссылку на протестированного проекта ASP.NET Core, вызывается *тестируемой системы* (SUT). _«SUT» используется в этом разделе для ссылки на проверенных приложений._
* Тестовый проект создает веб-сервер тестирования, для SUT и использует тестовый клиент сервера для обработки запросов и ответов на SUT.
* Средство выполнения тестов используется для выполнения тестов и отчете результаты теста.

Интеграционные тесты выполните последовательность событий, содержащих обычные *расположение*, *Act*, и *Assert* шаги теста:

1. Настраивается SUT веб-узел.
1. Тестовый клиент сервера создается для отправки запросов к приложению.
1. *Расположение* выполняется шаг теста: приложение тестирования готовит запрос.
1. *Act* выполняется шаг теста: клиент отправляет запрос и получает ответ.
1. *Assert* выполняется шаг теста: *фактическое* ответа проверяется как *передать* или *ошибкой* на основе *ожидается*  ответа.
1. Процесс продолжается, пока все тесты выполняются.
1. Выводятся результаты теста.

Как правило веб-сервер тестирования настраивается по-разному запускается обычный веб-узел приложения для теста. Например другой базе данных или параметры различных приложений может использоваться для тестов.

Компоненты инфраструктуры, такие как веб-сервер тестирования и тестирования в памяти сервера ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), предоставленные или под управлением [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) пакета. Использование этого пакета упрощает создание тестов и выполнения.

`Microsoft.AspNetCore.Mvc.Testing` Пакет выполняет следующие задачи:

* Копирует файл зависимостей (*\*.deps*) из SUT в тестовый проект *bin* папки.
* Задает корневой каталог содержимого корневой папки проекта SUT таким образом статических файлов и страниц и представлений при выполнении тестов.
* Предоставляет [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) класса для упрощения начальная загрузка SUT с `TestServer`.

[Модульные тесты](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) документации описывается настройка проектов и тестов средство запуска тестов, а также подробные инструкции о том, как выполнять тесты и рекомендации, касающиеся способа для проверки имен и классов.

> [!NOTE]
> При создании тестового проекта для приложения, отдельные модульные тесты из интеграционные тесты на разные проекты. Это гарантирует, что инфраструктура тестирования компонентов не случайно включены в модульных тестах. Разделение модульные и интеграционные тесты также позволяет контролировать, какие наборе тестов выполняются.

Нет практически никаких различий между конфигурацией для тестов приложений Razor Pages и MVC-приложения. Единственное различие заключается в том, в именах тесты. В приложении страницы Razor, тесты конечных точек страницы обычно даются классом модели страницы (например, `IndexPageTests` для тестирования интеграции компонентов для страницы индексов). В приложении MVC тесты обычно упорядоченный по классам контроллера и с именем контроллеры, они проверяют (например, `HomeControllerTests` для тестирования интеграции компонентов для контроллера Home).

## <a name="test-app-prerequisites"></a>Проверка необходимых компонентов приложения

Необходимо тестового проекта:

* Ссылаться на следующие пакеты:
  - [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  - [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* Укажите Web SDK в файле проекта (`<Project Sdk="Microsoft.NET.Sdk.Web">`). Web SDK является обязательным при ссылке на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Эти компоненты можно увидеть в [пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/). Проверьте *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* файла. Пример приложения использует [xUnit](https://xunit.github.io/) платформы тестирования и [AngleSharp](https://anglesharp.github.io/) библиотеки средство синтаксического анализа, поэтому в примере приложения также ссылается на:

* [xUnit](https://www.nuget.org/packages/xunit/)
* [xUnit.Runner.VisualStudio](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [AngleSharp](https://www.nuget.org/packages/AngleSharp/)

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Основные тесты со значением по умолчанию WebApplicationFactory

[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) используется для создания [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) интеграционные тесты. `TEntryPoint` обычно является класс точки входа SUT `Startup` класса.

Классы реализуют теста *тестового стенда класс* интерфейс (`IClassFixture`) для указания класса содержит тесты и укажите общий объект экземпляры всех тестах в классе.

### <a name="basic-test-of-app-endpoints"></a>Базовое тестирование конечные точки приложения

Следующие проверки класса, `BasicTests`, использует `WebApplicationFactory` для начальной загрузки SUT и предоставить [HttpClient](/dotnet/api/system.net.http.httpclient) с тестовым методом `Get_EndpointsReturnSuccessAndCorrectContentType`. Метод проверяет, при успешном выполнении код состояния ответа (коды состояния в диапазоне от 200 до 299) и `Content-Type` заголовок `text/html; charset=utf-8` для нескольких страниц приложения.

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) создает экземпляр класса `HttpClient` , автоматически следует за перенаправление и обрабатывает файлы cookie.

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

### <a name="test-a-secure-endpoint"></a>Проверка защищенной конечной точки

Еще один тест в `BasicTests` класс проверяет, что защищенную конечную точку перенаправляет без проверки подлинности пользователя на страницу входа для приложения.

В SUT `/SecurePage` странице использует [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) соглашение о применении [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) на страницу. Дополнительные сведения см. в разделе [соглашения об авторизации Razor Pages](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

В `Get_SecurePageRequiresAnAuthenticatedUser` протестировать, [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) присваивается запрет перенаправления, задав [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) для `false`:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

Запретив клиенту выполните перенаправления, можно сделать следующие проверки:

* Можно проверить код состояния, возвращаемый SUT от ожидаемого [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) результат, не конечный код состояния после перенаправления на страницу входа, что было бы [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).
* `Location` Проверяется значение заголовка в заголовке ответа, чтобы подтвердить, что он начинается с `http://localhost/Identity/Account/Login`, не окончательный входа страницы ответ, где `Location` заголовок не будет присутствовать.

Дополнительные сведения о `WebApplicationFactoryClientOptions`, см. в разделе [параметры клиента](#client-options) раздел.

## <a name="customize-webapplicationfactory"></a>Настройка WebApplicationFactory

Веб-узел конфигурации можно создать независимо от тестовых классов путем наследования от `WebApplicationFactory` для создания одного или нескольких пользовательских фабрик:

1. Наследовать от `WebApplicationFactory` и переопределить [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost). [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) позволяет настраивать службы коллекции с [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   Заполнение в базе данных [пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) осуществляется `InitializeDbForTests` метод. Метод описан в [интеграционные тесты пример: тестирование приложения организации](#test-app-organization) раздел.

2. Использование настраиваемого `CustomWebApplicationFactory` в тестовых классов. В следующем примере используется фабрики в `IndexPageTests` класса:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Пример приложения клиента настроен на предотвращение `HttpClient` из следующих перенаправления. Как описано в [тестирования защищенную конечную точку](#test-a-secure-endpoint) раздел, это позволяет тесты, чтобы проверить результат первого отклика приложения. Первого ответа — это перенаправление во многих из этих проверок с `Location` заголовка.

3. Типичный тест использует `HttpClient` и вспомогательные методы для обработки запроса и ответа:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

Любой запрос POST на SUT должны удовлетворять против подделки убедитесь, что в приложении автоматически становится [против подделки система защиты данных](xref:security/data-protection/introduction). Чтобы упорядочить для запроса POST теста, необходимо тестирование приложения:

1. Выполните запрос для страницы.
1. Выполните синтаксический анализ против подделки файла cookie и маркер проверки запроса из ответа.
1. Выполнения запроса POST против подделки файла cookie и запрос проверки подлинности маркера на месте.

`SendAsync` Вспомогательные методы расширения (*Helpers/HttpClientExtensions.cs*) и `GetDocumentAsync` вспомогательный метод (*Helpers/HtmlHelpers.cs*) в [пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/) использовать [AngleSharp](https://anglesharp.github.io/) средство синтаксического анализа для обработки против подделки проверку с помощью следующих методов:

* `GetDocumentAsync` &ndash; Получает [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) и возвращает `IHtmlDocument`. `GetDocumentAsync` использует фабрику, которая подготавливает *виртуального ответа* на основе исходного `HttpResponseMessage`. Дополнительные сведения см. в разделе [AngleSharp документации](https://github.com/AngleSharp/AngleSharp#documentation).
* `SendAsync` методы расширения для `HttpClient` compose [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) и вызвать [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) для отправки запросов на SUT. Перегрузок для `SendAsync` принять HTML-форма (`IHtmlFormElement`) и следующие:
  - Кнопка в форме "Отправить" (`IHtmlElement`)
  - Наборе значений формы (`IEnumerable<KeyValuePair<string, string>>`)
  - Кнопка "Отправить" (`IHtmlElement`) и формируют значения (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/) стороннего синтаксического анализа библиотека, используемая для демонстрационных целей в этой статье и в примере приложения. AngleSharp не поддерживается или для интеграционного тестирования приложений ASP.NET Core. Можно использовать другие средства синтаксического анализа, таких как [пакет гибкость получена строка Html (HAP)](http://html-agility-pack.net/). Другой подход заключается в написании кода непосредственно обрабатывать сложные системы токен проверки запроса и против подделки файла cookie.

## <a name="customize-the-client-with-withwebhostbuilder"></a>Настройка клиента с WithWebHostBuilder

Если в методе теста, необходима дополнительная настройка [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) создает новую `WebApplicationFactory` с [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) , дальнейшей настройки конфигурации.

`Post_DeleteMessageHandler_ReturnsRedirectToRoot` Метод из теста [пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) демонстрируется использование `WithWebHostBuilder`. Этот тест выполняет удаление записи в базе данных путем отправки формы в SUT активации.

Так как другой проверять в `IndexPageTests` класс выполняет операцию, которая удаляет все записи в базе данных и могут выполняться перед `Post_DeleteMessageHandler_ReturnsRedirectToRoot` метод, базы данных будет заполняться данными в этом тестовом методе, чтобы обеспечить наличие SUT для удаления записи. Выбрав `deleteBtn1` кнопки `messages` формы в SUT моделируется в запросе к SUT:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>Параметры клиента

В следующей таблице показаны по умолчанию [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) доступны при создании `HttpClient` экземпляров.

| Параметр | Описание: | Значение по умолчанию |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Возвращает или задает ли `HttpClient` экземпляров должен автоматически следовать ответам перенаправления. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Возвращает или задает базовый адрес `HttpClient` экземпляров. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Возвращает или задает ли `HttpClient` экземпляров должен обрабатывать файлы cookie. | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Возвращает или задает максимальное количество ответов переадресации, `HttpClient` экземпляры должны следовать. | 7 |

Создание `WebApplicationFactoryClientOptions` класса и передать его в [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) метод (в примере кода показаны значения по умолчанию):

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Как в инфраструктуре тестирования выводит путь корня содержимого приложения

`WebApplicationFactory` Конструктора определяет путь корня содержимого приложения, выполнив поиск [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) на сборку, содержащую интеграционные тесты с ключом, равным `TEntryPoint` сборки `System.Reflection.Assembly.FullName`. В случае, если атрибут с правильным ключом не найден, `WebApplicationFactory` переключение на поиск файла решения (*\*.sln*) и добавляет `TEntryPoint` имя сборки в каталоге решения. Корневой каталог приложения (путь корня содержимого) используется для обнаружения представления и файлы содержимого.

В большинстве случаев нет необходимости явно задать корневой каталог содержимого приложения, как логика поиска обычно находит правильный корневой каталог содержимого во время выполнения. В специальных случаях, где не найден корневой каталог содержимого с помощью алгоритма встроенные поисковые, содержимого, что корневой можно указать явным образом или с помощью пользовательской логики приложения. Чтобы задать корневой каталог содержимого приложения в этих сценариях, вызовите `UseSolutionRelativeContentRoot` метод расширения из [Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost) пакета. Укажите относительный путь решения и шаблон файла имя или glob дополнительное средство решения (по умолчанию = `*.sln`).

Вызовите [UseSolutionRelativeContentRoot](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.usesolutionrelativecontentroot) с помощью метода расширения *один* из следующих подходов:

* При настройке тестовых классов с `WebApplicationFactory`, предоставить пользовательскую конфигурацию с [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder):

   ```csharp
   public IndexPageTests(
       WebApplicationFactory<RazorPagesProject.Startup> factory)
   {
       var _factory = factory.WithWebHostBuilder(builder =>
       {
           builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

           ...
       });
   }
   ```

* При настройке тестовых классов с пользовательским `WebApplicationFactory`, наследуют от `WebApplicationFactory` и переопределить [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost):

   ```csharp
   public class CustomWebApplicationFactory<TStartup>
       : WebApplicationFactory<RazorPagesProject.Startup>
   {
       protected override void ConfigureWebHost(IWebHostBuilder builder)
       {
           builder.ConfigureServices(services =>
           {
               builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

               ...
           });
       }
   }
   ```

## <a name="disable-shadow-copying"></a>Отключить теневое копирование

Теневое копирование вызывает ошибки в тестах для выполнения в той же папке в выходную папку. Для тестов для правильной работы теневое копирование, необходимо отключить. [Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) использует xUnit и отключает теневого копирования для xUnit, включив *xunit.runner.json* файл с параметром правильная конфигурация. Дополнительные сведения см. в разделе [Настройка xUnit.net с JSON](https://xunit.github.io/docs/configuring-with-json.html).

Добавить *xunit.runner.json* файла корневой каталог тестового проекта со следующим содержимым:

```json
{
  "shadowCopy": false
}
```

## <a name="integration-tests-sample"></a>Пример тестов интеграции

[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) состоит из двух приложений:

| Приложение | Папка проекта | Описание: |
| --- | -------------- | ----------- |
| Сообщение приложения (SUT) | *src/RazorPagesProject* | Позволяет пользователю добавить, удалить один, удалить все и анализ сообщений. |
| Тестирование приложения | *tests/RazorPagesProject.Tests* | Используется для тестирования интеграции SUT. |

Тесты можно выполнять с помощью встроенной возможности интерфейса IDE, например [Visual Studio](https://www.visualstudio.com/vs/). При использовании [Visual Studio Code](https://code.visualstudio.com/) или из командной строки, выполните следующую команду в командной строке в *tests/RazorPagesProject.Tests* папку:

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>Сообщение приложения (SUT) организации

SUT — это система сообщение Razor Pages со следующими характеристиками:

* Страница индекса приложения (*Pages/Index.cshtml* и *Pages/Index.cshtml.cs*) предоставляет пользовательский Интерфейс и страницы модели методы для управления, добавление, удаление и анализа сообщений (среднее слов в сообщения) .
* Описывается сообщение `Message` класс (*Data/Message.cs*) с двумя свойствами: `Id` (ключ) и `Text` (сообщение). `Text` Свойство требуется, но более 200 символов.
* Сообщения хранятся с использованием [базы данных в памяти платформа Entity Framework](/ef/core/providers/in-memory/)&#8224;.
* Приложение содержит слой доступа к данным (DAL) в классе контекста базы данных, `AppDbContext` (*Data/AppDbContext.cs*).
* Если база данных пуста при запуске приложения, хранилище сообщений инициализируется с помощью этих сообщений.
* Приложение содержит `/SecurePage` , которые могут быть доступны только пользователем, прошедшим проверку.

&#8224;В разделе EF [теста с помощью InMemory](/ef/core/miscellaneous/testing/in-memory), объясняется, как использовать базу данных в памяти для тестов с использованием MSTest. В этом разделе используется [xUnit](https://xunit.github.io/) платформы тестирования. Концепциях тестирования и тестирования реализации различных тестовых платформ доступны, но не идентичен.

Несмотря на то, что приложение не использует [шаблон репозитория](http://martinfowler.com/eaaCatalog/repository.html) и не эффективный пример [шаблон единицы работы (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages поддерживает эти шаблоны разработки. Дополнительные сведения см. в разделе [проектирование уровня сохраняемости инфраструктуры](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [реализация репозитория и шаблонов единицы работы в приложении MVC ASP.NET](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), и [контроллера тестирования логика](/aspnet/core/mvc/controllers/testing) (этот пример реализует шаблон репозитория).

### <a name="test-app-organization"></a>Тестирование приложений организации

Тестирование приложения — это консольное приложение внутри *tests/RazorPagesProject.Tests* папки.

| Папка тестового приложения | Описание: |
| --------------- | ----------- |
| *BasicTests* | *BasicTests.cs* содержит методы для тестирования маршрутизации, доступ к защищенную страницу, не прошедшие проверку подлинности пользователя с правами и получить профиль пользователя GitHub и проверки входа пользователя для профиля. |
| *IntegrationTests* | *IndexPageTests.cs* содержит тесты интеграции для страницы индекса, с помощью пользовательских `WebApplicationFactory` класса. |
| *Вспомогательные функции и служебные программы* | <ul><li>*Utilities.cs* содержит `InitializeDbForTests` метод, используемый для заполнения базы тестовыми данными.</li><li>*HtmlHelpers.cs* предоставляет метод для возврата AngleSharp `IHtmlDocument` для использования в методы теста.</li><li>*HttpClientExtensions.cs* предоставляют перегрузки для `SendAsync` для отправки запросов на SUT.</li></ul> |

Платформа тестирования — [xUnit](https://xunit.github.io/). Интеграционные тесты выполняются с помощью [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), который включает [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver). Так как [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) пакет используется для настройки тестового сервера узла и тестирования, `TestHost` и `TestServer` пакетов не требуются ссылки на прямой пакеты в файле проекта приложение тестирования или Конфигурация разработчика в тестовом приложении.

**Заполнение базы данных для тестирования**

Интеграционные тесты обычно требуют небольшой набор данных в базе данных перед исполнением теста. Например удаление проверить вызовы для удаления записей базы данных, поэтому база данных должна иметь по крайней мере одну запись для успешного выполнения запроса на удаление.

Пример приложения заполняющий базу данных с помощью этих сообщений в *Utilities.cs* , тестов можно использовать при выполнении:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Модульные тесты](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Модульные тесты страниц Razor](xref:test/razor-pages-tests)
* [ПО промежуточного слоя](xref:fundamentals/middleware/index)
* [Контроллеры тестирования](xref:mvc/controllers/testing)
