---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Создайте конечную точку OData версии 4 с помощью ASP.NET Web API 2.2 | Документация Майкрософт
author: MikeWasson
description: Open Data Protocol (OData) — это протокол доступа к данным веб-приложений. OData предоставляет универсальный способ запросов и управление ими наборов данных с помощью операции CRUD...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: c6a4aa4eb563fd77d5afd9248175d5f5b7984d19
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667574"
---
# <a name="create-an-odata-v4-endpoint-using-aspnet-web-api"></a>Создайте конечную точку OData версии 4 с помощью веб-API ASP.NET 

> Open Data Protocol (OData) — это протокол доступа к данным веб-приложений. OData предоставляет универсальный способ запросов и управление ими наборов данных с помощью операции CRUD (Создание, чтение, обновление и удаление).
>
> Веб-API ASP.NET поддерживает v3 и v4 протокола. Может иметь конечную точку версии 4, которая запускает side-by-side с конечной точкой v3.
>
> Этом руководстве показано, как создать конечную точку OData v4, поддерживающий операции CRUD.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
>
> - Веб-API 5.2
> - OData v4
> - Visual Studio 2017 (скачать Visual Studio 2017 [здесь](https://visualstudio.microsoft.com/downloads/))
> - Entity Framework 6
> - .NET 4.7.2
>
> ## <a name="tutorial-versions"></a>Учебника по версии
>
> OData версии 3, см. в разделе [Создание конечной точки OData v3](../odata-v3/creating-an-odata-endpoint.md).

## <a name="create-the-visual-studio-project"></a>Создание проекта Visual Studio

В Visual Studio из **файл** меню, выберите **New** &gt; **проекта**.

Разверните **установленные** &gt; **Visual C#**  &gt; **Web**и выберите **веб-приложение ASP.NET (.NET Framework)**  шаблона. Назовите проект &quot;ProductService&quot;.

[![](create-an-odata-v4-endpoint/_static/image7.png)](create-an-odata-v4-endpoint/_static/image7.png)

Нажмите кнопку **ОК**.



[![](create-an-odata-v4-endpoint/_static/image8.png)](create-an-odata-v4-endpoint/_static/image8.png)

Выберите **пустой** шаблона. В разделе **добавить папки и основные ссылки для:** выберите **веб-API**. Нажмите кнопку **ОК**.

## <a name="install-the-odata-packages"></a>Установите пакеты OData

В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** &gt; **Консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующую команду:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

Эта команда устанавливает последнюю версию пакетов OData NuGet.

## <a name="add-a-model-class"></a>Добавление класса модели

Объект *модели* — это объект, представляющий сущность данных в приложении.

В обозревателе решений щелкните правой кнопкой мыши папку Models. В контекстном меню выберите **добавить** &gt; **класс**.

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> По соглашению классы моделей, помещаются в папку Models, но у вас нет следуют соглашению в своих собственных проектах.


Присвойте классу имя `Product`. В файле замените код шаблона следующим:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

`Id` Свойство является ключом сущности. Клиенты могут запрашивать сущности по ключу. Например, чтобы получить продукт с Идентификатором 5, URI имеет `/Products(5)`. `Id` Свойство также будет иметь первичный ключ в базе данных серверной части.

## <a name="enable-entity-framework"></a>Включить Entity Framework

В этом учебнике мы будем использовать Entity Framework (EF) Code First для создания серверной базы данных.

> [!NOTE]
> Веб-API OData EF не требуется. Используйте любой уровень доступа к данным, который можно преобразовать сущностей базы данных в модели.


Во-первых установите пакет NuGet для Entity FRAMEWORK. В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** &gt; **Консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующую команду:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

Откройте файл Web.config и добавьте следующий раздел внутри **конфигурации** элемент после **configSections** элемент.

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

Этот параметр добавляет строку подключения для базы данных LocalDB. Эта база данных будет использоваться при локальном запуске приложения.

Добавьте класс с именем `ProductsContext` папке «модели»:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

В конструкторе `"name=ProductsContext"` предоставляет имя строки подключения.

## <a name="configure-the-odata-endpoint"></a>Настройка конечной точки OData

Откройте файл приложения\_Start/WebApiConfig.cs. Добавьте следующий **с помощью** инструкции:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

Затем добавьте следующий код, чтобы **зарегистрировать** метод:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

Этот код делает две вещи:

- Создает Entity Data Model (EDM).
- Добавление маршрута.

EDM является абстрактной моделью данных. Модель EDM используется для создания документ метаданных службы. **ODataConventionModelBuilder** класс создает EDM с помощью соглашения об именовании по умолчанию. Этот подход требует наименьшего объема кода. Если требуется больший контроль над модели EDM, можно использовать **ODataModelBuilder** класс, чтобы создать модель EDM путем добавления свойств, разделов и свойств навигации явным образом.

Объект *маршрута* сообщает способ маршрутизации HTTP-запросы на конечную точку веб-API. Чтобы создать маршрут OData v4, вызовите **MapODataServiceRoute** метода расширения.

Если приложение имеет несколько конечных точек OData, создайте отдельный маршрут для каждого. Присвойте каждого маршрута, уникальное имя маршрута и префикса.

## <a name="add-the-odata-controller"></a>Добавление контроллера OData

Объект *контроллера* является классом, который обрабатывает HTTP-запросы. Можно создать отдельный контроллер для каждого набора сущностей в службе OData. В этом руководстве вы создадите один контроллер для `Product` сущности.

В обозревателе решений щелкните правой кнопкой мыши папку Controllers и выберите **добавить** &gt; **класс**. Присвойте классу имя `ProductsController`.

> [!NOTE]
> Версия этого руководства для OData v3 использует **Добавление контроллера** формирования шаблонов. В настоящее время нет без формирования шаблонов для OData v4.


Замените код в ProductsController.cs следующим.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

Использует контроллер `ProductsContext` класс для доступа к базе данных с помощью EF. Обратите внимание, что контроллер переопределяет **Dispose** метод **ProductsContext**.

Это отправной точкой для контроллера. Далее мы добавим методы для всех операций CRUD.

## <a name="query-the-entity-set"></a>Запрос набора сущностей

Добавьте следующие методы для `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

Без параметров версию `Get` метод возвращает всей коллекции продуктов. `Get` Метод с *ключ* параметр ищет продукта по его ключу (в этом случае `Id` свойство).

**[EnableQuery]** атрибут позволяет клиентам изменить этот запрос, используя параметры запроса $filter, $sort и $page. Дополнительные сведения см. в разделе [поддержки параметров запроса OData](../supporting-odata-query-options.md).

## <a name="add-an-entity-to-the-entity-set"></a>Добавление сущности набора сущностей

Чтобы клиенты могли добавить новый продукт в базу данных, добавьте следующий метод в `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="update-an-entity"></a>Обновить сущности

OData поддерживает два разных семантику для обновления сущностей, ИСПРАВЛЕНИЯМИ и PUT.

- PATCH выполняет частичное обновление. Клиент указывает только те свойства, для обновления.
- PUT заменяет всю сущность.

Недостатком PUT является то, что клиент должен отправлять значения для всех свойств в сущности, включая значения, которые не изменяются. [Спецификации OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) утверждается, что исправление является предпочтительным.

Ниже приведен код для методов PUT и PATCH в любом случае:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

В случае с ИСПРАВЛЕНИЯМИ, контроллер использует **разностных&lt;T&gt;**  тип для отслеживания изменений.

## <a name="delete-an-entity"></a>Удаление сущности

Чтобы клиенты могли удалить продукт из базы данных, добавьте следующий метод в `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
