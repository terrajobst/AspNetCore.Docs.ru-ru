---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: Модульное тестирование ASP.NET Web API 2 | Документация Майкрософт
author: Rick-Anderson
description: Это руководство и приложение демонстрируется создание простого модульных тестов для приложения веб-API 2. Этом руководстве показано, как включать proj модульного теста...
ms.author: riande
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 915610e6646ebe86dd8f16f290ecabd36bf7f48d
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020837"
---
<a name="unit-testing-aspnet-web-api-2"></a>Модульное тестирование ASP.NET Web API 2
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

[Скачать завершенный проект](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Это руководство и приложение демонстрируется создание простого модульных тестов для приложения веб-API 2. Этом руководстве показано, как включить проект модульного теста в решении, а написание методов теста, которые проверяют возвращаемые из метода контроллера.
>
> Предполагается, что вы знакомы с основными понятиями веб-API ASP.NET. Вводное руководство см. в разделе [Приступая к работе с веб-API ASP.NET 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
>
> В этом разделе модульные тесты намеренно ограничены простых сценариев. Модульное тестирование более сложных сценариев данных, см. в разделе [макетирование Entity Framework при модульного тестирования ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - Веб-API 2

## <a name="in-this-topic"></a>Содержание раздела

В этом разделе содержатся следующие подразделы.

- [Необходимые компоненты](#prereqs)
- [Скачать код](#download)
- [Создание приложения с помощью проекта модульного теста](#appwithunittest)
    - [Добавьте проект модульного теста, при создании приложения](#whencreate)
    - [Добавление проекта модульного тестирование в существующее приложение](#addtoexisting)
- [Настройка приложения веб-API 2](#setupproject)
- [Установите пакеты NuGet в проекте теста](#testpackages)
- [Создание тестов](#tests)
- [Выполнение тестов](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>Предварительные требования

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional или Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>Скачать код

Скачайте [завершенного проекта](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Загружаемый проект включает в себя код модульного теста для этого раздела, а также для [макетирование Entity Framework при модульного тестирования веб-API ASP.NET](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) раздела.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Создание приложения с помощью проекта модульного теста

Можно создать проект модульного теста, при создании приложения или добавить проект модульных тестов для существующего приложения. Мы продемонстрируем оба метода для создания проекта модульного теста. Чтобы выполнить это руководство, можно использовать любой из этих подходов.

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>Добавьте проект модульного теста, при создании приложения

Создание нового веб-приложения ASP.NET с именем **StoreApp**.

![Создание проекта](unit-testing-with-aspnet-web-api/_static/image1.png)

В окне нового проекта ASP.NET выберите **пустой** шаблона и добавить папки и основные ссылки для веб-API. Выберите **Добавление модульных тестов** параметр. Автоматически присваивается имя проекта модульного теста **StoreApp.Tests**. Можно оставить это имя.

![Создание проекта модульного теста](unit-testing-with-aspnet-web-api/_static/image2.png)

После создания приложения, вы увидите, что она содержит два проекта.

![два проекта](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>Добавление проекта модульного тестирование в существующее приложение

Если вы не создали проект модульного теста при создании приложения, его можно добавить в любое время. Например, предположим, что у вас уже есть приложение с именем StoreApp и вы хотите добавить модульные тесты. Чтобы добавить проект модульного теста, щелкните правой кнопкой мыши решение и выберите **добавить** и **новый проект**.

![Добавление нового проекта к решению](unit-testing-with-aspnet-web-api/_static/image4.png)

Выберите **теста** в левой панели и выберите **проект модульного теста** для типа проекта. Назовите проект **StoreApp.Tests**.

![Добавьте проект модульного теста](unit-testing-with-aspnet-web-api/_static/image5.png)

Вы увидите проект модульного теста в решении.

В проект модульного теста добавьте ссылку на проект в исходный проект.

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>Настройка приложения веб-API 2

В проекте StoreApp добавьте файл класса для **моделей** папку с именем **Product.cs**. Замените содержимое файла следующим кодом.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

Постройте решение.

Щелкните правой кнопкой мыши папку Controllers и выберите **добавить** и **создать шаблонный элемент**. Выберите **контроллер - пустой веб-API 2**.

![Добавление нового контроллера](unit-testing-with-aspnet-web-api/_static/image6.png)

Укажите имя контроллера **SimpleProductController**и нажмите кнопку **добавить**.

![Укажите контроллер](unit-testing-with-aspnet-web-api/_static/image7.png)

Замените существующий код следующим кодом: Чтобы упростить этот пример, данные хранятся в список, а не базы данных. Список, определенный в этот класс представляет рабочих данных. Обратите внимание на то, что контроллер включает конструктор, который принимает в качестве параметра список объектов продукта. Этот конструктор позволяет передать тестовые данные при модульном тестировании. Контроллер также включает две **async** методы, чтобы проиллюстрировать модульное тестирование асинхронных методов. Эти асинхронные методы были реализованы путем вызова **Task.FromResult** для минимизации лишнего кода, но обычно методы следует использовать ресурсоемкими операциями.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

Метод GetProduct возвращает экземпляр **IHttpActionResult** интерфейс. IHttpActionResult является одним из новых функций в веб-API 2, и он упрощает разработки модульных тестов. Классы, реализующие интерфейс IHttpActionResult находятся в [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) пространства имен. Эти классы представляют возможные ответы на запрос действие, и они соответствуют коды состояния HTTP.

Постройте решение.

Теперь вы готовы настроить тестовый проект.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Установите пакеты NuGet в проекте теста

При использовании пустого шаблона для создания приложения, проект модульного теста (StoreApp.Tests) не включает все установленные пакеты NuGet. Другие шаблоны, такие как шаблон веб-API, включают некоторые пакеты NuGet в проект модульного теста. Для этого руководства необходимо включить пакет Microsoft ASP.NET Web API 2 Core для тестового проекта.

Щелкните правой кнопкой мыши проект StoreApp.Tests и выберите **управление пакетами NuGet**. Необходимо выбрать проект StoreApp.Tests, чтобы добавить пакеты для этого проекта.

![Управление пакетами](unit-testing-with-aspnet-web-api/_static/image8.png)

Найдите и установите пакет Microsoft ASP.NET Web API 2 Core.

![Установите пакет core web api](unit-testing-with-aspnet-web-api/_static/image9.png)

Закройте окно Управление пакетами NuGet.

<a id="tests"></a>
## <a name="create-tests"></a>Создание тестов

По умолчанию тестовый проект входит пустой тестовый файл UnitTest1.cs. Этот файл представлены атрибуты, можно использовать для создания методов теста. Для модульных тестов можно использовать этот файл или создать свой собственный файл.

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

В этом руководстве вы создадите свой собственный класс теста. Можно удалить файл UnitTest1.cs. Добавьте класс с именем **TestSimpleProductController.cs**и замените код следующим кодом.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Выполнить тесты

Теперь вы готовы для выполнения тестов. Все, которые помечены с помощью метода **TestMethod** атрибут будет проверено. Из **теста** пункта меню, выполните тесты.

![Запуск тестов](unit-testing-with-aspnet-web-api/_static/image11.png)

Откройте **обозреватель тестов** окно и обратите внимание, что результаты тестов.

![результаты теста](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>Сводка

Вы завершили этот учебник. Данные в этом учебнике намеренно была упрощена, чтобы сосредоточиться на модульное тестирование условия. Модульное тестирование более сложных сценариев данных, см. в разделе [макетирование Entity Framework при модульного тестирования ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
