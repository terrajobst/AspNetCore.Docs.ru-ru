---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Макетирование Entity Framework при модульном тестировании ASP.NET Web API 2 | Документация Майкрософт
author: tfitzmac
description: Это руководство и приложения показано, как создавать модульные тесты для приложения веб-API 2, использующий Entity Framework. Показано, как изменить...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 0bc5ab59583a2be3f889ba05d26c6cda4589057d
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836758"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Макетирование Entity Framework при модульном тестировании ASP.NET Web API 2
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

[Скачать завершенный проект](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Это руководство и приложения показано, как создавать модульные тесты для приложения веб-API 2, использующий Entity Framework. Нем показано, как изменить сформированный контроллера для включения передачи объекта контекста для тестирования и создают тестовые объекты, которые работают с Entity Framework.
> 
> Введение в модульное тестирование с помощью веб-API ASP.NET, см. в разделе [модульное тестирование с помощью ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).
> 
> Предполагается, что вы знакомы с основными понятиями веб-API ASP.NET. Вводное руководство см. в разделе [Приступая к работе с веб-API ASP.NET 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Веб-API 2


## <a name="in-this-topic"></a>Содержание раздела

В этом разделе содержатся следующие подразделы.

- [Необходимые компоненты](#prereqs)
- [Скачать код](#download)
- [Создание приложения с помощью проекта модульного теста](#appwithunittest)
- [Создание класса модели](#modelclass)
- [Добавление контроллера](#controller)
- [Добавление внедрения зависимостей](#dependency)
- [Установите пакеты NuGet в проекте теста](#testpackages)
- [Создание контекста теста](#testcontext)
- [Создание тестов](#tests)
- [Выполнение тестов](#runtests)

Если вы уже выполнили действия, описанные в [модульное тестирование с помощью ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), можно перейти к разделу [добавить контроллер](#controller).

<a id="prereqs"></a>
## <a name="prerequisites"></a>Предварительные требования

Visual Studio 2017 Community, Professional или Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>Скачать код

Скачайте [завершенного проекта](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Загружаемый проект включает в себя код модульного теста для этого раздела, а также для [модульного тестирования ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) раздела.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Создание приложения с помощью проекта модульного теста

Можно создать проект модульного теста, при создании приложения или добавить проект модульных тестов для существующего приложения. Этом руководстве показано создание проекта модульного теста, при создании приложения.

Создание нового веб-приложения ASP.NET с именем **StoreApp**.

В окне нового проекта ASP.NET выберите **пустой** шаблона и добавить папки и основные ссылки для веб-API. Выберите **Добавление модульных тестов** параметр. Автоматически присваивается имя проекта модульного теста **StoreApp.Tests**. Можно оставить это имя.

![Создание проекта модульного теста](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

После создания приложения, вы увидите, он содержит два проекта — **StoreApp** и **StoreApp.Tests**.

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>Создание класса модели

В проекте StoreApp добавьте файл класса для **моделей** папку с именем **Product.cs**. Замените содержимое файла следующим кодом.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

Постройте решение.

<a id="controller"></a>
## <a name="add-the-controller"></a>Добавление контроллера

Щелкните правой кнопкой мыши папку Controllers и выберите **добавить** и **создать шаблонный элемент**. Выберите контроллер Web API 2 с действиями, использующий Entity Framework.

![Добавление нового контроллера](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

Установите следующие значения:

- Имя контроллера: **ProductController**
- Класс модели: **продукта**
- Класс контекста данных: [выберите **новый контекст данных** кнопки, который заполняет значения, показанный ниже]

![Укажите контроллер](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

Нажмите кнопку **добавить** для создания контроллера с автоматически созданным кодом. Код содержит методы для создания, извлечения, обновления и удаления экземпляров класса Product. В следующем коде показано метод для добавления продукта. Обратите внимание на то, что метод возвращает экземпляр **IHttpActionResult**.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

IHttpActionResult является одним из новых функций в веб-API 2, и он упрощает разработки модульных тестов.

В следующем разделе будет изменять созданный код для упрощения передачи объектов тестирования к контроллеру.

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>Добавление внедрения зависимостей

В настоящее время класс ProductController жестко заданной необходимости использовать экземпляр класса StoreAppContext. Для изменения приложения и удалить эту жестко заданную зависимость будет использовать шаблон под названием внедрения зависимостей. Разрывая эту зависимость, можно передать в макет объекта при тестировании.

Щелкните правой кнопкой мыши **моделей** папки и добавьте новый интерфейс с именем **IStoreAppContext**.

Замените код следующим кодом.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

Откройте файл StoreAppContext.cs и внесите следующие выделенные изменения. Важные изменения, следует отметить, —:

- Класс StoreAppContext реализует интерфейс IStoreAppContext
- Метод MarkAsModified реализуется


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

Откройте файл ProductController.cs. Измените существующий код в соответствии с выделенный код. Эти изменения нарушить зависимость на StoreAppContext и включить другие классы для передачи в другом объекте класса контекста. Это изменение позволит передать контекст теста во время модульных тестов.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

Есть еще одно изменение, которые необходимо внести в ProductController. В **PutProduct** метод, замените строку, которая задает состояние сущности изменять с помощью вызова метода MarkAsModified.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

Постройте решение.

Теперь вы готовы настроить тестовый проект.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Установите пакеты NuGet в проекте теста

При использовании пустого шаблона для создания приложения, проект модульного теста (StoreApp.Tests) не включает все установленные пакеты NuGet. Другие шаблоны, такие как шаблон веб-API, включают некоторые пакеты NuGet в проект модульного теста. Для этого руководства необходимо включить пакета Entity Framework и пакета Microsoft ASP.NET Web API 2 Core для тестового проекта.

Щелкните правой кнопкой мыши проект StoreApp.Tests и выберите **управление пакетами NuGet**. Необходимо выбрать проект StoreApp.Tests, чтобы добавить пакеты для этого проекта.

![Управление пакетами](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

Из Online пакетов найдите и установите пакет EntityFramework (версии 6.0 или более поздней версии). Если кажется, что EntityFramework пакет уже установлен, возможно, выбрана в StoreApp проект, а не StoreApp.Tests проекта.

![Добавление платформы Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

Найдите и установите пакет Microsoft ASP.NET Web API 2 Core.

![Установите пакет core web api](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

Закройте окно Управление пакетами NuGet.

<a id="testcontext"></a>
## <a name="create-test-context"></a>Создание контекста теста

Добавьте класс с именем **TestDbSet** в тестовый проект. Этот класс служит базовым классом для проверочного набора данных. Замените код следующим кодом.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

Добавьте класс с именем **TestProductDbSet** в тестовый проект, который содержит следующий код.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

Добавьте класс с именем **TestStoreAppContext** и замените существующий код следующим кодом.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>Создание тестов

По умолчанию тестовый проект содержит файл пустой тест с именем **UnitTest1.cs**. Этот файл представлены атрибуты, можно использовать для создания методов теста. В этом руководстве можно удалить файл, так как вы добавите нового тестового класса.

Добавьте класс с именем **TestProductController** в тестовый проект. Замените код следующим кодом.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Выполнить тесты

Теперь вы готовы для выполнения тестов. Все, которые помечены с помощью метода **TestMethod** атрибут будет проверено. Из **теста** пункта меню, выполните тесты.

![Запуск тестов](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

Откройте **обозреватель тестов** окно и обратите внимание, что результаты тестов.

![результаты теста](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
