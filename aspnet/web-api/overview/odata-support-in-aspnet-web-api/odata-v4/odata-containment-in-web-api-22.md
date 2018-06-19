---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Вложение в OData v4, с помощью веб-API 2.2 | Документы Microsoft
author: rick-anderson
description: 'В большинстве случаев сущность может осуществляться только, если он был инкапсулирован в наборе сущностей. Однако OData v4 содержит два дополнительных параметра: одноэлементных, так и Con...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 7d3c81bf3d2a43faa3e71155637e031f81143782
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508003"
---
<a name="containment-in-odata-v4-using-web-api-22"></a>Вложение в OData v4, с помощью веб-API 2.2
====================
по Jinfu Tan

> В большинстве случаев сущность может осуществляться только, если он был инкапсулирован в наборе сущностей. Но OData v4 предоставляет два дополнительных варианта одноэлементных, так и вложения, которые поддерживает WebAPI 2.2.


В этом разделе показано, как определить параметр автономности в конечную точку OData в WebApi 2.2. Дополнительные сведения о вложения см. в разделе [вложения поступает с OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx). Создание конечной точки OData V4 в веб-API — [создания OData v4 конечной точки с помощью веб-API ASP.NET 2.2](create-an-odata-v4-endpoint.md).

Во-первых мы создадим модель домена вложения в службе OData с помощью этой модели данных:

![Модель данных](odata-containment-in-web-api-22/_static/image1.png)

Учетная запись содержит много PaymentInstruments (PI), но мы не был определен набор сущностей для PI. Вместо этого команды обработки может осуществляться только через учетную запись.

Вы можете загрузить решение, используемые в данном разделе из [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).

## <a name="defining-the-data-model"></a>Определение модели данных

1. Определение типов среды CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    `Contained` Атрибут используется для включения свойства навигации.
2. Создание модели EDM на основе типов среды CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    `ODataConventionModelBuilder` Будет обрабатывать построение модели EDM, если `Contained` атрибут добавляется в соответствующее свойство навигации. Если свойство является типом коллекции, `GetCount(string NameContains)` также создается функция.

    Созданные метаданные будут выглядеть следующим образом:

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    `ContainsTarget` Атрибут указывает, что свойство навигации вложения.

## <a name="define-the-containing-entity-set-controller"></a>Определения содержащего контроллера набора сущностей

Автономной сущности не имеют своих собственных контроллера; действие определяется в содержащий контроллера набора сущностей. В этом образце есть AccountsController, но не PaymentInstrumentsController.

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

Если 4 или более сегментов пути OData, только атрибут работает маршрутизация, например `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` в контроллере выше. В противном случае работает как атрибут, так и Обычная маршрутизация: например, `GetPayInPIs(int key)` соответствует `GET ~/Accounts(1)/PayinPIs`.

*Спасибо, что для Leo Hu исходного содержимого этой статьи.*
