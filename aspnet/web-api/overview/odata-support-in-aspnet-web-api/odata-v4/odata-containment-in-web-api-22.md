---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Включение в OData v4, с помощью веб-API 2.2 | Документация Майкрософт
author: rick-anderson
description: В большинстве случаев сущности может осуществляться только если он был инкапсулирован в наборе сущностей. Но OData v4 содержит два дополнительных параметра, одноэлементные и Con...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 33ff49f69d70dd3a8179445d2895c418d2185e49
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365611"
---
<a name="containment-in-odata-v4-using-web-api-22"></a>Включение в OData v4, с помощью веб-API 2.2
====================
по Jinfu Tan

> В большинстве случаев сущности может осуществляться только если он был инкапсулирован в наборе сущностей. Но OData v4 содержит два дополнительных параметра, одноэлементные и вложения, каждый из которых поддерживает веб-API 2.2.


В этом разделе показано, как определить параметр автономности в конечную точку OData в веб-API 2.2. Дополнительные сведения о вложения, см. в разделе [вложения поступает с OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx). Чтобы создать конечную точку OData V4 в веб-API, см. в разделе [создания OData v4 конечной точки с помощью веб-API ASP.NET 2.2](create-an-odata-v4-endpoint.md).

Во-первых мы создадим модели домена контейнеров в службе OData с помощью этой модели данных:

![Модель данных](odata-containment-in-web-api-22/_static/image1.png)

Учетной записи содержит много PaymentInstruments (PI), но мы не определяют набор сущностей для PI. Вместо этого команды обработки может осуществляться только через учетную запись.

Вы можете скачать решение, используемые в данном разделе из [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).

## <a name="defining-the-data-model"></a>Определения модели данных

1. Определение типов среды CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    `Contained` Атрибут используется для включения свойств навигации.
2. Создание модели EDM на основе типов среды CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    `ODataConventionModelBuilder` Обрабатывающий построение модели EDM в том случае, если `Contained` атрибут добавляется к соответствующему свойству навигации. Если свойство является типом коллекции `GetCount(string NameContains)` также создается функция.

    Созданные метаданные будут выглядеть следующим образом:

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    `ContainsTarget` Атрибут указывает, что свойство навигации является параметр автономности.

## <a name="define-the-containing-entity-set-controller"></a>Определение содержащего контроллера набора сущностей

Автономной сущности не имеют свои собственные контроллера; действие определяется в контроллере, содержащего набор сущностей. В этом примере есть AccountsController, но не PaymentInstrumentsController.

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

Если путь OData 4 и более сегментов, только атрибут маршрутизации работает, например `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` выше контроллера. В противном случае — работает как атрибут, так и маршрутизации на основе соглашений: например, `GetPayInPIs(int key)` соответствует `GET ~/Accounts(1)/PayinPIs`.

*Благодаря Leo Hu для исходного содержимого этой статьи.*
