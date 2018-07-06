---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Включение в OData v4, с помощью веб-API 2.2 | Документация Майкрософт
author: rick-anderson
description: В большинстве случаев сущности может осуществляться только если он был инкапсулирован в наборе сущностей. Но OData v4 содержит два дополнительных параметра, одноэлементные и Con...
ms.author: aspnetcontent
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 56e550b56e9ad237dbf4fab04f2bd545164ee90a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824916"
---
<a name="containment-in-odata-v4-using-web-api-22"></a><span data-ttu-id="4bbaa-104">Включение в OData v4, с помощью веб-API 2.2</span><span class="sxs-lookup"><span data-stu-id="4bbaa-104">Containment in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="4bbaa-105">по Jinfu Tan</span><span class="sxs-lookup"><span data-stu-id="4bbaa-105">by Jinfu Tan</span></span>

> <span data-ttu-id="4bbaa-106">В большинстве случаев сущности может осуществляться только если он был инкапсулирован в наборе сущностей.</span><span class="sxs-lookup"><span data-stu-id="4bbaa-106">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="4bbaa-107">Но OData v4 содержит два дополнительных параметра, одноэлементные и вложения, каждый из которых поддерживает веб-API 2.2.</span><span class="sxs-lookup"><span data-stu-id="4bbaa-107">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="4bbaa-108">В этом разделе показано, как определить параметр автономности в конечную точку OData в веб-API 2.2.</span><span class="sxs-lookup"><span data-stu-id="4bbaa-108">This topic shows how to define a containment in an OData endpoint in WebApi 2.2.</span></span> <span data-ttu-id="4bbaa-109">Дополнительные сведения о вложения, см. в разделе [вложения поступает с OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span><span class="sxs-lookup"><span data-stu-id="4bbaa-109">For more information about containment, see [Containment is coming with OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span></span> <span data-ttu-id="4bbaa-110">Чтобы создать конечную точку OData V4 в веб-API, см. в разделе [создания OData v4 конечной точки с помощью веб-API ASP.NET 2.2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="4bbaa-110">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="4bbaa-111">Во-первых мы создадим модели домена контейнеров в службе OData с помощью этой модели данных:</span><span class="sxs-lookup"><span data-stu-id="4bbaa-111">First, we'll create a containment domain model in the OData service, using this data model:</span></span>

![Модель данных](odata-containment-in-web-api-22/_static/image1.png)

<span data-ttu-id="4bbaa-113">Учетной записи содержит много PaymentInstruments (PI), но мы не определяют набор сущностей для PI.</span><span class="sxs-lookup"><span data-stu-id="4bbaa-113">An account contains many PaymentInstruments (PI), but we don't define an entity set for a PI.</span></span> <span data-ttu-id="4bbaa-114">Вместо этого команды обработки может осуществляться только через учетную запись.</span><span class="sxs-lookup"><span data-stu-id="4bbaa-114">Instead, the PIs can only be accessed through an Account.</span></span>

<span data-ttu-id="4bbaa-115">Вы можете скачать решение, используемые в данном разделе из [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span><span class="sxs-lookup"><span data-stu-id="4bbaa-115">You can download the solution used in this topic from [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span></span>

## <a name="defining-the-data-model"></a><span data-ttu-id="4bbaa-116">Определения модели данных</span><span class="sxs-lookup"><span data-stu-id="4bbaa-116">Defining the data model</span></span>

1. <span data-ttu-id="4bbaa-117">Определение типов среды CLR.</span><span class="sxs-lookup"><span data-stu-id="4bbaa-117">Define the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    <span data-ttu-id="4bbaa-118">`Contained` Атрибут используется для включения свойств навигации.</span><span class="sxs-lookup"><span data-stu-id="4bbaa-118">The `Contained` attribute is used for containment navigation properties.</span></span>
2. <span data-ttu-id="4bbaa-119">Создание модели EDM на основе типов среды CLR.</span><span class="sxs-lookup"><span data-stu-id="4bbaa-119">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="4bbaa-120">`ODataConventionModelBuilder` Обрабатывающий построение модели EDM в том случае, если `Contained` атрибут добавляется к соответствующему свойству навигации.</span><span class="sxs-lookup"><span data-stu-id="4bbaa-120">The `ODataConventionModelBuilder` will handle building the EDM model if the `Contained` attribute is added to the corresponding navigation property.</span></span> <span data-ttu-id="4bbaa-121">Если свойство является типом коллекции `GetCount(string NameContains)` также создается функция.</span><span class="sxs-lookup"><span data-stu-id="4bbaa-121">If the property is a collection type, a `GetCount(string NameContains)` function will also be created.</span></span>

    <span data-ttu-id="4bbaa-122">Созданные метаданные будут выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="4bbaa-122">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    <span data-ttu-id="4bbaa-123">`ContainsTarget` Атрибут указывает, что свойство навигации является параметр автономности.</span><span class="sxs-lookup"><span data-stu-id="4bbaa-123">The `ContainsTarget` attribute indicates that the navigation property is a containment.</span></span>

## <a name="define-the-containing-entity-set-controller"></a><span data-ttu-id="4bbaa-124">Определение содержащего контроллера набора сущностей</span><span class="sxs-lookup"><span data-stu-id="4bbaa-124">Define the containing entity set controller</span></span>

<span data-ttu-id="4bbaa-125">Автономной сущности не имеют свои собственные контроллера; действие определяется в контроллере, содержащего набор сущностей.</span><span class="sxs-lookup"><span data-stu-id="4bbaa-125">Contained entities don't have their own controller; the action is defined in the containing entity set controller.</span></span> <span data-ttu-id="4bbaa-126">В этом примере есть AccountsController, но не PaymentInstrumentsController.</span><span class="sxs-lookup"><span data-stu-id="4bbaa-126">In this sample, there is an AccountsController, but no PaymentInstrumentsController.</span></span>

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

<span data-ttu-id="4bbaa-127">Если путь OData 4 и более сегментов, только атрибут маршрутизации работает, например `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` выше контроллера.</span><span class="sxs-lookup"><span data-stu-id="4bbaa-127">If the OData path is 4 or more segments, only attribute routing works, such as `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` in the above controller.</span></span> <span data-ttu-id="4bbaa-128">В противном случае — работает как атрибут, так и маршрутизации на основе соглашений: например, `GetPayInPIs(int key)` соответствует `GET ~/Accounts(1)/PayinPIs`.</span><span class="sxs-lookup"><span data-stu-id="4bbaa-128">Otherwise, both attribute and conventional routing works: for instance, `GetPayInPIs(int key)` matches `GET ~/Accounts(1)/PayinPIs`.</span></span>

<span data-ttu-id="4bbaa-129">*Благодаря Leo Hu для исходного содержимого этой статьи.*</span><span class="sxs-lookup"><span data-stu-id="4bbaa-129">*Thanks to Leo Hu for the original content of this article.*</span></span>
