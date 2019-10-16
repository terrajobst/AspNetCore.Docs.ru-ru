---
title: Интеграционные тесты в ASP.NET Core
author: guardrex
description: Узнайте, как с помощью интеграционных тестов можно проверить работу компонентов приложения на уровне инфраструктуры, включая базу данных, файловую систему и сеть.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2019
uid: test/integration-tests
ms.openlocfilehash: 863b95230d376d050c34a9ed585b7696e649cb05
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2019
ms.locfileid: "72378717"
---
# <a name="integration-tests-in-aspnet-core"></a>Интеграционные тесты в ASP.NET Core

[Люк ЛаСаМ](https://github.com/guardrex) и [Стив Смит](https://ardalis.com/)

::: moniker range=">= aspnetcore-3.0"

Интеграционные тесты гарантируют, что компоненты приложения правильно работают на уровне, который включает в себя поддерживающую инфраструктуру приложения, такую как база данных, файловая система и сеть. ASP.NET Core поддерживает интеграционные тесты с помощью платформы модульного тестирования с тестовым веб-узлом и тестовым сервером в памяти.

В этом разделе предполагается базовое понимание модульных тестов. Если вы не знакомы с концепциями тестирования, см. раздел [модульное тестирование в .NET Core и .NET Standard](/dotnet/core/testing/) разделе и связанное содержимое.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) ([как скачивать](xref:index#how-to-download-a-sample))

Пример приложения является Razor Pages приложением и предполагает базовое понимание Razor Pages. Если вы не знакомы с Razor Pages, см. следующие разделы:

* [Введение в Razor Pages](xref:razor-pages/index)
* [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Модульные тесты страниц Razor](xref:test/razor-pages-tests)

> [!NOTE]
> Для тестирования одностраничные приложения мы рекомендуем использовать средство, такое как [Selenium](https://www.seleniumhq.org/), которое может автоматизировать браузер.

## <a name="introduction-to-integration-tests"></a>Введение в интеграционные тесты

Интеграционные тесты оценивают компоненты приложения на более широком уровне, чем [модульные тесты](/dotnet/core/testing/). Модульные тесты используются для тестирования изолированных программных компонентов, например для отдельных методов класса. Тесты интеграции подтверждают, что два или несколько компонентов приложения работают вместе для получения ожидаемого результата, возможно, включая все компоненты, необходимые для полной обработки запроса.

Эти более широкие тесты используются для тестирования инфраструктуры приложения и всей платформы, часто включая следующие компоненты:

* База данных
* Файловая система
* Сетевые устройства
* Конвейер "запрос-ответ"

Модульные тесты используют готовые компоненты, известные как *фиктивные* или *Макетные объекты*вместо компонентов инфраструктуры.

В отличие от модульных тестов, интеграционные тесты:

* Используйте фактические компоненты, которые приложение использует в рабочей среде.
* Требовать больше кода и обработки данных.
* Выполнение займет больше времени.

Поэтому Ограничьте использование интеграционных тестов наиболее важными сценариями инфраструктуры. Если поведение можно проверить с помощью модульного теста или теста интеграции, выберите модульный тест.

> [!TIP]
> Не создавайте интеграционные тесты для каждого возможного перестановки данных и доступа к файлам с помощью баз данных и файловых систем. Независимо от того, сколько мест в приложении взаимодействуют с базами данных и файловыми системами, набор интеграционных тестов для чтения, записи, обновления и удаления, как правило, может обеспечить адекватное тестирование компонентов базы данных и файловой системы. Используйте модульные тесты для регулярных тестов логики метода, которые взаимодействуют с этими компонентами. В модульных тестах использование фиктивных и макетов инфраструктуры приводит к более быстрому выполнению тестов.

> [!NOTE]
> При обсуждении интеграционных тестов тестируемый проект часто называется тестируемой *системой*или "сут" для краткости.
>
> *В этом разделе используется "сут" для ссылки на проверенное приложение ASP.NET Core.*

## <a name="aspnet-core-integration-tests"></a>ASP.NET Core интеграционных тестов

Для интеграционных тестов в ASP.NET Core требуется следующее:

* Тестовый проект используется для хранения и выполнения тестов. Тестовый проект содержит ссылку на сут.
* Тестовый проект создает тестовый веб-узел для сут и использует клиент тестового сервера для обработки запросов и ответов с помощью сут.
* Средство запуска тестов используется для выполнения тестов и передачи результатов тестов.

Интеграционные тесты следуют последовательности событий, включающих стандартные шаги *упорядочения* *, действия*и *утверждения* .

1. Веб-узел сут настроен.
1. Для отправки запросов в приложение создается клиент тестового сервера.
1. Шаг " *упорядочение* теста" выполняется: тестовое приложение подготавливает запрос.
1. Выполняется *шаг теста действия* : клиент отправляет запрос и получает ответ.
1. Выполняется *проверочный шаг утверждения* : *фактический* ответ проверяется как *пройденный* или *сбой* на основе *ожидаемого* ответа.
1. Процесс продолжится до тех пор, пока не будут выполнены все тесты.
1. Выводятся результаты теста.

Обычно тестовый веб-узел настраивается иначе, чем обычный веб-узел приложения для тестовых запусков. Например, для тестов может использоваться другая база данных или другие параметры приложения.

Компоненты инфраструктуры, такие как тестовый веб-узел и сервер тестирования в памяти ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), предоставляются или управляются пакетом [Microsoft. AspNetCore. MVC. Test](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) . Использование этого пакета упрощает создание и выполнение тестов.

Пакет `Microsoft.AspNetCore.Mvc.Testing` обрабатывает следующие задачи:

* Копирует файл зависимостей ( *. Deps*) из сут в каталог *bin* тестового проекта.
* Задает [корневой каталог содержимого](xref:fundamentals/index#content-root) в корне проекта сут, чтобы при выполнении тестов были найдены статические файлы и страницы или представления.
* Предоставляет класс [вебаппликатионфактори](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) для упрощения начальной загрузки сут с `TestServer`.

В документации по [модульным тестам](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) описано, как настроить тестовый проект и средство запуска тестов, а также подробные инструкции по выполнению тестов и рекомендаций по именованию тестов и тестовых классов.

> [!NOTE]
> При создании тестового проекта для приложения разделите модульные тесты от интеграционных тестов на разные проекты. Это гарантирует, что компоненты тестирования инфраструктуры не будут случайно добавлены в модульные тесты. Разделение модульных и интеграционных тестов также позволяет контролировать, какой набор тестов выполняется.

В конфигурации для тестирования Razor Pages приложений и приложений MVC практически нет различий. Единственное отличие заключается в том, как имена тестов называются. В приложении Razor Pages тесты конечных точек страниц обычно именуются по классу модели страницы (например, `IndexPageTests`, чтобы проверить интеграцию компонентов для страницы индекса). В приложении MVC тесты, как правило, упорядочены по классам контроллеров и названы после тестирования контроллеров (например, `HomeControllerTests`, чтобы проверить интеграцию компонентов для контроллера Home).

## <a name="test-app-prerequisites"></a>Необходимые условия для тестирования приложения

Тестовый проект должен:

* Сослаться на пакет [Microsoft. AspNetCore. MVC. Test](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) .
* Укажите веб-пакет SDK в файле проекта (`<Project Sdk="Microsoft.NET.Sdk.Web">`).

Эти предварительные требования можно просмотреть в [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/). Изучите файл *Tests/разорпажеспрожект. Tests/разорпажеспрожект. Tests. csproj* . В примере приложения используется платформа тестирования [xUnit](https://xunit.github.io/) и библиотека средства синтаксического анализа [англешарп](https://anglesharp.github.io/) , поэтому пример приложения также ссылается на:

* [xUnit](https://www.nuget.org/packages/xunit)
* [xUnit. Runner. VisualStudio](https://www.nuget.org/packages/xunit.runner.visualstudio)
* [англешарп](https://www.nuget.org/packages/AngleSharp)

Entity Framework Core также используется в тестах. Ссылки на приложение:

* [Microsoft. AspNetCore. Diagnostics. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore)
* [Microsoft. AspNetCore. Identity. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity.EntityFrameworkCore)
* [Microsoft. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore)
* [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)
* [Microsoft. EntityFrameworkCore. Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools)

## <a name="sut-environment"></a>Среда сут

Если [Среда](xref:fundamentals/environments) сут не задана, среда по умолчанию имеет значение Development.

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Базовые тесты с Вебаппликатионфактори по умолчанию

[Вебаппликатионфактори @ no__t-1TEntryPoint >](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) используется для создания [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) для интеграционных тестов. `TEntryPoint` является классом точки входа сут, обычно классом `Startup`.

Тестовые классы реализуют интерфейс *основы класса* ([иклассфикстуре](https://xunit.github.io/docs/shared-context#class-fixture)), чтобы указать, что класс содержит тесты, и предоставить экземпляры общего объекта между тестами в классе.

### <a name="basic-test-of-app-endpoints"></a>Базовое тестирование конечных точек приложения

В следующем тестовом классе, `BasicTests`, используется `WebApplicationFactory` для начальной загрузки сут и предоставления [HttpClient](/dotnet/api/system.net.http.httpclient) методу теста, `Get_EndpointsReturnSuccessAndCorrectContentType`. Метод проверяет успешность кода состояния отклика (коды состояний в диапазоне 200-299), а заголовок `Content-Type` — `text/html; charset=utf-8` для нескольких страниц приложений.

[Креатеклиент](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) создает экземпляр `HttpClient`, который автоматически следует за перенаправляет и обрабатывает файлы cookie.

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

По умолчанию некритические файлы cookie не сохраняются в запросах, если включена [политика согласия GDPR](xref:security/gdpr) . Чтобы сохранить ненужные файлы cookie, например те, которые используются поставщиком TempData, пометьте их как основные в тестах. Инструкции по маркировке файла cookie в качестве основы см. в разделе [основные файлы cookie](xref:security/gdpr#essential-cookies).

### <a name="test-a-secure-endpoint"></a>Тестирование безопасной конечной точки

Другой тест в классе `BasicTests` проверяет, что защищенная конечная точка перенаправляет пользователя, не прошедшего проверку подлинности, на страницу входа приложения.

В сут на странице `/SecurePage` используется соглашение [аусоризепаже](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) для применения [аусоризефилтер](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) к странице. Дополнительные сведения см. в разделе [соглашения об авторизации Razor Pages](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

В тесте `Get_SecurePageRequiresAnAuthenticatedUser` параметр [вебаппликатионфакториклиентоптионс](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) имеет значение запретить перенаправления, установив [алловауторедирект](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) в значение `false`:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

Если запретить клиенту следовать перенаправлению, можно выполнить следующие проверки:

* Код состояния, возвращаемый сут, может быть проверен на соответствие ожидаемому [HttpStatusCode. результат перенаправления](/dotnet/api/system.net.httpstatuscode) , а не окончательный код состояния после перенаправления на страницу входа, что было бы [HttpStatusCode. ОК](/dotnet/api/system.net.httpstatuscode).
* Значение заголовка `Location` в заголовках ответа проверяется, чтобы подтвердить, что начинается с `http://localhost/Identity/Account/Login`, а не от последнего ответа страницы входа, где отсутствует заголовок `Location`.

Дополнительные сведения о `WebApplicationFactoryClientOptions` см. в разделе [Параметры клиента](#client-options) .

## <a name="customize-webapplicationfactory"></a>Настройка Вебаппликатионфактори

Конфигурацию веб-узла можно создать независимо от тестовых классов, наследуя от `WebApplicationFactory`, чтобы создать одну или несколько пользовательских фабрик:

1. Наследовать от `WebApplicationFactory` и переопределить [конфигуревебхост](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost). [Ивебхостбуилдер](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) позволяет настраивать коллекцию служб с помощью [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   Заполнение базы данных в [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) выполняется методом `InitializeDbForTests`. Этот метод описан в разделе [Пример интеграционных тестов: тестирование приложения в Организации](#test-app-organization) .

   Контекст базы данных сут регистрируется в своем методе `Startup.ConfigureServices`. Обратный вызов `builder.ConfigureServices` тестового приложения выполняется *после* выполнения кода `Startup.ConfigureServices` приложения. Чтобы использовать для тестов базу данных, отличную от базы данных приложения, необходимо заменить контекст базы данных приложения на `builder.ConfigureServices`.

   Пример приложения находит дескриптор службы для контекста базы данных и использует дескриптор для удаления регистрации службы. Затем фабрика добавляет новый `ApplicationDbContext`, который использует базу данных в памяти для тестов.

   Чтобы подключиться к базе данных, отличной от базы данных в памяти, измените вызов `UseInMemoryDatabase`, чтобы подключить контекст к другой базе данных. Чтобы использовать SQL Server тестовую базу данных, выполните следующие действия.

   * Сослаться на пакет NuGet [Microsoft. EntityFrameworkCore. SqlServer] https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/) в файле проекта.
   * Вызовите `UseSqlServer` со строкой подключения к базе данных.

   ```csharp
   services.AddDbContext<ApplicationDbContext>((options, context) => 
   {
       context.UseSqlServer(
           Configuration.GetConnectionString("TestingDbConnectionString"));
   });
   ```

2. Используйте пользовательский `CustomWebApplicationFactory` в тестовых классах. В следующем примере используется фабрика в классе `IndexPageTests`:

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Клиент примера приложения настроен так, чтобы запретить `HttpClient` от следующих перенаправлений. Как описано в разделе [тестирование защищенной конечной точки](#test-a-secure-endpoint) , это позволяет тестам проверять результат первого ответа приложения. Первый ответ — это перенаправление во многих из этих тестов с заголовком `Location`.

3. Типичный тест использует `HttpClient` и вспомогательные методы для обработки запроса и ответа:

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

Любой запрос POST к сут должен соответствовать проверке подделки, которая автоматически делается в [системе защиты данных в приложении с защитой от подделки](xref:security/data-protection/introduction). Чтобы упорядочить запрос POST теста, тестовое приложение должно:

1. Выполните запрос к странице.
1. Проанализируйте файл cookie подделки и запросите маркер проверки из ответа.
1. Выполните запрос POST с файлом cookie подделки и запросом маркера проверки на месте.

Методы расширения вспомогательной функции `SendAsync` (helpers */хттпклиентекстенсионс. CS*) и вспомогательный метод `GetDocumentAsync` (helpers */хтмлхелперс. CS*) в [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) используют средство синтаксического анализа [англешарп](https://anglesharp.github.io/) для обработки защиты от подделки с помощью следующие методы:

* `GetDocumentAsync` &ndash; получает [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) и возвращает `IHtmlDocument`. `GetDocumentAsync` использует фабрику, которая подготавливает *виртуальный ответ* на основе исходного `HttpResponseMessage`. Дополнительные сведения см. в [документации по англешарп](https://github.com/AngleSharp/AngleSharp#documentation).
* методы расширения `SendAsync` для `HttpClient` составляют [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) и вызывают [SendAsync (HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) для отправки запросов сут. Перегрузки для `SendAsync` принимают HTML-форму (`IHtmlFormElement`) и следующие элементы:
  * Кнопка "Отправить" в форме (`IHtmlElement`)
  * Коллекция значений форм (`IEnumerable<KeyValuePair<string, string>>`)
  * Кнопка "Отправить" (`IHtmlElement`) и значения формы (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [Англешарп](https://anglesharp.github.io/) — это библиотека анализа сторонних производителей, используемая для демонстрационных целей в этом разделе и в примере приложения. Англешарп не поддерживается или не требуется для интеграционного тестирования ASP.NET Core приложений. Можно использовать другие средства синтаксического анализа, такие как [пакет динамичности HTML (хап)](https://html-agility-pack.net/). Другой подход заключается в написании кода для непосредственной работы с маркером проверки запроса системы защиты от подделки и с помощью файла cookie подделки.

## <a name="customize-the-client-with-withwebhostbuilder"></a>Настройка клиента с помощью Висвебхостбуилдер

Если в методе теста требуется дополнительная настройка, [висвебхостбуилдер](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) создает новый `WebApplicationFactory` с [ивебхостбуилдер](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) , который еще настраивается конфигурацией.

Метод теста `Post_DeleteMessageHandler_ReturnsRedirectToRoot` [примера приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) демонстрирует использование `WithWebHostBuilder`. Этот тест выполняет запись удаления в базе данных, активируя отправку формы в сут.

Поскольку другой тест в классе `IndexPageTests` выполняет операцию, которая удаляет все записи в базе данных и может выполняться перед методом `Post_DeleteMessageHandler_ReturnsRedirectToRoot`, база данных повторно заполняется в этом методе теста, чтобы обеспечить наличие записи для удаления сут. Выбор первой кнопки удаления формы `messages` в сут имитируется в запросе к сут:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>Параметры клиента

В следующей таблице показаны [вебаппликатионфакториклиентоптионс](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) по умолчанию, доступные при создании экземпляров `HttpClient`.

| Параметр | Описание | Значение по умолчанию |
| ------ | ----------- | ------- |
| [алловауторедирект](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Возвращает или задает значение, указывающее, должны ли экземпляры `HttpClient` автоматически следовать ответам перенаправления. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Возвращает или задает базовый адрес экземпляров `HttpClient`. | `http://localhost` |
| [хандлекукиес](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Возвращает или задает значение, указывающее, должны ли экземпляры `HttpClient` управлять файлами cookie. | `true` |
| [максаутоматикредиректионс](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Возвращает или задает максимальное число ответов на перенаправление, которым должны следовать экземпляры `HttpClient`. | 7 |

Создайте класс `WebApplicationFactoryClientOptions` и передайте его в метод [креатеклиент](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) (значения по умолчанию показаны в примере кода):

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a>Вставка служб макетирования

Службы могут быть переопределены в тесте с помощью вызова [конфигуретестсервицес](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) в построителе узлов. **Чтобы внедрить службы макетирования, сут должен иметь класс `Startup` с методом `Startup.ConfigureServices`.**

Пример сут включает службу с заданной областью, которая возвращает кавычки. Цитата внедряется в скрытое поле на странице индекса при запросе страницы индекса.

*Services/IQuoteService. CS*:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

*Services/QuoteService. CS*:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

*Pages/Index.cshtml.cs*:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

*Pages/index. CS*:

[!code-cshtml[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

При запуске приложения сут создается следующая разметка:

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

Чтобы протестировать службу и внедрение цитат в интеграционный тест, служба макетирования будет внедрена в сут тестом. Макет службы заменяет @no__t (0) приложения на службу, предоставляемую тестовым приложением, с именем `TestQuoteService`:

*IntegrationTests.IndexPageTests.CS*:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

вызывается `ConfigureTestServices`, и служба с заданной областью зарегистрирована:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

Разметка, созданная во время выполнения теста, отражает текст цитаты, предоставленный `TestQuoteService`, поэтому утверждение передается следующим образом:

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Как инфраструктура тестирования определяет корневой путь к содержимому приложения

Конструктор `WebApplicationFactory` определяет корневой путь к [содержимому](xref:fundamentals/index#content-root) приложения, выполняя поиск [вебаппликатионфакториконтентрутаттрибуте](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) в сборке, содержащей тесты интеграции, с ключом, равным сборке `TEntryPoint` `System.Reflection.Assembly.FullName`. Если атрибут с правильным ключом не найден, `WebApplicationFactory` возвращается к поиску файла решения (*SLN*) и добавляет имя сборки `TEntryPoint` в каталог решения. Корневой каталог приложения (корневой путь к содержимому) используется для обнаружения представлений и файлов содержимого.

## <a name="disable-shadow-copying"></a>Отключение теневого копирования

Теневое копирование приводит к тому, что тесты выполняются в каталоге, отличном от каталога выходного каталога. Для правильной работы тестов необходимо отключить теневое копирование. [Пример приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) использует xUnit и отключает теневое копирование для xUnit, включая файл *xUnit. Runner. JSON* с правильным параметром конфигурации. Дополнительные сведения см. в разделе [Настройка xUnit с помощью JSON](https://xunit.github.io/docs/configuring-with-json.html).

Добавьте файл *xUnit. Runner. JSON* в корень тестового проекта со следующим содержимым:

```json
{
  "shadowCopy": false
}
```

## <a name="disposal-of-objects"></a>Удаление объектов

После выполнения тестов реализации `IClassFixture` [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) и [HttpClient](/dotnet/api/system.net.http.httpclient) удаляются, когда xUnit удаляет [вебаппликатионфактори](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1). Если для объектов, создаваемых разработчиком, требуется реализация, удалите их в реализации `IClassFixture`. Дополнительные сведения см. [в разделе Реализация метода Dispose](/dotnet/standard/garbage-collection/implementing-dispose).

## <a name="integration-tests-sample"></a>Пример интеграционных тестов

[Пример приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) состоит из двух приложений:

| Приложение | Каталог проекта | Описание |
| --- | ----------------- | ----------- |
| Приложение сообщений (сут) | *src/Разорпажеспрожект* | Позволяет пользователю добавлять, удалять, удалять все и анализировать сообщения. |
| Тестовое приложение | *Tests/Разорпажеспрожект. Tests* | Используется для интеграции тестирования сут. |

Тесты можно выполнять с помощью встроенных функций тестирования интегрированной среды разработки, например [Visual Studio](https://visualstudio.microsoft.com). При использовании [Visual Studio Code](https://code.visualstudio.com/) или командной строки выполните следующую команду в командной строке в каталоге *Tests/разорпажеспрожект. Tests* :

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>Организация приложения для сообщений (сут)

СУТ — это Razor Pages система сообщений со следующими характеристиками:

* Страница индекса приложения (*pages/index. cshtml* и *pages/index. cshtml. CS*) предоставляет методы пользовательского интерфейса и модели страницы для управления добавлением, удалением и анализом сообщений (среднее число слов на сообщение).
* Сообщение описывается классом `Message` (*Data/Message. CS*) с двумя свойствами: `Id` (ключ) и `Text` (сообщение). Свойство `Text` является обязательным и ограничено 200 символами.
* Сообщения хранятся с&#8224;помощью [Entity Framework базы данных в памяти](/ef/core/providers/in-memory/).
* Приложение содержит уровень доступа к данным (DAL) в своем классе контекста базы данных `AppDbContext` (*Data/аппдбконтекст. CS*).
* Если база данных пуста при запуске приложения, то хранилище сообщений инициализируется тремя сообщениями.
* Приложение включает `/SecurePage`, доступ к которому может получить только пользователь, прошедший проверку подлинности.

&#8224;Раздел EF, [тест с использованием памяти](/ef/core/miscellaneous/testing/in-memory), объясняет, как использовать базу данных в памяти для тестов с помощью MSTest. В этом разделе используется платформа тестирования [xUnit](https://xunit.github.io/) . Концепции тестирования и реализации тестов в разных платформах тестирования похожи, но не идентичны.

Хотя приложение не использует шаблон репозитория и не является эффективным примером [шаблона единицы работы (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages поддерживает эти шаблоны разработки. Дополнительные сведения см. в разделе Разработка логики [уровня сохраняемости инфраструктуры](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) и [контроллера тестирования](/aspnet/core/mvc/controllers/testing) (пример реализует шаблон репозитория).

### <a name="test-app-organization"></a>Тестирование организации приложения

Тестовое приложение — это консольное приложение внутри каталога *Tests/разорпажеспрожект. Tests* .

| Каталог тестового приложения | Описание |
| ------------------ | ----------- |
| *басиктестс* | *BasicTests.CS* содержит методы теста для маршрутизации, доступа к защищенной странице непроверенного пользователя и получения профиля пользователя GitHub и проверки имени входа пользователя профиля. |
| *Интеграционноетестирование* | *IndexPageTests.CS* содержит интеграционные тесты для страницы индекса с помощью пользовательского класса `WebApplicationFactory`. |
| *Вспомогательные функции и служебные программы* | <ul><li>*Utilities.CS* содержит метод `InitializeDbForTests`, используемый для заполнения базы данных тестовыми данными.</li><li>*HtmlHelpers.CS* предоставляет метод, возвращающий англешарп `IHtmlDocument` для использования методами теста.</li><li>*HttpClientExtensions.CS* предоставляют перегрузки для `SendAsync` для отправки запросов в сут.</li></ul> |

Платформа тестирования — [xUnit](https://xunit.github.io/). Интеграционные тесты проводятся с помощью [Microsoft. AspNetCore. TestHost](/dotnet/api/microsoft.aspnetcore.testhost), который включает [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver). Поскольку пакет [Microsoft. AspNetCore. MVC.](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) testing используется для настройки узла тестирования и тестового сервера, для пакетов `TestHost` и `TestServer` не требуются прямые ссылки на пакеты в файле проекта тестового приложения или конфигурации разработчика в тесте. приложения.

**Заполнение базы данных для тестирования**

Перед выполнением тестов интеграции обычно требуется небольшой набор данных в базе данных. Например, вызов теста удаления для удаления записей базы данных, поэтому база данных должна иметь по крайней мере одну запись, чтобы запрос на удаление был выполнен успешно.

Пример приложения заполняет базу данных тремя сообщениями в *Utilities.CS* , которые могут использоваться тестами при выполнении:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

Контекст базы данных сут регистрируется в своем методе `Startup.ConfigureServices`. Обратный вызов `builder.ConfigureServices` тестового приложения выполняется *после* выполнения кода `Startup.ConfigureServices` приложения. Чтобы использовать другую базу данных для тестов, необходимо заменить контекст базы данных приложения в `builder.ConfigureServices`. Дополнительные сведения см. в разделе [Настройка вебаппликатионфактори](#customize-webapplicationfactory) .

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Интеграционные тесты гарантируют, что компоненты приложения правильно работают на уровне, который включает в себя поддерживающую инфраструктуру приложения, такую как база данных, файловая система и сеть. ASP.NET Core поддерживает интеграционные тесты с помощью платформы модульного тестирования с тестовым веб-узлом и тестовым сервером в памяти.

В этом разделе предполагается базовое понимание модульных тестов. Если вы не знакомы с концепциями тестирования, см. раздел [модульное тестирование в .NET Core и .NET Standard](/dotnet/core/testing/) разделе и связанное содержимое.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) ([как скачивать](xref:index#how-to-download-a-sample))

Пример приложения является Razor Pages приложением и предполагает базовое понимание Razor Pages. Если вы не знакомы с Razor Pages, см. следующие разделы:

* [Введение в Razor Pages](xref:razor-pages/index)
* [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Модульные тесты страниц Razor](xref:test/razor-pages-tests)

> [!NOTE]
> Для тестирования одностраничные приложения мы рекомендуем использовать средство, такое как [Selenium](https://www.seleniumhq.org/), которое может автоматизировать браузер.

## <a name="introduction-to-integration-tests"></a>Введение в интеграционные тесты

Интеграционные тесты оценивают компоненты приложения на более широком уровне, чем [модульные тесты](/dotnet/core/testing/). Модульные тесты используются для тестирования изолированных программных компонентов, например для отдельных методов класса. Тесты интеграции подтверждают, что два или несколько компонентов приложения работают вместе для получения ожидаемого результата, возможно, включая все компоненты, необходимые для полной обработки запроса.

Эти более широкие тесты используются для тестирования инфраструктуры приложения и всей платформы, часто включая следующие компоненты:

* База данных
* Файловая система
* Сетевые устройства
* Конвейер "запрос-ответ"

Модульные тесты используют готовые компоненты, известные как *фиктивные* или *Макетные объекты*вместо компонентов инфраструктуры.

В отличие от модульных тестов, интеграционные тесты:

* Используйте фактические компоненты, которые приложение использует в рабочей среде.
* Требовать больше кода и обработки данных.
* Выполнение займет больше времени.

Поэтому Ограничьте использование интеграционных тестов наиболее важными сценариями инфраструктуры. Если поведение можно проверить с помощью модульного теста или теста интеграции, выберите модульный тест.

> [!TIP]
> Не создавайте интеграционные тесты для каждого возможного перестановки данных и доступа к файлам с помощью баз данных и файловых систем. Независимо от того, сколько мест в приложении взаимодействуют с базами данных и файловыми системами, набор интеграционных тестов для чтения, записи, обновления и удаления, как правило, может обеспечить адекватное тестирование компонентов базы данных и файловой системы. Используйте модульные тесты для регулярных тестов логики метода, которые взаимодействуют с этими компонентами. В модульных тестах использование фиктивных и макетов инфраструктуры приводит к более быстрому выполнению тестов.

> [!NOTE]
> При обсуждении интеграционных тестов тестируемый проект часто называется тестируемой *системой*или "сут" для краткости.
>
> *В этом разделе используется "сут" для ссылки на проверенное приложение ASP.NET Core.*

## <a name="aspnet-core-integration-tests"></a>ASP.NET Core интеграционных тестов

Для интеграционных тестов в ASP.NET Core требуется следующее:

* Тестовый проект используется для хранения и выполнения тестов. Тестовый проект содержит ссылку на сут.
* Тестовый проект создает тестовый веб-узел для сут и использует клиент тестового сервера для обработки запросов и ответов с помощью сут.
* Средство запуска тестов используется для выполнения тестов и передачи результатов тестов.

Интеграционные тесты следуют последовательности событий, включающих стандартные шаги *упорядочения* *, действия*и *утверждения* .

1. Веб-узел сут настроен.
1. Для отправки запросов в приложение создается клиент тестового сервера.
1. Шаг " *упорядочение* теста" выполняется: тестовое приложение подготавливает запрос.
1. Выполняется *шаг теста действия* : клиент отправляет запрос и получает ответ.
1. Выполняется *проверочный шаг утверждения* : *фактический* ответ проверяется как *пройденный* или *сбой* на основе *ожидаемого* ответа.
1. Процесс продолжится до тех пор, пока не будут выполнены все тесты.
1. Выводятся результаты теста.

Обычно тестовый веб-узел настраивается иначе, чем обычный веб-узел приложения для тестовых запусков. Например, для тестов может использоваться другая база данных или другие параметры приложения.

Компоненты инфраструктуры, такие как тестовый веб-узел и сервер тестирования в памяти ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), предоставляются или управляются пакетом [Microsoft. AspNetCore. MVC. Test](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) . Использование этого пакета упрощает создание и выполнение тестов.

Пакет `Microsoft.AspNetCore.Mvc.Testing` обрабатывает следующие задачи:

* Копирует файл зависимостей ( *. Deps*) из сут в каталог *bin* тестового проекта.
* Задает [корневой каталог содержимого](xref:fundamentals/index#content-root) в корне проекта сут, чтобы при выполнении тестов были найдены статические файлы и страницы или представления.
* Предоставляет класс [вебаппликатионфактори](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) для упрощения начальной загрузки сут с `TestServer`.

В документации по [модульным тестам](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) описано, как настроить тестовый проект и средство запуска тестов, а также подробные инструкции по выполнению тестов и рекомендаций по именованию тестов и тестовых классов.

> [!NOTE]
> При создании тестового проекта для приложения разделите модульные тесты от интеграционных тестов на разные проекты. Это гарантирует, что компоненты тестирования инфраструктуры не будут случайно добавлены в модульные тесты. Разделение модульных и интеграционных тестов также позволяет контролировать, какой набор тестов выполняется.

В конфигурации для тестирования Razor Pages приложений и приложений MVC практически нет различий. Единственное отличие заключается в том, как имена тестов называются. В приложении Razor Pages тесты конечных точек страниц обычно именуются по классу модели страницы (например, `IndexPageTests`, чтобы проверить интеграцию компонентов для страницы индекса). В приложении MVC тесты, как правило, упорядочены по классам контроллеров и названы после тестирования контроллеров (например, `HomeControllerTests`, чтобы проверить интеграцию компонентов для контроллера Home).

## <a name="test-app-prerequisites"></a>Необходимые условия для тестирования приложения

Тестовый проект должен:

* Сослаться на следующие пакеты:
  * [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  * [Microsoft. AspNetCore. MVC. Test](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* Укажите веб-пакет SDK в файле проекта (`<Project Sdk="Microsoft.NET.Sdk.Web">`). Веб-пакет SDK необходим при ссылке на [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app).

Эти предварительные требования можно просмотреть в [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/). Изучите файл *Tests/разорпажеспрожект. Tests/разорпажеспрожект. Tests. csproj* . В примере приложения используется платформа тестирования [xUnit](https://xunit.github.io/) и библиотека средства синтаксического анализа [англешарп](https://anglesharp.github.io/) , поэтому пример приложения также ссылается на:

* [xUnit](https://www.nuget.org/packages/xunit/)
* [xUnit. Runner. VisualStudio](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [англешарп](https://www.nuget.org/packages/AngleSharp/)

## <a name="sut-environment"></a>Среда сут

Если [Среда](xref:fundamentals/environments) сут не задана, среда по умолчанию имеет значение Development.

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Базовые тесты с Вебаппликатионфактори по умолчанию

[Вебаппликатионфактори @ no__t-1TEntryPoint >](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) используется для создания [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) для интеграционных тестов. `TEntryPoint` является классом точки входа сут, обычно классом `Startup`.

Тестовые классы реализуют интерфейс *основы класса* ([иклассфикстуре](https://xunit.github.io/docs/shared-context#class-fixture)), чтобы указать, что класс содержит тесты, и предоставить экземпляры общего объекта между тестами в классе.

### <a name="basic-test-of-app-endpoints"></a>Базовое тестирование конечных точек приложения

В следующем тестовом классе, `BasicTests`, используется `WebApplicationFactory` для начальной загрузки сут и предоставления [HttpClient](/dotnet/api/system.net.http.httpclient) методу теста, `Get_EndpointsReturnSuccessAndCorrectContentType`. Метод проверяет успешность кода состояния отклика (коды состояний в диапазоне 200-299), а заголовок `Content-Type` — `text/html; charset=utf-8` для нескольких страниц приложений.

[Креатеклиент](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) создает экземпляр `HttpClient`, который автоматически следует за перенаправляет и обрабатывает файлы cookie.

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

По умолчанию некритические файлы cookie не сохраняются в запросах, если включена [политика согласия GDPR](xref:security/gdpr) . Чтобы сохранить ненужные файлы cookie, например те, которые используются поставщиком TempData, пометьте их как основные в тестах. Инструкции по маркировке файла cookie в качестве основы см. в разделе [основные файлы cookie](xref:security/gdpr#essential-cookies).

### <a name="test-a-secure-endpoint"></a>Тестирование безопасной конечной точки

Другой тест в классе `BasicTests` проверяет, что защищенная конечная точка перенаправляет пользователя, не прошедшего проверку подлинности, на страницу входа приложения.

В сут на странице `/SecurePage` используется соглашение [аусоризепаже](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) для применения [аусоризефилтер](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) к странице. Дополнительные сведения см. в разделе [соглашения об авторизации Razor Pages](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

В тесте `Get_SecurePageRequiresAnAuthenticatedUser` параметр [вебаппликатионфакториклиентоптионс](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) имеет значение запретить перенаправления, установив [алловауторедирект](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) в значение `false`:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

Если запретить клиенту следовать перенаправлению, можно выполнить следующие проверки:

* Код состояния, возвращаемый сут, может быть проверен на соответствие ожидаемому [HttpStatusCode. результат перенаправления](/dotnet/api/system.net.httpstatuscode) , а не окончательный код состояния после перенаправления на страницу входа, что было бы [HttpStatusCode. ОК](/dotnet/api/system.net.httpstatuscode).
* Значение заголовка `Location` в заголовках ответа проверяется, чтобы подтвердить, что начинается с `http://localhost/Identity/Account/Login`, а не от последнего ответа страницы входа, где отсутствует заголовок `Location`.

Дополнительные сведения о `WebApplicationFactoryClientOptions` см. в разделе [Параметры клиента](#client-options) .

## <a name="customize-webapplicationfactory"></a>Настройка Вебаппликатионфактори

Конфигурацию веб-узла можно создать независимо от тестовых классов, наследуя от `WebApplicationFactory`, чтобы создать одну или несколько пользовательских фабрик:

1. Наследовать от `WebApplicationFactory` и переопределить [конфигуревебхост](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost). [Ивебхостбуилдер](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) позволяет настраивать коллекцию служб с помощью [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   Заполнение базы данных в [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) выполняется методом `InitializeDbForTests`. Этот метод описан в разделе [Пример интеграционных тестов: тестирование приложения в Организации](#test-app-organization) .

2. Используйте пользовательский `CustomWebApplicationFactory` в тестовых классах. В следующем примере используется фабрика в классе `IndexPageTests`:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Клиент примера приложения настроен так, чтобы запретить `HttpClient` от следующих перенаправлений. Как описано в разделе [тестирование защищенной конечной точки](#test-a-secure-endpoint) , это позволяет тестам проверять результат первого ответа приложения. Первый ответ — это перенаправление во многих из этих тестов с заголовком `Location`.

3. Типичный тест использует `HttpClient` и вспомогательные методы для обработки запроса и ответа:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

Любой запрос POST к сут должен соответствовать проверке подделки, которая автоматически делается в [системе защиты данных в приложении с защитой от подделки](xref:security/data-protection/introduction). Чтобы упорядочить запрос POST теста, тестовое приложение должно:

1. Выполните запрос к странице.
1. Проанализируйте файл cookie подделки и запросите маркер проверки из ответа.
1. Выполните запрос POST с файлом cookie подделки и запросом маркера проверки на месте.

Методы расширения вспомогательной функции `SendAsync` (helpers */хттпклиентекстенсионс. CS*) и вспомогательный метод `GetDocumentAsync` (helpers */хтмлхелперс. CS*) в [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) используют средство синтаксического анализа [англешарп](https://anglesharp.github.io/) для обработки защиты от подделки с помощью следующие методы:

* `GetDocumentAsync` &ndash; получает [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) и возвращает `IHtmlDocument`. `GetDocumentAsync` использует фабрику, которая подготавливает *виртуальный ответ* на основе исходного `HttpResponseMessage`. Дополнительные сведения см. в [документации по англешарп](https://github.com/AngleSharp/AngleSharp#documentation).
* методы расширения `SendAsync` для `HttpClient` составляют [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) и вызывают [SendAsync (HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) для отправки запросов сут. Перегрузки для `SendAsync` принимают HTML-форму (`IHtmlFormElement`) и следующие элементы:
  * Кнопка "Отправить" в форме (`IHtmlElement`)
  * Коллекция значений форм (`IEnumerable<KeyValuePair<string, string>>`)
  * Кнопка "Отправить" (`IHtmlElement`) и значения формы (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [Англешарп](https://anglesharp.github.io/) — это библиотека анализа сторонних производителей, используемая для демонстрационных целей в этом разделе и в примере приложения. Англешарп не поддерживается или не требуется для интеграционного тестирования ASP.NET Core приложений. Можно использовать другие средства синтаксического анализа, такие как [пакет динамичности HTML (хап)](https://html-agility-pack.net/). Другой подход заключается в написании кода для непосредственной работы с маркером проверки запроса системы защиты от подделки и с помощью файла cookie подделки.

## <a name="customize-the-client-with-withwebhostbuilder"></a>Настройка клиента с помощью Висвебхостбуилдер

Если в методе теста требуется дополнительная настройка, [висвебхостбуилдер](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) создает новый `WebApplicationFactory` с [ивебхостбуилдер](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) , который еще настраивается конфигурацией.

Метод теста `Post_DeleteMessageHandler_ReturnsRedirectToRoot` [примера приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) демонстрирует использование `WithWebHostBuilder`. Этот тест выполняет запись удаления в базе данных, активируя отправку формы в сут.

Поскольку другой тест в классе `IndexPageTests` выполняет операцию, которая удаляет все записи в базе данных и может выполняться перед методом `Post_DeleteMessageHandler_ReturnsRedirectToRoot`, база данных повторно заполняется в этом методе теста, чтобы обеспечить наличие записи для удаления сут. Выбор первой кнопки удаления формы `messages` в сут имитируется в запросе к сут:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>Параметры клиента

В следующей таблице показаны [вебаппликатионфакториклиентоптионс](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) по умолчанию, доступные при создании экземпляров `HttpClient`.

| Параметр | Описание | Значение по умолчанию |
| ------ | ----------- | ------- |
| [алловауторедирект](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Возвращает или задает значение, указывающее, должны ли экземпляры `HttpClient` автоматически следовать ответам перенаправления. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Возвращает или задает базовый адрес экземпляров `HttpClient`. | `http://localhost` |
| [хандлекукиес](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Возвращает или задает значение, указывающее, должны ли экземпляры `HttpClient` управлять файлами cookie. | `true` |
| [максаутоматикредиректионс](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Возвращает или задает максимальное число ответов на перенаправление, которым должны следовать экземпляры `HttpClient`. | 7 |

Создайте класс `WebApplicationFactoryClientOptions` и передайте его в метод [креатеклиент](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) (значения по умолчанию показаны в примере кода):

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a>Вставка служб макетирования

Службы могут быть переопределены в тесте с помощью вызова [конфигуретестсервицес](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) в построителе узлов. **Чтобы внедрить службы макетирования, сут должен иметь класс `Startup` с методом `Startup.ConfigureServices`.**

Пример сут включает службу с заданной областью, которая возвращает кавычки. Цитата внедряется в скрытое поле на странице индекса при запросе страницы индекса.

*Services/IQuoteService. CS*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

*Services/QuoteService. CS*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

*Pages/Index.cshtml.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

*Pages/index. CS*:

[!code-cshtml[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

При запуске приложения сут создается следующая разметка:

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

Чтобы протестировать службу и внедрение цитат в интеграционный тест, служба макетирования будет внедрена в сут тестом. Макет службы заменяет @no__t (0) приложения на службу, предоставляемую тестовым приложением, с именем `TestQuoteService`:

*IntegrationTests.IndexPageTests.CS*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

вызывается `ConfigureTestServices`, и служба с заданной областью зарегистрирована:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

Разметка, созданная во время выполнения теста, отражает текст цитаты, предоставленный `TestQuoteService`, поэтому утверждение передается следующим образом:

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Как инфраструктура тестирования определяет корневой путь к содержимому приложения

Конструктор `WebApplicationFactory` определяет корневой путь к [содержимому](xref:fundamentals/index#content-root) приложения, выполняя поиск [вебаппликатионфакториконтентрутаттрибуте](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) в сборке, содержащей тесты интеграции, с ключом, равным сборке `TEntryPoint` `System.Reflection.Assembly.FullName`. Если атрибут с правильным ключом не найден, `WebApplicationFactory` возвращается к поиску файла решения (*SLN*) и добавляет имя сборки `TEntryPoint` в каталог решения. Корневой каталог приложения (корневой путь к содержимому) используется для обнаружения представлений и файлов содержимого.

## <a name="disable-shadow-copying"></a>Отключение теневого копирования

Теневое копирование приводит к тому, что тесты выполняются в каталоге, отличном от каталога выходного каталога. Для правильной работы тестов необходимо отключить теневое копирование. [Пример приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) использует xUnit и отключает теневое копирование для xUnit, включая файл *xUnit. Runner. JSON* с правильным параметром конфигурации. Дополнительные сведения см. в разделе [Настройка xUnit с помощью JSON](https://xunit.github.io/docs/configuring-with-json.html).

Добавьте файл *xUnit. Runner. JSON* в корень тестового проекта со следующим содержимым:

```json
{
  "shadowCopy": false
}
```

При использовании Visual Studio задайте для свойства **Копировать в выходной каталог** файла значение **всегда копировать**. Если вы не используете Visual Studio, добавьте целевой объект `Content` в файл проекта тестового приложения:

```xml
<ItemGroup>
  <Content Update="xunit.runner.json">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
  </Content>
</ItemGroup>
```

## <a name="disposal-of-objects"></a>Удаление объектов

После выполнения тестов реализации `IClassFixture` [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) и [HttpClient](/dotnet/api/system.net.http.httpclient) удаляются, когда xUnit удаляет [вебаппликатионфактори](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1). Если для объектов, создаваемых разработчиком, требуется реализация, удалите их в реализации `IClassFixture`. Дополнительные сведения см. [в разделе Реализация метода Dispose](/dotnet/standard/garbage-collection/implementing-dispose).

## <a name="integration-tests-sample"></a>Пример интеграционных тестов

[Пример приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) состоит из двух приложений:

| Приложение | Каталог проекта | Описание |
| --- | ----------------- | ----------- |
| Приложение сообщений (сут) | *src/Разорпажеспрожект* | Позволяет пользователю добавлять, удалять, удалять все и анализировать сообщения. |
| Тестовое приложение | *Tests/Разорпажеспрожект. Tests* | Используется для интеграции тестирования сут. |

Тесты можно выполнять с помощью встроенных функций тестирования интегрированной среды разработки, например [Visual Studio](https://visualstudio.microsoft.com). При использовании [Visual Studio Code](https://code.visualstudio.com/) или командной строки выполните следующую команду в командной строке в каталоге *Tests/разорпажеспрожект. Tests* :

```dotnetcli
dotnet test
```

### <a name="message-app-sut-organization"></a>Организация приложения для сообщений (сут)

СУТ — это Razor Pages система сообщений со следующими характеристиками:

* Страница индекса приложения (*pages/index. cshtml* и *pages/index. cshtml. CS*) предоставляет методы пользовательского интерфейса и модели страницы для управления добавлением, удалением и анализом сообщений (среднее число слов на сообщение).
* Сообщение описывается классом `Message` (*Data/Message. CS*) с двумя свойствами: `Id` (ключ) и `Text` (сообщение). Свойство `Text` является обязательным и ограничено 200 символами.
* Сообщения хранятся с&#8224;помощью [Entity Framework базы данных в памяти](/ef/core/providers/in-memory/).
* Приложение содержит уровень доступа к данным (DAL) в своем классе контекста базы данных `AppDbContext` (*Data/аппдбконтекст. CS*).
* Если база данных пуста при запуске приложения, то хранилище сообщений инициализируется тремя сообщениями.
* Приложение включает `/SecurePage`, доступ к которому может получить только пользователь, прошедший проверку подлинности.

&#8224;Раздел EF, [тест с использованием памяти](/ef/core/miscellaneous/testing/in-memory), объясняет, как использовать базу данных в памяти для тестов с помощью MSTest. В этом разделе используется платформа тестирования [xUnit](https://xunit.github.io/) . Концепции тестирования и реализации тестов в разных платформах тестирования похожи, но не идентичны.

Хотя приложение не использует шаблон репозитория и не является эффективным примером [шаблона единицы работы (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages поддерживает эти шаблоны разработки. Дополнительные сведения см. в разделе Разработка логики [уровня сохраняемости инфраструктуры](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) и [контроллера тестирования](/aspnet/core/mvc/controllers/testing) (пример реализует шаблон репозитория).

### <a name="test-app-organization"></a>Тестирование организации приложения

Тестовое приложение — это консольное приложение внутри каталога *Tests/разорпажеспрожект. Tests* .

| Каталог тестового приложения | Описание |
| ------------------ | ----------- |
| *басиктестс* | *BasicTests.CS* содержит методы теста для маршрутизации, доступа к защищенной странице непроверенного пользователя и получения профиля пользователя GitHub и проверки имени входа пользователя профиля. |
| *Интеграционноетестирование* | *IndexPageTests.CS* содержит интеграционные тесты для страницы индекса с помощью пользовательского класса `WebApplicationFactory`. |
| *Вспомогательные функции и служебные программы* | <ul><li>*Utilities.CS* содержит метод `InitializeDbForTests`, используемый для заполнения базы данных тестовыми данными.</li><li>*HtmlHelpers.CS* предоставляет метод, возвращающий англешарп `IHtmlDocument` для использования методами теста.</li><li>*HttpClientExtensions.CS* предоставляют перегрузки для `SendAsync` для отправки запросов в сут.</li></ul> |

Платформа тестирования — [xUnit](https://xunit.github.io/). Интеграционные тесты проводятся с помощью [Microsoft. AspNetCore. TestHost](/dotnet/api/microsoft.aspnetcore.testhost), который включает [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver). Поскольку пакет [Microsoft. AspNetCore. MVC.](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) testing используется для настройки узла тестирования и тестового сервера, для пакетов `TestHost` и `TestServer` не требуются прямые ссылки на пакеты в файле проекта тестового приложения или конфигурации разработчика в тесте. приложения.

**Заполнение базы данных для тестирования**

Перед выполнением тестов интеграции обычно требуется небольшой набор данных в базе данных. Например, вызов теста удаления для удаления записей базы данных, поэтому база данных должна иметь по крайней мере одну запись, чтобы запрос на удаление был выполнен успешно.

Пример приложения заполняет базу данных тремя сообщениями в *Utilities.CS* , которые могут использоваться тестами при выполнении:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

::: moniker-end

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Модульные тесты](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Модульные тесты страниц Razor](xref:test/razor-pages-tests)
* [ПО промежуточного слоя](xref:fundamentals/middleware/index)
* [Контроллеры тестирования](xref:mvc/controllers/testing)
