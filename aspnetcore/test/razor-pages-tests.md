---
title: Тестирование страниц Razor в ASP.NET Core
author: guardrex
description: Сведения о создании модульных тестов для приложений Razor Pages.
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
uid: test/razor-pages-tests
ms.openlocfilehash: 5116ec3c3d6c27f9b0e098f82c82dd7b7337b8f6
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207502"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a>Тестирование страниц Razor в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

ASP.NET Core поддерживает модульные тесты приложений Razor Pages. Тесты данных доступа к уровню (DAL) и обеспечить моделей страниц:

* Части приложения Razor Pages работают независимо друг от друга и вместе как единица во время построения приложения.
* Классы и методы имеют ограниченную областей ответственности.
* Существует дополнительная документация на поведение приложения.
* Регрессии, которые откроются с обновлением кода ошибки, найденные во время автоматической сборки и развертывания.

В этом разделе предполагается, что имеется основные приложения Razor Pages и модульные тесты. Если вы не знакомы с приложениями Razor Pages или концепциях тестирования, см. в разделах:

* [Введение в Razor Pages](xref:razor-pages/index)
* [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Модульное тестирование C# в .NET Core с помощью команды dotnet test и xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([как скачивать](xref:index#how-to-download-a-sample))

Пример проекта состоит из двух приложений:

| Приложение         | Папка проекта                        | Описание |
| ----------- | ------------------------------------- | ----------- |
| Сообщение приложения | *src/RazorPagesTestSample*            | Позволяет пользователю добавить, удалить один, удалить все и анализ сообщений. |
| Тестирование приложения    | *tests/RazorPagesTestSample.Tests*    | Используется для сообщения приложения модульного теста: доступа к данным уровня (DAL) и модель страницы индекса. |

Тесты можно выполнять с помощью встроенной возможности интерфейса IDE, например [Visual Studio](https://www.visualstudio.com/vs/). При использовании [Visual Studio Code](https://code.visualstudio.com/) или из командной строки, выполните следующую команду в командной строке в *tests/RazorPagesTestSample.Tests* папку:

```console
dotnet test
```

## <a name="message-app-organization"></a>Сообщение приложения организации

Сообщение приложения — это простой система сообщение Razor Pages, со следующими характеристиками:

* Страница индекса приложения (*Pages/Index.cshtml* и *Pages/Index.cshtml.cs*) предоставляет пользовательский Интерфейс и страницы модели методы для управления, добавление, удаление и анализа сообщений (среднее слов в сообщения) .
* Описывается сообщение `Message` класс (*Data/Message.cs*) с двумя свойствами: `Id` (ключ) и `Text` (сообщение). `Text` Свойство требуется, но более 200 символов.
* Сообщения хранятся с использованием [базы данных в памяти платформа Entity Framework](/ef/core/providers/in-memory/)&#8224;.
* Приложение содержит слой доступа к данным (DAL) в классе контекста базы данных, `AppDbContext` (*Data/AppDbContext.cs*). Эти методы DAL отмечены `virtual`, что позволяет макетирование методы для использования в тестах.
* Если база данных пуста при запуске приложения, хранилище сообщений инициализируется с помощью этих сообщений. Эти *заполняется данными сообщений* также используются в тестах.

&#8224;В разделе EF [теста с помощью InMemory](/ef/core/miscellaneous/testing/in-memory), объясняется, как использовать базу данных в памяти для тестов с использованием MSTest. В этом разделе используется [xUnit](https://xunit.github.io/) платформы тестирования. Концепциях тестирования и тестирования реализации различных тестовых платформ доступны, но не идентичен.

Несмотря на то, что приложение не использует шаблон репозитория и не эффективный пример [шаблон единицы работы (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages поддерживает эти шаблоны разработки. Дополнительные сведения см. в разделе [проектирование уровня сохраняемости инфраструктуры](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) и [тестирование логики контроллера](/aspnet/core/mvc/controllers/testing) (этот пример реализует шаблон репозитория).

## <a name="test-app-organization"></a>Тестирование приложений организации

Тестирование приложения — это консольное приложение внутри *tests/RazorPagesTestSample.Tests* папки.

| Папка тестового приложения | Описание |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs* содержит модульные тесты для DAL.</li><li>*IndexPageTests.cs* содержит модульные тесты для модели страницы индекса.</li></ul> |
| *Служебные программы*     | Содержит `TestingDbContextOptions` метод, используемый для создания новой базы данных параметры контекста для каждого модульного теста DAL, таким образом, чтобы база данных будет переведена в состояние базовых показателей для каждого теста. |

Платформа тестирования — [xUnit](https://xunit.github.io/). Объект платформа имитированной [Moq](https://github.com/moq/moq4).

## <a name="unit-tests-of-the-data-access-layer-dal"></a>Модульные тесты данных доступа к уровню (DAL)

Приложение сообщение имеет DAL с четырех методов, содержащихся в `AppDbContext` класс (*src/RazorPagesTestSample/Data/AppDbContext.cs*). Каждый метод имеет один или два модульных тестов в тестовом приложении.

| Метод DAL               | Функция                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Получает `List<Message>` из базы данных, отсортированных по `Text` свойство. |
| `AddMessageAsync`        | Добавляет `Message` к базе данных.                                          |
| `DeleteAllMessagesAsync` | Удаляет все `Message` записей из базы данных.                           |
| `DeleteMessageAsync`     | Удаляет один `Message` из базы данных с `Id`.                      |

Модульные тесты из DAL требуют [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) при создании нового `AppDbContext` для каждого теста. Один подход к созданию `DbContextOptions` для каждого теста является использование [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

Проблема этого подхода является то, что каждый тест получает базы данных в то состояние, предыдущий тест остался. При попытке записать atomic модульные тесты, пересекаются друг с другом, это может быть проблематичным. Чтобы принудительно `AppDbContext` использовать новый контекст базы данных для каждого теста, укажите `DbContextOptions` экземпляр, который основан на новый поставщик службы. Тестирование приложения показано, как это сделать с помощью его `Utilities` метод класса `TestingDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

С помощью `DbContextOptions` в DAL модульных тестов позволяет каждого теста, чтобы выполняться атомарным образом с экземпляром свежей базы данных:

```csharp
using (var db = new AppDbContext(Utilities.TestingDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Все методы теста в `DataAccessLayerTest` класс (*UnitTests/DataAccessLayerTest.cs*) следует той же модели, расположение-Act-утверждение:

1. Упорядочить: База данных настроена для теста и/или определенных ожидаемый результат.
1. ACT: Выполняется тест.
1. Assert: Утверждения выполняются для определения того, является ли результат теста успешно.

Например `DeleteMessageAsync` метод отвечает за удаление одного сообщения определяется его `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

Существует два теста для этого метода. Один тест проверяет, что метод удаляет сообщение, когда сообщение помещается в базе данных. Другие тесты метод, базы данных не изменяется, если сообщение `Id` для удаления не существует. `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` Метод приведен ниже:

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Во-первых этот метод выполняет шаг расположение, где выполняется подготовка для шага Act. Заполнения сообщения полученные и находящиеся в `seedMessages`. Заполнения сообщения сохраняются в базе данных. Для этого сообщения `Id` из `1` задается для удаления. Когда `DeleteMessageAsync` метода, ожидаемые сообщения должны иметь все сообщения, за исключением с `Id` из `1`. `expectedMessages` Переменная представляет это ожидаемый результат.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Метод: `DeleteMessageAsync` метода передавая `recId` из `1`:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Наконец, метод получает `Messages` из контекста и сравнивает его `expectedMessages` они гарантируют, что они равны:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Чтобы сравнить два `List<Message>` одинаковы:

* Сообщения упорядочиваются по `Id`.
* Пары "сообщение" сравниваются на `Text` свойство.

Аналогичный метод теста, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` проверяет результат попытки удалить сообщение, которое не существует. В этом случае ожидаемые сообщения в базе данных должно быть равно сами сообщения после `DeleteMessageAsync` выполнения метода. Должна существовать без изменений в содержимое базы данных:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>Методы моделей модульные тесты, страницы

Другой набор модульных тестов несет ответственность за тесты из методов модели страницы. В нем сообщение моделей страниц индекса находятся в `IndexModel` в класс *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.

| Метод модели страницы | Функция |
| ----------------- | -------- |
| `OnGetAsync` | Получает сообщения из DAL для пользовательского интерфейса с помощью `GetMessagesAsync` метод. |
| `OnPostAddMessageAsync` | Если `ModelState` является допустимым, вызовы функций `AddMessageAsync` для добавления сообщения в базу данных. |
| `OnPostDeleteAllMessagesAsync` | Вызовы `DeleteAllMessagesAsync` удалить все сообщения в базе данных. |
| `OnPostDeleteMessageAsync` | Выполняет `DeleteMessageAsync` для удаления сообщения с `Id` указанного. |
| `OnPostAnalyzeMessagesAsync` | Если одно или несколько сообщений находятся в базе данных, вычисляет среднее количество слов в сообщении. |

Методы страниц модели проверяются с помощью семи тестов в `IndexPageTests` класс (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*). Тесты с помощью знакомых Assert Act расположение шаблона. Эти тесты сосредоточены на:

* Определение методов выполните правильное поведение при `ModelState` является недопустимым.
* Подтверждение методы создания правильного `IActionResult`.
* Проверка, что значение назначений свойств выполняются правильно.

Эта группа тесты часто макетировать методы DAL для получения ожидаемых данных, для которой выполняется метод модели страницы шага Act. Например `GetMessagesAsync` метод `AppDbContext` имитируется для получения выходных данных. При выполнении этого метода метод модели страницы макета возвращает результат. Данные не поступают из базы данных. При этом создается условия теста прогнозируемый, надежного по использованию DAL в тестах модели страницы.

`OnGetAsync_PopulatesThePageModel_WithAListOfMessages` Тестирования показан как `GetMessagesAsync` метод имитируется для модели страницы:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

Когда `OnGetAsync` метод выполняется на шаге Act, он вызывает метод страничной модели `GetMessagesAsync` метод.

Модульный тест шаг Act (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage` модель страницы `OnGetAsync` метод (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

`GetMessagesAsync` В DAL не возвращает результат для этого вызова метода. Макеты версию метода возвращает результат.

В `Assert` шага, фактическими сообщениями (`actualMessages`) назначаются из `Messages` свойство модели страницы. Проверка типа также выполняется при назначении сообщения. Ожидаемые и фактические сообщений сравниваются по их `Text` свойства. Тест подтверждает, что два `List<Message>` экземпляры содержат те же сообщения.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

Другие тесты, в этой группе Создать страницу объекты модели, включающие `DefaultHttpContext`, `ModelStateDictionary`, `ActionContext` для установления `PageContext`, `ViewDataDictionary`и `PageContext`. Они могут пригодиться при проведении тестов. Например, сообщение приложение устанавливает `ModelState` ошибка с `AddModelError` чтобы убедиться, что является допустимым `PageResult` возвращается, когда `OnPostAddMessageAsync` выполняется:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Модульное тестирование C# в .NET Core с помощью команды dotnet test и xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Контроллеры тестирования](xref:mvc/controllers/testing)
* [Модульное тестирование кода](/visualstudio/test/unit-test-your-code) (Visual Studio)
* [Интеграционные тесты](xref:test/integration-tests)
* [xUnit.net](https://xunit.github.io/)
* [Приступая к работе с xUnit.net (.NET Core/ASP.NET Core)](https://xunit.github.io/docs/getting-started-dotnet-core)
* [MOQ](https://github.com/moq/moq4)
* [Краткое руководство MOQ](https://github.com/Moq/moq4/wiki/Quickstart)
