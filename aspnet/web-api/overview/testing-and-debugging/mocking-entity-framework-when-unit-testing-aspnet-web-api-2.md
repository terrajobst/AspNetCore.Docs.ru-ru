---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Макетирование Entity Framework при модульного тестирования ASP.NET Web API 2 | Документы Microsoft
author: tfitzmac
description: Это руководство и приложения показано, как создавать модульные тесты для приложения веб-API 2, использующего платформу Entity Framework. Показано, как изменить...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: abfde7edec85812de3560f4edefb110c3e374580
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152869"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Макетирование Entity Framework при модульного тестирования ASP.NET Web API 2
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> Это руководство и приложения показано, как создавать модульные тесты для приложения веб-API 2, использующего платформу Entity Framework. Он показывает способы изменения контроллеру формирования шаблонов для включения передачи объекта контекста для тестирования и создают тестовые объекты, которые работают с Entity Framework.
> 
> Введение модульное тестирование с помощью веб-API ASP.NET см. в разделе [модульное тестирование с помощью ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).
> 
> В учебнике предполагается, что вы знакомы с основными понятиями веб-API ASP.NET. Вводное руководство см. в разделе [Приступая к работе с ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Веб-API 2


## <a name="in-this-topic"></a>Содержание раздела

В этом разделе содержатся следующие подразделы.

- [Необходимые компоненты](#prereqs)
- [Загрузить исходный код](#download)
- [Создайте приложение с проект модульного теста](#appwithunittest)
- [Создание класса модели](#modelclass)
- [Добавить контроллер](#controller)
- [Добавить внедрения зависимости](#dependency)
- [Установить пакеты NuGet в тестовый проект](#testpackages)
- [Создание контекста теста](#testcontext)
- [Создание тестов](#tests)
- [Выполнение тестов](#runtests)

Если вы уже выполнили действия, описанные в [модульное тестирование с помощью ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), можно перейти к разделу [добавить контроллер](#controller).

<a id="prereqs"></a>
## <a name="prerequisites"></a>Предварительные требования

Visual Studio 2017 г Community, Professional или Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>Загрузить исходный код

Загрузить [завершенного проекта](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Загружаемый проект включает в себя код модульного теста для этого раздела, а также для [веб-тестирования ASP.NET единицы API 2](unit-testing-with-aspnet-web-api.md) раздела.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Создайте приложение с проект модульного теста

Можно создать проект модульного теста, при создании приложения или добавить проект модульных тестов для существующего приложения. В этом учебнике показано создание проекта модульного теста при создании приложения.

Создание нового веб-приложения ASP.NET с именем **StoreApp**.

В окне нового проекта ASP.NET выберите **пустой** шаблона и добавить папки и основные ссылки для веб-API. Выберите **Добавление модульных тестов** параметр. Автоматически присваивается имя проекта модульного теста **StoreApp.Tests**. Можно оставить это имя.

![Создание проекта модульного теста](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

После создания приложения, вы увидите два проекта — **StoreApp** и **StoreApp.Tests**.

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>Создание класса модели

В проекте StoreApp добавьте файл класса для **моделей** папку с именем **Product.cs**. Замените содержимое файла следующим кодом.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

Постройте решение.

<a id="controller"></a>
## <a name="add-the-controller"></a>Добавить контроллер

Щелкните правой кнопкой мыши папку Controllers, а затем выберите **добавить** и **новый элемент формирования шаблонов**. Выберите Web API 2 контроллер с действиями, использующий Entity Framework.

![Добавить новый контроллер](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

Установите следующие значения:

- Имя контроллера: **ProductController**
- Класс модели: **продукта**
- Класс контекста данных: [выберите **новый контекст данных** кнопку, которая заполняет значения, показано ниже]

![Укажите контроллер](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

Нажмите кнопку **добавить** для создания контроллера с автоматически созданного кода. Код включает методы для создания, извлечения, обновления и удаления экземпляров класса Product. В следующем коде показан метод для добавления продукта. Обратите внимание, что метод возвращает экземпляр **IHttpActionResult**.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

IHttpActionResult является одним из новых функций в веб-API 2, и он упрощает разработки модульных тестов.

В следующем разделе будет настройки создаваемого кода для упрощения передачи объектов тестирования к контроллеру.

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>Добавить внедрения зависимости

В настоящее время класс ProductController жестко запрограммировано на использование экземпляра класса StoreAppContext. Для изменения приложения и удалить зависимость жестко будет использовать шаблон под названием внедрения зависимостей. Разрывая эту зависимость, можно передать в макет объекта при тестировании.

Щелкните правой кнопкой мыши **моделей** папки и добавьте новый интерфейс с именем **IStoreAppContext**.

Замените код следующим кодом.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

Откройте файл StoreAppContext.cs и внесите следующие изменения выделенный. Ниже перечислены важные изменения, обратите внимание

- Класс StoreAppContext реализует интерфейс IStoreAppContext
- Реализация метода MarkAsModified


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

Откройте файл ProductController.cs. Измените существующий код в соответствии выделенный код. Эти изменения нарушить зависимость на StoreAppContext и включить другие классы для передачи в другом объекте класс контекста. Это изменение будет позволяют передавать в контексте теста во время модульных тестов.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

Имеется один дополнительные изменения, необходимые в ProductController. В **PutProduct** метод, изменить строки, которое устанавливает состояние сущности с помощью вызова метода MarkAsModified замены.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

Постройте решение.

Теперь все готово для настройки тестового проекта.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Установить пакеты NuGet в тестовый проект

При использовании пустого шаблона для создания приложения проекта модульного теста (StoreApp.Tests) не включает все установленные пакеты NuGet. Другие шаблоны, например шаблона веб-API, включать несколько пакетов NuGet в проекте модульного теста. В этом учебнике необходимо включить packge Entity Framework и Microsoft ASP.NET Web API 2 Core пакета в тестовый проект.

Щелкните правой кнопкой мыши проект StoreApp.Tests и выберите **управление пакетами NuGet**. Необходимо выбрать проект StoreApp.Tests для добавления пакетов в проект.

![Управление пакетами](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

Из сети пакетов находить и устанавливать пакет EntityFramework (версии 6.0 или более поздней версии). Если предполагается, что EntityFramework пакет уже установлен, возможно, выбрана проекта StoreApp вместо StoreApp.Tests проекта.

![Добавление платформы Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

Найти и установить пакет Microsoft ASP.NET Web API 2 Core.

![Установка основных компонентов api веб-пакета](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

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

По умолчанию тестовый проект содержит файл пустой тест с именем **UnitTest1.cs**. Этот файл показаны атрибуты, можно использовать при создании методов теста. В этом учебнике можно удалить этот файл, поскольку будет добавлен новый класс теста.

Добавьте класс с именем **TestProductController** в тестовый проект. Замените код следующим кодом.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Выполнить тесты

Теперь вы готовы к выполнению тестов. Все, которые помечены с помощью метода **TestMethod** атрибут будет проверено. Из **тест** пункт меню, выполнить тесты.

![Запуск тестов](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

Откройте **Test Explorer** окна и проверьте результаты тестов.

![результаты теста](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
