---
title: "Единицы страниц Razor и тестирование в ASP.NET Core интеграции"
author: guardrex
description: "Описание способов создания модульных тестов и интеграции тестов для приложений страниц Razor."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 11/27/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: testing/razor-pages-testing
ms.openlocfilehash: 1ecdf010f7c283a0a08b224d570a5bc5cdf536df
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/03/2018
---
# <a name="razor-pages-unit-and-integration-testing-in-aspnet-core"></a>Единицы страниц Razor и тестирование в ASP.NET Core интеграции

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

ASP.NET Core поддерживает модульных тестов и интеграционного тестирования приложений страниц Razor. Тестирование доступа к данным (DAL), модели страниц и компонентов интеграции страницы обеспечивает:

* Часть приложения страниц Razor работают независимо друг от друга и вместе как единое целое во время построения приложения.
* Классы и методы ограничены областей ответственности.
* Дополнительную документацию существует на поведение приложения.
* Регрессии, которые являются ошибками, приводящими изменения в код, найденные во время автоматического построения и развертывания.

В этом разделе предполагается, что основные приложения страниц Razor, модульного тестирования и интеграции тестирования. Если вы знакомы со страниц Razor приложений или проверки концепции, см. в следующих разделах:

* [Введение в Razor Pages](xref:mvc/razor-pages/index)
* [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Модульное тестирование C# в .NET Core, используя dotnet теста и xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Тестирование интеграции](xref:testing/integration-testing)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/razor-pages-testing/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

Пример проекта состоит из двух приложений:

| Приложение         | Папка проекта                        | Описание: |
| ----------- | ------------------------------------- | ----------- |
| Сообщение приложения | *src/RazorPagesTestingSample*         | Позволяет пользователю добавить, удалить один, удалите все и проанализировать сообщения. |
| Тестовое приложение    | *tests/RazorPagesTestingSample.Tests* | Используется для проверки сообщений приложения.<ul><li>Модульные тесты: уровень доступа к данным (DAL), модель страницы индекса</li><li>Интеграционные тесты: страница индекса</li></ul> |

Тесты можно выполнять с помощью встроенной функции интегрированной среды разработки, тестирования, например [Visual Studio](https://www.visualstudio.com/vs/). При использовании [кода Visual Studio](https://code.visualstudio.com/) или из командной строки, выполните следующую команду в командной строке в *tests/RazorPagesTestingSample.Tests* папки:

```console
dotnet test
```

## <a name="message-app-organization"></a>Сообщение приложения организации

Сообщение приложения имеет простую систему страниц Razor сообщение со следующими характеристиками:

* Страница индекса приложения (*Pages/Index.cshtml* и *Pages/Index.cshtml.cs*) предоставляет пользовательский Интерфейс и страницы модели методы для управления Добавление, удаление и анализа сообщений (среднее слов в сообщение) .
* Описывается сообщение `Message` класса (*Data/Message.cs*) с двумя свойствами: `Id` (ключ) и `Text` (сообщение). `Text` Свойство становится необходимым и ограничением в 200 символов.
* Сообщения хранятся с использованием [базы данных в памяти Entity Framework](/ef/core/providers/in-memory/)&#8224;.
* Приложение содержит слой доступа к данным (DAL) в своем классе контекст базы данных, `AppDbContext` (*Data/AppDbContext.cs*). Эти методы DAL отмечены `virtual`, что позволяет макетирование методы для использования в тестах.
* Если база данных пуста при запуске приложения, три сообщения инициализирован хранилище сообщений. Эти *заполнена сообщения* также используются при тестировании.

&#8224; В разделе EF [тестирование с помощью InMemory](/ef/core/miscellaneous/testing/in-memory), объясняется, как использовать базу данных в памяти для тестирования MSTest. В этом разделе используется [xUnit](https://xunit.github.io/) модульного тестирования. Тестирование основные понятия и реализации теста для различных платформ тестирования похожие, но не идентичен.

Несмотря на то, что приложение не использует [шаблон репозитория](http://martinfowler.com/eaaCatalog/repository.html) и не действующие примером [единицы работы (UoW) шаблон](https://martinfowler.com/eaaCatalog/unitOfWork.html), эти шаблоны разработки поддерживает страниц Razor. Дополнительные сведения см. в разделе [проектирование уровень сохраняемости инфраструктуры](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [реализации репозитория и шаблоны единицы работы в приложении MVC ASP.NET](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), и [тестирования Логика контроллера](/aspnet/core/mvc/controllers/testing) (в этом образце реализуется шаблон репозитория).

## <a name="test-app-organization"></a>Тестирование приложения организации

Приложение тестирования является консоль внутри *tests/RazorPagesTestingSample.Tests* папки:

| Папка тестового приложения    | Описание: |
| ------------------ | ----------- |
| *IntegrationTests* | <ul><li>*IndexPageTest.cs* содержит тесты интеграции для страницы индекса.</li><li>*TestFixture.cs* создает узел теста для проверки сообщений приложения.</li></ul> |
| *UnitTests*        | <ul><li>*DataAccessLayerTest.cs* содержит модульные тесты для DAL.</li><li>*IndexPageTest.cs* содержит модульные тесты для модели страницы индекса.</li></ul> |
| *Служебные программы*        | *Utilities.cs* содержит:<ul><li>`TestingDbContextOptions`метод, используемый для создания новой базы данных, параметры контекста для каждого модульного теста DAL, чтобы база данных будет переведена в состояние базового плана для каждого теста.</li><li>`GetRequestContentAsync`метод, используемый для подготовки `HttpClient` и содержимого для запросов, отправленных сообщений приложение во время тестирования интеграции.</li></ul>

Платформа тестирования — [xUnit](https://xunit.github.io/). Объект имитации framework [заказа](https://github.com/moq/moq4). Интеграционные тесты выполняются с помощью [тестового узла ASP.NET Core](xref:testing/integration-testing#the-test-host).

## <a name="unit-testing-the-data-access-layer-dal"></a>Модульное тестирование уровня доступа к данным (DAL)

Сообщение приложения содержит DAL с четырьмя методы, содержащиеся в `AppDbContext` класса (*src/RazorPagesTestingSample/Data/AppDbContext.cs*). У каждого метода есть один или два модульных тестов в тестовом приложении.

| Метод DAL               | Функция                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Получает `List<Message>` из базы данных, отсортированных по `Text` свойство. |
| `AddMessageAsync`        | Добавляет `Message` в базу данных.                                          |
| `DeleteAllMessagesAsync` | Удаляет все `Message` записей из базы данных.                           |
| `DeleteMessageAsync`     | Удаляет один `Message` из базы данных по `Id`.                      |

Модульные тесты DAL требуют [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) при создании нового `AppDbContext` для каждого теста. Один подход к созданию `DbContextOptions` для каждого теста является использование [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

Проблема этого подхода является то, что каждый тест получает базы данных в то состояние, предыдущий тест остался. Это может быть проблематично, при попытке создать atomic модульные тесты, пересекаются друг с другом. Чтобы принудительно `AppDbContext` для нового контекста базы данных для каждого теста, предоставить `DbContextOptions` экземпляр, который основан на нового поставщика услуг. Тестовое приложение показано, как это сделать с помощью его `Utilities` методу класса `TestingDbContextOptions` (*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs?name=snippet1)]

С помощью `DbContextOptions` DAL модульных тестов позволяет каждый тест, атомарным образом с помощью экземпляра свежей базы данных:

```csharp
using (var db = new AppDbContext(Utilities.TestingDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Каждый метод теста в `DataAccessLayerTest` класса (*UnitTests/DataAccessLayerTest.cs*) следует той же модели, расположение Act утверждение:

1. Упорядочить: База данных настроена для проверки и/или определенных ожидаемый результат.
1. ACT: Выполняется тест.
1. Assert: Утверждения выполняются для определения успешного результата теста.

Например `DeleteMessageAsync` метод отвечает за удаление одного сообщения, определяемый его `Id` (*src/RazorPagesTestingSample/Data/AppDbContext.cs*):

[!code-csharp[Main](razor-pages-testing/sample/src/RazorPagesTestingSample/Data/AppDbContext.cs?name=snippet4)]

Существует два теста для этого метода. Один тест проверяет, что метод удаляет сообщение, если сообщение находится в базе данных. Другой метод тесты, которые база данных не изменится, если сообщение `Id` для удаления не существует. `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` Метода показан ниже:

[!code-csharp[Main](razor-pages-testing/sample_snapshot/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Во-первых этот метод выполняет шаг расположение, где выполняется подготовка для шага Act. Заполнения сообщения, полученные и содержатся в `seedMessages`. Заполнения сообщения сохраняются в базе данных. Сообщение с `Id` из `1` задано для удаления. Когда `DeleteMessageAsync` метод выполняется, и ожидаемые сообщения должны иметь все сообщения, за исключением с `Id` из `1`. `expectedMessages` Переменная представляет это ожидаемый результат.

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Метод: `DeleteMessageAsync` метод выполняется передача в `recId` из `1`:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Наконец, метод получает `Messages` из контекста и сравнивает его `expectedMessages` подтверждение, что они равны:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Для сравнения, два `List<Message>` совпадают:

* Сообщения, упорядоченные по `Id`.
* Пар сообщений сравниваются на `Text` свойство.

Аналогичный метод теста, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` проверяет результат попытки удаления сообщения, не существует. В этом случае и ожидаемые сообщения в базе данных должно быть равно сами сообщения после `DeleteMessageAsync` выполнения метода. Должно быть никаких изменений базы данных содержимого:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-testing-the-page-model-methods"></a>Модульное тестирование методы модели страницы

Другой набор модульных тестов несет ответственность за тестирование методы модели страницы. В приложении сообщение модели страниц индекса находятся в `IndexModel` класса в *src/RazorPagesTestingSample/Pages/Index.cshtml.cs*.

| Метод модели страницы | Функция |
| ----------------- | -------- | 
| `OnGetAsync` | Получает сообщения из DAL для пользовательского интерфейса с помощью `GetMessagesAsync` метод. |
| `OnPostAddMessageAsync` | Если `ModelState` является допустимым, вызовы `AddMessageAsync` на добавление сообщений в базу данных. | 
| `OnPostDeleteAllMessagesAsync` | Вызовы `DeleteAllMessagesAsync` для удаления всех сообщений в базе данных. |
| `OnPostDeleteMessageAsync` | Выполняет `DeleteMessageAsync` для удаления сообщения с `Id` указанного. |
| `OnPostAnalyzeMessagesAsync` | Если одно или несколько сообщений в базе данных, вычисляет среднее количество слов в сообщение. |

Методы модели страницы сравниваются с использованием семь тесты в `IndexPageTest` класса (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*). Тесты можно использовать знакомые шаблон Assert Act расположение. Эти тесты сосредоточены на:

* Определение методов выполните правильное поведение при `ModelState` является недопустимым.
* Подтверждение методы создания правильного `IActionResult`.
* Проверка, присвоения значений свойства выполняются правильно.

Эта группа тестов часто макета методы DAL для получения ожидаемых данных, для которой выполняется метод модели страницы шага Act. Например `GetMessagesAsync` метод `AppDbContext` макетирование для вывода. При выполнении этого метода метод модели страницы макетов возвращает результат. Данные не поступают из базы данных. При этом создается условий теста прогнозируемый, надежные использование DAL в тестах модели страницы.

`OnGetAsync_PopulatesThePageModel_WithAListOfMessages` Тестирования показано как `GetMessagesAsync` метод является макетирование для модели страницы:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet1&highlight=3-4)]

Когда `OnGetAsync` метод выполняется в действии Act, он вызывает модель страницы `GetMessagesAsync` метод.

Модульный тест шаг Act (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet2)]

`IndexPage`модель страницы `OnGetAsync` метод (*src/RazorPagesTestingSample/Pages/Index.cshtml.cs*):

[!code-csharp[Main](razor-pages-testing/sample/src/RazorPagesTestingSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

`GetMessagesAsync` Метод в DAL не возвращает результат для вызова этого метода. Макеты версия метода возвращает результат.

В `Assert` шаг, сами сообщения (`actualMessages`) назначаются из `Messages` модели страницы. При назначении сообщения также выполняется проверка типа. Используются для сравнения сообщений ожидаемых и фактических их `Text` свойства. Тест подтверждает, что два `List<Message>` экземпляры содержат те же сообщения.

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet3)]

Других тестов в этой группе Создать страницу объекты модели, включающие `DefaultHttpContext`, `ModelStateDictionary`, `ActionContext` для установления `PageContext`, `ViewDataDictionary`и `PageContext`. Это полезно при проведении тестов. Например, сообщение приложение устанавливает `ModelState` ошибки с `AddModelError` чтобы убедиться, что является допустимым `PageResult` возвращается, когда `OnPostAddMessageAsync` выполняется:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="integration-testing-the-app"></a>Тестирование приложения интеграции

Интеграция проверяет сосредоточиться на тестировании, совместная работа компонентов приложения. Интеграционные тесты выполняются с помощью [тестового узла ASP.NET Core](xref:testing/integration-testing#the-test-host). Тестируется полный запрос ответ жизненный цикл обработки. Эти тесты утверждать, что страницы выводятся верного кода состояния и `Location` заголовка, если задать.

Тестирование пример из примера интеграции проверяет результат запроса страницы индекса сообщения приложения (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet1)]

Нет этап не расположение. `GetAsync` Метод будет вызван на `HttpClient` для отправки запроса GET к конечной точке. Тест подтверждает, что результат будет код состояния 200 OK.

Любой запрос POST на сообщение приложения должны удовлетворять сложные проверки, который выполняется автоматически в приложении [сложные системы защиты данных](xref:security/data-protection/introduction). Чтобы упорядочить для запроса POST теста необходимо тестового приложения:

1. Сделать запрос для страницы.
1. Анализировать сложные куки-файл и маркер проверки запроса из ответа.
1. Сделать запрос POST сложные куки-файл и запрос проверки подлинности маркеров в месте.

`Post_AddMessageHandler_ReturnsRedirectToRoot` Метод теста:

* Подготавливает сообщение и `HttpClient`.
* Отправляет запрос POST в приложение.
* Проверяет ответ перенаправление обратно на страницу индекса.

`Post_AddMessageHandler_ReturnsRedirectToRoot `метод (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet2)]

