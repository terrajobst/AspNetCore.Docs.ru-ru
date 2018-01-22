---
title: "Частей приложения в ASP.NET Core"
author: ardalis
description: "Сведения об использовании частей приложения, которые являются abstrations ресурсами приложения, чтобы настроить приложение для обнаружения или избегать загрузки компонентов из сборки."
ms.author: riande
manager: wpickett
ms.date: 01/04/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 12c34b7a9521835533998c5609870bc712a6d48c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="application-parts-in-aspnet-core"></a><span data-ttu-id="d52d3-103">Частей приложения в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d52d3-103">Application Parts in ASP.NET Core</span></span>

<span data-ttu-id="d52d3-104">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d52d3-104">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="d52d3-105">*Часть приложения* — это абстракция над ресурсы приложения, из которого MVC такими функциями, как контроллеров, просмотр компонентов или вспомогательных функций тегов могут быть обнаружены.</span><span class="sxs-lookup"><span data-stu-id="d52d3-105">An *Application Part* is an abstraction over the resources of an application, from which MVC features like controllers, view components, or tag helpers may be discovered.</span></span> <span data-ttu-id="d52d3-106">Одним из примеров части приложения является AssemblyPart, который содержит ссылку на сборку и предоставляет типы и ссылки на компиляцию.</span><span class="sxs-lookup"><span data-stu-id="d52d3-106">One example of an application part is an AssemblyPart, which encapsulates an assembly reference and exposes types and compilation references.</span></span> <span data-ttu-id="d52d3-107">*Функции поставщиков* работают с частей приложения для заполнения компонентов приложения ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="d52d3-107">*Feature providers* work with application parts to populate the features of an ASP.NET Core MVC app.</span></span> <span data-ttu-id="d52d3-108">Является позволяют настроить приложения для обнаружения (или избежать загрузки) основного варианта использования для частей приложения MVC компоненты из сборки.</span><span class="sxs-lookup"><span data-stu-id="d52d3-108">The main use case for application parts is to allow you to configure your app to discover (or avoid loading) MVC features from an assembly.</span></span>

## <a name="introducing-application-parts"></a><span data-ttu-id="d52d3-109">Знакомство с приложением частей приложения</span><span class="sxs-lookup"><span data-stu-id="d52d3-109">Introducing Application Parts</span></span>

