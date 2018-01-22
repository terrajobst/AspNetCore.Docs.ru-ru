---
title: "Миграция с ASP.NET на ASP.NET Core 2.0"
author: isaac2004
description: "Данный документ содержит инструкции по миграции существующих приложений с ASP.NET MVC или веб-API на ASP.NET Core 2.0."
ms.author: scaddie
manager: wpickett
ms.date: 08/27/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/proper-to-2x/index
ms.openlocfilehash: 96e645129fa53b10d352dcfda8f1ebb152c4dbac
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="migrating-from-aspnet-to-aspnet-core-20"></a><span data-ttu-id="6682c-103">Миграция с ASP.NET на ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="6682c-103">Migrating from ASP.NET to ASP.NET Core 2.0</span></span>

<span data-ttu-id="6682c-104">Автор [Айзек Левин](https://isaaclevin.com) (Isaac Levin)</span><span class="sxs-lookup"><span data-stu-id="6682c-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="6682c-105">Данная статья служит руководством по миграции приложений ASP.NET на ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="6682c-105">This article serves as a reference guide for migrating ASP.NET applications to ASP.NET Core 2.0.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6682c-106">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="6682c-106">Prerequisites</span></span>

* <span data-ttu-id="6682c-107">[Пакет SDK .NET Core 2.0.0](https://dot.net/core) или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="6682c-107">[.NET Core 2.0.0 SDK](https://dot.net/core) or later.</span></span>

## <a name="target-frameworks"></a><span data-ttu-id="6682c-108">Требуемые версии .NET Framework</span><span class="sxs-lookup"><span data-stu-id="6682c-108">Target Frameworks</span></span>
<span data-ttu-id="6682c-109">Проекты ASP.NET Core 2.0 предлагают разработчикам гибкость работы с .NET Core и .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="6682c-109">ASP.NET Core 2.0 projects offer developers the flexibility of targeting .NET Core, .NET Framework, or both.</span></span> <span data-ttu-id="6682c-110">Определить наиболее подходящую платформу поможет статья [Выбор между .NET Core и .NET Framework для серверных приложений](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="6682c-110">See [Choosing between .NET Core and .NET Framework for server apps](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) to determine which target framework is most appropriate.</span></span>

<span data-ttu-id="6682c-111">При разработке для .NET Framework проекты должны ссылаться на отдельные пакеты NuGet.</span><span class="sxs-lookup"><span data-stu-id="6682c-111">When targeting .NET Framework, projects need to reference individual NuGet packages.</span></span>

<span data-ttu-id="6682c-112">Работа с .NET Core позволяет избавиться от многочисленных явных ссылок на пакеты, благодаря [метапакету](xref:fundamentals/metapackage) ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="6682c-112">Targeting .NET Core allows you to eliminate numerous explicit package references, thanks to the ASP.NET Core 2.0 [metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="6682c-113">Установите метапакет `Microsoft.AspNetCore.All` в свой проект.</span><span class="sxs-lookup"><span data-stu-id="6682c-113">Install the `Microsoft.AspNetCore.All` metapackage in your project:</span></span>

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

<span data-ttu-id="6682c-114">При использовании метапакета никакие указанные по ссылкам пакеты с приложением не развертываются.</span><span class="sxs-lookup"><span data-stu-id="6682c-114">When the metapackage is used, no packages referenced in the metapackage are deployed with the app.</span></span> <span data-ttu-id="6682c-115">Все эти ресурсы входят в хранилище среды выполнения .NET Core и предварительно компилируются для повышения производительности.</span><span class="sxs-lookup"><span data-stu-id="6682c-115">The .NET Core Runtime Store includes these assets, and they are precompiled to improve performance.</span></span> <span data-ttu-id="6682c-116">Дополнительные сведения см. в статье [Метапакет Microsoft.AspNetCore.All для ASP.NET Core 2](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="6682c-116">See [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x](xref:fundamentals/metapackage) for more detail.</span></span>

## <a name="project-structure-differences"></a><span data-ttu-id="6682c-117">Различия в структуре пакетов</span><span class="sxs-lookup"><span data-stu-id="6682c-117">Project structure differences</span></span>
<span data-ttu-id="6682c-118">В ASP.NET Core упрощен формат файла *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="6682c-118">The *.csproj* file format has been simplified in ASP.NET Core.</span></span> <span data-ttu-id="6682c-119">Вот некоторые основные изменения.</span><span class="sxs-lookup"><span data-stu-id="6682c-119">Some notable changes include:</span></span>
- <span data-ttu-id="6682c-120">Для того чтобы файлы считались частью проекта, включать их явно теперь не требуется.</span><span class="sxs-lookup"><span data-stu-id="6682c-120">Explicit inclusion of files is not necessary for them to be considered part of the project.</span></span> <span data-ttu-id="6682c-121">Это уменьшает вероятность конфликтов слияния XML при работе в больших командах.</span><span class="sxs-lookup"><span data-stu-id="6682c-121">This reduces the risk of XML merge conflicts when working on large teams.</span></span>
- <span data-ttu-id="6682c-122">GUID-ссылки на другие проекты не используются, что повышает удобочитаемость файла.</span><span class="sxs-lookup"><span data-stu-id="6682c-122">There are no GUID-based references to other projects, which improves file readability.</span></span>
- <span data-ttu-id="6682c-123">Файл можно редактировать, не выгружая его в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6682c-123">The file can be edited without unloading it in Visual Studio:</span></span>

    ![Параметр изменения файла CSPROJ в контекстном меню в Visual Studio 2017](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a><span data-ttu-id="6682c-125">Замена файла Global.asax</span><span class="sxs-lookup"><span data-stu-id="6682c-125">Global.asax file replacement</span></span>
<span data-ttu-id="6682c-126">В ASP.NET Core появился новый механизм для начальной загрузки приложения.</span><span class="sxs-lookup"><span data-stu-id="6682c-126">ASP.NET Core introduced a new mechanism for bootstrapping an app.</span></span> <span data-ttu-id="6682c-127">Точкой входа для приложений ASP.NET стал файл *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="6682c-127">The entry point for ASP.NET applications is the *Global.asax* file.</span></span> <span data-ttu-id="6682c-128">Такие задачи, как конфигурация маршрута, а также регистрации фильтров и областей, теперь выполняются в файле *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="6682c-128">Tasks such as route configuration and filter and area registrations are handled in the *Global.asax* file.</span></span>

[!code-csharp[Main](samples/globalasax-sample.cs)]

<span data-ttu-id="6682c-129">При этом подходе приложение сопоставляется с сервером, на котором оно развертывается, таким образом, чтобы это не мешало реализации.</span><span class="sxs-lookup"><span data-stu-id="6682c-129">This approach couples the application and the server to which it's deployed in a way that interferes with the implementation.</span></span> <span data-ttu-id="6682c-130">Чтобы отделить приложение от сервера, был введен [OWIN](http://owin.org/), который обеспечивает более точный способ совместного использования нескольких платформ.</span><span class="sxs-lookup"><span data-stu-id="6682c-130">In an effort to decouple, [OWIN](http://owin.org/) was introduced to provide a cleaner way to use multiple frameworks together.</span></span> <span data-ttu-id="6682c-131">OWIN предоставляет конвейер для добавления только необходимых модулей.</span><span class="sxs-lookup"><span data-stu-id="6682c-131">OWIN provides a pipeline to add only the modules needed.</span></span> <span data-ttu-id="6682c-132">Среда внешнего размещения принимает функцию [Startup](xref:fundamentals/startup) для настройки служб и конвейера обработки запросов приложения.</span><span class="sxs-lookup"><span data-stu-id="6682c-132">The hosting environment takes a [Startup](xref:fundamentals/startup) function to configure services and the app's request pipeline.</span></span> <span data-ttu-id="6682c-133">`Startup` регистрирует набор ПО промежуточного слоя в приложении.</span><span class="sxs-lookup"><span data-stu-id="6682c-133">`Startup` registers a set of middleware with the application.</span></span> <span data-ttu-id="6682c-134">Для каждого запроса приложение вызывает каждый из компонентов ПО промежуточного слоя с головным указателем связанного списка в существующий набор обработчиков.</span><span class="sxs-lookup"><span data-stu-id="6682c-134">For each request, the application calls each of the the middleware components with the head pointer of a linked list to an existing set of handlers.</span></span> <span data-ttu-id="6682c-135">Каждый компонент ПО промежуточного слоя может добавлять в конвейер обработки запросов один обработчик или несколько.</span><span class="sxs-lookup"><span data-stu-id="6682c-135">Each middleware component can add one or more handlers to the request handling pipeline.</span></span> <span data-ttu-id="6682c-136">Это достигается путем возвращения ссылки на обработчик, который представляет собой новый заголовок списка.</span><span class="sxs-lookup"><span data-stu-id="6682c-136">This is accomplished by returning a reference to the handler that is the new head of the list.</span></span> <span data-ttu-id="6682c-137">Каждый обработчик отвечает за запоминание и вызов следующего обработчика в списке.</span><span class="sxs-lookup"><span data-stu-id="6682c-137">Each handler is responsible for remembering and invoking the next handler in the list.</span></span> <span data-ttu-id="6682c-138">При работе с ASP.NET Core точкой входа в приложения становится `Startup`, и зависимость от файла *Global.asax* исчезает.</span><span class="sxs-lookup"><span data-stu-id="6682c-138">With ASP.NET Core, the entry point to an application is `Startup`, and you no longer have a dependency on *Global.asax*.</span></span> <span data-ttu-id="6682c-139">Используя OWIN с .NET Framework, применяйте для конвейера код следующего вида:</span><span class="sxs-lookup"><span data-stu-id="6682c-139">When using OWIN with .NET Framework, use something like the following as a pipeline:</span></span>

[!code-csharp[Main](samples/webapi-owin.cs)]

<span data-ttu-id="6682c-140">Он определяет ваши маршруты по умолчанию и изначально предусматривает XmlSerialization по Json.</span><span class="sxs-lookup"><span data-stu-id="6682c-140">This configures your default routes, and defaults to XmlSerialization over Json.</span></span> <span data-ttu-id="6682c-141">При необходимости добавьте другое ПО промежуточного слоя для этого конвейера (загрузка служб, параметры конфигурации, статические файлы и т. д.).</span><span class="sxs-lookup"><span data-stu-id="6682c-141">Add other Middleware to this pipeline as needed (loading services, configuration settings, static files, etc.).</span></span>

<span data-ttu-id="6682c-142">ASP.NET Core использует аналогичный подход, но не требует OWIN для обработки запроса.</span><span class="sxs-lookup"><span data-stu-id="6682c-142">ASP.NET Core uses a similar approach, but doesn't rely on OWIN to handle the entry.</span></span> <span data-ttu-id="6682c-143">Вместо этого применяется метод *Program.cs* `Main` (как в консольных приложениях) и `Startup` загружается через него.</span><span class="sxs-lookup"><span data-stu-id="6682c-143">Instead, that is done through the *Program.cs* `Main` method (similar to console applications) and `Startup` is loaded through there.</span></span>

[!code-csharp[Main](samples/program.cs)]

<span data-ttu-id="6682c-144">`Startup` должен включать метод `Configure`.</span><span class="sxs-lookup"><span data-stu-id="6682c-144">`Startup` must include a `Configure` method.</span></span> <span data-ttu-id="6682c-145">В `Configure` добавьте в конвейер необходимое ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="6682c-145">In `Configure`, add the necessary middleware to the pipeline.</span></span> <span data-ttu-id="6682c-146">В следующем примере (на основе шаблона веб-сайта по умолчанию) используются несколько методов расширения для настройки конвейера с поддержкой следующих компонентов.</span><span class="sxs-lookup"><span data-stu-id="6682c-146">In the following example (from the default web site template), several extension methods are used to configure the pipeline with support for:</span></span>

* [<span data-ttu-id="6682c-147">BrowserLink</span><span class="sxs-lookup"><span data-stu-id="6682c-147">BrowserLink</span></span>](http://vswebessentials.com/features/browserlink)
* <span data-ttu-id="6682c-148">Страницы ошибок</span><span class="sxs-lookup"><span data-stu-id="6682c-148">Error pages</span></span>
* <span data-ttu-id="6682c-149">Статические файлы</span><span class="sxs-lookup"><span data-stu-id="6682c-149">Static files</span></span>
* <span data-ttu-id="6682c-150">ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="6682c-150">ASP.NET Core MVC</span></span>
* <span data-ttu-id="6682c-151">идентификации</span><span class="sxs-lookup"><span data-stu-id="6682c-151">Identity</span></span>

[!code-csharp[Main](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

<span data-ttu-id="6682c-152">Сервер и приложение разделены, что позволит вам в будущем легко перейти на другую платформу.</span><span class="sxs-lookup"><span data-stu-id="6682c-152">The host and application have been decoupled, which provides the flexibility of moving to a different platform in the future.</span></span>

<span data-ttu-id="6682c-153">**Примечание**. Более подробный справочник по запуску ASP.NET Core и ПО промежуточного слоя см. в статье [Запуск в ASP.NET Core](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="6682c-153">**Note:** For a more in-depth reference to ASP.NET Core Startup and Middleware, see [Startup in ASP.NET Core](xref:fundamentals/startup)</span></span>

## <a name="storing-configurations"></a><span data-ttu-id="6682c-154">Конфигурации хранения</span><span class="sxs-lookup"><span data-stu-id="6682c-154">Storing Configurations</span></span>
<span data-ttu-id="6682c-155">ASP.NET поддерживает параметры хранения.</span><span class="sxs-lookup"><span data-stu-id="6682c-155">ASP.NET supports storing settings.</span></span> <span data-ttu-id="6682c-156">Эти параметры используются, например, для поддержки среды, в которой развертываются приложения.</span><span class="sxs-lookup"><span data-stu-id="6682c-156">These setting are used, for example, to support the environment to which the applications were deployed.</span></span> <span data-ttu-id="6682c-157">Как правило, все пользовательские пары ключей и значений хранятся в разделе `<appSettings>` файла *Web.config*:</span><span class="sxs-lookup"><span data-stu-id="6682c-157">A common practice was to store all custom key-value pairs in the `<appSettings>` section of the *Web.config* file:</span></span>

[!code-xml[Main](samples/webconfig-sample.xml)]

<span data-ttu-id="6682c-158">Приложения считывают эти параметры с помощью коллекции `ConfigurationManager.AppSettings` в пространстве имен `System.Configuration`:</span><span class="sxs-lookup"><span data-stu-id="6682c-158">Applications read these settings using the `ConfigurationManager.AppSettings` collection in the `System.Configuration` namespace:</span></span>

[!code-csharp[Main](samples/read-webconfig.cs)]

<span data-ttu-id="6682c-159">ASP.NET Core может сохранять данные конфигурации для приложения из любого файла и загружать их в процессе начальной загрузки ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="6682c-159">ASP.NET Core can store configuration data for the application in any file and load them as part of middleware bootstrapping.</span></span> <span data-ttu-id="6682c-160">По умолчанию в шаблонах проектов используется файл *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="6682c-160">The default file used in the project templates is *appsettings.json*:</span></span>

[!code-json[Main](samples/appsettings-sample.json)]

<span data-ttu-id="6682c-161">Загрузка этого файла в экземпляр `IConfiguration` в вашем приложении выполняется в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="6682c-161">Loading this file into an instance of `IConfiguration` inside your application is done in *Startup.cs*:</span></span>

[!code-csharp[Main](samples/startup-builder.cs)]

<span data-ttu-id="6682c-162">Для получения этих параметров приложение считывает данные из `Configuration`:</span><span class="sxs-lookup"><span data-stu-id="6682c-162">The app reads from `Configuration` to get the settings:</span></span>

[!code-csharp[Main](samples/read-appsettings.cs)]

<span data-ttu-id="6682c-163">Существуют расширения, повышающие надежность этого подхода, например, [внедрение зависимостей](xref:fundamentals/dependency-injection) позволяет загружать эти значения в службу.</span><span class="sxs-lookup"><span data-stu-id="6682c-163">There are extensions to this approach to make the process more robust, such as using [Dependency Injection](xref:fundamentals/dependency-injection) (DI) to load a service with these values.</span></span> <span data-ttu-id="6682c-164">Метод внедрения зависимостей предоставляет строго типизированный набор объектов конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6682c-164">The DI approach provides a strongly-typed set of configuration objects.</span></span>

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

<span data-ttu-id="6682c-165">**Примечание**. Более подробное руководство по конфигурации ASP.NET Core см. в статье [Конфигурация в ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="6682c-165">**Note:** For a more in-depth reference to ASP.NET Core configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

## <a name="native-dependency-injection"></a><span data-ttu-id="6682c-166">Введение зависимостей в собственный код</span><span class="sxs-lookup"><span data-stu-id="6682c-166">Native Dependency Injection</span></span>
<span data-ttu-id="6682c-167">При сборке больших, масштабируемых приложений важно обеспечить слабые взаимозависимости между компонентами и службами.</span><span class="sxs-lookup"><span data-stu-id="6682c-167">An important goal when building large, scalable applications is the loose coupling of components and services.</span></span> <span data-ttu-id="6682c-168">[Внедрение зависимостей](xref:fundamentals/dependency-injection) — популярный способ решения этой задачи и собственный компонент ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6682c-168">[Dependency Injection](xref:fundamentals/dependency-injection) is a popular technique for achieving this, and it is a native component of ASP.NET Core.</span></span>

<span data-ttu-id="6682c-169">В приложениях ASP.NET разработчики используют для внедрения зависимостей стороннюю библиотеку.</span><span class="sxs-lookup"><span data-stu-id="6682c-169">In ASP.NET applications, developers rely on a third-party library to implement Dependency Injection.</span></span> <span data-ttu-id="6682c-170">Одна из таких библиотек, [Unity](https://github.com/unitycontainer/unity), входит в шаблоны и рекомендации Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="6682c-170">One such library is [Unity](https://github.com/unitycontainer/unity), provided by Microsoft Patterns & Practices.</span></span> 

<span data-ttu-id="6682c-171">Пример внедрения зависимостей с использованием библиотеки Unity — это реализация `IDependencyResolver` как оболочки для `UnityContainer`:</span><span class="sxs-lookup"><span data-stu-id="6682c-171">An example of setting up Dependency Injection with Unity is implementing `IDependencyResolver` that wraps a `UnityContainer`:</span></span>

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

<span data-ttu-id="6682c-172">Создайте экземпляр `UnityContainer`, зарегистрируйте свою службу и установите средство разрешения зависимостей `HttpConfiguration` в новый экземпляр `UnityResolver` для вашего контейнера:</span><span class="sxs-lookup"><span data-stu-id="6682c-172">Create an instance of your `UnityContainer`, register your service, and set the dependency resolver of `HttpConfiguration` to the new instance of `UnityResolver` for your container:</span></span>

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

<span data-ttu-id="6682c-173">Там, где необходимо, вставьте `IProductRepository`:</span><span class="sxs-lookup"><span data-stu-id="6682c-173">Inject `IProductRepository` where needed:</span></span>

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

<span data-ttu-id="6682c-174">Так как внедрение зависимостей является частью ASP.NET Core, вы можете добавить свою службу в метод `ConfigureServices` файла *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="6682c-174">Because Dependency Injection is part of ASP.NET Core, you can add your service in the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](samples/configure-services.cs)]

<span data-ttu-id="6682c-175">Репозиторий, как и в Unity, можно внедрять где угодно.</span><span class="sxs-lookup"><span data-stu-id="6682c-175">The repository can be injected anywhere, as was true with Unity.</span></span>

<span data-ttu-id="6682c-176">**Примечание**. Подробное руководство по внедрению зависимостей в ASP.NET Core см. в статье [Внедрение зависимостей в ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container).</span><span class="sxs-lookup"><span data-stu-id="6682c-176">**Note:** For an in-depth reference to dependency injection in ASP.NET Core, see [Dependency Injection in ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)</span></span>

## <a name="serving-static-files"></a><span data-ttu-id="6682c-177">Обработка статических файлов</span><span class="sxs-lookup"><span data-stu-id="6682c-177">Serving Static Files</span></span>
<span data-ttu-id="6682c-178">Важной частью разработки веб-приложений является возможность обслуживания статических ресурсов на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="6682c-178">An important part of web development is the ability to serve static, client-side assets.</span></span> <span data-ttu-id="6682c-179">Наиболее распространенные примеры статических файлов — это HTML, каскадные таблицы стилей, Javascript и изображения.</span><span class="sxs-lookup"><span data-stu-id="6682c-179">The most common examples of static files are HTML, CSS, Javascript, and images.</span></span> <span data-ttu-id="6682c-180">Эти файлы необходимо сохранять в место публикации приложения (или CDN) и указывать по ссылкам, чтобы они могли загружаться по запросу.</span><span class="sxs-lookup"><span data-stu-id="6682c-180">These files need to be saved in the published location of the app (or CDN) and referenced so they can be loaded by a request.</span></span> <span data-ttu-id="6682c-181">В ASP.NET Core ситуация изменилась.</span><span class="sxs-lookup"><span data-stu-id="6682c-181">This process has changed in ASP.NET Core.</span></span>

<span data-ttu-id="6682c-182">В ASP.NET статические файлы хранятся в разных папках со ссылками в представлениях.</span><span class="sxs-lookup"><span data-stu-id="6682c-182">In ASP.NET, static files are stored in various directories and referenced in the views.</span></span>

<span data-ttu-id="6682c-183">В ASP.NET Core, если не заданы другие настройки, статические файлы хранятся на корневом веб-узле (*&lt;содержимое корневой папки&gt;/wwwroot*).</span><span class="sxs-lookup"><span data-stu-id="6682c-183">In ASP.NET Core, static files are stored in the "web root" (*&lt;content root&gt;/wwwroot*), unless configured otherwise.</span></span> <span data-ttu-id="6682c-184">Файлы загружаются в конвейер запросов путем вызова метода расширения `UseStaticFiles` из `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="6682c-184">The files are loaded into the request pipeline by invoking the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

[!code-csharp[Main](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

<span data-ttu-id="6682c-185">**Примечание**. Для работы с .NET Framework установите пакет NuGet `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="6682c-185">**Note:** If targeting .NET Framework, install the NuGet package `Microsoft.AspNetCore.StaticFiles`.</span></span>

<span data-ttu-id="6682c-186">Например, ресурс изображения в папке *wwwroot/images* доступен для браузера в расположении `http://<app>/images/<imageFileName>`.</span><span class="sxs-lookup"><span data-stu-id="6682c-186">For example, an image asset in the *wwwroot/images* folder is accessible to the browser at a location such as `http://<app>/images/<imageFileName>`.</span></span>

<span data-ttu-id="6682c-187">**Примечание**. Более подробное руководство по обработке статических файлов в ASP.NET Core см. в статье [Введение в работу со статическими файлами в ASP.NET Core](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="6682c-187">**Note:** For a more in-depth reference to serving static files in ASP.NET Core, see [Introduction to working with static files in ASP.NET Core](xref:fundamentals/static-files).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6682c-188">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6682c-188">Additional Resources</span></span>
* [<span data-ttu-id="6682c-189">Перенос библиотек в .NET Core</span><span class="sxs-lookup"><span data-stu-id="6682c-189">Porting Libraries to .NET Core</span></span>](https://docs.microsoft.com/dotnet/core/porting/libraries)
