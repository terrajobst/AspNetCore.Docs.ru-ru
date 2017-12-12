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
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a><span data-ttu-id="c7537-103">Создание единственного экземпляра с помощью веб-API 2.2 OData v4</span><span class="sxs-lookup"><span data-stu-id="c7537-103">Create a Singleton in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="c7537-104">по Zoe Luo</span><span class="sxs-lookup"><span data-stu-id="c7537-104">by Zoe Luo</span></span>

> <span data-ttu-id="c7537-105">В большинстве случаев сущность может осуществляться только, если он был инкапсулирован в наборе сущностей.</span><span class="sxs-lookup"><span data-stu-id="c7537-105">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="c7537-106">Но OData v4 предоставляет два дополнительных варианта одноэлементных, так и вложения, которые поддерживает WebAPI 2.2.</span><span class="sxs-lookup"><span data-stu-id="c7537-106">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="c7537-107">В этой статье показан способ определения одноэлементный в конечную точку OData в Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="c7537-107">This article shows how to define a singleton in an OData endpoint in Web API 2.2.</span></span> <span data-ttu-id="c7537-108">Сведения о какой одноэлементный и как можно повысить его использования в разделе [использование единственное значение для определения специальные сущности](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span><span class="sxs-lookup"><span data-stu-id="c7537-108">For information on what a singleton is and how you can benefit from using it, see [Using a singleton to define your special entity](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span></span> <span data-ttu-id="c7537-109">Создание конечной точки OData V4 в веб-API — [создания OData v4 конечной точки с помощью веб-API ASP.NET 2.2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="c7537-109">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span> 

<span data-ttu-id="c7537-110">Мы создадим Singleton-классом в проекте веб-API, с помощью следующей модели данных:</span><span class="sxs-lookup"><span data-stu-id="c7537-110">We'll create a singleton in your Web API project using the following data model:</span></span>

![Модель данных](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

<span data-ttu-id="c7537-112">Singleton-классом с именем `Umbrella` определяются на основе типа `Company`и набор именованных сущностей `Employees` определяются на основе типа `Employee`.</span><span class="sxs-lookup"><span data-stu-id="c7537-112">A singleton named `Umbrella` will be defined based on type `Company`, and an entity set named `Employees` will be defined based on type `Employee`.</span></span>

<span data-ttu-id="c7537-113">Решения, используемые в этом учебнике можно загрузить из [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span><span class="sxs-lookup"><span data-stu-id="c7537-113">The solution used in this tutorial can be downloaded from [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="c7537-114">Определение модели данных</span><span class="sxs-lookup"><span data-stu-id="c7537-114">Define the data model</span></span>

1. <span data-ttu-id="c7537-115">Определение типов среды CLR.</span><span class="sxs-lookup"><span data-stu-id="c7537-115">Define the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. <span data-ttu-id="c7537-116">Создание модели EDM на основе типов среды CLR.</span><span class="sxs-lookup"><span data-stu-id="c7537-116">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="c7537-117">Здесь `builder.Singleton<Company>("Umbrella")` сообщает построителю модели для создания Singleton-классом с именем `Umbrella` модели EDM.</span><span class="sxs-lookup"><span data-stu-id="c7537-117">Here, `builder.Singleton<Company>("Umbrella")` tells the model builder to create a singleton named `Umbrella` in the EDM model.</span></span>

    <span data-ttu-id="c7537-118">Созданные метаданные будут выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="c7537-118">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    <span data-ttu-id="c7537-119">Из метаданных можно видеть, что свойство навигации `Company` в `Employees` набора сущностей, привязанный к singleton `Umbrella`.</span><span class="sxs-lookup"><span data-stu-id="c7537-119">From the metadata we can see that the navigation property `Company` in the `Employees` entity set is bound to the singleton `Umbrella`.</span></span> <span data-ttu-id="c7537-120">Привязка выполняется автоматически `ODataConventionModelBuilder`, так как только `Umbrella` имеет `Company` типа.</span><span class="sxs-lookup"><span data-stu-id="c7537-120">The binding is done automatically by `ODataConventionModelBuilder`, since only `Umbrella` has the `Company` type.</span></span> <span data-ttu-id="c7537-121">Если любая неопределенность в модели, можно использовать `HasSingletonBinding` явно привязать свойство навигации в один элемент; `HasSingletonBinding` действует так же, как с помощью `Singleton` атрибута в определении типа CLR:</span><span class="sxs-lookup"><span data-stu-id="c7537-121">If there is any ambiguity in the model, you can use `HasSingletonBinding` to explicitly bind a navigation property to a singleton; `HasSingletonBinding` has the same effect as using the `Singleton` attribute in the CLR type definition:</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a><span data-ttu-id="c7537-122">Определить одноэлементный контроллера</span><span class="sxs-lookup"><span data-stu-id="c7537-122">Define the singleton controller</span></span>

<span data-ttu-id="c7537-123">Как контроллер EntitySet, одноэлементный контроллер наследует от `ODataController`, и имя контроллера singleton должен быть `[singletonName]Controller`.</span><span class="sxs-lookup"><span data-stu-id="c7537-123">Like the EntitySet controller, the singleton controller inherits from `ODataController`, and the singleton controller name should be `[singletonName]Controller`.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

<span data-ttu-id="c7537-124">Для обработки различных типов запросов, действия должны быть предварительно определенные в контроллере.</span><span class="sxs-lookup"><span data-stu-id="c7537-124">In order to handle different kinds of requests, actions are required to be pre-defined in the controller.</span></span> <span data-ttu-id="c7537-125">**Атрибут маршрутизации** включена по умолчанию в WebApi 2.2.</span><span class="sxs-lookup"><span data-stu-id="c7537-125">**Attribute routing** is enabled by default in WebApi 2.2.</span></span> <span data-ttu-id="c7537-126">Например, чтобы определить действие для обработки запросов `Revenue` из `Company` с помощью атрибута маршрутизации, используйте следующее:</span><span class="sxs-lookup"><span data-stu-id="c7537-126">For example, to define an action to handle querying `Revenue` from `Company` using attribute routing, use the following:</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

<span data-ttu-id="c7537-127">Если вы не хотите определить атрибуты для каждого действия, достаточно определить свои действия после [соглашение о маршрутизации OData](../odata-routing-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="c7537-127">If you are not willing to define attributes for each action, just define your actions following [OData Routing Conventions](../odata-routing-conventions.md).</span></span> <span data-ttu-id="c7537-128">Поскольку ключ не является обязательным для одноэлементный запрос, действия, определенные в контроллере одноэлементный несколько отличаются от действий, указанных в этом контроллере entityset.</span><span class="sxs-lookup"><span data-stu-id="c7537-128">Since a key is not required for querying a singleton, the actions defined in the singleton controller are slightly different from actions defined in the entityset controller.</span></span>

<span data-ttu-id="c7537-129">Справочник по сигнатуры методов для каждого определения действия в контроллере singleton, перечислены ниже.</span><span class="sxs-lookup"><span data-stu-id="c7537-129">For reference, method signatures for every action definition in the singleton controller are listed below.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

<span data-ttu-id="c7537-130">По сути это все, что необходимо выполнить на стороне службы.</span><span class="sxs-lookup"><span data-stu-id="c7537-130">Basically, this is all you need to do on the service side.</span></span> <span data-ttu-id="c7537-131">[Образец проекта](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) содержит весь код для решения и клиента OData, который показывает использование единственного экземпляра.</span><span class="sxs-lookup"><span data-stu-id="c7537-131">The [sample project](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contains all of the code for the solution and the OData client that shows how to use the singleton.</span></span> <span data-ttu-id="c7537-132">Клиент создается с помощью инструкции в [Создание клиентского приложения OData v4](create-an-odata-v4-client-app.md).</span><span class="sxs-lookup"><span data-stu-id="c7537-132">The client is built by following the steps in [Create an OData v4 Client App](create-an-odata-v4-client-app.md).</span></span>

<span data-ttu-id="c7537-133">.</span><span class="sxs-lookup"><span data-stu-id="c7537-133">.</span></span> 

<span data-ttu-id="c7537-134">*Спасибо, что для Leo Hu исходного содержимого этой статьи.*</span><span class="sxs-lookup"><span data-stu-id="c7537-134">*Thanks to Leo Hu for the original content of this article.*</span></span>
