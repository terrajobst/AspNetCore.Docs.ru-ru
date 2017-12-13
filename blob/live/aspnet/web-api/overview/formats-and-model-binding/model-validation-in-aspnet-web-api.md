---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: "Проверка в ASP.NET Web API модели | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: dc91ddb64294e686825076d5bcc636766f2f6f01
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="model-validation-in-aspnet-web-api"></a><span data-ttu-id="e4b14-102">Проверка модели в веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e4b14-102">Model Validation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="e4b14-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e4b14-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e4b14-104">Когда клиент отправляет данные на веб-API, часто требуется проверить данные перед выполнением какой-либо обработки.</span><span class="sxs-lookup"><span data-stu-id="e4b14-104">When a client sends data to your web API, often you want to validate the data before doing any processing.</span></span> <span data-ttu-id="e4b14-105">В этой статье показано, как для заметок к моделям, использование примечаний для проверки данных и обработки ошибок проверки в веб-API.</span><span class="sxs-lookup"><span data-stu-id="e4b14-105">This article shows how to annotate your models, use the annotations for data validation, and handle validation errors in your web API.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e4b14-106">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="e4b14-106">Data Annotations</span></span>

<span data-ttu-id="e4b14-107">В веб-API ASP.NET, можно использовать атрибуты из [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) пространства имен, чтобы задать правила проверки для свойства в модели.</span><span class="sxs-lookup"><span data-stu-id="e4b14-107">In ASP.NET Web API, you can use attributes from the [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) namespace to set validation rules for properties on your model.</span></span> <span data-ttu-id="e4b14-108">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="e4b14-108">Consider the following model:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="e4b14-109">Если используется проверка модели в ASP.NET MVC, должна быть знакома.</span><span class="sxs-lookup"><span data-stu-id="e4b14-109">If you have used model validation in ASP.NET MVC, this should look familiar.</span></span> <span data-ttu-id="e4b14-110">**Необходимые** атрибут говорится, что `Name` свойство не должно иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="e4b14-110">The **Required** attribute says that the `Name` property must not be null.</span></span> <span data-ttu-id="e4b14-111">**Диапазон** говорится, что атрибут `Weight` должен находиться в диапазоне от 0 до 999.</span><span class="sxs-lookup"><span data-stu-id="e4b14-111">The **Range** attribute says that `Weight` must be between zero and 999.</span></span>

<span data-ttu-id="e4b14-112">Предположим, что клиент отправляет запрос POST с следующее представление JSON:</span><span class="sxs-lookup"><span data-stu-id="e4b14-112">Suppose that a client sends a POST request with the following JSON representation:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

