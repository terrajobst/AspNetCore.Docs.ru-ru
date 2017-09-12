---
title: "Миграция с ASP.NET в ASP.NET 2.0 Core"
author: isaac2004
description: "Этот справочник документ содержит инструкции для переноса существующих приложений ASP.NET MVC или веб-API ASP.NET 2.0 Core."
keywords: "Миграция Core, MVC ASP.NET"
ms.author: scaddie
manager: wpickett
ms.date: 08/27/2017
ms.topic: article
ms.assetid: 3155cc9e-d0c9-424b-886c-35c0ec6f9f4e
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc2
ms.openlocfilehash: e0691b276b63ee12d3163ac48d1392696fb97aa6
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="migrating-from-aspnet-to-aspnet-core-20"></a><span data-ttu-id="6a8c7-104">Миграция с ASP.NET в ASP.NET 2.0 Core</span><span class="sxs-lookup"><span data-stu-id="6a8c7-104">Migrating From ASP.NET to ASP.NET Core 2.0</span></span>

<span data-ttu-id="6a8c7-105">По [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="6a8c7-105">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="6a8c7-106">В этой статье служит руководство предназначено для переноса приложений ASP.NET в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-106">This article serves as a reference guide for migrating ASP.NET applications to ASP.NET Core 2.0.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6a8c7-107">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="6a8c7-107">Prerequisites</span></span>

* <span data-ttu-id="6a8c7-108">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-108">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

## <a name="target-frameworks"></a><span data-ttu-id="6a8c7-109">Требуемые версии .NET Framework</span><span class="sxs-lookup"><span data-stu-id="6a8c7-109">Target Frameworks</span></span>
<span data-ttu-id="6a8c7-110">Проекты ASP.NET Core 2.0 предлагает разработчикам гибкость нацеливания .NET Core и .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-110">ASP.NET Core 2.0 projects offer developers the flexibility of targeting .NET Core, .NET Framework, or both.</span></span> <span data-ttu-id="6a8c7-111">В разделе [Выбор между .NET Core и .NET Framework для сервера приложений](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) для определения наиболее подходящего какие требуемой версии .NET framework.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-111">See [Choosing between .NET Core and .NET Framework for server apps](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) to determine which target framework is most appropriate.</span></span>

<span data-ttu-id="6a8c7-112">При разработке для .NET Framework, проекты должны ссылаться на отдельные пакеты NuGet.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-112">When targeting .NET Framework, projects need to reference individual NuGet packages.</span></span>

<span data-ttu-id="6a8c7-113">Отбор информации о .NET Core позволяет устранить множество ссылок явную пакета, благодаря ASP.NET Core 2.0 [metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="6a8c7-113">Targeting .NET Core allows you to eliminate numerous explicit package references, thanks to the ASP.NET Core 2.0 [metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="6a8c7-114">Установка `Microsoft.AspNetCore.All` metapackage в проекте:</span><span class="sxs-lookup"><span data-stu-id="6a8c7-114">Install the `Microsoft.AspNetCore.All` metapackage in your project:</span></span>

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

<span data-ttu-id="6a8c7-115">При использовании metapackage нет пакетов, на которые ссылается metapackage развертываются с приложением.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-115">When the metapackage is used, no packages referenced in the metapackage are deployed with the app.</span></span> <span data-ttu-id="6a8c7-116">Хранилище среды выполнения .NET Core включает эти ресурсы, и они компилируются для повышения производительности.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-116">The .NET Core Runtime Store includes these assets, and they are precompiled to improve performance.</span></span> <span data-ttu-id="6a8c7-117">В разделе [metapackage Microsoft.AspNetCore.All для ASP.NET Core 2.x](xref:fundamentals/metapackage) для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-117">See [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x](xref:fundamentals/metapackage) for more detail.</span></span>

## <a name="project-structure-differences"></a><span data-ttu-id="6a8c7-118">Различия в структуре проекта</span><span class="sxs-lookup"><span data-stu-id="6a8c7-118">Project structure differences</span></span>
<span data-ttu-id="6a8c7-119">*.Csproj* формат файла был упрощен в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-119">The *.csproj* file format has been simplified in ASP.NET Core.</span></span> <span data-ttu-id="6a8c7-120">Ниже перечислены некоторые важные изменения.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-120">Some notable changes include:</span></span>
- <span data-ttu-id="6a8c7-121">Явное включение файлов не является обязательным для них считается частью проекта.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-121">Explicit inclusion of files is not necessary for them to be considered part of the project.</span></span> <span data-ttu-id="6a8c7-122">Это уменьшает вероятность конфликтов слияния XML при работе на больших команд.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-122">This reduces the risk of XML merge conflicts when working on large teams.</span></span>
- <span data-ttu-id="6a8c7-123">Не существует на основе GUID ссылок на другие проекты, что повышает удобочитаемость файла.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-123">There are no GUID-based references to other projects, which improves file readability.</span></span>
- <span data-ttu-id="6a8c7-124">Файл можно изменять без выгрузки его в Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="6a8c7-124">The file can be edited without unloading it in Visual Studio:</span></span>

    ![Измените CSPROJ контекстного меню в Visual Studio 2017 г.](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a><span data-ttu-id="6a8c7-126">Замена файла Global.asax</span><span class="sxs-lookup"><span data-stu-id="6a8c7-126">Global.asax file replacement</span></span>
<span data-ttu-id="6a8c7-127">ASP.NET Core появился новый механизм для начальной загрузки приложения.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-127">ASP.NET Core introduced a new mechanism for bootstrapping an app.</span></span> <span data-ttu-id="6a8c7-128">Точка входа для приложений ASP.NET — *Global.asax* файла.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-128">The entry point for ASP.NET applications is the *Global.asax* file.</span></span> <span data-ttu-id="6a8c7-129">Задачи, например настройка маршрутов и регистрации фильтров и область, обрабатываются в *Global.asax* файла.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-129">Tasks such as route configuration and filter and area registrations are handled in the *Global.asax* file.</span></span>

<span data-ttu-id="6a8c7-130">[!code-csharp[Main](samples/globalasax-sample.cs)]</span><span class="sxs-lookup"><span data-stu-id="6a8c7-130">[!code-csharp[Main](samples/globalasax-sample.cs)]</span></span>

<span data-ttu-id="6a8c7-131">Этот подход связывает приложением и сервером, на котором развертывается в результате которого мешает реализации.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-131">This approach couples the application and the server to which it's deployed in a way that interferes with the implementation.</span></span> <span data-ttu-id="6a8c7-132">С целью отделения [OWIN](http://owin.org/) был введен для обеспечения более точный способ совместное использование нескольких платформ.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-132">In an effort to decouple, [OWIN](http://owin.org/) was introduced to provide a cleaner way to use multiple frameworks together.</span></span> <span data-ttu-id="6a8c7-133">OWIN предоставляет конвейера только те модули, которые требуется добавить.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-133">OWIN provides a pipeline to add only the modules needed.</span></span> <span data-ttu-id="6a8c7-134">Среда размещения принимает [запуска](xref:fundamentals/startup) функции для настройки служб и конвейер обработки запросов приложения.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-134">The hosting environment takes a [Startup](xref:fundamentals/startup) function to configure services and the app's request pipeline.</span></span> <span data-ttu-id="6a8c7-135">`Startup`Регистрирует набор по промежуточного слоя в приложении.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-135">`Startup` registers a set of middleware with the application.</span></span> <span data-ttu-id="6a8c7-136">Для каждого запроса, приложение вызывает каждый из компонентов по промежуточного слоя с головного указателя к существующему набору обработчиков связанного списка.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-136">For each request, the application calls each of the the middleware components with the head pointer of a linked list to an existing set of handlers.</span></span> <span data-ttu-id="6a8c7-137">Каждый компонент по промежуточного слоя можно добавить один или несколько обработчиков для обработки конвейера запросов.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-137">Each middleware component can add one or more handlers to the request handling pipeline.</span></span> <span data-ttu-id="6a8c7-138">Это достигается путем возвращения ссылку на обработчик, который нового заголовка списка.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-138">This is accomplished by returning a reference to the handler that is the new head of the list.</span></span> <span data-ttu-id="6a8c7-139">Каждый обработчик отвечает за запоминание и вызова обработчика далее в списке.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-139">Each handler is responsible for remembering and invoking the next handler in the list.</span></span> <span data-ttu-id="6a8c7-140">С помощью ASP.NET Core является точкой входа приложения `Startup`, и больше не имеют зависимость от *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-140">With ASP.NET Core, the entry point to an application is `Startup`, and you no longer have a dependency on *Global.asax*.</span></span> <span data-ttu-id="6a8c7-141">При использовании OWIN с .NET Framework, используйте следующим как конвейера:</span><span class="sxs-lookup"><span data-stu-id="6a8c7-141">When using OWIN with .NET Framework, use something like the following as a pipeline:</span></span>

<span data-ttu-id="6a8c7-142">[!code-csharp[Main](samples/webapi-owin.cs)]</span><span class="sxs-lookup"><span data-stu-id="6a8c7-142">[!code-csharp[Main](samples/webapi-owin.cs)]</span></span>

<span data-ttu-id="6a8c7-143">Это настраивает ваш маршруты по умолчанию и по умолчанию используется значение XmlSerialization по Json.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-143">This configures your default routes, and defaults to XmlSerialization over Json.</span></span> <span data-ttu-id="6a8c7-144">Добавьте другого по промежуточного слоя для данного конвейера (загрузка служб, параметры конфигурации, статические файлы, и т. д.).</span><span class="sxs-lookup"><span data-stu-id="6a8c7-144">Add other Middleware to this pipeline as needed (loading services, configuration settings, static files, etc.).</span></span>

<span data-ttu-id="6a8c7-145">ASP.NET Core подобный подход используется, но не зависит от OWIN для обработки операции.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-145">ASP.NET Core uses a similar approach, but doesn't rely on OWIN to handle the entry.</span></span> <span data-ttu-id="6a8c7-146">Вместо этого, выполняется с помощью *Program.cs* `Main` метода (аналогично консольных приложений) и `Startup` загружается через него.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-146">Instead, that is done through the *Program.cs* `Main` method (similar to console applications) and `Startup` is loaded through there.</span></span>

<span data-ttu-id="6a8c7-147">[!code-csharp[Main](samples/program.cs)]</span><span class="sxs-lookup"><span data-stu-id="6a8c7-147">[!code-csharp[Main](samples/program.cs)]</span></span>

<span data-ttu-id="6a8c7-148">`Startup`необходимо включить `Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-148">`Startup` must include a `Configure` method.</span></span> <span data-ttu-id="6a8c7-149">В `Configure`, добавьте необходимые по промежуточного слоя в конвейере.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-149">In `Configure`, add the necessary middleware to the pipeline.</span></span> <span data-ttu-id="6a8c7-150">В следующем примере (на основе шаблона веб-сайта по умолчанию) несколько методов расширения используются для настройки конвейера с поддержкой:</span><span class="sxs-lookup"><span data-stu-id="6a8c7-150">In the following example (from the default web site template), several extension methods are used to configure the pipeline with support for:</span></span>

* [<span data-ttu-id="6a8c7-151">BrowserLink</span><span class="sxs-lookup"><span data-stu-id="6a8c7-151">BrowserLink</span></span>](http://vswebessentials.com/features/browserlink)
* <span data-ttu-id="6a8c7-152">Страницы ошибок</span><span class="sxs-lookup"><span data-stu-id="6a8c7-152">Error pages</span></span>
* <span data-ttu-id="6a8c7-153">Статические файлы</span><span class="sxs-lookup"><span data-stu-id="6a8c7-153">Static files</span></span>
* <span data-ttu-id="6a8c7-154">Основные ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="6a8c7-154">ASP.NET Core MVC</span></span>
* <span data-ttu-id="6a8c7-155">Удостоверение</span><span class="sxs-lookup"><span data-stu-id="6a8c7-155">Identity</span></span>

<span data-ttu-id="6a8c7-156">[!code-csharp[Main](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]</span><span class="sxs-lookup"><span data-stu-id="6a8c7-156">[!code-csharp[Main](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]</span></span>

<span data-ttu-id="6a8c7-157">Основное приложение и приложение разделены, благодаря чему обеспечивается гибкость перехода на другой платформе, в будущем.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-157">The host and application have been decoupled, which provides the flexibility of moving to a different platform in the future.</span></span>

<span data-ttu-id="6a8c7-158">**Примечание:** более подробный справочник для запуска ASP.NET Core и по промежуточного слоя см [запуска в ASP.NET Core](xref:fundamentals/startup)</span><span class="sxs-lookup"><span data-stu-id="6a8c7-158">**Note:** For a more in-depth reference to ASP.NET Core Startup and Middleware, see [Startup in ASP.NET Core](xref:fundamentals/startup)</span></span>

## <a name="storing-configurations"></a><span data-ttu-id="6a8c7-159">Сохранение конфигураций</span><span class="sxs-lookup"><span data-stu-id="6a8c7-159">Storing Configurations</span></span>
<span data-ttu-id="6a8c7-160">ASP.NET поддерживает сохранения параметров.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-160">ASP.NET supports storing settings.</span></span> <span data-ttu-id="6a8c7-161">Эти параметры используются, например, для поддержки среды, к которому развернутых приложений.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-161">These setting are used, for example, to support the environment to which the applications were deployed.</span></span> <span data-ttu-id="6a8c7-162">Было принято для хранения всех пользовательских пар ключ значение в `<appSettings>` раздел *Web.config* файла:</span><span class="sxs-lookup"><span data-stu-id="6a8c7-162">A common practice was to store all custom key-value pairs in the `<appSettings>` section of the *Web.config* file:</span></span>

<span data-ttu-id="6a8c7-163">[!code-xml[Main](samples/webconfig-sample.xml)]</span><span class="sxs-lookup"><span data-stu-id="6a8c7-163">[!code-xml[Main](samples/webconfig-sample.xml)]</span></span>

<span data-ttu-id="6a8c7-164">Приложения считывают эти параметры с помощью `ConfigurationManager.AppSettings` коллекции в `System.Configuration` пространство имен:</span><span class="sxs-lookup"><span data-stu-id="6a8c7-164">Applications read these settings using the `ConfigurationManager.AppSettings` collection in the `System.Configuration` namespace:</span></span>

<span data-ttu-id="6a8c7-165">[!code-csharp[Main](samples/read-webconfig.cs)]</span><span class="sxs-lookup"><span data-stu-id="6a8c7-165">[!code-csharp[Main](samples/read-webconfig.cs)]</span></span>

<span data-ttu-id="6a8c7-166">ASP.NET Core можно хранить данные конфигурации для приложения из любого файла и загрузить их как часть начальную загрузку по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-166">ASP.NET Core can store configuration data for the application in any file and load them as part of middleware bootstrapping.</span></span> <span data-ttu-id="6a8c7-167">Файл по умолчанию, используемые в шаблонах проектов *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="6a8c7-167">The default file used in the project templates is *appsettings.json*:</span></span>

<span data-ttu-id="6a8c7-168">[!code-json[Main](samples/appsettings-sample.json)]</span><span class="sxs-lookup"><span data-stu-id="6a8c7-168">[!code-json[Main](samples/appsettings-sample.json)]</span></span>

<span data-ttu-id="6a8c7-169">Загрузить этот файл в экземпляр `IConfiguration` приложение выполняется в внутри *файла Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="6a8c7-169">Loading this file into an instance of `IConfiguration` inside your application is done in *Startup.cs*:</span></span>

<span data-ttu-id="6a8c7-170">[!code-csharp[Main](samples/startup-builder.cs)]</span><span class="sxs-lookup"><span data-stu-id="6a8c7-170">[!code-csharp[Main](samples/startup-builder.cs)]</span></span>

<span data-ttu-id="6a8c7-171">Приложение считывает из `Configuration` для получения этих параметров:</span><span class="sxs-lookup"><span data-stu-id="6a8c7-171">The app reads from `Configuration` to get the settings:</span></span>

<span data-ttu-id="6a8c7-172">[!code-csharp[Main](samples/read-appsettings.cs)]</span><span class="sxs-lookup"><span data-stu-id="6a8c7-172">[!code-csharp[Main](samples/read-appsettings.cs)]</span></span>

<span data-ttu-id="6a8c7-173">В этот подход, чтобы облегчить процесс более надежными, например с помощью расширений [внедрения зависимостей](xref:fundamentals/dependency-injection) (DI) для загрузки службы с этими значениями.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-173">There are extensions to this approach to make the process more robust, such as using [Dependency Injection](xref:fundamentals/dependency-injection) (DI) to load a service with these values.</span></span> <span data-ttu-id="6a8c7-174">DI подход предоставляет набор строго типизированных объектов конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-174">The DI approach provides a strongly-typed set of configuration objects.</span></span>

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

<span data-ttu-id="6a8c7-175">**Примечание:** более подробная справка по конфигурации ASP.NET Core, в разделе [конфигурации в ASP.NET Core](xref:fundamentals/configuration).</span><span class="sxs-lookup"><span data-stu-id="6a8c7-175">**Note:** For a more in-depth reference to ASP.NET Core configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration).</span></span>

## <a name="native-dependency-injection"></a><span data-ttu-id="6a8c7-176">Внедрение зависимостей в машинном коде</span><span class="sxs-lookup"><span data-stu-id="6a8c7-176">Native Dependency Injection</span></span>
<span data-ttu-id="6a8c7-177">При построении больших масштабируемых приложений из важнейших целей — слабых связей между компонентами и службами.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-177">An important goal when building large, scalable applications is the loose coupling of components and services.</span></span> <span data-ttu-id="6a8c7-178">[Внедрение зависимостей](xref:fundamentals/dependency-injection) -это популярный способ для достижения этой цели, и это собственный компонент ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-178">[Dependency Injection](xref:fundamentals/dependency-injection) is a popular technique for achieving this, and it is a native component of ASP.NET Core.</span></span>

<span data-ttu-id="6a8c7-179">В приложениях ASP.NET разработчики используют стороннюю библиотеку для реализации внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-179">In ASP.NET applications, developers rely on a third-party library to implement Dependency Injection.</span></span> <span data-ttu-id="6a8c7-180">Одна библиотека является [Unity](https://github.com/unitycontainer/unity), предоставляемого шаблонов и практик Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-180">One such library is [Unity](https://github.com/unitycontainer/unity), provided by Microsoft Patterns & Practices.</span></span> 

<span data-ttu-id="6a8c7-181">Реализация пример настройки внедрение зависимостей в Unity `IDependencyResolver` -оболочки `UnityContainer`:</span><span class="sxs-lookup"><span data-stu-id="6a8c7-181">An example of setting up Dependency Injection with Unity is implementing `IDependencyResolver` that wraps a `UnityContainer`:</span></span>

<span data-ttu-id="6a8c7-182">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]</span><span class="sxs-lookup"><span data-stu-id="6a8c7-182">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]</span></span>

<span data-ttu-id="6a8c7-183">Создайте экземпляр вашего `UnityContainer`, зарегистрировать службу и установить средство разрешения зависимостей `HttpConfiguration` новому экземпляру `UnityResolver` для контейнера:</span><span class="sxs-lookup"><span data-stu-id="6a8c7-183">Create an instance of your `UnityContainer`, register your service, and set the dependency resolver of `HttpConfiguration` to the new instance of `UnityResolver` for your container:</span></span>

<span data-ttu-id="6a8c7-184">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]</span><span class="sxs-lookup"><span data-stu-id="6a8c7-184">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]</span></span>

<span data-ttu-id="6a8c7-185">Вставить `IProductRepository` при необходимости:</span><span class="sxs-lookup"><span data-stu-id="6a8c7-185">Inject `IProductRepository` where needed:</span></span>

<span data-ttu-id="6a8c7-186">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]</span><span class="sxs-lookup"><span data-stu-id="6a8c7-186">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]</span></span>

<span data-ttu-id="6a8c7-187">Поскольку внедрения зависимостей является частью ASP.NET Core, можно добавить службу в `ConfigureServices` метод *файла Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="6a8c7-187">Because Dependency Injection is part of ASP.NET Core, you can add your service in the `ConfigureServices` method of *Startup.cs*:</span></span>

<span data-ttu-id="6a8c7-188">[!code-csharp[Main](samples/configure-services.cs)]</span><span class="sxs-lookup"><span data-stu-id="6a8c7-188">[!code-csharp[Main](samples/configure-services.cs)]</span></span>

<span data-ttu-id="6a8c7-189">Репозитории могут быть добавлены в любом месте, как и в Unity.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-189">The repository can be injected anywhere, as was true with Unity.</span></span>

<span data-ttu-id="6a8c7-190">**Примечание:** подробная справка по внедрение зависимостей в ASP.NET Core, в разделе [внедрение зависимостей в ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)</span><span class="sxs-lookup"><span data-stu-id="6a8c7-190">**Note:** For an in-depth reference to dependency injection in ASP.NET Core, see [Dependency Injection in ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)</span></span>

## <a name="serving-static-files"></a><span data-ttu-id="6a8c7-191">Обработку статических файлов</span><span class="sxs-lookup"><span data-stu-id="6a8c7-191">Serving Static Files</span></span>
<span data-ttu-id="6a8c7-192">Важной частью разработки веб-приложений является возможность обслуживания статических, клиентского средства.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-192">An important part of web development is the ability to serve static, client-side assets.</span></span> <span data-ttu-id="6a8c7-193">Наиболее распространенными примерами статические файлы, HTML, CSS, Javascript и изображения.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-193">The most common examples of static files are HTML, CSS, Javascript, and images.</span></span> <span data-ttu-id="6a8c7-194">Эти файлы должны быть сохранены в каталог публикации приложения (или CDN) и ссылки, поэтому они могут быть загружены по запросу.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-194">These files need to be saved in the published location of the app (or CDN) and referenced so they can be loaded by a request.</span></span> <span data-ttu-id="6a8c7-195">Этот процесс был изменен в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-195">This process has changed in ASP.NET Core.</span></span>

<span data-ttu-id="6a8c7-196">В ASP.NET статические файлы хранятся в разных каталогах и ссылки в представлениях.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-196">In ASP.NET, static files are stored in various directories and referenced in the views.</span></span>

<span data-ttu-id="6a8c7-197">В ASP.NET Core статические файлы хранятся в «root web» (*&lt;содержимое корневого&gt;/wwwroot*), если не заданы другие настройки.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-197">In ASP.NET Core, static files are stored in the "web root" (*&lt;content root&gt;/wwwroot*), unless configured otherwise.</span></span> <span data-ttu-id="6a8c7-198">Файлы загружаются в конвейер запросов путем вызова `UseStaticFiles` метод расширения из `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="6a8c7-198">The files are loaded into the request pipeline by invoking the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

<span data-ttu-id="6a8c7-199">[!code-csharp[Main](../../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="6a8c7-199">[!code-csharp[Main](../../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]</span></span>

<span data-ttu-id="6a8c7-200">**Примечание:** Если нужна платформа .NET Framework, установите пакет NuGet `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-200">**Note:** If targeting .NET Framework, install the NuGet package `Microsoft.AspNetCore.StaticFiles`.</span></span>

<span data-ttu-id="6a8c7-201">Например, ресурс изображения в *wwwroot/images* папка доступна в браузер в расположении например `http://<app>/images/<imageFileName>`.</span><span class="sxs-lookup"><span data-stu-id="6a8c7-201">For example, an image asset in the *wwwroot/images* folder is accessible to the browser at a location such as `http://<app>/images/<imageFileName>`.</span></span>

<span data-ttu-id="6a8c7-202">**Примечание:** более подробный справочник для обработки статических файлов в ASP.NET Core см [введение в работу с статических файлов в ASP.NET Core](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="6a8c7-202">**Note:** For a more in-depth reference to serving static files in ASP.NET Core, see [Introduction to working with static files in ASP.NET Core](xref:fundamentals/static-files).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6a8c7-203">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6a8c7-203">Additional Resources</span></span>
* [<span data-ttu-id="6a8c7-204">Перенос библиотеки для .NET Core</span><span class="sxs-lookup"><span data-stu-id="6a8c7-204">Porting Libraries to .NET Core</span></span>](https://docs.microsoft.com/dotnet/core/porting/libraries)
