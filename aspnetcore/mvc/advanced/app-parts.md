---
title: Части приложения в ASP.NET Core
author: rick-anderson
description: Совместное использование контроллеров, представлений, Razor Pages и других ресурсов с помощью частей приложений в ASP.NET Core
ms.author: riande
ms.date: 05/14/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 4b4c8c554a7045a180b56cf9998ab1a8496cde1b
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2019
ms.locfileid: "71207350"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts-in-aspnet-core"></a><span data-ttu-id="cf4c3-103">Совместное использование контроллеров, представлений, Razor Pages и других ресурсов с помощью частей приложений в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cf4c3-103">Share controllers, views, Razor Pages and more with Application Parts in ASP.NET Core</span></span>

<span data-ttu-id="cf4c3-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="cf4c3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cf4c3-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cf4c3-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="cf4c3-106">*Часть приложения* — это абстракция для ресурсов приложения.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-106">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="cf4c3-107">Части приложений позволяют ASP.NET Core обнаруживать контроллеры, компоненты представлений, вспомогательные функции тегов, Razor Pages, источники компиляции Razor и другие ресурсы.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-107">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="cf4c3-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) — это часть приложения.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is an Application part.</span></span> <span data-ttu-id="cf4c3-109">`AssemblyPart` инкапсулирует ссылку на сборку и предоставляет типы и ссылки на компиляцию.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-109">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="cf4c3-110">*Поставщики компонентов* работают с частями приложения для заполнения компонентов приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-110">*Feature providers* work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="cf4c3-111">Основной вариант использования частей приложения заключается в настройке приложения для обнаружения (или запрета загрузки) компонентов ASP.NET Core из сборки.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-111">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="cf4c3-112">Например, может потребоваться совместное использование общих функциональных возможностей несколькими приложениями.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-112">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="cf4c3-113">С помощью частей приложения можно совместно использовать сборку (библиотека DLL), содержащую контроллеры, представления, Razor Pages, источники компиляции Razor, вспомогательные функции тегов и другие ресурсы, с несколькими приложениями.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-113">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="cf4c3-114">Желательно предоставить общий доступ к сборке для дублирования кода в нескольких проектах.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-114">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="cf4c3-115">Приложения ASP.NET Core загружают компоненты из <xref:System.Web.WebPages.ApplicationPart>.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-115">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="cf4c3-116">Класс <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> представляет часть приложения, поддерживаемую сборкой.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-116">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="cf4c3-117">Загрузка компонентов ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cf4c3-117">Load ASP.NET Core features</span></span>

<span data-ttu-id="cf4c3-118">Используйте классы `ApplicationPart` и `AssemblyPart` для обнаружения и загрузки компонентов ASP.NET Core (контроллеры, компоненты представления и т. д.).</span><span class="sxs-lookup"><span data-stu-id="cf4c3-118">Use the `ApplicationPart` and `AssemblyPart` classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="cf4c3-119"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> отслеживает доступные части приложения и поставщики компонентов.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-119">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="cf4c3-120">Функция `ApplicationPartManager` настраивается в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-120">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="cf4c3-121">Следующий код предоставляет альтернативный подход к настройке `ApplicationPartManager` с использованием `AssemblyPart`:</span><span class="sxs-lookup"><span data-stu-id="cf4c3-121">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="cf4c3-122">Предыдущие два примера кода загружают `SharedController` из сборки.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-122">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="cf4c3-123">`SharedController` не находится в проекте приложения.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-123">The `SharedController` is not in the application's project.</span></span> <span data-ttu-id="cf4c3-124">См. пример [решения WebAppParts](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) для загрузки.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-124">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="cf4c3-125">Включение представлений</span><span class="sxs-lookup"><span data-stu-id="cf4c3-125">Include views</span></span>

<span data-ttu-id="cf4c3-126">Чтобы включить представления в сборку, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-126">To include views in the assembly:</span></span>

* <span data-ttu-id="cf4c3-127">Добавьте следующую разметку в файл общего проекта:</span><span class="sxs-lookup"><span data-stu-id="cf4c3-127">Add the following markup to the shared project file:</span></span>

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* <span data-ttu-id="cf4c3-128">Добавьте <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> в <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span><span class="sxs-lookup"><span data-stu-id="cf4c3-128">Add the <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> to the <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span></span>

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a><span data-ttu-id="cf4c3-129">Запрет загрузки ресурсов</span><span class="sxs-lookup"><span data-stu-id="cf4c3-129">Prevent loading resources</span></span>

