---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: "Создание единственного экземпляра с помощью веб-API 2.2 OData v4 | Документы Microsoft"
author: rick-anderson
description: "В этом разделе показано, как определить одноэлементный в конечную точку OData в Web API 2.2."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 92c5056548b1e39defb28ac36f83b001dd108f5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Создание единственного экземпляра с помощью веб-API 2.2 OData v4
====================
по Zoe Luo

> В большинстве случаев сущность может осуществляться только, если он был инкапсулирован в наборе сущностей. Но OData v4 предоставляет два дополнительных варианта одноэлементных, так и вложения, которые поддерживает WebAPI 2.2.


В этой статье показан способ определения одноэлементный в конечную точку OData в Web API 2.2. Сведения о какой одноэлементный и как можно повысить его использования в разделе [использование единственное значение для определения специальные сущности](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx). Создание конечной точки OData V4 в веб-API — [создания OData v4 конечной точки с помощью веб-API ASP.NET 2.2](create-an-odata-v4-endpoint.md). 

Мы создадим Singleton-классом в проекте веб-API, с помощью следующей модели данных:

![Модель данных](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

Singleton-классом с именем `Umbrella` определяются на основе типа `Company`и набор именованных сущностей `Employees` определяются на основе типа `Employee`.

Решения, используемые в этом учебнике можно загрузить из [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).

## <a name="define-the-data-model"></a>Определение модели данных

1. Определение типов среды CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. Создание модели EDM на основе типов среды CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    Здесь `builder.Singleton<Company>("Umbrella")` сообщает построителю модели для создания Singleton-классом с именем `Umbrella` модели EDM.

    Созданные метаданные будут выглядеть следующим образом:

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    Из метаданных можно видеть, что свойство навигации `Company` в `Employees` набора сущностей, привязанный к singleton `Umbrella`. Привязка выполняется автоматически `ODataConventionModelBuilder`, так как только `Umbrella` имеет `Company` типа. Если любая неопределенность в модели, можно использовать `HasSingletonBinding` явно привязать свойство навигации в один элемент; `HasSingletonBinding` действует так же, как с помощью `Singleton` атрибута в определении типа CLR:

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>Определить одноэлементный контроллера

Как контроллер EntitySet, одноэлементный контроллер наследует от `ODataController`, и имя контроллера singleton должен быть `[singletonName]Controller`.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

Для обработки различных типов запросов, действия должны быть предварительно определенные в контроллере. **Атрибут маршрутизации** включена по умолчанию в WebApi 2.2. Например, чтобы определить действие для обработки запросов `Revenue` из `Company` с помощью атрибута маршрутизации, используйте следующее:

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

Если вы не хотите определить атрибуты для каждого действия, достаточно определить свои действия после [соглашение о маршрутизации OData](../odata-routing-conventions.md). Поскольку ключ не является обязательным для одноэлементный запрос, действия, определенные в контроллере одноэлементный несколько отличаются от действий, указанных в этом контроллере entityset.

Справочник по сигнатуры методов для каждого определения действия в контроллере singleton, перечислены ниже.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

По сути это все, что необходимо выполнить на стороне службы. [Образец проекта](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) содержит весь код для решения и клиента OData, который показывает использование единственного экземпляра. Клиент создается с помощью инструкции в [Создание клиентского приложения OData v4](create-an-odata-v4-client-app.md).

. 

*Спасибо, что для Leo Hu исходного содержимого этой статьи.*
