---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Создание единичного экземпляра в OData v4 с помощью веб-API 2.2 | Документация Майкрософт
author: rick-anderson
description: В этом разделе показано, как определить одноэлементный в конечную точку OData в веб-API 2.2.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 3ae9b23fae356e387a011a190119d760dc46d022
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390931"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Создание единичного экземпляра в OData v4 с помощью веб-API 2.2
====================
с ло Zoe

> В большинстве случаев сущности может осуществляться только если он был инкапсулирован в наборе сущностей. Но OData v4 содержит два дополнительных параметра, одноэлементные и вложения, каждый из которых поддерживает веб-API 2.2.


В этой статье показано, как определить одноэлементный в конечную точку OData в веб-API 2.2. Сведения о какие единственный экземпляр есть, и как можно обеспечить с помощью его, см. в разделе [с помощью является одноэлементным множеством, определяющих специальные сущности](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx). Чтобы создать конечную точку OData V4 в веб-API, см. в разделе [создания OData v4 конечной точки с помощью веб-API ASP.NET 2.2](create-an-odata-v4-endpoint.md). 

Мы создадим единственный элемент в проекте веб-API с использованием следующей модели данных:

![Модель данных](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

Единственный экземпляр с именем `Umbrella` будет определяться в зависимости от типа `Company`и набор именованных сущностей `Employees` будет определяться в зависимости от типа `Employee`.

Решения, используемые в этом руководстве можно загрузить из [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).

## <a name="define-the-data-model"></a>Определение модели данных

1. Определение типов среды CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. Создание модели EDM на основе типов среды CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    Здесь `builder.Singleton<Company>("Umbrella")` сообщает построитель модели, чтобы создать единственный экземпляр с именем `Umbrella` модели EDM.

    Созданные метаданные будут выглядеть следующим образом:

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    Из метаданных мы видим, что свойство навигации `Company` в `Employees` набор сущностей привязан к одноэлементного `Umbrella`. Привязка выполняется автоматически `ODataConventionModelBuilder`, так как только `Umbrella` имеет `Company` типа. Есть ли какая-либо неопределенность в модели, можно использовать `HasSingletonBinding` явно связать свойства навигации в один элемент; `HasSingletonBinding` имеет тот же эффект, как с помощью `Singleton` атрибута в определение типа CLR:

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>Определение контроллера одноэлементный

Как контроллер EntitySet, одноэлементный контроллер наследует от `ODataController`, и должно быть имя контроллера одноэлементный `[singletonName]Controller`.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

Для обработки различных типов запросов, действия, должны быть предварительно определенные в контроллере. **Маршрутизация атрибутов** включена по умолчанию в веб-API 2.2. Например, чтобы определить действие для обработки запросов `Revenue` из `Company` использующим маршрутизацию атрибутов, используйте следующую команду:

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

Если вы не хотите определить атрибуты для каждого действия, просто определите следующие действия [соглашений о маршрутизации протокола OData](../odata-routing-conventions.md). Поскольку ключ не является обязательным для выполнения запроса является одноэлементным множеством, действия, определенные в контроллере одноэлементный немного отличаются от действий, указанных в этом контроллере entityset.

Для справки сигнатуры методов для каждого определения действий в контроллере, singleton, перечислены ниже.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

По сути это все, что вам нужно сделать на стороне службы. [Пример проекта](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) содержит весь код для решения и клиент OData, показывающий, как использовать единственный экземпляр. Построение клиента, выполнив действия, описанные в [Создание клиентского приложения OData v4](create-an-odata-v4-client-app.md).

. 

*Благодаря Leo Hu для исходного содержимого этой статьи.*
