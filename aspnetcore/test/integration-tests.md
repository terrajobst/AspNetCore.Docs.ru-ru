---
title: Интеграционные тесты в ASP.NET Core
author: guardrex
description: Узнайте, как с помощью интеграционных тестов можно проверить работу компонентов приложения на уровне инфраструктуры, включая базу данных, файловую систему и сеть.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
uid: test/integration-tests
ms.openlocfilehash: a136a362cd8973b3684f9a70bd4792d75238eab0
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207879"
---
# <a name="integration-tests-in-aspnet-core"></a>Интеграционные тесты в ASP.NET Core

По [Люк Лэтем](https://github.com/guardrex) и [Стив Смит](https://ardalis.com/)

Интеграционные тесты гарантируют, что компоненты приложения правильно работают на уровне, который включает в себя поддерживаемую приложением инфраструктуру, например базу данных, файловую систему и сеть. ASP.NET Core поддерживает интеграционные тесты, используя платформу модульного тестирования с тестовым веб-сервером и сервером тестирования в памяти.

Этот раздел предполагает базовое понимание модульных тестов. Если вы не знакомы с основными понятиями тестирования, см. раздел [модульное тестирование в .NET Core и .NET Standard](/dotnet/core/testing/) и связанное с ним содержимое.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) ([как скачивать](xref:index#how-to-download-a-sample))

Пример приложения — это приложение Razor Pages, и оно предполагает базовое понимание страниц Razor. Если вы не знакомы с Razor Pages, см. следующие разделы:

* [Введение в Razor Pages](xref:razor-pages/index)
* [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Модульные тесты страниц Razor](xref:test/razor-pages-tests)

> [!NOTE]
> Для тестирования одностраничных приложений мы рекомендуем инструмент типа [Selenium](https://www.seleniumhq.org/), которым можно автоматизировать браузер.

## <a name="introduction-to-integration-tests"></a>Введение в интеграционные тесты

Интеграционные тесты оценивают компоненты приложения на более обширном уровне, чем [модульные тесты](/dotnet/core/testing/). Модульные тесты используются для проверки изолированных программных компонентов, таких как отдельные методы класса. Интеграционные тесты подтверждают, что как минимум два компонента приложения работают совместно для создания ожидаемого результата, и эти тесты могут включать все компоненты, необходимые для полной обработки запроса.

Эти расширенные тесты, используемые для тестирования инфраструктуры и всей платформы приложения, часто включают в себя следующие компоненты:

* База данных
* Файловая система
* Сетевые устройства
* Конвейер обработки запросов и ответов

Модульные тесты используют поддельные компоненты, известные как *fakes* (фэйки) или *макеты объектов* (моки), вместо компонентов инфраструктуры.

В противоположность модульным тестам, интеграционные тесты:

* Используют фактические компоненты, которые использует приложение в рабочей среде.
* Требуют больше кода и обработки данных.
* Выполняются дольше.

Таким образом, используйте интеграционные тесты только для наиболее важных сценариев инфраструктуры. Если поведение может быть проверено с использованием модульного теста или интеграционного теста, выбирайте модульный тест.

> [!TIP]
> Не создавайте интеграционные тесты для каждого возможного варианта доступа к базам данных и файловым системам. В скольких бы местах приложение ни взаимодействовало с ними, фокусный набор интеграционных тестов на чтение, запись, обновление и удаление обычно способен адекватно протестировать их компоненты. Используйте модульные тесты для стандартных тестов логики методов, взаимодействующих с этими компонентами. В модульных тестах использование инфраструктурных подделок приводит к более быстрому выполнению теста.

> [!NOTE]
> В обсуждениях интеграционных тестов тестируемый проект часто называют *system under test* (тестируемая система), или "SUT" для краткости.

## <a name="aspnet-core-integration-tests"></a>Интеграционные тесты ASP.NET Core

Интеграционным тестам в ASP.NET Core необходимо следующее:

* Тестовый проект используется для хранения и выполнения тестов. Он содержит ссылку на тестируемый проект ASP.NET Core, называемый *тестируемой системой* (SUT). _Словом "SUT" в этом разделе будет обозначаться тестируемое приложение._
* Тестовый проект создает веб-сервер тестирования для SUT и использует клиент тестового сервера для обработки запросов и ответов к SUT.
* Средство выполнения тестов проводит тесты и предоставляет отчет о результатах.

Интеграционные тесты придерживаются последовательности событий из обычных шагов теста *Подготовка*, *Действие*, и *Проверка*:

1. Настраивается веб сервер SUT.
1. Создается клиент тестового сервера для отправки запросов к приложению.
1. Выполняется шаг теста *Подготовка*: тестовое приложение готовит запрос.
1. Выполняется шаг теста *Действие*: клиент отправляет запрос и получает ответ.
1. *Assert* выполняется шаг теста: *фактическое* ответа проверяется как *передать* или *ошибкой* на основе *ожидается*  ответа.
1. Процесс продолжается, пока выполняются все тесты.
1. Выводятся результаты теста.

Как правило, веб-сервер тестирования настраивается отлично от обычного веб-сервера приложения для тестовых запусков. Например, для тестов может быть использована другая база данных или настройки приложения.

Такие компоненты инфраструктуры, как веб-сервер тестирования и сервер тестирования в памяти ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), предоставляются или управляются пакетом [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing). Использование этого пакета упрощает создание и выполнение тестов.

Пакет `Microsoft.AspNetCore.Mvc.Testing` выполняет следующие задачи:

* Копирует файл зависимостей (*\*.deps*) из SUT в папку *bin* тестового проекта.
* Назначает корневой каталог проекта SUT в качестве корневого каталога контента, чтобы статические файлы и страницы/представления были найдены при выполнении тестов.
* Предоставляет класс [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) для упрощения начальной загрузки SUT с `TestServer`.

Документация по [модульным тестам](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) описывает настройку тестового проекта и средства запуска тестов, а также содержит подробные инструкции по выполнению тестов и рекомендации по именованию тестов и тестовых классов.

> [!NOTE]
> При создании тестового проекта для приложения отделяйте модульные тесты от интеграционных, помещая их в разные проекты. Это гарантирует, что компоненты тестовой инфраструктуры не попадут по случайности в модульные тесты. Разделение модульных и интеграционных тестов также позволяет контролировать, какой набор тестов выполняется.

В сущности, между конфигурацией для тестов приложений Razor Pages и MVC-приложений нет никаких отличий. Единственное отличие заключается в том, как именуются тесты. В приложении Razor Pages тесты конечных точек страницы обычно именуются по классу модели страницы (например, `IndexPageTests` для тестирования интеграции компонентов на странице Index). В приложении MVC тесты обычно организованы по классам контроллеров и именуются по контроллеру, который они проверяют (например, `HomeControllerTests` для тестирования интеграции компонентов на контроллере Home).

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

## <a name="inject-mock-services"></a>Ввести макетов служб

Службы могут переопределяться в тесте с помощью вызова [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) в построителе узла. **Чтобы внедрить службы имитации, необходимо иметь SUT `Startup` класса `Startup.ConfigureServices` метод.**

Пример SUT включает областью действия службы, которая возвращает знак кавычек. Квоты внедряется в скрытом поле на странице индекса при запросе страницы индекса.

*Services/IQuoteService.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

*Services/QuoteService.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

*Pages/Index.cshtml.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

*Pages/Index.cs*:

[!code-cshtml[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

Следующая разметка создается при запуске приложения SUT:

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

Для тестирования внедрения службы и квоты при тестировании интеграции, макеты службы внедряется в SUT в тесте. Макеты службы заменяет приложения `QuoteService` и службу, предоставляемую тестовое приложение, с именем `TestQuoteService`:

*IntegrationTests.IndexPageTests.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

`ConfigureTestServices` вызывается, и областью действия служба зарегистрирована:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

Разметки, созданный во время выполнения теста отражает текст предложения, предоставляемые `TestQuoteService`, таким образом передает утверждения:

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
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

Несмотря на то, что приложение не использует шаблон репозитория и не эффективный пример [шаблон единицы работы (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages поддерживает эти шаблоны разработки. Дополнительные сведения см. в разделе [проектирование уровня сохраняемости инфраструктуры](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) и [тестирование логики контроллера](/aspnet/core/mvc/controllers/testing) (этот пример реализует шаблон репозитория).

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
