---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: "Модульное тестирование ASP.NET Web API 2 | Документы Microsoft"
author: tfitzmac
description: "Это руководство и приложения показано, как создать простой модульные тесты для приложения веб-API 2. Этот учебник показывает, как включать proj модульных тестов..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2014
ms.topic: article
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4d6102dd81589e41894d8ecd95bf9ddd761a65bd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="unit-testing-aspnet-web-api-2"></a>Модульное тестирование ASP.NET Web API 2
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> Это руководство и приложения показано, как создать простой модульные тесты для приложения веб-API 2. Этот учебник демонстрирует включают проект модульного теста в решении, а также написать методы теста, проверьте значения, возвращаемого из метода контроллера.
> 
> В учебнике предполагается, что вы знакомы с основными понятиями веб-API ASP.NET. Вводное руководство см. в разделе [Приступая к работе с ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
> 
> Модульные тесты в этом разделе намеренно ограничены простых сценариев. Модульное тестирование более сложных сценариев, данных, в разделе [имитации Entity Framework при веб-тестировании ASP.NET единицы API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
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

    - [При создании приложения, добавьте проект модульного теста](#whencreate)
    - [Добавьте проект модульных тестов для существующего приложения](#addtoexisting)
- [Настройка приложения веб-API 2](#setupproject)
- [Установить пакеты NuGet в тестовый проект](#testpackages)
- [Создание тестов](#tests)
- [Выполнение тестов](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>Предварительные требования

Visual Studio 2017 г Community, Professional или Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>Загрузить исходный код

Загрузить [завершенного проекта](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Загружаемый проект включает в себя код модульного теста для этого раздела, а также для [имитации Entity Framework при модульного тестирования ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) раздела.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Создайте приложение с проект модульного теста

Можно создать проект модульного теста, при создании приложения или добавить проект модульных тестов для существующего приложения. Этот учебник показывает обоих методов для создания проекта модульного теста. Чтобы выполнить этот учебник, можно использовать любой из этих подходов.

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>При создании приложения, добавьте проект модульного теста

Создание нового веб-приложения ASP.NET с именем **StoreApp**.

![Создание проекта](unit-testing-with-aspnet-web-api/_static/image1.png)

В окне нового проекта ASP.NET выберите **пустой** шаблона и добавить папки и основные ссылки для веб-API. Выберите **Добавление модульных тестов** параметр. Автоматически присваивается имя проекта модульного теста **StoreApp.Tests**. Можно оставить это имя.

![Создание проекта модульного теста](unit-testing-with-aspnet-web-api/_static/image2.png)

После создания приложения, вы увидите, что он содержит два проекта.

![два проекта](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>Добавьте проект модульных тестов для существующего приложения

Если вы не создали проект модульного теста при создании приложения, его можно добавить в любое время. Например предположим, имеется приложение с именем StoreApp и вы хотите добавить модульные тесты. Чтобы добавить проект модульного теста, щелкните правой кнопкой мыши решение и выберите **добавить** и **новый проект**.

![Добавить новый проект в решение](unit-testing-with-aspnet-web-api/_static/image4.png)

Выберите **тест** в левой области и выберите **проект модульного теста** для типа проекта. Назовите проект **StoreApp.Tests**.

![Добавьте проект модульного теста](unit-testing-with-aspnet-web-api/_static/image5.png)

Вы увидите проект модульного теста в решении.

В проекте модульного теста добавьте ссылку на проект в исходном проекте.

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>Настройка приложения веб-API 2

В проекте StoreApp добавьте файл класса для **моделей** папку с именем **Product.cs**. Замените содержимое файла следующим кодом.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

Постройте решение.

Щелкните правой кнопкой мыши папку Controllers, а затем выберите **добавить** и **новый элемент формирования шаблонов**. Выберите **пустой контроллер — веб-API 2**.

![Добавить новый контроллер](unit-testing-with-aspnet-web-api/_static/image6.png)

Задайте имя контроллера **SimpleProductController**и нажмите кнопку **добавить**.

![Укажите контроллер](unit-testing-with-aspnet-web-api/_static/image7.png)

Замените существующий код следующим кодом: Чтобы упростить этот пример, данные хранятся в список, а не базы данных. Список, определенный в этот класс представляет производственных данных. Обратите внимание, что контроллер включает конструктор, который принимает список объектов продукта в качестве параметра. Этот конструктор позволяет передавать данные теста при модульном тестировании. Контроллер также содержит две **async** методы для иллюстрации модульное тестирование асинхронных методов. Эти асинхронные методы были реализованы путем вызова **Task.FromResult** для минимизации лишнего кода, но обычно методы включить ресурсоемких операций.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

Метод GetProduct возвращает экземпляр **IHttpActionResult** интерфейса. IHttpActionResult является одним из новых функций в веб-API 2, и он упрощает разработки модульных тестов. Классы, реализующие интерфейс IHttpActionResult находятся в [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) пространства имен. Эти классы представляют возможные ответы из действия запроса, и они соответствуют коды состояния HTTP.

Постройте решение.

Теперь все готово для настройки тестового проекта.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Установить пакеты NuGet в тестовый проект

При использовании пустого шаблона для создания приложения проекта модульного теста (StoreApp.Tests) не включает все установленные пакеты NuGet. Другие шаблоны, например шаблона веб-API, включать несколько пакетов NuGet в проекте модульного теста. В этом учебнике необходимо включить пакет Microsoft ASP.NET Web API 2 Core в тестовый проект.

Щелкните правой кнопкой мыши проект StoreApp.Tests и выберите **управление пакетами NuGet**. Необходимо выбрать проект StoreApp.Tests для добавления пакетов в проект.

![Управление пакетами](unit-testing-with-aspnet-web-api/_static/image8.png)

Найти и установить пакет Microsoft ASP.NET Web API 2 Core.

![Установка основных компонентов api веб-пакета](unit-testing-with-aspnet-web-api/_static/image9.png)

Закройте окно Управление пакетами NuGet.

<a id="tests"></a>
## <a name="create-tests"></a>Создание тестов

По умолчанию тестовый проект входит пустой тестовый файл UnitTest1.cs. Этот файл показаны атрибуты, можно использовать при создании методов теста. Для модульных тестов можно использовать этот файл или создать свой собственный файл.

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

В этом учебнике вы создадите класс теста. Можно удалить файл UnitTest1.cs. Добавьте класс с именем **TestSimpleProductController.cs**и замените код следующим кодом.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Выполнить тесты

Теперь вы готовы к выполнению тестов. Все, которые помечены с помощью метода **TestMethod** атрибут будет проверено. Из **тест** пункт меню, выполнить тесты.

![Запуск тестов](unit-testing-with-aspnet-web-api/_static/image11.png)

Откройте **Test Explorer** окна и проверьте результаты тестов.

![результаты теста](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>Сводка

Учебник успешно пройден. Данные в этом учебнике намеренно была упрощена, чтобы сосредоточиться на модульное тестирование условия. Модульное тестирование более сложных сценариев, данных, в разделе [имитации Entity Framework при веб-тестировании ASP.NET единицы API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