<span data-ttu-id="cf4c3-130">Части приложения можно использовать для *запрета* загрузки ресурсов в определенной сборке или расположении.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-130">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="cf4c3-131">Добавьте или удалите элементы коллекции <xref:Microsoft.AspNetCore.Mvc.ApplicationParts>, чтобы скрыть ресурсы или сделать их доступными.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-131">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="cf4c3-132">Порядок записей в коллекции `ApplicationParts` не имеет значения.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-132">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="cf4c3-133">Настройте класс `ApplicationPartManager` перед его использованием для конфигурации служб в контейнере.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-133">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="cf4c3-134">Например, настройте класс `ApplicationPartManager` перед вызовом метода `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-134">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="cf4c3-135">Вызовите метод `Remove` в коллекции `ApplicationParts`, чтобы удалить ресурс.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-135">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="cf4c3-136">В следующем коде для удаления `MyDependentLibrary` из приложения используется <xref:Microsoft.AspNetCore.Mvc.ApplicationParts>: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="cf4c3-136">The following code uses <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> to remove `MyDependentLibrary` from the app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span></span>

<span data-ttu-id="cf4c3-137">`ApplicationPartManager` содержит части для:</span><span class="sxs-lookup"><span data-stu-id="cf4c3-137">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="cf4c3-138">сборки приложения и зависимых сборок;</span><span class="sxs-lookup"><span data-stu-id="cf4c3-138">The app's assembly and dependent assemblies.</span></span>
* <span data-ttu-id="cf4c3-139">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-139">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span></span>
* <span data-ttu-id="cf4c3-140">`Microsoft.AspNetCore.Mvc.Razor`.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-140">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="cf4c3-141">Поставщики компонентов приложений</span><span class="sxs-lookup"><span data-stu-id="cf4c3-141">Application feature providers</span></span>

<span data-ttu-id="cf4c3-142">Поставщики компонентов приложений проверяют части приложения и предоставляют для них компоненты.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-142">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="cf4c3-143">Доступны встроенные поставщики компонентов для следующих компонентов ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="cf4c3-143">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* [<span data-ttu-id="cf4c3-144">Контроллеры</span><span class="sxs-lookup"><span data-stu-id="cf4c3-144">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="cf4c3-145">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="cf4c3-145">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="cf4c3-146">Компоненты представлений</span><span class="sxs-lookup"><span data-stu-id="cf4c3-146">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="cf4c3-147">Поставщики компонентов наследуют от <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, где `T` является типом компонента.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-147">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="cf4c3-148">Поставщики компонентов можно реализовать для любого из перечисленных выше типов компонентов.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-148">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="cf4c3-149">Порядок поставщиков компонентов в `ApplicationPartManager.FeatureProviders` может повлиять на поведение во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-149">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="cf4c3-150">Поставщики, добавленные позднее, могут реагировать на действия, выполняемые ранее добавленными поставщиками.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-150">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="generic-controller-feature"></a><span data-ttu-id="cf4c3-151">компонент универсального контроллера</span><span class="sxs-lookup"><span data-stu-id="cf4c3-151">Generic controller feature</span></span>

<span data-ttu-id="cf4c3-152">ASP.NET Core игнорирует [универсальные контроллеры](/dotnet/csharp/programming-guide/generics/generic-classes).</span><span class="sxs-lookup"><span data-stu-id="cf4c3-152">ASP.NET Core ignores [generic controllers](/dotnet/csharp/programming-guide/generics/generic-classes).</span></span> <span data-ttu-id="cf4c3-153">Универсальный контроллер содержит параметр типа (например, `MyController<T>`).</span><span class="sxs-lookup"><span data-stu-id="cf4c3-153">A generic controller has a type parameter (for example, `MyController<T>`).</span></span> <span data-ttu-id="cf4c3-154">В следующем примере добавляются экземпляры универсального контроллера для указанного списка типов:</span><span class="sxs-lookup"><span data-stu-id="cf4c3-154">The following sample adds generic controller instances for a specified list of types:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

<span data-ttu-id="cf4c3-155">Типы определены в файле `EntityTypes.Types`.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-155">The types are defined in `EntityTypes.Types`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

<span data-ttu-id="cf4c3-156">Поставщик компонентов добавляется в `Startup`:</span><span class="sxs-lookup"><span data-stu-id="cf4c3-156">The feature provider is added in `Startup`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

<span data-ttu-id="cf4c3-157">Имена универсальных контроллеров, используемых для маршрутизации, будут иметь вид *GenericController\`1[Widget]* вместо *Widget*.</span><span class="sxs-lookup"><span data-stu-id="cf4c3-157">Generic controller names used for routing are of the form *GenericController\`1[Widget]* rather than *Widget*.</span></span> <span data-ttu-id="cf4c3-158">Для изменения имени в соответствии с универсальным типом, используемым контроллером, применяется следующий атрибут:</span><span class="sxs-lookup"><span data-stu-id="cf4c3-158">The following attribute modifies the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="cf4c3-159">Класс `GenericController`:</span><span class="sxs-lookup"><span data-stu-id="cf4c3-159">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

<span data-ttu-id="cf4c3-160">Например, запрос URL-адреса `https://localhost:5001/Sprocket` приводит к следующему ответу:</span><span class="sxs-lookup"><span data-stu-id="cf4c3-160">For example, requesting a URL of `https://localhost:5001/Sprocket` results in the following response:</span></span>

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a><span data-ttu-id="cf4c3-161">отображение доступных компонентов</span><span class="sxs-lookup"><span data-stu-id="cf4c3-161">Display available features</span></span>

<span data-ttu-id="cf4c3-162">Компоненты, доступные для приложения, можно перечислить путем запроса `ApplicationPartManager` с помощью [внедрения зависимостей](../../fundamentals/dependency-injection.md):</span><span class="sxs-lookup"><span data-stu-id="cf4c3-162">The features available to an app can be enumerated by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="cf4c3-163">В [примере для скачивания](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) используется приведенный выше код для отображения компонентов приложения:</span><span class="sxs-lookup"><span data-stu-id="cf4c3-163">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features:</span></span>

```text
Controllers:
    - FeaturesController
    - HomeController
    - HelloController
    - GenericController`1
    - GenericController`1
Tag Helpers:
    - PrerenderTagHelper
    - AnchorTagHelper
    - CacheTagHelper
    - DistributedCacheTagHelper
    - EnvironmentTagHelper
    - Additional Tag Helpers omitted for brevity.
View Components:
    - SampleViewComponent
```
