---
title: Интеграционные тесты в ASP.NET Core
author: guardrex
description: Узнайте, как тесты интеграции гарантировать правильную работу компонентов приложения на уровне инфраструктуры, включая базы данных, файловой системы и сети.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: test/integration-tests
ms.openlocfilehash: a402c3f5f6a75917eba4058e6cc6926f25b214d4
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2018
ms.locfileid: "35217683"
---
# <a name="integration-tests-in-aspnet-core"></a>Интеграционные тесты в ASP.NET Core

По [Latham Люк](https://github.com/guardrex) и [Стив Смит](https://ardalis.com/)

Интеграционные тесты гарантировать правильную работу компонентов приложения на уровне, который включает в себя приложения вспомогательной инфраструктуры, такие как базы данных, файловой системы и сети. ASP.NET Core поддерживает интеграционных тестов, с помощью модульного тестирования с помощью на веб-узел тестирования и проверки в памяти сервера.

В этом разделе предполагается базовое представление о модульных тестов. Если знакомы с основными понятиями тестирования, см. раздел [модульное тестирование в .NET Core и .NET Standard](/dotnet/core/testing/) раздел и его связанное содержимое.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

Пример приложения представляет собой приложение страниц Razor и предполагает базовое представление о страниц Razor. Если знакомы со страниц Razor, см. в следующих разделах:

* [Введение в Razor Pages](xref:mvc/razor-pages/index)
* [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Модульные тесты страниц Razor](xref:test/razor-pages-tests)

## <a name="introduction-to-integration-tests"></a>Общие сведения об интеграции тестов

Интеграционные тесты оценки компонентов приложения на более широкий уровень, чем [модульные тесты](/dotnet/core/testing/). Модульные тесты используются для тестирования компонентов изолированной программного обеспечения, таких как отдельные методы. Интеграционные тесты убедитесь, что два или более компонентов приложения совместно для создания ожидаемым результатом, возможно, включающий все компоненты, необходимые для полной обработки запроса.

Эти тесты более широкой, используются для тестирования приложения инфраструктуры и всей framework, часто, включая следующие компоненты:

* База данных
* Файловая система
* Сетевые устройства
* Конвейер запросов ответов

Модульные тесты с целью использования компонентов, известный как *fakes* или *макета объектов*, вместо компонентов инфраструктуры.

В отличие от модульные тесты интеграции тестов.

* Используйте фактическое компоненты, которые использует приложение в рабочей среде.
* Требуются дополнительные кода и обработки данных.
* Выполняться дольше.

Таким образом следует ограничьте использование интеграционные тесты для наиболее важных сценариях инфраструктуры. Если это поведение можно проверить при помощи модульного теста или интеграционного теста, выберите модульный тест.

> [!TIP]
> Не создавайте интеграционные тесты для каждого возможные перестановки данных и общих доступ с базами данных и файловые системы. Независимо от того, сколько первых по приложению взаимодействовать с базами данных и файловые системы, конкретное набор чтения, записи, обновления и удаления интеграции тестов обычно поддерживают адекватно тестирования базы данных и компоненты файловой системы. Модульные тесты используются для подпрограмм тесты логику метода, которые взаимодействуют с этими компонентами. В модульных тестах использование инфраструктуры fakes/фиктивных результат более быстрое выполнение теста.

> [!NOTE]
> В обсуждениях интеграционных тестов, часто называют протестированного проекта *Тестируемая система*, или «SUT» для краткости.

## <a name="aspnet-core-integration-tests"></a>Тесты интеграции ASP.NET Core

Интеграционные тесты в ASP.NET Core требуется следующее.

* Тестовый проект используется для хранения и выполнения тестов. Тестовый проект содержит ссылку на протестированного проекта ASP.NET Core, вызывается *Тестируемая система* (SUT). _«SUT» используется в этом разделе для обращения к тестируемой приложения._
* Тестовый проект создает на веб-узел теста для SUT и использует для обработки запросов и ответов на SUT тестового сервера клиента.
* Средства выполнения тестов используется для выполнения тестов и отчетов результатов теста.

Интеграционные тесты выполните последовательность событий, включая обычные *расположение*, *Act*, и *Assert* шаги теста:

1. SUT веб-узел настроен.
1. Сервер тестового клиента создается для отправки запросов к приложению.
1. *Расположение* выполняется шаг теста: тестовое приложение подготавливает запрос.
1. *Act* выполняется шаг теста: клиент отправляет запрос и получает ответ.
1. *Assert* выполняется шаг теста: *фактическое* ответ проверяется как *передачи* или *ошибкой* на основе *ожидается*  ответа.
1. Процесс продолжается, пока выполняются все тесты.
1. Результаты теста отображаются.

Как правило тест веб-узел настроен по-разному, чем обычные веб-узел приложения для теста выполняется. Например другую базу данных или параметры различных приложений могут использоваться для выполнения тестов.

Компоненты инфраструктуры, такие как тест веб-размещения и проверки в памяти сервера ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), предоставленные или управляется [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) пакета. Использование этого пакета упрощает создание тестов и выполнения.