<span data-ttu-id="d52d3-110">Приложения MVC загрузить их компоненты из [частей приложения](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span><span class="sxs-lookup"><span data-stu-id="d52d3-110">MVC apps load their features from [application parts](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span></span> <span data-ttu-id="d52d3-111">В частности [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) класс представляет часть приложения, поддерживаемый сборки.</span><span class="sxs-lookup"><span data-stu-id="d52d3-111">In particular, the [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) class represents an application part that is backed by an assembly.</span></span> <span data-ttu-id="d52d3-112">Эти классы можно использовать для обнаружения и загрузки MVC функции, например контроллеров, просмотр компонентов, вспомогательных функций тегов и razor компиляции источников.</span><span class="sxs-lookup"><span data-stu-id="d52d3-112">You can use these classes to discover and load MVC features, such as controllers, view components, tag helpers, and razor compilation sources.</span></span> <span data-ttu-id="d52d3-113">[ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) отвечает за отслеживание частей приложения и функции поставщиков, доступных для приложения MVC.</span><span class="sxs-lookup"><span data-stu-id="d52d3-113">The [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) is responsible for tracking the application parts and feature providers available to the MVC app.</span></span> <span data-ttu-id="d52d3-114">Вы можете взаимодействовать с `ApplicationPartManager` в `Startup` при настройке MVC:</span><span class="sxs-lookup"><span data-stu-id="d52d3-114">You can interact with the `ApplicationPartManager` in `Startup` when you configure MVC:</span></span>

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => p.ApplicationParts.Add(part));
```

<span data-ttu-id="d52d3-115">По умолчанию MVC поиск в дереве зависимостей и поиск контроллеров (даже в других сборок).</span><span class="sxs-lookup"><span data-stu-id="d52d3-115">By default MVC will search the dependency tree and find controllers (even in other assemblies).</span></span> <span data-ttu-id="d52d3-116">Попытка загрузить сборку произвольные (например, от подключаемого модуля, который не обращается во время компиляции), можно использовать часть приложения.</span><span class="sxs-lookup"><span data-stu-id="d52d3-116">To load an arbitrary assembly (for instance, from a plugin that isn't referenced at compile time), you can use an application part.</span></span>

<span data-ttu-id="d52d3-117">Можно использовать частей приложения для *избежать* поиск контроллеров в конкретной сборке или в другом расположении.</span><span class="sxs-lookup"><span data-stu-id="d52d3-117">You can use application parts to *avoid* looking for controllers in a particular assembly or location.</span></span> <span data-ttu-id="d52d3-118">Можно выбрать, какие части (или сборок) доступны для приложения, изменив `ApplicationParts` коллекцию `ApplicationPartManager`.</span><span class="sxs-lookup"><span data-stu-id="d52d3-118">You can control which parts (or assemblies) are available to the app by modifying the `ApplicationParts` collection of the `ApplicationPartManager`.</span></span> <span data-ttu-id="d52d3-119">Порядок записей в `ApplicationParts` сбора не имеет значения.</span><span class="sxs-lookup"><span data-stu-id="d52d3-119">The order of the entries in the `ApplicationParts` collection is not important.</span></span> <span data-ttu-id="d52d3-120">Очень важно, чтобы полностью настроить `ApplicationPartManager` перед его использованием для настройки служб в контейнере.</span><span class="sxs-lookup"><span data-stu-id="d52d3-120">It is important to fully configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="d52d3-121">Например, необходимо полностью настроить `ApplicationPartManager` перед вызовом `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="d52d3-121">For example, you should fully configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="d52d3-122">Несоблюдение этого, означает, что в частях приложения добавить контроллеры после вызова метода не будут затронуты (не получить зарегистрирована как службы) что может привести неправильное bevavior приложения.</span><span class="sxs-lookup"><span data-stu-id="d52d3-122">Failing to do so, will mean that controllers in application parts added after that method call will not be affected (will not get registered as services) which might result in incorrect bevavior of your application.</span></span>

<span data-ttu-id="d52d3-123">Если у вас есть к сборке, содержащей контроллеры, вы не хотите использовать, удалите его из `ApplicationPartManager`:</span><span class="sxs-lookup"><span data-stu-id="d52d3-123">If you have an assembly that contains controllers you do not want to be used, remove it from the `ApplicationPartManager`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p =>
    {
        var dependentLibrary = p.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           p.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

<span data-ttu-id="d52d3-124">Помимо сборки проекта и его зависимых сборок `ApplicationPartManager` будет включать части для `Microsoft.AspNetCore.Mvc.TagHelpers` и `Microsoft.AspNetCore.Mvc.Razor` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d52d3-124">In addition to your project's assembly and its dependent assemblies, the `ApplicationPartManager` will include parts for `Microsoft.AspNetCore.Mvc.TagHelpers` and `Microsoft.AspNetCore.Mvc.Razor` by default.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="d52d3-125">Поставщики компонентов приложений</span><span class="sxs-lookup"><span data-stu-id="d52d3-125">Application Feature Providers</span></span>

<span data-ttu-id="d52d3-126">Поставщики функций приложения проверьте частей приложения и предоставляет функциональные возможности для этих частей.</span><span class="sxs-lookup"><span data-stu-id="d52d3-126">Application Feature Providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="d52d3-127">Нет встроенной функции поставщиков для следующих компонентов MVC:</span><span class="sxs-lookup"><span data-stu-id="d52d3-127">There are built-in feature providers for the following MVC features:</span></span>

* [<span data-ttu-id="d52d3-128">Контроллеры</span><span class="sxs-lookup"><span data-stu-id="d52d3-128">Controllers</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="d52d3-129">Ссылка на метаданные</span><span class="sxs-lookup"><span data-stu-id="d52d3-129">Metadata Reference</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [<span data-ttu-id="d52d3-130">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="d52d3-130">Tag Helpers</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="d52d3-131">Просмотр компонентов</span><span class="sxs-lookup"><span data-stu-id="d52d3-131">View Components</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="d52d3-132">Наследовать поставщиков функций `IApplicationFeatureProvider<T>`, где `T` является типом функции.</span><span class="sxs-lookup"><span data-stu-id="d52d3-132">Feature providers inherit from `IApplicationFeatureProvider<T>`, where `T` is the type of the feature.</span></span> <span data-ttu-id="d52d3-133">Вы можете реализовать собственные поставщики для любого типа в MVC функции, перечисленные выше функции.</span><span class="sxs-lookup"><span data-stu-id="d52d3-133">You can implement your own feature providers for any of MVC's feature types listed above.</span></span> <span data-ttu-id="d52d3-134">Порядок поставщиков функций в `ApplicationPartManager.FeatureProviders` коллекции может быть важно, поскольку поставщики более поздних версий можно реагировать на действия, производимые предыдущие поставщики.</span><span class="sxs-lookup"><span data-stu-id="d52d3-134">The order of feature providers in the `ApplicationPartManager.FeatureProviders` collection can be important, since later providers can react to actions taken by previous providers.</span></span>

### <a name="sample-generic-controller-feature"></a><span data-ttu-id="d52d3-135">Образец: Функция универсального контроллера</span><span class="sxs-lookup"><span data-stu-id="d52d3-135">Sample: Generic controller feature</span></span>

<span data-ttu-id="d52d3-136">По умолчанию ASP.NET Core MVC игнорирует универсального контроллеров (например, `SomeController<T>`).</span><span class="sxs-lookup"><span data-stu-id="d52d3-136">By default, ASP.NET Core MVC ignores generic controllers (for example, `SomeController<T>`).</span></span> <span data-ttu-id="d52d3-137">В этом примере используется поставщик функций контроллера, который выполняется после поставщика по умолчанию и добавляет экземпляры универсального контроллера для указанного списка типов (определенные в `EntityTypes.Types`):</span><span class="sxs-lookup"><span data-stu-id="d52d3-137">This sample uses a controller feature provider that runs after the default provider and adds generic controller instances for a specified list of types (defined in `EntityTypes.Types`):</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

<span data-ttu-id="d52d3-138">Типы сущностей:</span><span class="sxs-lookup"><span data-stu-id="d52d3-138">The entity types:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

<span data-ttu-id="d52d3-139">Поставщик функций добавляется в `Startup`:</span><span class="sxs-lookup"><span data-stu-id="d52d3-139">The feature provider is added in `Startup`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p => 
        p.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

<span data-ttu-id="d52d3-140">По умолчанию контроллера, универсальных имен, используемых для маршрутизации будет иметь вид *GenericController "1 [мини-приложение]* вместо *мини-приложение*.</span><span class="sxs-lookup"><span data-stu-id="d52d3-140">By default, the generic controller names used for routing would be of the form *GenericController\`1[Widget]* instead of *Widget*.</span></span> <span data-ttu-id="d52d3-141">Измените имя, чтобы они соответствовали универсального типа, используемое контроллером используется следующий атрибут:</span><span class="sxs-lookup"><span data-stu-id="d52d3-141">The following attribute is used to modify the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="d52d3-142">`GenericController` Класса:</span><span class="sxs-lookup"><span data-stu-id="d52d3-142">The `GenericController` class:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

<span data-ttu-id="d52d3-143">Результат, когда запрашивается совпадающий маршрут:</span><span class="sxs-lookup"><span data-stu-id="d52d3-143">The result, when a matching route is requested:</span></span>

![Считывает пример выходных данных из примера приложения «Hello из универсального контроллера Sproket».](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a><span data-ttu-id="d52d3-145">Образца: Доступные функции для отображения</span><span class="sxs-lookup"><span data-stu-id="d52d3-145">Sample: Display available features</span></span>

<span data-ttu-id="d52d3-146">Можно выполнять итерацию заполненных функции, доступные для приложения, запрашивая `ApplicationPartManager` через [внедрения зависимостей](../../fundamentals/dependency-injection.md) и использовать его для заполнения экземпляры соответствующих компонентов:</span><span class="sxs-lookup"><span data-stu-id="d52d3-146">You can iterate through the populated features available to your app by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md) and using it to populate instances of the appropriate features:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="d52d3-147">Пример выходных данных:</span><span class="sxs-lookup"><span data-stu-id="d52d3-147">Example output:</span></span>

![Пример выходных данных из примера приложения](app-parts/_static/available-features.png)