`GetRequestContentAsync` Вспомогательный метод управляет Подготовка клиента с сложные куки-файл и маркер проверки запроса. Обратите внимание на то, как метод получает `IDictionary` , позволяющий вызывающего метода теста для передачи данных для запроса для кодирования, а также маркер проверки запроса (*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs?name=snippet2&highlight=1-2,8-9,29)]

Интеграционные тесты можно также передать неверные данные приложения для тестирования поведения ответа приложения. Сообщение приложения ограничивает длину сообщения до 200 символов (*src/RazorPagesTestingSample/Data/Message.cs*):

[!code-csharp[Main](razor-pages-testing/sample/src/RazorPagesTestingSample/Data/Message.cs?name=snippet1&highlight=7)]

`Post_AddMessageHandler_ReturnsSuccess_WhenMessageTextTooLong` Тестирования `Message` явно передает в текста с символами 201 «X». В результате `ModelState` ошибки. POST не выполняется перенаправление на страницу индекса. Он возвращает 200 ОК с `null` `Location` заголовок (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet3&highlight=7,16-17)]

## <a name="see-also"></a>См. также

* [Модульное тестирование C# в .NET Core, используя dotnet теста и xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Тестирование интеграции](xref:testing/integration-testing)
* [Контроллеры тестирования](xref:mvc/controllers/testing)
* [Модульный тест кода](/visualstudio/test/unit-test-your-code) (Visual Studio)
* [xUnit.net](https://xunit.github.io/)
* [Приступая к работе с xUnit.net (Core/ASP.NET .NET Core)](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Заказа](https://github.com/moq/moq4)
* [Краткое руководство заказа](https://github.com/Moq/moq4/wiki/Quickstart)
