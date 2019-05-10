---
title: Части приложения в ASP.NET Core
author: ardalis
description: Сведения о том, как использовать части приложения, которые являются абстракциями по отношению к ресурсам приложения, для обнаружения или запрета загрузки компонентов из сборки.
ms.author: riande
ms.date: 01/04/2017
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 5d5e79d3afe0eee6d0b126d4642ccd5f61bff409
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64888139"
---
# <a name="application-parts-in-aspnet-core"></a><span data-ttu-id="e6fff-103">Части приложения в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6fff-103">Application Parts in ASP.NET Core</span></span>

<span data-ttu-id="e6fff-104">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e6fff-104">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e6fff-105">*Часть приложения* — это абстракция по отношению к ресурсам приложения, из которой могут быть обнаружены такие элементы MVC, как контроллеры, компоненты представлений или вспомогательные функции тегов.</span><span class="sxs-lookup"><span data-stu-id="e6fff-105">An *Application Part* is an abstraction over the resources of an application, from which MVC features like controllers, view components, or tag helpers may be discovered.</span></span> <span data-ttu-id="e6fff-106">Одним из примеров части приложения является AssemblyPart, который содержит ссылку на сборку и предоставляет типы и ссылки на компиляцию.</span><span class="sxs-lookup"><span data-stu-id="e6fff-106">One example of an application part is an AssemblyPart, which encapsulates an assembly reference and exposes types and compilation references.</span></span> <span data-ttu-id="e6fff-107">*Поставщики компонентов* работают с частями приложения для заполнения компонентов приложения ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="e6fff-107">*Feature providers* work with application parts to populate the features of an ASP.NET Core MVC app.</span></span> <span data-ttu-id="e6fff-108">Основной вариант использования частей приложения заключается в настройке приложения для обнаружения (или запрета загрузки) компонентов MVC из сборки.</span><span class="sxs-lookup"><span data-stu-id="e6fff-108">The main use case for application parts is to allow you to configure your app to discover (or avoid loading) MVC features from an assembly.</span></span>

## <a name="introducing-application-parts"></a><span data-ttu-id="e6fff-109">Общие сведения о частях приложения</span><span class="sxs-lookup"><span data-stu-id="e6fff-109">Introducing Application Parts</span></span>

