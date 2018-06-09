---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Создайте конечную точку OData v4 с помощью ASP.NET Web API 2.2 | Документы Microsoft
author: MikeWasson
description: Протокол Open Data Protocol (OData) — это протокол доступа к данным для веб. OData предоставляет единообразный способ запроса и управления наборами данных через операции CRUD...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/24/2014
ms.topic: article
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: a3f94818f9674b0e1e9a45b2a6cc9455edc79726
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "26508053"
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a>Создайте конечную точку OData v4 с помощью ASP.NET Web API 2.2
====================
по [Mike Wasson](https://github.com/MikeWasson)

> Протокол Open Data Protocol (OData) — это протокол доступа к данным для веб. OData предоставляет единообразный способ запроса и управления наборами данных через операций CRUD (Создание, чтение, обновление и удаление).
> 
> Веб-API ASP.NET поддерживает v3 и v4 протокола. Может содержать конечную точку v4, запускаемую side-by-side с конечной точкой v3.
> 
> Этого учебника показано, как создать конечную точку OData v4, которая поддерживает операции CRUD.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - Веб-API 2.2
> - OData v4
> - [Visual Studio 2013 с обновлением 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Версии учебника
> 
> OData версии 3, в разделе [Создание конечной точки OData v3](../odata-v3/creating-an-odata-endpoint.md).


## <a name="create-the-visual-studio-project"></a>Создание проекта Visual Studio

В Visual Studio из **файл** последовательно выберите пункты **New** &gt; **проекта**.

Разверните **установленные** &gt; **шаблоны** &gt; **Visual C#** &gt; **Web**и выберите  **Веб-приложение ASP.NET** шаблона. Назовите проект &quot;ProductService&quot;.

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

В **новый проект** диалогового окна выберите **пустой** шаблона. В разделе &quot;добавить папки и основные ссылки... &quot;, нажмите кнопку **веб-API**. Нажмите кнопку **ОК**.

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a>Установить пакеты OData

Из **средства** последовательно выберите пункты **диспетчера пакетов NuGet** &gt; **консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующее:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

Эта команда устанавливает последние пакеты OData NuGet.

## <a name="add-a-model-class"></a>Добавьте класс модели

Объект *модель* — это объект, представляющий сущность данных в приложении.

В обозревателе решений щелкните правой кнопкой мыши папку модели. В контекстном меню выберите **добавить** &gt; **класса**.

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> По соглашению классы модели, помещаются в папку Models, но нет необходимости соответствуют этому соглашению в собственных проектах.


Присвойте классу имя `Product`. В файле Product.cs замените стандартный код следующим:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

`Id` Является ключом сущности. Клиент может запрашивать сущности по ключу. Например, чтобы получить продукта с Идентификатором 5, URI имеет следующий вид `/Products(5)`. `Id` Свойство будет первичного ключа в серверную базу данных.

## <a name="enable-entity-framework"></a>Включение платформы Entity Framework

В этом учебнике мы будем использовать Entity Framework (EF) Code First для создания базы данных с таблицами.

> [!NOTE]
> EF не требует OData веб-API. Используйте любой слой доступа к данным, может преобразовывать сущностей базы данных в модели.


Во-первых необходимо установите пакет NuGet для EF. Из **средства** последовательно выберите пункты **диспетчера пакетов NuGet** &gt; **консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующее:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

Откройте файл Web.config и добавьте следующий раздел в **конфигурации** элемент после того, как **configSections** элемента.

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

Этот параметр добавляет строку подключения для базы данных LocalDB. Эта база данных будет использоваться при локальном запуске приложения.

Добавьте класс с именем `ProductsContext` папке «модели»:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

В конструкторе `"name=ProductsContext"` предоставляет имя строки подключения.

## <a name="configure-the-odata-endpoint"></a>Настройка конечной точки OData

Откройте файл приложения\_Start/WebApiConfig.cs. Добавьте следующие **с помощью** инструкции:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

Затем добавьте следующий код в **зарегистрировать** метод:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

Этот код выполняет следующие операции:

- Создает модель EDM (модель EDM).
- Добавляет маршрут.

EDM-это абстрактный модель данных. Модель EDM используется для создания документ метаданных службы. **ODataConventionModelBuilder** класс создает EDM, с использованием контекста именования по умолчанию. Этот подход требует наименьшего объема кода. Если требуется больший контроль над модели EDM, можно использовать **ODataModelBuilder** класса для создания модели EDM, добавив явным образом свойства, ключи и свойства навигации.

Объект *маршрута* узнает перенаправления HTTP-запросов к конечной точке веб-API. Чтобы создать маршрут OData v4, вызовите **MapODataServiceRoute** метода расширения.

Если приложение имеет несколько конечных точек OData, создайте отдельный маршрут для каждого. Укажите каждый маршрут маршрута, уникальное имя и префикс.

## <a name="add-the-odata-controller"></a>Добавить контроллер OData

Объект *контроллера* — это класс, который обрабатывает HTTP-запросы. Создается отдельный контроллер для каждого набора сущностей в службе OData. В этом учебнике вы создадите один контроллер для `Product` сущности.

В обозревателе решений щелкните правой кнопкой мыши папку Controllers и выбрать **добавить** &gt; **класса**. Присвойте классу имя `ProductsController`.

> [!NOTE]
> Версия этого учебника для OData v3 использует **добавить контроллер** формирования шаблонов. В настоящее время нет без формирования шаблонов для OData v4.


Замените код для шаблона в ProductsController.cs ниже.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

Использует контроллер `ProductsContext` класса для доступа к базе данных с помощью EF. Обратите внимание, что контроллер переопределяет **Dispose** метод **ProductsContext**.

Это является начальной точкой для контроллера. Далее мы добавим методы для всех операций CRUD.

## <a name="querying-the-entity-set"></a>Запрос к набору сущностей

Добавьте следующие методы для `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

Без параметров версию `Get` метод возвращает всю коллекцию продуктов. `Get` Метод с *ключ* параметр ищет продукта по его ключу (в этом случае `Id` свойство).

**[EnableQuery]** атрибутов позволяет изменить этот запрос, используя параметры запроса $filter, $sort и $page клиентам. Дополнительные сведения см. в разделе [поддержки параметров запроса OData](../supporting-odata-query-options.md).

## <a name="adding-an-entity-to-the-entity-set"></a>Добавление сущности в наборе сущностей

Чтобы клиенты могли добавить новый продукт в базе данных, добавьте следующий метод `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a>Обновление сущности

Обновление сущности, PUT и PATCH OData поддерживает два различную семантику.

- ИСПРАВЛЕНИЕ выполняет частичное обновление. Клиент указывает только те свойства, для обновления.
- PUT заменяет всю сущность.

Недостатком PUT является то, что клиент должен отправить значения для всех свойств в сущности, включая значения, которые не изменяются. [Спецификации OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) утверждается, что исправление является предпочтительным.

Ниже приведен код для методов PUT и PATCH в любом случае:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

В случае обновления, контроллер использует **дельта&lt;T&gt;**  тип для отслеживания изменений.

## <a name="deleting-an-entity"></a>Удаление сущности

Чтобы клиенты могли удалить продукт из базы данных, добавьте следующий метод `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
