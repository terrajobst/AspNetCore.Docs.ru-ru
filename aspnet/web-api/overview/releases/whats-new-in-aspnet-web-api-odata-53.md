---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: Новые возможности в ASP.NET Web API OData 5.3 | Документы Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: e918f86ebd813f31ad0c21566e617482fc7dd963
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508113"
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a><span data-ttu-id="8bdc7-102">Новые возможности в ASP.NET Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="8bdc7-102">What's New in ASP.NET Web API OData 5.3</span></span>
====================
<span data-ttu-id="8bdc7-103">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="8bdc7-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="8bdc7-104">В этом разделе описываются новые возможности в ASP.NET Web API OData 5.3.</span><span class="sxs-lookup"><span data-stu-id="8bdc7-104">This topic describes what's new for ASP.NET Web API OData 5.3.</span></span>

- [<span data-ttu-id="8bdc7-105">Скачать</span><span class="sxs-lookup"><span data-stu-id="8bdc7-105">Download</span></span>](#download)
- [<span data-ttu-id="8bdc7-106">Документация</span><span class="sxs-lookup"><span data-stu-id="8bdc7-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="8bdc7-107">Основные OData библиотеки</span><span class="sxs-lookup"><span data-stu-id="8bdc7-107">OData Core Libraries</span></span>](#corelib)
- [<span data-ttu-id="8bdc7-108">Новые возможности</span><span class="sxs-lookup"><span data-stu-id="8bdc7-108">New Features</span></span>](#newf)
- [<span data-ttu-id="8bdc7-109">Известные проблемы и критические изменения</span><span class="sxs-lookup"><span data-stu-id="8bdc7-109">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="8bdc7-110">Исправления ошибок</span><span class="sxs-lookup"><span data-stu-id="8bdc7-110">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="8bdc7-111">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="8bdc7-111">ASP.NET Web API OData 5.3.1</span></span>](#OD)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="8bdc7-112">Скачать</span><span class="sxs-lookup"><span data-stu-id="8bdc7-112">Download</span></span>

<span data-ttu-id="8bdc7-113">Компоненты среды выполнения выпущены в виде пакетов NuGet при галереи NuGet.</span><span class="sxs-lookup"><span data-stu-id="8bdc7-113">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="8bdc7-114">Можно установить или обновить выпущенных пакетов NuGet, используя консоль диспетчера пакетов NuGet:</span><span class="sxs-lookup"><span data-stu-id="8bdc7-114">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="8bdc7-115">Документация</span><span class="sxs-lookup"><span data-stu-id="8bdc7-115">Documentation</span></span>

<span data-ttu-id="8bdc7-116">Можно найти учебники и другую документацию по ASP.NET Web API OData в [веб-сайт ASP.NET](../odata-support-in-aspnet-web-api/index.md).</span><span class="sxs-lookup"><span data-stu-id="8bdc7-116">You can find tutorials and other documentation about ASP.NET Web API OData at the [ASP.NET web site](../odata-support-in-aspnet-web-api/index.md).</span></span>

<a id="corelib"></a>
## <a name="odata-core-libraries"></a><span data-ttu-id="8bdc7-117">Основные OData библиотеки</span><span class="sxs-lookup"><span data-stu-id="8bdc7-117">OData Core Libraries</span></span>

<span data-ttu-id="8bdc7-118">Для OData v4 веб-API теперь использует ODataLib версии 6.5.0</span><span class="sxs-lookup"><span data-stu-id="8bdc7-118">For OData v4, Web API now uses ODataLib version 6.5.0</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a><span data-ttu-id="8bdc7-119">Новые возможности в ASP.NET веб-API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="8bdc7-119">New Features in ASP.NET Web API OData 5.3</span></span>

### <a name="support-for-levels-in-expand"></a><span data-ttu-id="8bdc7-120">Разверните поддержка $levels $</span><span class="sxs-lookup"><span data-stu-id="8bdc7-120">Support for $levels in $expand</span></span>

<span data-ttu-id="8bdc7-121">Можно использовать $levels запроса в $разверните запросов.</span><span class="sxs-lookup"><span data-stu-id="8bdc7-121">You can use the $levels query option in $expand queries.</span></span> <span data-ttu-id="8bdc7-122">Пример:</span><span class="sxs-lookup"><span data-stu-id="8bdc7-122">For example:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

<span data-ttu-id="8bdc7-123">Этот запрос эквивалентен:</span><span class="sxs-lookup"><span data-stu-id="8bdc7-123">This query is equivalent to:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a><span data-ttu-id="8bdc7-124">Поддержка типов открытые сущности</span><span class="sxs-lookup"><span data-stu-id="8bdc7-124">Support for Open Entity Types</span></span>

<span data-ttu-id="8bdc7-125">*Открытый тип* — stuctured тип, который содержит динамические свойства, помимо любые свойства, объявленные в определении типа.</span><span class="sxs-lookup"><span data-stu-id="8bdc7-125">An *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="8bdc7-126">Открытые типы позволяют повысить гибкость модели анализа данных.</span><span class="sxs-lookup"><span data-stu-id="8bdc7-126">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="8bdc7-127">Дополнительные сведения см. в разделе xxxx.</span><span class="sxs-lookup"><span data-stu-id="8bdc7-127">For more information, see xxxx.</span></span>

### <a name="support-for-dynamic-collection-properties-in-open-types"></a><span data-ttu-id="8bdc7-128">Поддержка динамическую коллекцию свойств в открытых типах</span><span class="sxs-lookup"><span data-stu-id="8bdc7-128">Support for dynamic collection properties in open types</span></span>

<span data-ttu-id="8bdc7-129">Раньше динамических свойств должны были быть одно значение.</span><span class="sxs-lookup"><span data-stu-id="8bdc7-129">Previously, a dynamic property had to be a single value.</span></span> <span data-ttu-id="8bdc7-130">В 5.3 динамические свойства могут иметь значения коллекции.</span><span class="sxs-lookup"><span data-stu-id="8bdc7-130">In 5.3, dynamic properties can have collection values.</span></span> <span data-ttu-id="8bdc7-131">Например, в следующем полезные данные JSON `Emails` свойства является динамическим и коллекции строкового типа:</span><span class="sxs-lookup"><span data-stu-id="8bdc7-131">For example, in the following JSON payload, the `Emails` property is a dynamic property and is of collection of string type:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a><span data-ttu-id="8bdc7-132">Поддержка наследования для сложных типов</span><span class="sxs-lookup"><span data-stu-id="8bdc7-132">Support for inheritance for complex types</span></span>

<span data-ttu-id="8bdc7-133">Теперь сложные типы могут наследовать от базового типа.</span><span class="sxs-lookup"><span data-stu-id="8bdc7-133">Now complex types can inherit from a base type.</span></span> <span data-ttu-id="8bdc7-134">Например службы OData определить следующих сложных типов:</span><span class="sxs-lookup"><span data-stu-id="8bdc7-134">For example, an OData service could define the following complex types:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

<span data-ttu-id="8bdc7-135">Вот модели EDM для этого примера.</span><span class="sxs-lookup"><span data-stu-id="8bdc7-135">Here is the EDM for this example:</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

<span data-ttu-id="8bdc7-136">Дополнительные сведения см. в разделе [OData сложный пример наследования типов](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="8bdc7-136">For more information, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="8bdc7-137">Известные проблемы и критические изменения</span><span class="sxs-lookup"><span data-stu-id="8bdc7-137">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="8bdc7-138">В этом разделе описываются известные проблемы и критические изменения в ASP.NET Web API OData 5.3.</span><span class="sxs-lookup"><span data-stu-id="8bdc7-138">This section describes known issues and breaking changes in the ASP.NET Web API OData 5.3.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="8bdc7-139">OData v4</span><span class="sxs-lookup"><span data-stu-id="8bdc7-139">OData v4</span></span>

#### <a name="query-options"></a><span data-ttu-id="8bdc7-140">Параметры запросов</span><span class="sxs-lookup"><span data-stu-id="8bdc7-140">Query Options</span></span>

<span data-ttu-id="8bdc7-141">Проблема: С помощью $ вложенного разверните с $levels = максимальное число результатов в глубину неверные расширения.</span><span class="sxs-lookup"><span data-stu-id="8bdc7-141">Issue: Using nested $expand with $levels=max results in an incorrect expansion depth.</span></span>

<span data-ttu-id="8bdc7-142">Например рассмотрим следующий запрос:</span><span class="sxs-lookup"><span data-stu-id="8bdc7-142">For example, given the following request:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

<span data-ttu-id="8bdc7-143">Если `MaxExpansionDepth` равно 5, этот запрос приведет к появлению расширения глубина 6.</span><span class="sxs-lookup"><span data-stu-id="8bdc7-143">If `MaxExpansionDepth` is 5, this query would result in an expansion depth of 6.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="8bdc7-144">Исправления и обновления вспомогательных компонентов</span><span class="sxs-lookup"><span data-stu-id="8bdc7-144">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="8bdc7-145">Этот выпуск включает несколько исправлений ошибок, а также дополнительный компонент обновления.</span><span class="sxs-lookup"><span data-stu-id="8bdc7-145">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="8bdc7-146">Можно найти полный список здесь:</span><span class="sxs-lookup"><span data-stu-id="8bdc7-146">You can find the complete list here:</span></span>

- [<span data-ttu-id="8bdc7-147">Исправления ошибок</span><span class="sxs-lookup"><span data-stu-id="8bdc7-147">Bug fixes</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a><span data-ttu-id="8bdc7-148">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="8bdc7-148">ASP.NET Web API OData 5.3.1</span></span>

<span data-ttu-id="8bdc7-149">В этом выпуске мы внесли [исправления ошибки](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) к некоторым AllowedFunctions перечислений.</span><span class="sxs-lookup"><span data-stu-id="8bdc7-149">In this release we made a [bug fix](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) to some of the AllowedFunctions enums.</span></span> <span data-ttu-id="8bdc7-150">Этот выпуск не имеет других исправления или новые функции.</span><span class="sxs-lookup"><span data-stu-id="8bdc7-150">This release doesn't have any other bug fixes or new features.</span></span>
