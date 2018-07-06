---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Создание единичного экземпляра в OData v4 с помощью веб-API 2.2 | Документация Майкрософт
author: rick-anderson
description: В этом разделе показано, как определить одноэлементный в конечную точку OData в веб-API 2.2.
ms.author: aspnetcontent
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: baccfed79949ae4efd45395bed3ad0058549cb4c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830520"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a><span data-ttu-id="f056b-103">Создание единичного экземпляра в OData v4 с помощью веб-API 2.2</span><span class="sxs-lookup"><span data-stu-id="f056b-103">Create a Singleton in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="f056b-104">с ло Zoe</span><span class="sxs-lookup"><span data-stu-id="f056b-104">by Zoe Luo</span></span>

> <span data-ttu-id="f056b-105">В большинстве случаев сущности может осуществляться только если он был инкапсулирован в наборе сущностей.</span><span class="sxs-lookup"><span data-stu-id="f056b-105">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="f056b-106">Но OData v4 содержит два дополнительных параметра, одноэлементные и вложения, каждый из которых поддерживает веб-API 2.2.</span><span class="sxs-lookup"><span data-stu-id="f056b-106">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="f056b-107">В этой статье показано, как определить одноэлементный в конечную точку OData в веб-API 2.2.</span><span class="sxs-lookup"><span data-stu-id="f056b-107">This article shows how to define a singleton in an OData endpoint in Web API 2.2.</span></span> <span data-ttu-id="f056b-108">Сведения о какие единственный экземпляр есть, и как можно обеспечить с помощью его, см. в разделе [с помощью является одноэлементным множеством, определяющих специальные сущности](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span><span class="sxs-lookup"><span data-stu-id="f056b-108">For information on what a singleton is and how you can benefit from using it, see [Using a singleton to define your special entity](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span></span> <span data-ttu-id="f056b-109">Чтобы создать конечную точку OData V4 в веб-API, см. в разделе [создания OData v4 конечной точки с помощью веб-API ASP.NET 2.2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="f056b-109">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span> 

<span data-ttu-id="f056b-110">Мы создадим единственный элемент в проекте веб-API с использованием следующей модели данных:</span><span class="sxs-lookup"><span data-stu-id="f056b-110">We'll create a singleton in your Web API project using the following data model:</span></span>

![Модель данных](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

<span data-ttu-id="f056b-112">Единственный экземпляр с именем `Umbrella` будет определяться в зависимости от типа `Company`и набор именованных сущностей `Employees` будет определяться в зависимости от типа `Employee`.</span><span class="sxs-lookup"><span data-stu-id="f056b-112">A singleton named `Umbrella` will be defined based on type `Company`, and an entity set named `Employees` will be defined based on type `Employee`.</span></span>

<span data-ttu-id="f056b-113">Решения, используемые в этом руководстве можно загрузить из [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span><span class="sxs-lookup"><span data-stu-id="f056b-113">The solution used in this tutorial can be downloaded from [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="f056b-114">Определение модели данных</span><span class="sxs-lookup"><span data-stu-id="f056b-114">Define the data model</span></span>

1. <span data-ttu-id="f056b-115">Определение типов среды CLR.</span><span class="sxs-lookup"><span data-stu-id="f056b-115">Define the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. <span data-ttu-id="f056b-116">Создание модели EDM на основе типов среды CLR.</span><span class="sxs-lookup"><span data-stu-id="f056b-116">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="f056b-117">Здесь `builder.Singleton<Company>("Umbrella")` сообщает построитель модели, чтобы создать единственный экземпляр с именем `Umbrella` модели EDM.</span><span class="sxs-lookup"><span data-stu-id="f056b-117">Here, `builder.Singleton<Company>("Umbrella")` tells the model builder to create a singleton named `Umbrella` in the EDM model.</span></span>

    <span data-ttu-id="f056b-118">Созданные метаданные будут выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f056b-118">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    <span data-ttu-id="f056b-119">Из метаданных мы видим, что свойство навигации `Company` в `Employees` набор сущностей привязан к одноэлементного `Umbrella`.</span><span class="sxs-lookup"><span data-stu-id="f056b-119">From the metadata we can see that the navigation property `Company` in the `Employees` entity set is bound to the singleton `Umbrella`.</span></span> <span data-ttu-id="f056b-120">Привязка выполняется автоматически `ODataConventionModelBuilder`, так как только `Umbrella` имеет `Company` типа.</span><span class="sxs-lookup"><span data-stu-id="f056b-120">The binding is done automatically by `ODataConventionModelBuilder`, since only `Umbrella` has the `Company` type.</span></span> <span data-ttu-id="f056b-121">Есть ли какая-либо неопределенность в модели, можно использовать `HasSingletonBinding` явно связать свойства навигации в один элемент; `HasSingletonBinding` имеет тот же эффект, как с помощью `Singleton` атрибута в определение типа CLR:</span><span class="sxs-lookup"><span data-stu-id="f056b-121">If there is any ambiguity in the model, you can use `HasSingletonBinding` to explicitly bind a navigation property to a singleton; `HasSingletonBinding` has the same effect as using the `Singleton` attribute in the CLR type definition:</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a><span data-ttu-id="f056b-122">Определение контроллера одноэлементный</span><span class="sxs-lookup"><span data-stu-id="f056b-122">Define the singleton controller</span></span>

<span data-ttu-id="f056b-123">Как контроллер EntitySet, одноэлементный контроллер наследует от `ODataController`, и должно быть имя контроллера одноэлементный `[singletonName]Controller`.</span><span class="sxs-lookup"><span data-stu-id="f056b-123">Like the EntitySet controller, the singleton controller inherits from `ODataController`, and the singleton controller name should be `[singletonName]Controller`.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

<span data-ttu-id="f056b-124">Для обработки различных типов запросов, действия, должны быть предварительно определенные в контроллере.</span><span class="sxs-lookup"><span data-stu-id="f056b-124">In order to handle different kinds of requests, actions are required to be pre-defined in the controller.</span></span> <span data-ttu-id="f056b-125">**Маршрутизация атрибутов** включена по умолчанию в веб-API 2.2.</span><span class="sxs-lookup"><span data-stu-id="f056b-125">**Attribute routing** is enabled by default in WebApi 2.2.</span></span> <span data-ttu-id="f056b-126">Например, чтобы определить действие для обработки запросов `Revenue` из `Company` использующим маршрутизацию атрибутов, используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="f056b-126">For example, to define an action to handle querying `Revenue` from `Company` using attribute routing, use the following:</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

<span data-ttu-id="f056b-127">Если вы не хотите определить атрибуты для каждого действия, просто определите следующие действия [соглашений о маршрутизации протокола OData](../odata-routing-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="f056b-127">If you are not willing to define attributes for each action, just define your actions following [OData Routing Conventions](../odata-routing-conventions.md).</span></span> <span data-ttu-id="f056b-128">Поскольку ключ не является обязательным для выполнения запроса является одноэлементным множеством, действия, определенные в контроллере одноэлементный немного отличаются от действий, указанных в этом контроллере entityset.</span><span class="sxs-lookup"><span data-stu-id="f056b-128">Since a key is not required for querying a singleton, the actions defined in the singleton controller are slightly different from actions defined in the entityset controller.</span></span>

<span data-ttu-id="f056b-129">Для справки сигнатуры методов для каждого определения действий в контроллере, singleton, перечислены ниже.</span><span class="sxs-lookup"><span data-stu-id="f056b-129">For reference, method signatures for every action definition in the singleton controller are listed below.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

<span data-ttu-id="f056b-130">По сути это все, что вам нужно сделать на стороне службы.</span><span class="sxs-lookup"><span data-stu-id="f056b-130">Basically, this is all you need to do on the service side.</span></span> <span data-ttu-id="f056b-131">[Пример проекта](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) содержит весь код для решения и клиент OData, показывающий, как использовать единственный экземпляр.</span><span class="sxs-lookup"><span data-stu-id="f056b-131">The [sample project](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contains all of the code for the solution and the OData client that shows how to use the singleton.</span></span> <span data-ttu-id="f056b-132">Построение клиента, выполнив действия, описанные в [Создание клиентского приложения OData v4](create-an-odata-v4-client-app.md).</span><span class="sxs-lookup"><span data-stu-id="f056b-132">The client is built by following the steps in [Create an OData v4 Client App](create-an-odata-v4-client-app.md).</span></span>

<span data-ttu-id="f056b-133">.</span><span class="sxs-lookup"><span data-stu-id="f056b-133">.</span></span> 

<span data-ttu-id="f056b-134">*Благодаря Leo Hu для исходного содержимого этой статьи.*</span><span class="sxs-lookup"><span data-stu-id="f056b-134">*Thanks to Leo Hu for the original content of this article.*</span></span>