`Microsoft.AspNetCore.Mvc.Testing` Пакет выполняет следующие задачи:

* Копирует файл зависимостей (*\*.deps*) из SUT в тестовый проект *bin* папки.
* Задает содержимое корневого SUT корневой папки проекта, чтобы статических файлов и страниц и представления можно найти при выполнении тестов.
* Предоставляет [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) класс для упрощения начальную загрузку SUT с `TestServer`.

[Модульные тесты](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) документации описывается настройка проекта и теста тестов, а также подробные инструкции о том, как выполнять тесты и рекомендации по для тестирования имен и классов.

> [!NOTE]
> При создании тестового проекта для приложения, отдельные модульные тесты интеграционные тесты в разных проектах. Это позволяет гарантировать, что инфраструктура тестирования компонентов не случайно включены в модульных тестах. Разделение интеграции и модульных тестов также позволяет контролировать, какие наборе тестов выполняются.

Нет практически никаких различий между конфигурации для тестов, страниц Razor приложений и приложений MVC. Единственное отличие заключается в том, в именах тесты. В приложении страниц Razor, тесты, страницы конечных точек обычно названы класс модели страницы (например, `IndexPageTests` для тестирования интеграции компонентов для страницы индекса). В приложении MVC тесты обычно организованы классами контроллера и контроллеров, которые они тестируют с именем (например, `HomeControllerTests` для тестирования интеграции компонентов для контроллера Home).

## <a name="test-app-prerequisites"></a>Необходимые условия теста приложения

Тестовый проект необходимо:

* Иметь ссылку на пакет для [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/).
* Используйте Web SDK в файле проекта (`<Project Sdk="Microsoft.NET.Sdk.Web">`).

Эти prerequesities можно увидеть в [пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/). Проверить *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* файла.

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Основные тесты по умолчанию WebApplicationFactory

[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) используется для создания [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) для тестов интеграции. `TEntryPoint` обычно является класс точки входа SUT `Startup` класса.

Реализуйте классы тестов *оборудования класс* интерфейса (`IClassFixture`) для указания класса содержит тесты и предоставить общий объект экземпляров тестов в классе.

### <a name="basic-test-of-app-endpoints"></a>Базовое тестирование конечные точки приложения

Следующие проверки класса, `BasicTests`, использует `WebApplicationFactory` SUT начальной загрузки и предоставлять [HttpClient](/dotnet/api/system.net.http.httpclient) с тестовым методом `Get_EndpointsReturnSuccessAndCorrectContentType`. Метод проверяет, является ли код состояния ответа успешно (коды состояния в диапазоне 200 299) и `Content-Type` заголовок `text/html; charset=utf-8` для нескольких страниц приложения.

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) создает экземпляр `HttpClient` автоматически, следует переадресации и обрабатывает куки-файлы.

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

### <a name="test-a-secure-endpoint"></a>Тестирование защищенной конечной точки

Еще один тест в `BasicTests` класс проверяет, не прошедшие проверку подлинности пользователь перенаправляется защищенной конечной точки на страницу входа для приложения.