<span data-ttu-id="e6fff-110">Приложения MVC загружают свои компоненты из [частей приложения](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span><span class="sxs-lookup"><span data-stu-id="e6fff-110">MVC apps load their features from [application parts](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span></span> <span data-ttu-id="e6fff-111">В частности, класс [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) представляет часть приложения, поддерживаемую сборкой.</span><span class="sxs-lookup"><span data-stu-id="e6fff-111">In particular, the [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) class represents an application part that's backed by an assembly.</span></span> <span data-ttu-id="e6fff-112">Эти классы можно использовать для обнаружения и загрузки компонентов MVC, таких как контроллеры, компоненты представлений, вспомогательные функции тегов и источники компиляции Razor.</span><span class="sxs-lookup"><span data-stu-id="e6fff-112">You can use these classes to discover and load MVC features, such as controllers, view components, tag helpers, and razor compilation sources.</span></span> <span data-ttu-id="e6fff-113">[ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) отвечает за отслеживание частей приложения и поставщиков компонентов, доступных для приложения MVC.</span><span class="sxs-lookup"><span data-stu-id="e6fff-113">The [ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) is responsible for tracking the application parts and feature providers available to the MVC app.</span></span> <span data-ttu-id="e6fff-114">При настройке MVC можно взаимодействовать с классом `ApplicationPartManager` в `Startup`:</span><span class="sxs-lookup"><span data-stu-id="e6fff-114">You can interact with the `ApplicationPartManager` in `Startup` when you configure MVC:</span></span>

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => apm.ApplicationParts.Add(part));
```

<span data-ttu-id="e6fff-115">По умолчанию MVC выполняет поиск в дереве зависимостей и находит контроллеры (даже в других сборках).</span><span class="sxs-lookup"><span data-stu-id="e6fff-115">By default MVC will search the dependency tree and find controllers (even in other assemblies).</span></span> <span data-ttu-id="e6fff-116">Часть приложения можно использовать для загрузки произвольной сборки (например, из подключаемого модуля, который не указывается во время компиляции).</span><span class="sxs-lookup"><span data-stu-id="e6fff-116">To load an arbitrary assembly (for instance, from a plugin that isn't referenced at compile time), you can use an application part.</span></span>

<span data-ttu-id="e6fff-117">С помощью частей приложения можно *запретить* поиск контроллеров в конкретной сборке или расположении.</span><span class="sxs-lookup"><span data-stu-id="e6fff-117">You can use application parts to *avoid* looking for controllers in a particular assembly or location.</span></span> <span data-ttu-id="e6fff-118">Чтобы контролировать части (или сборки), доступные для приложения, следует изменить коллекцию `ApplicationParts` класса `ApplicationPartManager`.</span><span class="sxs-lookup"><span data-stu-id="e6fff-118">You can control which parts (or assemblies) are available to the app by modifying the `ApplicationParts` collection of the `ApplicationPartManager`.</span></span> <span data-ttu-id="e6fff-119">Порядок записей в коллекции `ApplicationParts` не имеет значения.</span><span class="sxs-lookup"><span data-stu-id="e6fff-119">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="e6fff-120">Очень важно полностью настроить класс `ApplicationPartManager` перед его использованием для конфигурации служб в контейнере.</span><span class="sxs-lookup"><span data-stu-id="e6fff-120">It's important to fully configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="e6fff-121">Например, необходимо полностью настроить класс `ApplicationPartManager` перед вызовом метода `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="e6fff-121">For example, you should fully configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="e6fff-122">В противном случае контроллеры в частях приложения, добавленные после вызова этого метода, не будут затронуты (не будут зарегистрированы как службы), что может привести к неправильной работе приложения.</span><span class="sxs-lookup"><span data-stu-id="e6fff-122">Failing to do so, will mean that controllers in application parts added after that method call won't be affected (won't get registered as services) which might result in incorrect behavior of your application.</span></span>

