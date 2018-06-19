---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Сложный тип наследования в OData v4 с веб-API ASP.NET | Документы Microsoft
author: microsoft
description: Согласно спецификации OData v4 сложный тип может наследовать от другого сложного типа. (Сложного типа является структурированного типа без ключа). Веб-API...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: be2dbfa82b99b6c48928e4e767716852c14a463b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508423"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="31da4-104">Сложный тип наследования в OData v4 с веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="31da4-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="31da4-105">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="31da4-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="31da4-106">В соответствии с OData v4 [спецификации](http://www.odata.org/documentation/odata-version-4-0/), сложный тип может наследовать от другого сложного типа.</span><span class="sxs-lookup"><span data-stu-id="31da4-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="31da4-107">(A *сложных* тип — структурированного типа без ключа.) Веб-API OData 5.3 поддерживает наследование сложного типа.</span><span class="sxs-lookup"><span data-stu-id="31da4-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="31da4-108">В этом разделе показано, как построить модель EDM (модель EDM) с наследование, сложные типы.</span><span class="sxs-lookup"><span data-stu-id="31da4-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="31da4-109">Полный исходный код в разделе [OData сложный пример наследования типов](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="31da4-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="31da4-110">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="31da4-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="31da4-111">OData веб-API 5.3</span><span class="sxs-lookup"><span data-stu-id="31da4-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="31da4-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="31da4-112">OData v4</span></span>


## <a name="model-hierarchy"></a><span data-ttu-id="31da4-113">Иерархия модели</span><span class="sxs-lookup"><span data-stu-id="31da4-113">Model Hierarchy</span></span>

<span data-ttu-id="31da4-114">Чтобы продемонстрировать наследование сложных типов, мы будем использовать следующие иерархии классов.</span><span class="sxs-lookup"><span data-stu-id="31da4-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="31da4-115">`Shape`является абстрактным сложным типом.</span><span class="sxs-lookup"><span data-stu-id="31da4-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="31da4-116">`Rectangle`, `Triangle`, и `Circle` являются сложными типами, производными от `Shape`, и `RoundRectangle` является производным от `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="31da4-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="31da4-117">`Window`Тип сущности и содержит `Shape` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="31da4-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="31da4-118">Ниже приведены классы среды CLR, которые определяют эти типы.</span><span class="sxs-lookup"><span data-stu-id="31da4-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="31da4-119">Построение модели EDM</span><span class="sxs-lookup"><span data-stu-id="31da4-119">Build the EDM Model</span></span>

<span data-ttu-id="31da4-120">Создание модели EDM, можно использовать **ODataConventionModelBuilder**, котором выводит отношения наследования из типов среды CLR.</span><span class="sxs-lookup"><span data-stu-id="31da4-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="31da4-121">Можно также создать модель EDM явно, с помощью **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="31da4-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="31da4-122">Принимает дополнительный код, но обеспечивает больший контроль над модели EDM.</span><span class="sxs-lookup"><span data-stu-id="31da4-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="31da4-123">Эти два примера создания одной и той же схемы модели EDM.</span><span class="sxs-lookup"><span data-stu-id="31da4-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="31da4-124">Документ метаданных</span><span class="sxs-lookup"><span data-stu-id="31da4-124">Metadata Document</span></span>

<span data-ttu-id="31da4-125">Ниже приведен документ метаданных OData, показывающая наследование сложного типа.</span><span class="sxs-lookup"><span data-stu-id="31da4-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="31da4-126">Из документа метаданных можно видеть, что:</span><span class="sxs-lookup"><span data-stu-id="31da4-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="31da4-127">`Shape` Сложный тип является абстрактным.</span><span class="sxs-lookup"><span data-stu-id="31da4-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="31da4-128">`Rectangle`, `Triangle`, И `Circle` сложный тип имеет базовый тип `Shape`.</span><span class="sxs-lookup"><span data-stu-id="31da4-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="31da4-129">`RoundRectangle` Тип имеет базовый тип `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="31da4-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="31da4-130">Приведение сложных типов</span><span class="sxs-lookup"><span data-stu-id="31da4-130">Casting Complex Types</span></span>

<span data-ttu-id="31da4-131">Теперь поддерживается приведение в сложных типах.</span><span class="sxs-lookup"><span data-stu-id="31da4-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="31da4-132">Например, следующий запрос приведения `Shape` для `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="31da4-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="31da4-133">Вот полезные данные ответа.</span><span class="sxs-lookup"><span data-stu-id="31da4-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