В SUT `/SecurePage` страница использует [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) соглашение для применения [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) на страницу. Дополнительные сведения см. в разделе [страниц Razor авторизации соглашения](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

В `Get_SecurePageRequiresAnAuthenticatedUser` проверить, [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) задано для запрета переадресации, задав [свойству AllowAutoRedirect значения](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) для `false`:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

С помощью запрета клиенту выполните перенаправления, можно сделать следующие проверки:

* Можно проверить код состояния, возвращенный SUT от предполагаемого [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) результат, не конечный код состояния после перенаправления к странице входа, что было бы [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).
* `Location` Значение заголовка в заголовке ответа проверяется, чтобы подтвердить, что он начинается с `http://localhost/Identity/Account/Login`, не окончательный входа страницы ответ, где `Location` заголовок не будет присутствовать.

Дополнительные сведения о `WebApplicationFactoryClientOptions`, в разделе [параметры клиента](#client-options) раздела.

## <a name="customize-webapplicationfactory"></a>Настройка WebApplicationFactory

Конфигурация веб-узла могут быть созданы независимо от тестовых классов путем наследования от `WebApplicationFactory` для создания одного или нескольких пользовательских фабрик:

1. Наследовать от `WebApplicationFactory` и Переопределите [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost). [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) позволяет настраивать службы коллекции с помощью [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   Заполнение в базе данных [пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) выполняет `InitializeDbForTests` метод. Метод описан в [тесты интеграции образец: тестирования приложения организации](#test-app-organization) раздела.

2. Использование пользовательских `CustomWebApplicationFactory` в тестовых классах. В следующем примере используется фабрики в `IndexPageTests` класса:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Пример приложения клиент настроен для предотвращения `HttpClient` из следующих перенаправления. Как описано в статье [тестирования защищенной конечной точки](#test-a-secure-endpoint) раздел, это позволяет тесты для проверки результата первого ответа приложения. Ответ был перенаправления в большинстве случаев эти тесты с `Location` заголовок.

3. Типичный тест использует `HttpClient` и вспомогательные методы для обработки запроса и ответа:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

Любой запрос POST на SUT должны удовлетворять сложные проверки, который выполняется автоматически в приложении [сложные системы защиты данных](xref:security/data-protection/introduction). Чтобы упорядочить для запроса POST теста необходимо тестового приложения:

1. Сделать запрос для страницы.
1. Анализировать сложные куки-файл и маркер проверки запроса из ответа.
1. Сделать запрос POST сложные куки-файл и запрос проверки подлинности маркеров в месте.

`SendAsync` Вспомогательные методы расширения (*Helpers/HttpClientExtensions.cs*) и `GetDocumentAsync` вспомогательный метод (*Helpers/HtmlHelpers.cs*) в [пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/) использовать [AngleSharp](https://anglesharp.github.io/) средство синтаксического анализа для обработки сложные проверку с помощью следующих методов:

* `GetDocumentAsync` &ndash; Получает [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) и возвращает `IHtmlDocument`. `GetDocumentAsync` использует фабрику, которая подготавливает *виртуального ответа* на основе исходного `HttpResponseMessage`. Дополнительные сведения см. в разделе [AngleSharp документации](https://github.com/AngleSharp/AngleSharp#documentation).
* `SendAsync` методы расширения для `HttpClient` составления [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) и вызвать [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) для отправки запросов на SUT. Перегрузки для `SendAsync` принять формы HTML (`IHtmlFormElement`) и следующие:
  - Кнопка формы отправки (`IHtmlElement`)
  - Коллекция значений формы (`IEnumerable<KeyValuePair<string, string>>`)
  - Кнопка отправки (`IHtmlElement`) и формируют значения (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/) библиотека, используемая для демонстрации в этом разделе и в пример приложения синтаксический анализ сторонних разработчиков. AngleSharp не поддерживается или необходимые для интеграции тестирования приложений ASP.NET Core. Можно использовать другие средства синтаксического анализа, например [пакет гибкость получена строка Html (HAP)](http://html-agility-pack.net/). Другой подход заключается в написании кода непосредственно обрабатывать сложные системы маркер проверки запроса и сложные куки-файл.

## <a name="customize-the-client-with-withwebhostbuilder"></a>Настройка клиента с WithWebHostBuilder

Если требуется дополнительная настройка в методе теста, [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) создает новую `WebApplicationFactory` с [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) , дальнейшей настройки конфигурации.

`Post_DeleteMessageHandler_ReturnsRedirectToRoot` Метод из теста [пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) показано использование функции `WithWebHostBuilder`. Проверка выполнения удаления записей в базе данных путем отправки формы в SUT активации.

Поскольку другой тестов в `IndexPageTests` класс выполняет операцию, которая удаляет все записи в базе данных и может выполняться до `Post_DeleteMessageHandler_ReturnsRedirectToRoot` метод базы данных запускается в этом тестовом методе, чтобы убедитесь, что запись для SUT для удаления. При выборе `deleteBtn1` кнопки `messages` формы в SUT моделируется в запросе SUT:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>Параметры клиента

В следующей таблице показаны значение по умолчанию [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) доступны при создании `HttpClient` экземпляров.

| Параметр | Описание: | Значение по умолчанию |
| ------ | ----------- | ------- |
| [Свойству AllowAutoRedirect значения](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Возвращает или задает ли `HttpClient` экземпляров в автоматически должна следовать ответам перенаправления. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Возвращает или задает базовый адрес `HttpClient` экземпляров. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Возвращает или задает ли `HttpClient` экземпляры должны обрабатывать файлы cookie. | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Возвращает или задает максимальное количество ответов переадресации, `HttpClient` экземпляры должны следовать. | 7 |

Создание `WebApplicationFactoryClientOptions` класса и передавать его [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) метода (в примере кода показаны значения по умолчанию):

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Как в инфраструктуре тестирования делает вывод содержимого корневого пути приложения

`WebApplicationFactory` Конструктор выводит содержимое корневого пути приложения, выполняя поиск [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) на сборку, содержащую интеграционные тесты с ключом, равным `TEntryPoint` сборки `System.Reflection.Assembly.FullName`. В случае, если атрибут с правильным ключом не найден, `WebApplicationFactory` будет использоваться для поиска файла решения (*\*.sln*) и добавляет `TEntryPoint` имя сборки в каталоге решения. Корневой каталог приложения (путь содержимое корневого) используется для обнаружения представления и файлы содержимого.

В большинстве случаев нет необходимости явно задавать содержимое корневого приложения, как логика поиска обычно находит правильный корневой содержимого во время выполнения. В специальных случаях, когда не удается найти содержимое корневого с помощью алгоритма поиска встроенных содержимого корневой можно указать явным образом или с помощью пользовательской логики приложения. Чтобы задать содержимое корневого приложения в этих сценариях, вызовите `UseSolutionRelativeContentRoot` метод расширения из [Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost) пакета. Указать относительный путь решения и решения необязательный шаблон файла имя или glob (по умолчанию = `*.sln`).

Вызовите [UseSolutionRelativeContentRoot](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.usesolutionrelativecontentroot) расширение с помощью метода *один* из следующих подходов:

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

* При настройке тестовых классов с пользовательским `WebApplicationFactory`, наследуют `WebApplicationFactory` и Переопределите [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost):

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

Теневое копирование вызывает тесты можно выполнять в папку, отличную от выходную папку. Для правильной работы тестов теневое копирование, необходимо отключить. [Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) использует xUnit и отключает теневого копирования для xUnit, включая *xunit.runner.json* файла с помощью параметра правильную конфигурацию. Дополнительные сведения см. в разделе [Настройка xUnit.net с JSON](https://xunit.github.io/docs/configuring-with-json.html).

Добавить *xunit.runner.json* файл корень тестового проекта со следующим содержимым:

```json
{
  "shadowCopy": false
}
```

## <a name="integration-tests-sample"></a>Пример интеграции тестов

[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) состоит из двух приложений:

| Приложение | Папка проекта | Описание: |
| --- | -------------- | ----------- |
| Сообщение приложения (SUT) | *src/RazorPagesProject* | Позволяет пользователю добавить, удалить один, удалите все и проанализировать сообщения. |
| Тестовое приложение | *tests/RazorPagesProject.Tests* | Используется для тестирования интеграции SUT. |

Тесты можно выполнять с помощью встроенных тестов функций интегрированной среды разработки, такие как [Visual Studio](https://www.visualstudio.com/vs/). При использовании [кода Visual Studio](https://code.visualstudio.com/) или из командной строки, выполните следующую команду в командной строке в *tests/RazorPagesProject.Tests* папки:

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>Сообщение приложения (SUT) организации

SUT — это система сообщение страниц Razor со следующими характеристиками:

* Страница индекса приложения (*Pages/Index.cshtml* и *Pages/Index.cshtml.cs*) предоставляет пользовательский Интерфейс и страницы модели методы для управления Добавление, удаление и анализа сообщений (среднее слов в сообщение) .
* Описывается сообщение `Message` класса (*Data/Message.cs*) с двумя свойствами: `Id` (ключ) и `Text` (сообщение). `Text` Свойство становится необходимым и ограничением в 200 символов.
* Сообщения хранятся с использованием [базы данных в памяти Entity Framework](/ef/core/providers/in-memory/)&#8224;.
* Приложение содержит слой доступа к данным (DAL) в своем классе контекст базы данных, `AppDbContext` (*Data/AppDbContext.cs*).
* Если база данных пуста при запуске приложения, три сообщения инициализирован хранилище сообщений.
* Приложение включает в себя `/SecurePage` , может осуществляться только пользователем, прошедшим проверку.

&#8224;В разделе EF [теста с InMemory](/ef/core/miscellaneous/testing/in-memory), объясняется, как использовать базу данных в памяти для тесты с MSTest. В этом разделе используется [xUnit](https://xunit.github.io/) платформы тестирования. Основные понятия тест и тест реализации различных тестовых платформ похожие, но не идентичен.

Несмотря на то, что приложение не использует [шаблон репозитория](http://martinfowler.com/eaaCatalog/repository.html) и не действующие примером [единицы работы (UoW) шаблон](https://martinfowler.com/eaaCatalog/unitOfWork.html), эти шаблоны разработки поддерживает страниц Razor. Дополнительные сведения см. в разделе [проектирование уровень сохраняемости инфраструктуры](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [реализации репозитория и шаблоны единицы работы в приложении MVC ASP.NET](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), и [контроллера тестирования логика](/aspnet/core/mvc/controllers/testing) (в этом образце реализуется шаблон репозитория).

### <a name="test-app-organization"></a>Тестирование приложения организации

Приложение тестирования является консоль внутри *tests/RazorPagesProject.Tests* папки.

| Папка тестового приложения | Описание: |
| --------------- | ----------- |
| *BasicTests* | *BasicTests.cs* содержит методы тестирования маршрутизации, доступ к безопасной странице неавторизованного пользователя и получение профиля пользователя GitHub и проверки входа профиля пользователя. |
| *IntegrationTests* | *IndexPageTests.cs* содержит тесты интеграции с помощью пользовательской страницы индекса `WebApplicationFactory` класса. |
| *Вспомогательные методы и служебные программы* | <ul><li>*Utilities.cs* содержит `InitializeDbForTests` метод, используемый для инициализации базы данных с тестовыми данными.</li><li>*HtmlHelpers.cs* предоставляет метод для возврата AngleSharp `IHtmlDocument` для использования в методы теста.</li><li>*HttpClientExtensions.cs* предоставляют перегрузки для `SendAsync` для отправки запросов на SUT.</li></ul> |

Платформа тестирования — [xUnit](https://xunit.github.io/). Интеграционные тесты выполняются с помощью [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), которая содержит [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver). Поскольку [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) пакет используется для настройки тестового сервера узла и тестирования, `TestHost` и `TestServer` пакетов не требуется прямое пакет будет ссылаться в файле проекта для тестирования приложения или Конфигурация разработчика в тестовом приложении.

**Заполнение базы данных для тестирования**

Интеграционные тесты обычно требуют небольшого набора данных в базе данных перед выполнением теста. Инструкции delete, например, проверить вызовы для удаления записей базы данных, поэтому базы данных должны иметь по крайней мере одну запись для успешного запроса на удаление.

Пример приложения начальные значения базы данных с помощью этих сообщений в *Utilities.cs* , тестов можно использовать при выполнении:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Модульные тесты](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Модульные тесты страниц Razor](xref:test/razor-pages-tests)
* [ПО промежуточного слоя](xref:fundamentals/middleware/index)
* [Контроллеры тестирования](xref:mvc/controllers/testing)
