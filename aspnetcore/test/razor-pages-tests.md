---
title: Razor страниц в модульные тесты ASP.NET Core
author: guardrex
description: Описание способов создания модульных тестов для приложений страниц Razor.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: test/razor-pages-tests
ms.openlocfilehash: 460c35754750691d3d940dac04d06823083133c2
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2018
ms.locfileid: "35217698"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a>Razor страниц в модульные тесты ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

ASP.NET Core поддерживает модульные тесты приложений страниц Razor. Тесты данных доступа к уровню (DAL) и обеспечить модели страниц:

* Часть приложения страниц Razor работают независимо друг от друга и вместе как единое целое во время построения приложения.
* Классы и методы ограничены областей ответственности.
* Дополнительную документацию существует на поведение приложения.
* Регрессии, которые являются ошибками, приводящими изменения в код, найденные во время автоматического построения и развертывания.

В этом разделе предполагается наличие основные приложения страниц Razor и модульные тесты. Если вы знакомы со страниц Razor приложений или тестирования концепции, см. в следующих разделах:

* [Введение в Razor Pages](xref:mvc/razor-pages/index)
* [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Модульное тестирование C# в .NET Core, используя dotnet теста и xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tests/razor-pages-tests/samples/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

Пример проекта состоит из двух приложений:

| Приложение         | Папка проекта                        | Описание: |
| ----------- | ------------------------------------- | ----------- |
| Сообщение приложения | *src/RazorPagesTestSample*            | Позволяет пользователю добавить, удалить один, удалите все и проанализировать сообщения. |
| Тестовое приложение    | *tests/RazorPagesTestSample.Tests*    | Используется для модульного теста приложения сообщения: доступ к данным, слой (DAL) и модель страницы индекса. |

Тесты можно выполнять с помощью встроенных тестов функций интегрированной среды разработки, такие как [Visual Studio](https://www.visualstudio.com/vs/). При использовании [кода Visual Studio](https://code.visualstudio.com/) или из командной строки, выполните следующую команду в командной строке в *tests/RazorPagesTestSample.Tests* папки:

```console
dotnet test
```

## <a name="message-app-organization"></a>Сообщение приложения организации

Сообщение приложения имеет простую систему страниц Razor сообщение со следующими характеристиками:

* Страница индекса приложения (*Pages/Index.cshtml* и *Pages/Index.cshtml.cs*) предоставляет пользовательский Интерфейс и страницы модели методы для управления Добавление, удаление и анализа сообщений (среднее слов в сообщение) .
* Описывается сообщение `Message` класса (*Data/Message.cs*) с двумя свойствами: `Id` (ключ) и `Text` (сообщение). `Text` Свойство становится необходимым и ограничением в 200 символов.
* Сообщения хранятся с использованием [базы данных в памяти Entity Framework](/ef/core/providers/in-memory/)&#8224;.
* Приложение содержит слой доступа к данным (DAL) в своем классе контекст базы данных, `AppDbContext` (*Data/AppDbContext.cs*). Эти методы DAL отмечены `virtual`, что позволяет макетирование методы для использования в тестах.
* Если база данных пуста при запуске приложения, три сообщения инициализирован хранилище сообщений. Эти *заполнена сообщения* также используются в тестах.

&#8224;В разделе EF [теста с InMemory](/ef/core/miscellaneous/testing/in-memory), объясняется, как использовать базу данных в памяти для тесты с MSTest. В этом разделе используется [xUnit](https://xunit.github.io/) платформы тестирования. Основные понятия тест и тест реализации различных тестовых платформ похожие, но не идентичен.

Несмотря на то, что приложение не использует [шаблон репозитория](http://martinfowler.com/eaaCatalog/repository.html) и не действующие примером [единицы работы (UoW) шаблон](https://martinfowler.com/eaaCatalog/unitOfWork.html), эти шаблоны разработки поддерживает страниц Razor. Дополнительные сведения см. в разделе [проектирование уровень сохраняемости инфраструктуры](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [реализации репозитория и шаблоны единицы работы в приложении MVC ASP.NET](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), и [контроллера тестирования логика](/aspnet/core/mvc/controllers/testing) (в этом образце реализуется шаблон репозитория).

## <a name="test-app-organization"></a>Тестирование приложения организации

Приложение тестирования является консоль внутри *tests/RazorPagesTestSample.Tests* папки.

| Папка тестового приложения | Описание: |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs* содержит модульные тесты для DAL.</li><li>*IndexPageTests.cs* содержит модульные тесты для модели страницы индекса.</li></ul> |
| *Служебные программы*     | Содержит `TestingDbContextOptions` метод, используемый для создания новой базы данных, параметры контекста для каждого модульного теста DAL, чтобы база данных будет переведена в состояние базового плана для каждого теста. |

Платформа тестирования — [xUnit](https://xunit.github.io/). Объект имитации framework [заказа](https://github.com/moq/moq4).

## <a name="unit-tests-of-the-data-access-layer-dal"></a>Модульные тесты данных доступа к уровню (DAL)

Сообщение приложения содержит DAL с четырьмя методы, содержащиеся в `AppDbContext` класса (*src/RazorPagesTestSample/Data/AppDbContext.cs*). У каждого метода есть один или два модульных тестов в тестовом приложении.

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

Проблема этого подхода является то, что каждый тест получает базы данных в то состояние, предыдущий тест остался. Это может быть проблематично, при попытке создать atomic модульные тесты, пересекаются друг с другом. Чтобы принудительно `AppDbContext` для нового контекста базы данных для каждого теста, предоставить `DbContextOptions` экземпляр, который основан на нового поставщика услуг. Тестовое приложение показано, как это сделать с помощью его `Utilities` методу класса `TestingDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

С помощью `DbContextOptions` DAL модульных тестов позволяет каждый тест, атомарным образом с экземпляром свежей базы данных:

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

Например `DeleteMessageAsync` метод отвечает за удаление одного сообщения, определяемый его `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

Существует два теста для этого метода. Один тест проверяет, что метод удаляет сообщение, если сообщение находится в базе данных. Другой метод тесты, которые база данных не изменится, если сообщение `Id` для удаления не существует. `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` Метода показан ниже:

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Во-первых этот метод выполняет шаг расположение, где выполняется подготовка для шага Act. Заполнения сообщения, полученные и содержатся в `seedMessages`. Заполнения сообщения сохраняются в базе данных. Сообщение с `Id` из `1` задано для удаления. Когда `DeleteMessageAsync` метод выполняется, и ожидаемые сообщения должны иметь все сообщения, за исключением с `Id` из `1`. `expectedMessages` Переменная представляет это ожидаемый результат.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Метод: `DeleteMessageAsync` метод выполняется передача в `recId` из `1`:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Наконец, метод получает `Messages` из контекста и сравнивает его `expectedMessages` подтверждение, что они равны:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Для сравнения, два `List<Message>` совпадают:

* Сообщения, упорядоченные по `Id`.
* Пар сообщений сравниваются на `Text` свойство.

Аналогичный метод теста, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` проверяет результат попытки удаления сообщения, не существует. В этом случае и ожидаемые сообщения в базе данных должно быть равно сами сообщения после `DeleteMessageAsync` выполнения метода. Должно быть никаких изменений базы данных содержимого:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>Модульные тесты из методов модели страницы

Другой набор модульных тестов отвечает за тесты методов модели страницы. В приложении сообщение модели страниц индекса находятся в `IndexModel` класса в *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.

| Метод модели страницы | Функция |
| ----------------- | -------- |
| `OnGetAsync` | Получает сообщения из DAL для пользовательского интерфейса с помощью `GetMessagesAsync` метод. |
| `OnPostAddMessageAsync` | Если `ModelState` является допустимым, вызовы `AddMessageAsync` на добавление сообщений в базу данных. |
| `OnPostDeleteAllMessagesAsync` | Вызовы `DeleteAllMessagesAsync` для удаления всех сообщений в базе данных. |
| `OnPostDeleteMessageAsync` | Выполняет `DeleteMessageAsync` для удаления сообщения с `Id` указанного. |
| `OnPostAnalyzeMessagesAsync` | Если одно или несколько сообщений в базе данных, вычисляет среднее количество слов в сообщение. |

Методы модели страницы сравниваются с использованием семь тесты в `IndexPageTests` класса (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*). Тесты можно использовать знакомые шаблон Assert Act расположение. Эти тесты сосредоточены на:

* Определение методов выполните правильное поведение при `ModelState` является недопустимым.
* Подтверждение методы создания правильного `IActionResult`.
* Проверка, присвоения значений свойства выполняются правильно.

Эта группа тестов часто макета методы DAL для получения ожидаемых данных, для которой выполняется метод модели страницы шага Act. Например `GetMessagesAsync` метод `AppDbContext` макетирование для вывода. При выполнении этого метода метод модели страницы макетов возвращает результат. Данные не поступают из базы данных. При этом создается условий теста прогнозируемый, надежные использование DAL в тестах модели страницы.

`OnGetAsync_PopulatesThePageModel_WithAListOfMessages` Тестирования показано как `GetMessagesAsync` метод является макетирование для модели страницы:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

Когда `OnGetAsync` метод выполняется в действии Act, он вызывает модель страницы `GetMessagesAsync` метод.

Модульный тест шаг Act (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage` модель страницы `OnGetAsync` метод (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

`GetMessagesAsync` Метод в DAL не возвращает результат для вызова этого метода. Макеты версия метода возвращает результат.

В `Assert` шаг, сами сообщения (`actualMessages`) назначаются из `Messages` модели страницы. При назначении сообщения также выполняется проверка типа. Используются для сравнения сообщений ожидаемых и фактических их `Text` свойства. Тест подтверждает, что два `List<Message>` экземпляры содержат те же сообщения.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

Других тестов в этой группе Создать страницу объекты модели, включающие `DefaultHttpContext`, `ModelStateDictionary`, `ActionContext` для установления `PageContext`, `ViewDataDictionary`и `PageContext`. Это полезно при проведении тестов. Например, сообщение приложение устанавливает `ModelState` ошибки с `AddModelError` чтобы убедиться, что является допустимым `PageResult` возвращается, когда `OnPostAddMessageAsync` выполняется:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Модульное тестирование C# в .NET Core, используя dotnet теста и xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Контроллеры тестирования](xref:mvc/controllers/testing)
* [Модульный тест кода](/visualstudio/test/unit-test-your-code) (Visual Studio)
* [Интеграционные тесты](xref:test/integration-tests)
* [xUnit.net](https://xunit.github.io/)
* [Приступая к работе с xUnit.net (Core/ASP.NET .NET Core)](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Заказа](https://github.com/moq/moq4)
* [Краткое руководство заказа](https://github.com/Moq/moq4/wiki/Quickstart)