<span data-ttu-id="e6fff-123">Если у вас есть сборка, содержащая контроллеры, которые не требуется использовать, удалите ее из `ApplicationPartManager`:</span><span class="sxs-lookup"><span data-stu-id="e6fff-123">If you have an assembly that contains controllers you don't want to be used, remove it from the `ApplicationPartManager`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm =>
    {
        var dependentLibrary = apm.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           apm.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

<span data-ttu-id="e6fff-124">Помимо сборки проекта и зависимых сборок, `ApplicationPartManager` будет содержать части для `Microsoft.AspNetCore.Mvc.TagHelpers` и `Microsoft.AspNetCore.Mvc.Razor` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e6fff-124">In addition to your project's assembly and its dependent assemblies, the `ApplicationPartManager` will include parts for `Microsoft.AspNetCore.Mvc.TagHelpers` and `Microsoft.AspNetCore.Mvc.Razor` by default.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="e6fff-125">Поставщики компонентов приложений</span><span class="sxs-lookup"><span data-stu-id="e6fff-125">Application Feature Providers</span></span>

<span data-ttu-id="e6fff-126">Поставщики компонентов приложений проверяют части приложения и предоставляют для них компоненты.</span><span class="sxs-lookup"><span data-stu-id="e6fff-126">Application Feature Providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="e6fff-127">Доступны встроенные поставщики компонентов для следующих компонентов MVC:</span><span class="sxs-lookup"><span data-stu-id="e6fff-127">There are built-in feature providers for the following MVC features:</span></span>

* [<span data-ttu-id="e6fff-128">Контроллеры</span><span class="sxs-lookup"><span data-stu-id="e6fff-128">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="e6fff-129">Ссылка на метаданные</span><span class="sxs-lookup"><span data-stu-id="e6fff-129">Metadata Reference</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [<span data-ttu-id="e6fff-130">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="e6fff-130">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="e6fff-131">Компоненты представлений</span><span class="sxs-lookup"><span data-stu-id="e6fff-131">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="e6fff-132">Поставщики компонентов наследуют от `IApplicationFeatureProvider<T>`, где `T` является типом компонента.</span><span class="sxs-lookup"><span data-stu-id="e6fff-132">Feature providers inherit from `IApplicationFeatureProvider<T>`, where `T` is the type of the feature.</span></span> <span data-ttu-id="e6fff-133">Для всех перечисленных выше типов компонентов MVC можно реализовать собственные поставщики.</span><span class="sxs-lookup"><span data-stu-id="e6fff-133">You can implement your own feature providers for any of MVC's feature types listed above.</span></span> <span data-ttu-id="e6fff-134">Порядок поставщиков компонентов в коллекции `ApplicationPartManager.FeatureProviders` имеет важное значение, поскольку поставщики более поздних версий способны реагировать на действия поставщиков предыдущих версий.</span><span class="sxs-lookup"><span data-stu-id="e6fff-134">The order of feature providers in the `ApplicationPartManager.FeatureProviders` collection can be important, since later providers can react to actions taken by previous providers.</span></span>

### <a name="sample-generic-controller-feature"></a><span data-ttu-id="e6fff-135">Пример: компонент универсального контроллера</span><span class="sxs-lookup"><span data-stu-id="e6fff-135">Sample: Generic controller feature</span></span>

<span data-ttu-id="e6fff-136">По умолчанию ASP.NET Core MVC игнорирует универсальные контроллеры (например, `SomeController<T>`).</span><span class="sxs-lookup"><span data-stu-id="e6fff-136">By default, ASP.NET Core MVC ignores generic controllers (for example, `SomeController<T>`).</span></span> <span data-ttu-id="e6fff-137">В этом примере используется поставщик компонентов контроллера, который запускается после поставщика по умолчанию и добавляет экземпляры универсального контроллера для указанного списка типов (определенного в `EntityTypes.Types`):</span><span class="sxs-lookup"><span data-stu-id="e6fff-137">This sample uses a controller feature provider that runs after the default provider and adds generic controller instances for a specified list of types (defined in `EntityTypes.Types`):</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

<span data-ttu-id="e6fff-138">Типы сущностей:</span><span class="sxs-lookup"><span data-stu-id="e6fff-138">The entity types:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

<span data-ttu-id="e6fff-139">Поставщик компонентов добавляется в `Startup`:</span><span class="sxs-lookup"><span data-stu-id="e6fff-139">The feature provider is added in `Startup`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm => 
        apm.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

<span data-ttu-id="e6fff-140">По умолчанию имена универсальных контроллеров, используемых для маршрутизации, будут иметь вид *GenericController\`1[Widget]* вместо *Widget*.</span><span class="sxs-lookup"><span data-stu-id="e6fff-140">By default, the generic controller names used for routing would be of the form *GenericController\`1[Widget]* instead of *Widget*.</span></span> <span data-ttu-id="e6fff-141">Для изменения имени в соответствии с универсальным типом, используемым контроллером, применяется следующий атрибут:</span><span class="sxs-lookup"><span data-stu-id="e6fff-141">The following attribute is used to modify the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="e6fff-142">Класс `GenericController`:</span><span class="sxs-lookup"><span data-stu-id="e6fff-142">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

<span data-ttu-id="e6fff-143">При запросе совпадающего маршрута выводится следующий результат:</span><span class="sxs-lookup"><span data-stu-id="e6fff-143">The result, when a matching route is requested:</span></span>

![Пример выходных данных из примера приложения: "Hello from a generic Sproket controller".](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a><span data-ttu-id="e6fff-145">Пример: отображение доступных компонентов</span><span class="sxs-lookup"><span data-stu-id="e6fff-145">Sample: Display available features</span></span>

<span data-ttu-id="e6fff-146">Для итерации по заполненным компонентам, доступным для приложения, можно запросить класс `ApplicationPartManager` через [внедрение зависимостей](../../fundamentals/dependency-injection.md) и использовать его для заполнения экземпляров соответствующих компонентов:</span><span class="sxs-lookup"><span data-stu-id="e6fff-146">You can iterate through the populated features available to your app by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md) and using it to populate instances of the appropriate features:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="e6fff-147">Пример выходных данных:</span><span class="sxs-lookup"><span data-stu-id="e6fff-147">Example output:</span></span>

![Пример выходных данных из примера приложения](app-parts/_static/available-features.png)
