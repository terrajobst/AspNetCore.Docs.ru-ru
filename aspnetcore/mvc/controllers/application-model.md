---
title: "Работа с моделью приложения"
author: ardalis
description: 
keywords: "Core,ASP.NET Core ASP.NET MVC, модель приложения"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4eb7e52f-5665-41a4-a3e3-e348d07337f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/application-model
ms.openlocfilehash: 3c35184921dbe26cde100fd3d5124e38ea0d06cf
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="working-with-the-application-model"></a><span data-ttu-id="77b10-103">Работа с моделью приложения</span><span class="sxs-lookup"><span data-stu-id="77b10-103">Working with the Application Model</span></span>

<span data-ttu-id="77b10-104">По [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="77b10-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="77b10-105">Определяет основные ASP.NET MVC *модель приложения* представляющие компоненты приложения MVC.</span><span class="sxs-lookup"><span data-stu-id="77b10-105">ASP.NET Core MVC defines an *application model* representing the components of an MVC app.</span></span> <span data-ttu-id="77b10-106">Можно читать и управлять этой модели, чтобы изменить поведение элементов MVC.</span><span class="sxs-lookup"><span data-stu-id="77b10-106">You can read and manipulate this model to modify how MVC elements behave.</span></span> <span data-ttu-id="77b10-107">По умолчанию MVC соблюдаются определенные соглашения для определения классов, которые считаются контроллеров, какие методы этих классов являются действия и поведение маршрутизации и параметры.</span><span class="sxs-lookup"><span data-stu-id="77b10-107">By default, MVC follows certain conventions to determine which classes are considered to be controllers, which methods on those classes are actions, and how parameters and routing behave.</span></span> <span data-ttu-id="77b10-108">Вы можете настроить это поведение в соответствии с потребностями приложения путем создания собственного правила и применения их глобально или как атрибуты.</span><span class="sxs-lookup"><span data-stu-id="77b10-108">You can customize this behavior to suit your app's needs by creating your own conventions and applying them globally or as attributes.</span></span>

## <a name="models-and-providers"></a><span data-ttu-id="77b10-109">Модели и поставщики</span><span class="sxs-lookup"><span data-stu-id="77b10-109">Models and Providers</span></span>

<span data-ttu-id="77b10-110">Модель приложения ASP.NET Core MVC включают абстрактные интерфейсы и конкретные реализации классов, описывающих приложения MVC.</span><span class="sxs-lookup"><span data-stu-id="77b10-110">The ASP.NET Core MVC application model include both abstract interfaces and concrete implementation classes that describe an MVC application.</span></span> <span data-ttu-id="77b10-111">Эта модель является результатом обнаружения контроллеров, действия, параметров действия, маршруты и фильтров в соответствии с соглашениями по умолчанию приложения MVC.</span><span class="sxs-lookup"><span data-stu-id="77b10-111">This model is the result of MVC discovering the app's controllers, actions, action parameters, routes, and filters according to default conventions.</span></span> <span data-ttu-id="77b10-112">Работая с моделью приложения, можно изменить приложения для разных соглашениям от поведения MVC по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="77b10-112">By working with the application model, you can modify your app to follow different conventions from the default MVC behavior.</span></span> <span data-ttu-id="77b10-113">Параметры, имена, маршрутов и фильтры всех используются как данные конфигурации для контроллеров и действий.</span><span class="sxs-lookup"><span data-stu-id="77b10-113">The parameters, names, routes, and filters are all used as configuration data for actions and controllers.</span></span>

<span data-ttu-id="77b10-114">Модель приложения MVC ASP.NET Core имеет следующую структуру:</span><span class="sxs-lookup"><span data-stu-id="77b10-114">The ASP.NET Core MVC Application Model has the following structure:</span></span>

* <span data-ttu-id="77b10-115">ApplicationModel</span><span class="sxs-lookup"><span data-stu-id="77b10-115">ApplicationModel</span></span>
    * <span data-ttu-id="77b10-116">Контроллеры (ControllerModel)</span><span class="sxs-lookup"><span data-stu-id="77b10-116">Controllers (ControllerModel)</span></span>
        * <span data-ttu-id="77b10-117">Действия (ActionModel)</span><span class="sxs-lookup"><span data-stu-id="77b10-117">Actions (ActionModel)</span></span>
            * <span data-ttu-id="77b10-118">Параметры (ParameterModel)</span><span class="sxs-lookup"><span data-stu-id="77b10-118">Parameters (ParameterModel)</span></span>

<span data-ttu-id="77b10-119">Каждый уровень модели имеет доступ к обычное `Properties` коллекции, а более низкие уровни можно получить доступ к и перезаписать значения свойств, заданные на более высоких уровнях иерархии.</span><span class="sxs-lookup"><span data-stu-id="77b10-119">Each level of the model has access to a common `Properties` collection, and lower levels can access and overwrite property values set by higher levels in the hierarchy.</span></span> <span data-ttu-id="77b10-120">Свойства, сохраняются в `ActionDescriptor.Properties` при создании действия.</span><span class="sxs-lookup"><span data-stu-id="77b10-120">The properties are persisted to the `ActionDescriptor.Properties` when the actions are created.</span></span> <span data-ttu-id="77b10-121">Затем при обработке запроса какие-либо свойства соглашение о добавлении или изменении может осуществляться через `ActionContext.ActionDescriptor.Properties`.</span><span class="sxs-lookup"><span data-stu-id="77b10-121">Then when a request is being handled, any properties a convention added or modified can be accessed through `ActionContext.ActionDescriptor.Properties`.</span></span> <span data-ttu-id="77b10-122">С помощью свойств является хорошим способом настроить фильтры, связыватели моделей, т. д., отдельно для каждого действия.</span><span class="sxs-lookup"><span data-stu-id="77b10-122">Using properties is a great way to configure your filters, model binders, etc. on a per-action basis.</span></span>

> [!NOTE]
> <span data-ttu-id="77b10-123">`ActionDescriptor.Properties` Коллекции не является потокобезопасным (для операций записи), после завершения процесса запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="77b10-123">The `ActionDescriptor.Properties` collection is not thread safe (for writes) once app startup has finished.</span></span> <span data-ttu-id="77b10-124">Соглашения — это лучший способ, чтобы добавить данные в эту коллекцию.</span><span class="sxs-lookup"><span data-stu-id="77b10-124">Conventions are the best way to safely add data to this collection.</span></span>

### <a name="iapplicationmodelprovider"></a><span data-ttu-id="77b10-125">IApplicationModelProvider</span><span class="sxs-lookup"><span data-stu-id="77b10-125">IApplicationModelProvider</span></span>

<span data-ttu-id="77b10-126">ASP.NET Core MVC загружает модели приложения, с помощью шаблона поставщика, определяется [IApplicationModelProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) интерфейса.</span><span class="sxs-lookup"><span data-stu-id="77b10-126">ASP.NET Core MVC loads the application model using a provider pattern, defined by the [IApplicationModelProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) interface.</span></span> <span data-ttu-id="77b10-127">В этом разделе освещаются некоторые сведения о внутренней реализации этой функции поставщика.</span><span class="sxs-lookup"><span data-stu-id="77b10-127">This section covers some of the internal implementation details of how this provider functions.</span></span> <span data-ttu-id="77b10-128">Это довольно сложная тема - большинства приложений, использующих модель приложения следует это делать, работая с соглашения.</span><span class="sxs-lookup"><span data-stu-id="77b10-128">This is an advanced topic - most apps that leverage the application model should do so by working with conventions.</span></span>

<span data-ttu-id="77b10-129">Реализации `IApplicationModelProvider` интерфейс «wrap» друг с другом, с каждого вызова реализации `OnProvidersExecuting` по возрастанию на основе его `Order` свойство.</span><span class="sxs-lookup"><span data-stu-id="77b10-129">Implementations of the `IApplicationModelProvider` interface "wrap" one another, with each implementation calling `OnProvidersExecuting` in ascending order based on its `Order` property.</span></span> <span data-ttu-id="77b10-130">`OnProvidersExecuted` Затем вызывается метод в обратном порядке.</span><span class="sxs-lookup"><span data-stu-id="77b10-130">The `OnProvidersExecuted` method is then called in reverse order.</span></span> <span data-ttu-id="77b10-131">Платформа framework определяет несколько поставщиков:</span><span class="sxs-lookup"><span data-stu-id="77b10-131">The framework defines several providers:</span></span>

<span data-ttu-id="77b10-132">Первый (`Order=-1000`):</span><span class="sxs-lookup"><span data-stu-id="77b10-132">First (`Order=-1000`):</span></span>

* [`DefaultApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

<span data-ttu-id="77b10-133">Затем (`Order=-990`):</span><span class="sxs-lookup"><span data-stu-id="77b10-133">Then (`Order=-990`):</span></span>

* [`AuthorizationApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> <span data-ttu-id="77b10-134">Порядок, в которой два поставщика с тем же значением для `Order` называются не определено и поэтому не следует полагаться на.</span><span class="sxs-lookup"><span data-stu-id="77b10-134">The order in which two providers with the same value for `Order` are called is undefined, and therefore should not be relied upon.</span></span>

> [!NOTE]
> <span data-ttu-id="77b10-135">`IApplicationModelProvider`является расширенные концепции для авторов framework для расширения.</span><span class="sxs-lookup"><span data-stu-id="77b10-135">`IApplicationModelProvider` is an advanced concept for framework authors to extend.</span></span> <span data-ttu-id="77b10-136">В общем случае приложения должны использовать соглашения и платформ следует использовать поставщиков.</span><span class="sxs-lookup"><span data-stu-id="77b10-136">In general, apps should use conventions and frameworks should use providers.</span></span> <span data-ttu-id="77b10-137">Ключевое различие — всегда поставщики выполняются перед соглашения.</span><span class="sxs-lookup"><span data-stu-id="77b10-137">The key distinction is that providers always run before conventions.</span></span>

<span data-ttu-id="77b10-138">`DefaultApplicationModelProvider` Устанавливает множество вариантов поведения по умолчанию, используемые ядра ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="77b10-138">The `DefaultApplicationModelProvider` establishes many of the default behaviors used by ASP.NET Core MVC.</span></span> <span data-ttu-id="77b10-139">Его обязанности входит:</span><span class="sxs-lookup"><span data-stu-id="77b10-139">Its responsibilities include:</span></span>

* <span data-ttu-id="77b10-140">Добавление глобальных фильтров контекста</span><span class="sxs-lookup"><span data-stu-id="77b10-140">Adding global filters to the context</span></span>
* <span data-ttu-id="77b10-141">Добавление контроллеров в контексте</span><span class="sxs-lookup"><span data-stu-id="77b10-141">Adding controllers to the context</span></span>
* <span data-ttu-id="77b10-142">Добавление контроллера открытых методов в качестве действия</span><span class="sxs-lookup"><span data-stu-id="77b10-142">Adding public controller methods as actions</span></span>
* <span data-ttu-id="77b10-143">Добавление параметров методов действий в контексте</span><span class="sxs-lookup"><span data-stu-id="77b10-143">Adding action method parameters to the context</span></span>
* <span data-ttu-id="77b10-144">Применение маршрута и другие атрибуты</span><span class="sxs-lookup"><span data-stu-id="77b10-144">Applying route and other attributes</span></span>

<span data-ttu-id="77b10-145">Некоторые встроенные поведения реализуются `DefaultApplicationModelProvider`.</span><span class="sxs-lookup"><span data-stu-id="77b10-145">Some built-in behaviors are implemented by the `DefaultApplicationModelProvider`.</span></span> <span data-ttu-id="77b10-146">Этот поставщик отвечает за создание [ `ControllerModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), который в свою очередь ссылается на [ `ActionModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [ `PropertyModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), и [ `ParameterModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) экземпляров.</span><span class="sxs-lookup"><span data-stu-id="77b10-146">This provider is responsible for constructing the [`ControllerModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), which in turn references [`ActionModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [`PropertyModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), and [`ParameterModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) instances.</span></span> <span data-ttu-id="77b10-147">`DefaultApplicationModelProvider` Класса является элементом реализации внутреннюю структуру, можно и приведет к изменению в будущем.</span><span class="sxs-lookup"><span data-stu-id="77b10-147">The `DefaultApplicationModelProvider` class is an internal framework implementation detail that can and will change in the future.</span></span> 

<span data-ttu-id="77b10-148">`AuthorizationApplicationModelProvider` Отвечает за применение поведение, связанное с `AuthorizeFilter` и `AllowAnonymousFilter` атрибуты.</span><span class="sxs-lookup"><span data-stu-id="77b10-148">The `AuthorizationApplicationModelProvider` is responsible for applying the behavior associated with the `AuthorizeFilter` and `AllowAnonymousFilter` attributes.</span></span> <span data-ttu-id="77b10-149">[Дополнительные сведения об этих атрибутов](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="77b10-149">[Learn more about these attributes](xref:security/authorization/simple).</span></span>

<span data-ttu-id="77b10-150">`CorsApplicationModelProvider` Реализует поведение, связанное с `IEnableCorsAttribute` и `IDisableCorsAttribute`и `DisableCorsAuthorizationFilter`.</span><span class="sxs-lookup"><span data-stu-id="77b10-150">The `CorsApplicationModelProvider` implements behavior associated with the `IEnableCorsAttribute` and `IDisableCorsAttribute`, and the `DisableCorsAuthorizationFilter`.</span></span> <span data-ttu-id="77b10-151">[Дополнительные сведения о CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="77b10-151">[Learn more about CORS](xref:security/cors).</span></span>

## <a name="conventions"></a><span data-ttu-id="77b10-152">Соглашения</span><span class="sxs-lookup"><span data-stu-id="77b10-152">Conventions</span></span>

<span data-ttu-id="77b10-153">Модель приложения определяет соглашение о абстрактные классы, которые предоставляют простой способ настроить поведение модели, чем переопределение всей модели или поставщика.</span><span class="sxs-lookup"><span data-stu-id="77b10-153">The application model defines convention abstractions that provide a simpler way to customize the behavior of the models than overriding the entire model or provider.</span></span> <span data-ttu-id="77b10-154">Эти абстракции являются рекомендуемый способ изменения поведения приложения.</span><span class="sxs-lookup"><span data-stu-id="77b10-154">These abstractions are the recommended way to modify your app's behavior.</span></span> <span data-ttu-id="77b10-155">Соглашения позволяют писать код, который будет динамически применения настроек.</span><span class="sxs-lookup"><span data-stu-id="77b10-155">Conventions provide a way for you to write code that will dynamically apply customizations.</span></span> <span data-ttu-id="77b10-156">Хотя [фильтры](xref:mvc/controllers/filters) позволяют изменить поведение платформы, настройки позволяют пользователю контролировать, как совместно проводной всего приложения.</span><span class="sxs-lookup"><span data-stu-id="77b10-156">While [filters](xref:mvc/controllers/filters) provide a means of modifying the framework's behavior, customizations let you control how the whole app is wired together.</span></span>

<span data-ttu-id="77b10-157">Доступны следующие соглашения:</span><span class="sxs-lookup"><span data-stu-id="77b10-157">The following conventions are available:</span></span>

* [`IApplicationModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

<span data-ttu-id="77b10-158">Соглашения применяются, добавив их в MVC параметры либо путем реализации `Attribute`s и применении этих параметров действия, контроллеров и действий (аналогично [ `Filters` ](xref:mvc/controllers/filters)).</span><span class="sxs-lookup"><span data-stu-id="77b10-158">Conventions are applied by adding them to MVC options or by implementing `Attribute`s and applying them to controllers, actions, or action parameters (similar to [`Filters`](xref:mvc/controllers/filters)).</span></span> <span data-ttu-id="77b10-159">В отличие от фильтров соглашения о выполняются только при запуске приложения, не являющийся частью каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="77b10-159">Unlike filters, conventions are only executed when the app is starting, not as part of each request.</span></span>

### <a name="sample-modifying-the-applicationmodel"></a><span data-ttu-id="77b10-160">Пример: Изменение ApplicationModel</span><span class="sxs-lookup"><span data-stu-id="77b10-160">Sample: Modifying the ApplicationModel</span></span>

<span data-ttu-id="77b10-161">Чтобы добавить свойство в модели приложения используется следующее соглашение.</span><span class="sxs-lookup"><span data-stu-id="77b10-161">The following convention is used to add a property to the application model.</span></span> 

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]

<span data-ttu-id="77b10-162">Соглашения о модели приложения применяются как параметры, при добавлении в MVC `ConfigureServices` в `Startup`.</span><span class="sxs-lookup"><span data-stu-id="77b10-162">Application model conventions are applied as options when MVC is added in `ConfigureServices` in `Startup`.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]

<span data-ttu-id="77b10-163">Свойства доступны из `ActionDescriptor` свойства коллекции в течение действия контроллера:</span><span class="sxs-lookup"><span data-stu-id="77b10-163">Properties are accessible from the `ActionDescriptor` properties collection within controller actions:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]

### <a name="sample-modifying-the-controllermodel-description"></a><span data-ttu-id="77b10-164">Пример: Изменение описания ControllerModel</span><span class="sxs-lookup"><span data-stu-id="77b10-164">Sample: Modifying the ControllerModel Description</span></span>

<span data-ttu-id="77b10-165">Как в предыдущем примере контроллера модели можно изменить, чтобы включить настраиваемые свойства.</span><span class="sxs-lookup"><span data-stu-id="77b10-165">As in the previous example, the controller model can also be modified to include custom properties.</span></span> <span data-ttu-id="77b10-166">Они переопределят существующие свойства с именем, указанным в модели приложения.</span><span class="sxs-lookup"><span data-stu-id="77b10-166">These will override existing properties with the same name specified in the application model.</span></span> <span data-ttu-id="77b10-167">Следующий атрибут соглашение добавляет описание на уровне контроллера:</span><span class="sxs-lookup"><span data-stu-id="77b10-167">The following convention attribute adds a description at the controller level:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]

<span data-ttu-id="77b10-168">Это соглашение будет применяться в качестве атрибута на контроллере.</span><span class="sxs-lookup"><span data-stu-id="77b10-168">This convention is applied as an attribute on a controller.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]

<span data-ttu-id="77b10-169">Свойство «описание» осуществляется так же, как и в предыдущих примерах.</span><span class="sxs-lookup"><span data-stu-id="77b10-169">The "description" property is accessed in the same manner as in previous examples.</span></span>

### <a name="sample-modifying-the-actionmodel-description"></a><span data-ttu-id="77b10-170">Пример: Изменение описания ActionModel</span><span class="sxs-lookup"><span data-stu-id="77b10-170">Sample: Modifying the ActionModel Description</span></span>

<span data-ttu-id="77b10-171">Соглашение отдельный атрибут может применяться к отдельных действий, переопределение поведения, уже примененные на уровне приложения или контроллера.</span><span class="sxs-lookup"><span data-stu-id="77b10-171">A separate attribute convention can be applied to individual actions, overriding behavior already applied at the application or controller level.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]

<span data-ttu-id="77b10-172">Применение этого действия в предыдущем примере контроллера показано, как оно переопределяет соглашение уровня контроллера:</span><span class="sxs-lookup"><span data-stu-id="77b10-172">Applying this to an action within the previous example's controller demonstrates how it overrides the controller-level convention:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]

### <a name="sample-modifying-the-parametermodel"></a><span data-ttu-id="77b10-173">Пример: Изменение ParameterModel</span><span class="sxs-lookup"><span data-stu-id="77b10-173">Sample: Modifying the ParameterModel</span></span>

<span data-ttu-id="77b10-174">Следующее соглашение может применяться к параметрам действия, чтобы изменить их `BindingInfo`.</span><span class="sxs-lookup"><span data-stu-id="77b10-174">The following convention can be applied to action parameters to modify their `BindingInfo`.</span></span> <span data-ttu-id="77b10-175">Следующее соглашение требует, чтобы параметр был параметр маршрута; Другие возможные источники привязки (например, значения строки запроса) учитываются.</span><span class="sxs-lookup"><span data-stu-id="77b10-175">The following convention requires that the parameter be a route parameter; other potential binding sources (such as query string values) are ignored.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]

<span data-ttu-id="77b10-176">Атрибут может применяться к любой параметр действия:</span><span class="sxs-lookup"><span data-stu-id="77b10-176">The attribute may be applied to any action parameter:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]

### <a name="sample-modifying-the-actionmodel-name"></a><span data-ttu-id="77b10-177">Пример: Изменение имени ActionModel</span><span class="sxs-lookup"><span data-stu-id="77b10-177">Sample: Modifying the ActionModel Name</span></span>

<span data-ttu-id="77b10-178">Изменяет следующее соглашение `ActionModel` обновление *имя* действия, к которому он применяется.</span><span class="sxs-lookup"><span data-stu-id="77b10-178">The following convention modifies the `ActionModel` to update the *name* of the action to which it is applied.</span></span> <span data-ttu-id="77b10-179">Новое имя предоставляется как параметр атрибута.</span><span class="sxs-lookup"><span data-stu-id="77b10-179">The new name is provided as a parameter to the attribute.</span></span> <span data-ttu-id="77b10-180">Это новое имя используется по маршрутизация, поэтому влияет на маршрута, используемого для доступа данным методом действия.</span><span class="sxs-lookup"><span data-stu-id="77b10-180">This new name is used by routing, so it will affect the route used to reach this action method.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]

<span data-ttu-id="77b10-181">Этот атрибут применяется к методу действия в `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="77b10-181">This attribute is applied to an action method in the `HomeController`:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]

<span data-ttu-id="77b10-182">Несмотря на то, что имя метода `SomeName`, атрибут MVC соглашение о том, используя имя метода, заменяет имя действия с `MyCoolAction`.</span><span class="sxs-lookup"><span data-stu-id="77b10-182">Even though the method name is `SomeName`, the attribute overrides the MVC convention of using the method name and replaces the action name with `MyCoolAction`.</span></span> <span data-ttu-id="77b10-183">Таким образом, включающий это действие маршрута — `/Home/MyCoolAction`.</span><span class="sxs-lookup"><span data-stu-id="77b10-183">Thus, the route used to reach this action is `/Home/MyCoolAction`.</span></span>

> [!NOTE]
> <span data-ttu-id="77b10-184">В этом примере в основном одинаков применения встроенного [ActionName](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.actionnameattribute) атрибута.</span><span class="sxs-lookup"><span data-stu-id="77b10-184">This example is essentially the same as using the built-in [ActionName](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.actionnameattribute) attribute.</span></span>

### <a name="sample-custom-routing-convention"></a><span data-ttu-id="77b10-185">Образец: Соглашение о маршрутизации пользовательский</span><span class="sxs-lookup"><span data-stu-id="77b10-185">Sample: Custom Routing Convention</span></span>

<span data-ttu-id="77b10-186">Можно использовать `IApplicationModelConvention` для настройки, как работает маршрутизация.</span><span class="sxs-lookup"><span data-stu-id="77b10-186">You can use an `IApplicationModelConvention` to customize how routing works.</span></span> <span data-ttu-id="77b10-187">Например, следующее соглашение будет включать в себя пространств имен контроллеров в маршруты, заменив `.` в пространстве имен с `/` маршрута:</span><span class="sxs-lookup"><span data-stu-id="77b10-187">For example, the following convention will incorporate Controllers' namespaces into their routes, replacing `.` in the namespace with `/` in the route:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]

<span data-ttu-id="77b10-188">Соглашение о добавляется в качестве альтернативы при запуске.</span><span class="sxs-lookup"><span data-stu-id="77b10-188">The convention is added as an option in Startup.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]

> [!TIP]
> <span data-ttu-id="77b10-189">Можно добавить соглашения в вашей [по промежуточного слоя](xref:fundamentals/middleware) , обратившись к `MvcOptions` с помощью`services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`</span><span class="sxs-lookup"><span data-stu-id="77b10-189">You can add conventions to your [middleware](xref:fundamentals/middleware) by accessing `MvcOptions` using `services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`</span></span>

<span data-ttu-id="77b10-190">В этом примере применяется это соглашение маршруты, не использующие атрибут маршрутизации, где контроллер имеет «Пространство имен» в имени.</span><span class="sxs-lookup"><span data-stu-id="77b10-190">This sample applies this convention to routes that are not using attribute routing where the controller has  "Namespace" in its name.</span></span> <span data-ttu-id="77b10-191">Следующий контроллер демонстрирует это соглашение:</span><span class="sxs-lookup"><span data-stu-id="77b10-191">The following controller demonstrates this convention:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]

## <a name="application-model-usage-in-webapicompatshim"></a><span data-ttu-id="77b10-192">Использование модели приложения в WebApiCompatShim</span><span class="sxs-lookup"><span data-stu-id="77b10-192">Application Model Usage in WebApiCompatShim</span></span>

<span data-ttu-id="77b10-193">ASP.NET Core MVC использует другой набор соглашений из веб-API ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="77b10-193">ASP.NET Core MVC uses a different set of conventions from ASP.NET Web API 2.</span></span> <span data-ttu-id="77b10-194">С помощью пользовательского соглашения, можно изменить поведение приложения ASP.NET Core MVC для соответствия, приложения веб-API.</span><span class="sxs-lookup"><span data-stu-id="77b10-194">Using custom conventions, you can modify an ASP.NET Core MVC app's behavior to be consistent with that of a Web API app.</span></span> <span data-ttu-id="77b10-195">Поставляется с Microsoft [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) специально для этой цели.</span><span class="sxs-lookup"><span data-stu-id="77b10-195">Microsoft ships the [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) specifically for this purpose.</span></span>

> [!NOTE]
> <span data-ttu-id="77b10-196">Дополнительные сведения о [миграции с веб-API ASP.NET](xref:migration/webapi).</span><span class="sxs-lookup"><span data-stu-id="77b10-196">Learn more about [migrating from ASP.NET Web API](xref:migration/webapi).</span></span>

<span data-ttu-id="77b10-197">Чтобы использовать оболочку совместимости Web API, необходимо добавить пакет в проект и добавить условные обозначения MVC с помощью `AddWebApiConventions` в `Startup`:</span><span class="sxs-lookup"><span data-stu-id="77b10-197">To use the Web API Compatibility Shim, you need to add the package to your project and then add the conventions to MVC by calling `AddWebApiConventions` in `Startup`:</span></span>

```c#
services.AddMvc().AddWebApiConventions();
```

<span data-ttu-id="77b10-198">Соглашения, предоставляемые прокладка применяются только к части приложения, для которых определенные атрибуты, примененные к ним.</span><span class="sxs-lookup"><span data-stu-id="77b10-198">The conventions provided by the shim are only applied to parts of the app that have had certain attributes applied to them.</span></span> <span data-ttu-id="77b10-199">Следующие четыре атрибуты, используемые для элемента управления, имеющих контроллеры следует их соглашения, была изменена соглашения оболочки.</span><span class="sxs-lookup"><span data-stu-id="77b10-199">The following four attributes are used to control which controllers should have their conventions modified by the shim's conventions:</span></span>

* [<span data-ttu-id="77b10-200">UseWebApiActionConventionsAttribute</span><span class="sxs-lookup"><span data-stu-id="77b10-200">UseWebApiActionConventionsAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [<span data-ttu-id="77b10-201">UseWebApiOverloadingAttribute</span><span class="sxs-lookup"><span data-stu-id="77b10-201">UseWebApiOverloadingAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [<span data-ttu-id="77b10-202">UseWebApiParameterConventionsAttribute</span><span class="sxs-lookup"><span data-stu-id="77b10-202">UseWebApiParameterConventionsAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [<span data-ttu-id="77b10-203">UseWebApiRoutesAttribute</span><span class="sxs-lookup"><span data-stu-id="77b10-203">UseWebApiRoutesAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a><span data-ttu-id="77b10-204">Действия соглашения</span><span class="sxs-lookup"><span data-stu-id="77b10-204">Action Conventions</span></span>

<span data-ttu-id="77b10-205">`UseWebApiActionConventionsAttribute` Используется для сопоставления метода HTTP для действия в зависимости от их имени (например, `Get` будут сопоставлены `HttpGet`).</span><span class="sxs-lookup"><span data-stu-id="77b10-205">The `UseWebApiActionConventionsAttribute` is used to map the HTTP method to actions based on their name (for instance, `Get` would map to `HttpGet`).</span></span> <span data-ttu-id="77b10-206">Он применяется только к действиям, которые не используют атрибут маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="77b10-206">It only applies to actions that do not use attribute routing.</span></span>

### <a name="overloading"></a><span data-ttu-id="77b10-207">Перегрузка</span><span class="sxs-lookup"><span data-stu-id="77b10-207">Overloading</span></span>

<span data-ttu-id="77b10-208">`UseWebApiOverloadingAttribute` Используется для применения `WebApiOverloadingApplicationModelConvention` соглашение.</span><span class="sxs-lookup"><span data-stu-id="77b10-208">The `UseWebApiOverloadingAttribute` is used to apply the `WebApiOverloadingApplicationModelConvention` convention.</span></span> <span data-ttu-id="77b10-209">Это соглашение добавляет `OverloadActionConstraint` для завершения процесса выбора действия, что ограничивает действия кандидатом для тех, для которых запрос удовлетворяет всех обязательных параметров.</span><span class="sxs-lookup"><span data-stu-id="77b10-209">This convention adds an `OverloadActionConstraint` to the action selection process, which limits candidate actions to those for which the request satisfies all non-optional parameters.</span></span>

### <a name="parameter-conventions"></a><span data-ttu-id="77b10-210">Параметр соглашения</span><span class="sxs-lookup"><span data-stu-id="77b10-210">Parameter Conventions</span></span>

<span data-ttu-id="77b10-211">`UseWebApiParameterConventionsAttribute` Используется для применения `WebApiParameterConventionsApplicationModelConvention` действия соглашения.</span><span class="sxs-lookup"><span data-stu-id="77b10-211">The `UseWebApiParameterConventionsAttribute` is used to apply the `WebApiParameterConventionsApplicationModelConvention` action convention.</span></span> <span data-ttu-id="77b10-212">Это соглашение указывает, что простые типы, используемые в качестве параметров действия привязаны из URI по умолчанию во время сложных типов привязаны в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="77b10-212">This convention specifies that simple types used as action parameters are bound from the URI by default, while complex types are bound from the request body.</span></span>

### <a name="routes"></a><span data-ttu-id="77b10-213">Маршруты</span><span class="sxs-lookup"><span data-stu-id="77b10-213">Routes</span></span>

<span data-ttu-id="77b10-214">`UseWebApiRoutesAttribute` Элементы управления ли `WebApiApplicationModelConvention` применяется соглашение контроллера.</span><span class="sxs-lookup"><span data-stu-id="77b10-214">The `UseWebApiRoutesAttribute` controls whether the `WebApiApplicationModelConvention` controller convention is applied.</span></span> <span data-ttu-id="77b10-215">Если включено, это обозначение используется для добавления поддержки [областей](xref:mvc/controllers/areas) для маршрута.</span><span class="sxs-lookup"><span data-stu-id="77b10-215">When enabled, this convention is used to add support for [areas](xref:mvc/controllers/areas) to the route.</span></span>

<span data-ttu-id="77b10-216">В дополнение к набор соглашений, включает пакет совместимости `System.Web.Http.ApiController` базового класса, который заменяет, предоставляемых веб-API.</span><span class="sxs-lookup"><span data-stu-id="77b10-216">In addition to a set of conventions, the compatibility package includes a `System.Web.Http.ApiController` base class that replaces the one provided by Web API.</span></span> <span data-ttu-id="77b10-217">Это позволяет контроллеров, написанные для веб-API и наследование из его `ApiController` будут работать, как они были разработаны, при выполнении с ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="77b10-217">This allows your controllers written for Web API and inheriting from its `ApiController` to work as they were designed, while running on ASP.NET Core MVC.</span></span> <span data-ttu-id="77b10-218">Этот базовый класс, отмеченный со всеми `UseWebApi*` атрибуты, приведенные выше.</span><span class="sxs-lookup"><span data-stu-id="77b10-218">This base controller class is decorated with all of the `UseWebApi*` attributes listed above.</span></span> <span data-ttu-id="77b10-219">`ApiController` Предоставляет свойства, методы и типы результата, совместимые с имеющимся в веб-API.</span><span class="sxs-lookup"><span data-stu-id="77b10-219">The `ApiController` exposes properties, methods, and result types that are compatible with those found in Web API.</span></span>

## <a name="using-apiexplorer-to-document-your-app"></a><span data-ttu-id="77b10-220">С помощью ApiExplorer для документирования приложения</span><span class="sxs-lookup"><span data-stu-id="77b10-220">Using ApiExplorer to Document Your App</span></span>

<span data-ttu-id="77b10-221">Модель приложения предоставляет [ `ApiExplorer` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) свойства на каждом уровне, который может использоваться для обхода структуры приложения.</span><span class="sxs-lookup"><span data-stu-id="77b10-221">The application model exposes an [`ApiExplorer`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) property at each level that can be used to traverse the app's structure.</span></span> <span data-ttu-id="77b10-222">Это может быть использована для [создания страницы справки для вашего веб-API, с помощью таких средств, как Swagger](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span><span class="sxs-lookup"><span data-stu-id="77b10-222">This can be used to [generate help pages for your Web APIs using tools like Swagger](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="77b10-223">`ApiExplorer` Предоставляет свойство `IsVisible` свойство, которое можно задать, чтобы указать, какие части модель приложения должен быть предоставлен.</span><span class="sxs-lookup"><span data-stu-id="77b10-223">The `ApiExplorer` property exposes an `IsVisible` property that can be set to specify which parts of your app's model should be exposed.</span></span> <span data-ttu-id="77b10-224">Можно настроить этот параметр, используя соглашение:</span><span class="sxs-lookup"><span data-stu-id="77b10-224">You can configure this setting using a convention:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]

<span data-ttu-id="77b10-225">Используя этот подход (и дополнительные соглашения, если это необходимо), можно включить или отключить видимость API на любом уровне в приложении.</span><span class="sxs-lookup"><span data-stu-id="77b10-225">Using this approach (and additional conventions if required), you can enable or disable API visibility at any level within your app.</span></span> 