<span data-ttu-id="e4b14-113">Вы увидите, что клиент не включил `Name` свойство, которое отмечено как требуется.</span><span class="sxs-lookup"><span data-stu-id="e4b14-113">You can see that the client did not include the `Name` property, which is marked as required.</span></span> <span data-ttu-id="e4b14-114">Если веб-API преобразует JSON в `Product` экземпляр, он проверяет `Product` атрибутов проверки.</span><span class="sxs-lookup"><span data-stu-id="e4b14-114">When Web API converts the JSON into a `Product` instance, it validates the `Product` against the validation attributes.</span></span> <span data-ttu-id="e4b14-115">В действии контроллера можно проверить, допустима ли модель:</span><span class="sxs-lookup"><span data-stu-id="e4b14-115">In your controller action, you can check whether the model is valid:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="e4b14-116">Проверка модели не гарантирует безопасные данные клиента.</span><span class="sxs-lookup"><span data-stu-id="e4b14-116">Model validation does not guarantee that client data is safe.</span></span> <span data-ttu-id="e4b14-117">В других слоях приложения может потребоваться дополнительная проверка.</span><span class="sxs-lookup"><span data-stu-id="e4b14-117">Additional validation might be needed in other layers of the application.</span></span> <span data-ttu-id="e4b14-118">(Например, на уровне данных может применять ограничения внешних ключей). Учебник [с помощью веб-API с Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) рассматриваются некоторые из этих проблем.</span><span class="sxs-lookup"><span data-stu-id="e4b14-118">(For example, the data layer might enforce foreign key constraints.) The tutorial [Using Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explores some of these issues.</span></span>

<span data-ttu-id="e4b14-119">**«Снизу учета»**: недостаточное учета происходит, когда клиент опущены некоторые свойства.</span><span class="sxs-lookup"><span data-stu-id="e4b14-119">**"Under-Posting"**: Under-posting happens when the client leaves out some properties.</span></span> <span data-ttu-id="e4b14-120">Например предположим, что клиент отправляет следующее:</span><span class="sxs-lookup"><span data-stu-id="e4b14-120">For example, suppose the client sends the following:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

<span data-ttu-id="e4b14-121">Здесь, клиент не указывает значения для `Price` или `Weight`.</span><span class="sxs-lookup"><span data-stu-id="e4b14-121">Here, the client did not specify values for `Price` or `Weight`.</span></span> <span data-ttu-id="e4b14-122">Модуль форматирования JSON назначает нулевое значение по умолчанию для отсутствующие свойства.</span><span class="sxs-lookup"><span data-stu-id="e4b14-122">The JSON formatter assigns a default value of zero to the missing properties.</span></span>

![](model-validation-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="e4b14-123">Состояние модели является допустимым, поскольку ноль является допустимым значением для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="e4b14-123">The model state is valid, because zero is a valid value for these properties.</span></span> <span data-ttu-id="e4b14-124">Является ли это проблемой зависит от сценария.</span><span class="sxs-lookup"><span data-stu-id="e4b14-124">Whether this is a problem depends on your scenario.</span></span> <span data-ttu-id="e4b14-125">Например операция обновления может потребоваться для различения «ноль» и «не установлено».</span><span class="sxs-lookup"><span data-stu-id="e4b14-125">For example, in an update operation, you might want to distinguish between "zero" and "not set."</span></span> <span data-ttu-id="e4b14-126">Чтобы заставить клиентов, чтобы задать значение, сделайте свойство допускает значения NULL и задать **необходимые** атрибута:</span><span class="sxs-lookup"><span data-stu-id="e4b14-126">To force clients to set a value, make the property nullable and set the **Required** attribute:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

<span data-ttu-id="e4b14-127">**«Чрезмерно учета»**: клиент также может отправлять *дополнительные* данных, чем ожидалось.</span><span class="sxs-lookup"><span data-stu-id="e4b14-127">**"Over-Posting"**: A client can also send *more* data than you expected.</span></span> <span data-ttu-id="e4b14-128">Пример:</span><span class="sxs-lookup"><span data-stu-id="e4b14-128">For example:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

<span data-ttu-id="e4b14-129">Здесь JSON включает свойство («цвет»), которые отсутствуют в `Product` модели.</span><span class="sxs-lookup"><span data-stu-id="e4b14-129">Here, the JSON includes a property ("Color") that does not exist in the `Product` model.</span></span> <span data-ttu-id="e4b14-130">В этом случае модуль форматирования JSON просто игнорирует это значение.</span><span class="sxs-lookup"><span data-stu-id="e4b14-130">In this case, the JSON formatter simply ignores this value.</span></span> <span data-ttu-id="e4b14-131">(Модуль форматирования XML делает то же самое.) Чрезмерное учета вызывает проблемы, если модель содержит свойства, которые предназначены только для чтения.</span><span class="sxs-lookup"><span data-stu-id="e4b14-131">(The XML formatter does the same.) Over-posting causes problems if your model has properties that you intended to be read-only.</span></span> <span data-ttu-id="e4b14-132">Пример:</span><span class="sxs-lookup"><span data-stu-id="e4b14-132">For example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="e4b14-133">Требуется запретить пользователям обновлять `IsAdmin` свойства и может повышать уровень администраторам!</span><span class="sxs-lookup"><span data-stu-id="e4b14-133">You don't want users to update the `IsAdmin` property and elevate themselves to administrators!</span></span> <span data-ttu-id="e4b14-134">Наиболее безопасным стратегия состоит в использовании класс модели, который точно соответствует, что клиент может отправлять:</span><span class="sxs-lookup"><span data-stu-id="e4b14-134">The safest strategy is to use a model class that exactly matches what the client is allowed to send:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="e4b14-135">Сообщение в блоге Брэда Уилсона «[vs проверки входных данных. Проверка в ASP.NET MVC модели](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)«содержит подробное обсуждение перевыборки учета и чрезмерное учет.</span><span class="sxs-lookup"><span data-stu-id="e4b14-135">Brad Wilson's blog post "[Input Validation vs. Model Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" has a good discussion of under-posting and over-posting.</span></span> <span data-ttu-id="e4b14-136">Хотя сообщение о ASP.NET MVC 2, проблемы, по-прежнему актуальны для веб-API.</span><span class="sxs-lookup"><span data-stu-id="e4b14-136">Although the post is about ASP.NET MVC 2, the issues are still relevant to Web API.</span></span>


## <a name="handling-validation-errors"></a><span data-ttu-id="e4b14-137">Обработка ошибок проверки</span><span class="sxs-lookup"><span data-stu-id="e4b14-137">Handling Validation Errors</span></span>

<span data-ttu-id="e4b14-138">Веб-API не возвращает автоматически ошибку клиенту при сбое проверки.</span><span class="sxs-lookup"><span data-stu-id="e4b14-138">Web API does not automatically return an error to the client when validation fails.</span></span> <span data-ttu-id="e4b14-139">Именно действия контроллера, чтобы проверить состояние модели и реагировать соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="e4b14-139">It is up to the controller action to check the model state and respond appropriately.</span></span>

<span data-ttu-id="e4b14-140">Также можно создать фильтр действий, чтобы проверить состояние модели до вызова действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="e4b14-140">You can also create an action filter to check the model state before the controller action is invoked.</span></span> <span data-ttu-id="e4b14-141">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="e4b14-141">The following code shows an example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="e4b14-142">При сбое проверки модели, этот фильтр возвращает HTTP-ответа, содержащего ошибки проверки.</span><span class="sxs-lookup"><span data-stu-id="e4b14-142">If model validation fails, this filter returns an HTTP response that contains the validation errors.</span></span> <span data-ttu-id="e4b14-143">В этом случае действия контроллера не вызывается.</span><span class="sxs-lookup"><span data-stu-id="e4b14-143">In that case, the controller action is not invoked.</span></span>

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

<span data-ttu-id="e4b14-144">Чтобы применить фильтр ко всем контроллерам веб-API, нужно добавить экземпляр фильтра **HttpConfiguration.Filters** коллекции во время настройки:</span><span class="sxs-lookup"><span data-stu-id="e4b14-144">To apply this filter to all Web API controllers, add an instance of the filter to the **HttpConfiguration.Filters** collection during configuration:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="e4b14-145">Другой вариант — Чтобы установить фильтр как атрибут на отдельных контроллерах или действия контроллера:</span><span class="sxs-lookup"><span data-stu-id="e4b14-145">Another option is to set the filter as an attribute on individual controllers or controller actions:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
