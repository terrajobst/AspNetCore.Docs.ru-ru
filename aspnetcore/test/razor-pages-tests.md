---
title: Razor Pages модульных тестов в ASP.NET Core
author: guardrex
description: Узнайте, как создавать модульные тесты для Razor Pages приложений.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2019
uid: test/razor-pages-tests
ms.openlocfilehash: 35feb5dd95fa79ceca7ff03523cef30d29ccbdd3
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022575"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a>Razor Pages модульных тестов в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core поддерживает модульные тесты Razor Pages приложений. Тесты уровня доступа к данным (DAL) и модели страниц помогают обеспечить следующее:

* Части приложения Razor Pages работают независимо друг от друга и вместе как единое целое во время создания приложения.
* Классы и методы имеют ограниченные области ответственности.
* Существует дополнительная документация по ведению приложения.
* Регрессии, которые являются ошибками, вызванными обновлениями кода, обнаруживаются во время автоматического создания и развертывания.

В этом разделе предполагается, что у вас есть базовое представление о Razor Pagesных приложениях и модульных тестах. Если вы не знакомы с Razor Pages приложениями или понятиями тестирования, см. следующие разделы:

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [Модульное C# тестирование в .NET Core с помощью DotNet Test и xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([как скачивать](xref:index#how-to-download-a-sample))

Пример проекта состоит из двух приложений:

| Приложение         | Папка проекта                     | Описание |
| ----------- | ---------------------------------- | ----------- |
| Приложение сообщений | *src/Разорпажестестсампле*         | Позволяет пользователю добавлять сообщение, удалять одно сообщение, удалять все сообщения и анализировать сообщения (найти среднее количество слов на сообщение). |
| Тестовое приложение    | *Tests/Разорпажестестсампле. Tests* | Используется для модульного тестирования модели DAL и индексной страницы приложения "сообщения". |

Тесты можно выполнять с помощью встроенных функций тестирования интегрированной среды разработки, таких как [Visual Studio](/visualstudio/test/unit-test-your-code) или [Visual Studio для Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution). При использовании [Visual Studio Code](https://code.visualstudio.com/) или командной строки выполните следующую команду в командной строке в папке *Tests/разорпажестестсампле. Tests* :

```console
dotnet test
```

## <a name="message-app-organization"></a>Организация приложения для сообщений

Приложение для обмена сообщениями — это Razor Pages система сообщений со следующими характеристиками:

* Страница индекса приложения (*pages/index. cshtml* и Pages */index. cshtml. CS*) предоставляет методы пользовательского интерфейса и модели страницы для управления добавлением, удалением и анализом сообщений (поиск среднего числа слов на сообщение).
* Сообщение описывается `Message` классом (*Data/Message. CS*) с двумя свойствами: `Id` (Key) и `Text` (Message). `Text` Свойство является обязательным и ограничено 200 символами.
* Сообщения хранятся с&#8224;помощью [Entity Framework базы данных в памяти](/ef/core/providers/in-memory/).
* Приложение содержит слой DAL в своем классе `AppDbContext` контекста базы данных (*Data/аппдбконтекст. CS*). Методы DAL помечены `virtual`, что позволяет макетирование методы для использования в тестах.
* Если база данных пуста при запуске приложения, то хранилище сообщений инициализируется тремя сообщениями. Эти *начальные сообщения* также используются в тестах.

&#8224;Раздел EF, [тест с использованием памяти](/ef/core/miscellaneous/testing/in-memory), объясняет, как использовать базу данных в памяти для тестов с помощью MSTest. В этом разделе используется платформа тестирования [xUnit](https://xunit.github.io/) . Концепции тестирования и реализации тестов в разных платформах тестирования похожи, но не идентичны.

Хотя пример приложения не использует шаблон репозитория и не является эффективным примером [шаблона единицы работы (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages поддерживает эти шаблоны разработки. Дополнительные сведения см. в разделе [Разработка уровня сохраняемости инфраструктуры](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) и <xref:mvc/controllers/testing> (образец реализует шаблон репозитория).

## <a name="test-app-organization"></a>Тестирование организации приложения

Тестовое приложение — это консольное приложение внутри папки *Tests/разорпажестестсампле. Tests* .

| Папка тестового приложения | Описание |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.CS* содержит модульные тесты для DAL.</li><li>*IndexPageTests.CS* содержит модульные тесты для модели индексной страницы.</li></ul> |
| *Служебные программы*     | `TestDbContextOptions` Содержит метод, используемый для создания новых параметров контекста базы данных для каждого модульного теста DAL, чтобы база данных была сброшена в базовое состояние для каждого теста. |

Платформа тестирования — [xUnit](https://xunit.github.io/). Инфраструктура макетирования объектов — [MOQ](https://github.com/moq/moq4).

## <a name="unit-tests-of-the-data-access-layer-dal"></a>Модульные тесты уровня доступа к данным (DAL)

Приложение сообщений имеет DAL с четырьмя методами, `AppDbContext` содержащимися в классе (*src/разорпажестестсампле/Data/аппдбконтекст. CS*). Каждый метод содержит один или два модульных теста в тестовом приложении.

| DAL, метод               | Функция                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Получает объект `List<Message>` из базы данных `Text` , отсортированной по свойству. |
| `AddMessageAsync`        | `Message` Добавляет в базу данных.                                          |
| `DeleteAllMessagesAsync` | Удаляет все `Message` записи из базы данных.                           |
| `DeleteMessageAsync`     | Удаляет один `Message` объект из базы данных с `Id`помощью.                      |

Модульные тесты DAL требуются <xref:Microsoft.EntityFrameworkCore.DbContextOptions> при создании нового `AppDbContext` для каждого теста. Один из подходов к `DbContextOptions` созданию для каждого теста заключается в <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>использовании:

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

Проблема с этим подходом заключается в том, что каждый тест получает базу данных в любом состоянии, которое прокинуло предыдущий тест. Это может быть проблемой при попытке написания атомарных модульных тестов, которые не мешают друг другу. Чтобы принудительно `AppDbContext` использовать новый контекст базы данных для каждого теста, `DbContextOptions` укажите экземпляр, основанный на новом поставщике служб. В тестовом приложении показано, как это сделать с `Utilities` помощью метода `TestDbContextOptions` класса (*Tests/разорпажестестсампле. Tests/Utilities/Utilities. CS*):

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

`DbContextOptions` Использование в модульных тестах DAL позволяет каждому тесту выполняться атомарно с использованием нового экземпляра базы данных:

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Каждый метод теста в `DataAccessLayerTest` классе (*UnitTests/датаакцесслайертест. CS*) соответствует аналогичному шаблону компоновки-акт-Assert:

1. Упорядочить База данных настроена для проверки и/или определен ожидаемый результат.
1. ACT Тест выполняется.
1. Утверждающе Утверждения выполняются, чтобы определить, является ли результат теста успешным.

Например, `DeleteMessageAsync` метод отвечает за удаление одного сообщения, идентифицируемого его `Id` (*src/разорпажестестсампле/Data/аппдбконтекст. CS*):

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

Для этого метода существует два теста. Один тест проверяет, что метод удаляет сообщение, когда сообщение содержится в базе данных. Другой метод проверяет, что база данных не изменяется, если сообщение `Id` для удаления не существует. Ниже `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` показан метод.

[!code-csharp[](razor-pages-tests/samples_snapshot/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Сначала метод выполняет шаг упорядочивания, где выполняется подготовка для этапа действия. Начальные сообщения получаются и хранятся в `seedMessages`. Начальные сообщения сохраняются в базе данных. Сообщение с `Id` `1` параметром имеет значение, установленное для удаления. При выполнении `Id` `1`метода ожидаемые сообщения должны иметь все сообщения, за исключением одного с. `DeleteMessageAsync` `expectedMessages` Переменная представляет ожидаемый результат.

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Метод выполняет следующие действия: Метод выполняется путем передачи `recId` в `1`: `DeleteMessageAsync`

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Наконец, метод получает `Messages` из контекста и сравнивает его `expectedMessages` с утверждением того, что два значения равны:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Чтобы сравнить эти два `List<Message>` , выполните следующие действия.

* Сообщения упорядочиваются по `Id`.
* Пары сообщений сравниваются по `Text` свойству.

Аналогичный метод `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` теста проверяет результат попытки удаления несуществующего сообщения. В этом случае ожидаемые сообщения в базе данных должны совпадать с фактическими сообщениями после `DeleteMessageAsync` выполнения метода. Содержимое базы данных не должно изменяться:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>Модульные тесты методов модели страницы

Другой набор модульных тестов отвечает за тестирование методов модели страницы. В приложении "сообщения" модели индексных страниц находятся в `IndexModel` классе в *src/разорпажестестсампле/Pages/index. cshtml. CS*.

| Метод модели страницы | Функция |
| ----------------- | -------- |
| `OnGetAsync` | Получает сообщения из DAL для пользовательского интерфейса с помощью `GetMessagesAsync` метода. |
| `OnPostAddMessageAsync` | Если [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) является допустимым, вызывает `AddMessageAsync` метод, чтобы добавить сообщение в базу данных. |
| `OnPostDeleteAllMessagesAsync` | Вызывает `DeleteAllMessagesAsync` метод для удаления всех сообщений в базе данных. |
| `OnPostDeleteMessageAsync` | Выполняет, чтобы удалить сообщение `Id` с указанным. `DeleteMessageAsync` |
| `OnPostAnalyzeMessagesAsync` | Если одно или несколько сообщений находятся в базе данных, вычисляет среднее количество слов на сообщение. |

Методы модели страницы тестируются с помощью семи тестов в `IndexPageTests` классе (Tests */разорпажестестсампле. Tests/UnitTests/индекспажетестс. CS*). В тестах используется привычный шаблон "упорядочение-утверждение-действие". Эти тесты сосредоточены на:

* Определение правильности поведения методов, если [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) является недопустимым.
* Подтверждение того, что методы возвращают правильное <xref:Microsoft.AspNetCore.Mvc.IActionResult>значение.
* Проверка того, что назначения значений свойств сделаны правильно.

Эта группа тестов часто размещает методы DAL для получения ожидаемых данных для этапа действия, в котором выполняется метод модели страницы. Например, `GetMessagesAsync` метод `AppDbContext` макета предназначен для создания выходных данных. Когда метод модели страницы выполняет этот метод, макет возвращает результат. Данные не поступают из базы данных. Это создает предсказуемые и надежные тестовые условия для использования DAL в тестовой модели страницы.

Тест показывает, `GetMessagesAsync` как размещается метод для модели страницы: `OnGetAsync_PopulatesThePageModel_WithAListOfMessages`

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

При выполнении `GetMessagesAsync` метода на шаге действия он вызывает метод модели страницы. `OnGetAsync`

Шаг действия модульного теста (*Tests/разорпажестестсампле. Tests/UnitTests/индекспажетестс. CS*):

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage``OnGetAsync` метод модели страницы (*src/разорпажестестсампле/Pages/index. cshtml. CS*):

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

`GetMessagesAsync` Метод DAL не возвращает результат для этого вызова метода. Макетная версия метода возвращает результат.

На этом `Assert` шаге фактические сообщения (`actualMessages` `Messages` ) назначаются из свойства модели страницы. Проверка типа также выполняется при назначении сообщений. Ожидаемые и фактические сообщения сравниваются по `Text` их свойствам. Тест утверждает, что два `List<Message>` экземпляра содержат одинаковые сообщения.

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

Другие тесты в этой группе создают объекты модели страницы, включающие <xref:Microsoft.AspNetCore.Http.DefaultHttpContext> <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>, <xref:Microsoft.AspNetCore.Mvc.ActionContext> , `PageContext`для установки `PageContext` `ViewDataDictionary`, и. Они полезны при проведении тестов. Например, приложение `ModelState` messages создает ошибку с <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> целью проверки того, что при `OnPostAddMessageAsync` выполнении возвращается <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> допустимое значение.

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Модульное C# тестирование в .NET Core с помощью DotNet Test и xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* [Модульное тестирование кода](/visualstudio/test/unit-test-your-code) (Visual Studio)
* <xref:test/integration-tests>
* [xUnit.net](https://xunit.github.io/)
* [Создание полноценного решения .NET Core на базе macOS с помощью Visual Studio для Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [Начало работы с xUnit.net: Использование .NET Core с командной строкой пакета SDK для .NET](https://xunit.github.io/docs/getting-started-dotnet-core)
* [MOQ](https://github.com/moq/moq4)
* [Краткое руководство по MOQ](https://github.com/Moq/moq4/wiki/Quickstart)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core поддерживает модульные тесты Razor Pages приложений. Тесты уровня доступа к данным (DAL) и модели страниц помогают обеспечить следующее:

* Части приложения Razor Pages работают независимо друг от друга и вместе как единое целое во время создания приложения.
* Классы и методы имеют ограниченные области ответственности.
* Существует дополнительная документация по ведению приложения.
* Регрессии, которые являются ошибками, вызванными обновлениями кода, обнаруживаются во время автоматического создания и развертывания.

В этом разделе предполагается, что у вас есть базовое представление о Razor Pagesных приложениях и модульных тестах. Если вы не знакомы с Razor Pages приложениями или понятиями тестирования, см. следующие разделы:

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [Модульное C# тестирование в .NET Core с помощью DotNet Test и xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([как скачивать](xref:index#how-to-download-a-sample))

Пример проекта состоит из двух приложений:

| Приложение         | Папка проекта                     | Описание |
| ----------- | ---------------------------------- | ----------- |
| Приложение сообщений | *src/Разорпажестестсампле*         | Позволяет пользователю добавлять сообщение, удалять одно сообщение, удалять все сообщения и анализировать сообщения (найти среднее количество слов на сообщение). |
| Тестовое приложение    | *Tests/Разорпажестестсампле. Tests* | Используется для модульного тестирования модели DAL и индексной страницы приложения "сообщения". |

Тесты можно выполнять с помощью встроенных функций тестирования интегрированной среды разработки, таких как [Visual Studio](/visualstudio/test/unit-test-your-code) или [Visual Studio для Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution). При использовании [Visual Studio Code](https://code.visualstudio.com/) или командной строки выполните следующую команду в командной строке в папке *Tests/разорпажестестсампле. Tests* :

```console
dotnet test
```

## <a name="message-app-organization"></a>Организация приложения для сообщений

Приложение для обмена сообщениями — это Razor Pages система сообщений со следующими характеристиками:

* Страница индекса приложения (*pages/index. cshtml* и Pages */index. cshtml. CS*) предоставляет методы пользовательского интерфейса и модели страницы для управления добавлением, удалением и анализом сообщений (поиск среднего числа слов на сообщение).
* Сообщение описывается `Message` классом (*Data/Message. CS*) с двумя свойствами: `Id` (Key) и `Text` (Message). `Text` Свойство является обязательным и ограничено 200 символами.
* Сообщения хранятся с&#8224;помощью [Entity Framework базы данных в памяти](/ef/core/providers/in-memory/).
* Приложение содержит слой DAL в своем классе `AppDbContext` контекста базы данных (*Data/аппдбконтекст. CS*). Методы DAL помечены `virtual`, что позволяет макетирование методы для использования в тестах.
* Если база данных пуста при запуске приложения, то хранилище сообщений инициализируется тремя сообщениями. Эти *начальные сообщения* также используются в тестах.

&#8224;Раздел EF, [тест с использованием памяти](/ef/core/miscellaneous/testing/in-memory), объясняет, как использовать базу данных в памяти для тестов с помощью MSTest. В этом разделе используется платформа тестирования [xUnit](https://xunit.github.io/) . Концепции тестирования и реализации тестов в разных платформах тестирования похожи, но не идентичны.

Хотя пример приложения не использует шаблон репозитория и не является эффективным примером [шаблона единицы работы (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages поддерживает эти шаблоны разработки. Дополнительные сведения см. в разделе [Разработка уровня сохраняемости инфраструктуры](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) и <xref:mvc/controllers/testing> (образец реализует шаблон репозитория).

## <a name="test-app-organization"></a>Тестирование организации приложения

Тестовое приложение — это консольное приложение внутри папки *Tests/разорпажестестсампле. Tests* .

| Папка тестового приложения | Описание |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.CS* содержит модульные тесты для DAL.</li><li>*IndexPageTests.CS* содержит модульные тесты для модели индексной страницы.</li></ul> |
| *Служебные программы*     | `TestDbContextOptions` Содержит метод, используемый для создания новых параметров контекста базы данных для каждого модульного теста DAL, чтобы база данных была сброшена в базовое состояние для каждого теста. |

Платформа тестирования — [xUnit](https://xunit.github.io/). Инфраструктура макетирования объектов — [MOQ](https://github.com/moq/moq4).

## <a name="unit-tests-of-the-data-access-layer-dal"></a>Модульные тесты уровня доступа к данным (DAL)

Приложение сообщений имеет DAL с четырьмя методами, `AppDbContext` содержащимися в классе (*src/разорпажестестсампле/Data/аппдбконтекст. CS*). Каждый метод содержит один или два модульных теста в тестовом приложении.

| DAL, метод               | Функция                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Получает объект `List<Message>` из базы данных `Text` , отсортированной по свойству. |
| `AddMessageAsync`        | `Message` Добавляет в базу данных.                                          |
| `DeleteAllMessagesAsync` | Удаляет все `Message` записи из базы данных.                           |
| `DeleteMessageAsync`     | Удаляет один `Message` объект из базы данных с `Id`помощью.                      |

Модульные тесты DAL требуются <xref:Microsoft.EntityFrameworkCore.DbContextOptions> при создании нового `AppDbContext` для каждого теста. Один из подходов к `DbContextOptions` созданию для каждого теста заключается в <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>использовании:

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

Проблема с этим подходом заключается в том, что каждый тест получает базу данных в любом состоянии, которое прокинуло предыдущий тест. Это может быть проблемой при попытке написания атомарных модульных тестов, которые не мешают друг другу. Чтобы принудительно `AppDbContext` использовать новый контекст базы данных для каждого теста, `DbContextOptions` укажите экземпляр, основанный на новом поставщике служб. В тестовом приложении показано, как это сделать с `Utilities` помощью метода `TestDbContextOptions` класса (*Tests/разорпажестестсампле. Tests/Utilities/Utilities. CS*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

`DbContextOptions` Использование в модульных тестах DAL позволяет каждому тесту выполняться атомарно с использованием нового экземпляра базы данных:

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Каждый метод теста в `DataAccessLayerTest` классе (*UnitTests/датаакцесслайертест. CS*) соответствует аналогичному шаблону компоновки-акт-Assert:

1. Упорядочить База данных настроена для проверки и/или определен ожидаемый результат.
1. ACT Тест выполняется.
1. Утверждающе Утверждения выполняются, чтобы определить, является ли результат теста успешным.

Например, `DeleteMessageAsync` метод отвечает за удаление одного сообщения, идентифицируемого его `Id` (*src/разорпажестестсампле/Data/аппдбконтекст. CS*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

Для этого метода существует два теста. Один тест проверяет, что метод удаляет сообщение, когда сообщение содержится в базе данных. Другой метод проверяет, что база данных не изменяется, если сообщение `Id` для удаления не существует. Ниже `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` показан метод.

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Сначала метод выполняет шаг упорядочивания, где выполняется подготовка для этапа действия. Начальные сообщения получаются и хранятся в `seedMessages`. Начальные сообщения сохраняются в базе данных. Сообщение с `Id` `1` параметром имеет значение, установленное для удаления. При выполнении `Id` `1`метода ожидаемые сообщения должны иметь все сообщения, за исключением одного с. `DeleteMessageAsync` `expectedMessages` Переменная представляет ожидаемый результат.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Метод выполняет следующие действия: Метод выполняется путем передачи `recId` в `1`: `DeleteMessageAsync`

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Наконец, метод получает `Messages` из контекста и сравнивает его `expectedMessages` с утверждением того, что два значения равны:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Чтобы сравнить эти два `List<Message>` , выполните следующие действия.

* Сообщения упорядочиваются по `Id`.
* Пары сообщений сравниваются по `Text` свойству.

Аналогичный метод `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` теста проверяет результат попытки удаления несуществующего сообщения. В этом случае ожидаемые сообщения в базе данных должны совпадать с фактическими сообщениями после `DeleteMessageAsync` выполнения метода. Содержимое базы данных не должно изменяться:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>Модульные тесты методов модели страницы

Другой набор модульных тестов отвечает за тестирование методов модели страницы. В приложении "сообщения" модели индексных страниц находятся в `IndexModel` классе в *src/разорпажестестсампле/Pages/index. cshtml. CS*.

| Метод модели страницы | Функция |
| ----------------- | -------- |
| `OnGetAsync` | Получает сообщения из DAL для пользовательского интерфейса с помощью `GetMessagesAsync` метода. |
| `OnPostAddMessageAsync` | Если [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) является допустимым, вызывает `AddMessageAsync` метод, чтобы добавить сообщение в базу данных. |
| `OnPostDeleteAllMessagesAsync` | Вызывает `DeleteAllMessagesAsync` метод для удаления всех сообщений в базе данных. |
| `OnPostDeleteMessageAsync` | Выполняет, чтобы удалить сообщение `Id` с указанным. `DeleteMessageAsync` |
| `OnPostAnalyzeMessagesAsync` | Если одно или несколько сообщений находятся в базе данных, вычисляет среднее количество слов на сообщение. |

Методы модели страницы тестируются с помощью семи тестов в `IndexPageTests` классе (Tests */разорпажестестсампле. Tests/UnitTests/индекспажетестс. CS*). В тестах используется привычный шаблон "упорядочение-утверждение-действие". Эти тесты сосредоточены на:

* Определение правильности поведения методов, если [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) является недопустимым.
* Подтверждение того, что методы возвращают правильное <xref:Microsoft.AspNetCore.Mvc.IActionResult>значение.
* Проверка того, что назначения значений свойств сделаны правильно.

Эта группа тестов часто размещает методы DAL для получения ожидаемых данных для этапа действия, в котором выполняется метод модели страницы. Например, `GetMessagesAsync` метод `AppDbContext` макета предназначен для создания выходных данных. Когда метод модели страницы выполняет этот метод, макет возвращает результат. Данные не поступают из базы данных. Это создает предсказуемые и надежные тестовые условия для использования DAL в тестовой модели страницы.

Тест показывает, `GetMessagesAsync` как размещается метод для модели страницы: `OnGetAsync_PopulatesThePageModel_WithAListOfMessages`

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

При выполнении `GetMessagesAsync` метода на шаге действия он вызывает метод модели страницы. `OnGetAsync`

Шаг действия модульного теста (*Tests/разорпажестестсампле. Tests/UnitTests/индекспажетестс. CS*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage``OnGetAsync` метод модели страницы (*src/разорпажестестсампле/Pages/index. cshtml. CS*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

`GetMessagesAsync` Метод DAL не возвращает результат для этого вызова метода. Макетная версия метода возвращает результат.

На этом `Assert` шаге фактические сообщения (`actualMessages` `Messages` ) назначаются из свойства модели страницы. Проверка типа также выполняется при назначении сообщений. Ожидаемые и фактические сообщения сравниваются по `Text` их свойствам. Тест утверждает, что два `List<Message>` экземпляра содержат одинаковые сообщения.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

Другие тесты в этой группе создают объекты модели страницы, включающие <xref:Microsoft.AspNetCore.Http.DefaultHttpContext> <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>, <xref:Microsoft.AspNetCore.Mvc.ActionContext> , `PageContext`для установки `PageContext` `ViewDataDictionary`, и. Они полезны при проведении тестов. Например, приложение `ModelState` messages создает ошибку с <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> целью проверки того, что при `OnPostAddMessageAsync` выполнении возвращается <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> допустимое значение.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Модульное C# тестирование в .NET Core с помощью DotNet Test и xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* [Модульное тестирование кода](/visualstudio/test/unit-test-your-code) (Visual Studio)
* <xref:test/integration-tests>
* [xUnit.net](https://xunit.github.io/)
* [Создание полноценного решения .NET Core на базе macOS с помощью Visual Studio для Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [Начало работы с xUnit.net: Использование .NET Core с командной строкой пакета SDK для .NET](https://xunit.github.io/docs/getting-started-dotnet-core)
* [MOQ](https://github.com/moq/moq4)
* [Краткое руководство по MOQ](https://github.com/Moq/moq4/wiki/Quickstart)

::: moniker-end
