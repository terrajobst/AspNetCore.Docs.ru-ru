---
title: "Миграция с ASP.NET Core 1.x на 2.0"
author: scottaddie
description: "В этой статье описываются предварительные требования и стандартные этапы миграции проекта ASP.NET Core 1.x в ASP.NET Core 2.0."
keywords: "ASP.NET Core, миграция"
ms.author: scaddie
manager: wpickett
ms.date: 08/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/index
ms.openlocfilehash: db277ee6b079eab973a565983d6661bf95fce2e3
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="migrating-from-aspnet-core-1x-to-aspnet-core-20"></a><span data-ttu-id="cde93-104">Миграция с ASP.NET Core 1.x на ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="cde93-104">Migrating from ASP.NET Core 1.x to ASP.NET Core 2.0</span></span>

<span data-ttu-id="cde93-105">Автор: [Скотт Адди](https://github.com/scottaddie) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="cde93-105">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="cde93-106">В этой статье поэтапно рассматривается процесс обновления существующего проекта ASP.NET Core 1.x до версии ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="cde93-106">In this article, we'll walk you through updating an existing ASP.NET Core 1.x project to ASP.NET Core 2.0.</span></span> <span data-ttu-id="cde93-107">Миграция приложения в ASP.NET Core 2.0 позволяет воспользоваться [множеством новых функций и улучшений производительности](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="cde93-107">Migrating your application to ASP.NET Core 2.0 enables you to take advantage of [many new features and performance improvements](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0).</span></span> 

<span data-ttu-id="cde93-108">Существующие приложения ASP.NET Core 1.x основаны на шаблонах проектов для определенной версии.</span><span class="sxs-lookup"><span data-stu-id="cde93-108">Existing ASP.NET Core 1.x applications are based off of version-specific project templates.</span></span> <span data-ttu-id="cde93-109">По мере развития платформы ASP.NET Core совершенствуются шаблоны проектов и содержащийся в них начальный код.</span><span class="sxs-lookup"><span data-stu-id="cde93-109">As the ASP.NET Core framework evolves, so do the project templates and the starter code contained within them.</span></span> <span data-ttu-id="cde93-110">Помимо обновления платформы ASP.NET Core, необходимо также обновить код приложения.</span><span class="sxs-lookup"><span data-stu-id="cde93-110">In addition to updating the ASP.NET Core framework, you need to update the code for your application.</span></span>

<a name="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="cde93-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="cde93-111">Prerequisites</span></span>
<span data-ttu-id="cde93-112">См. статью [Начало работы с ASP.NET Core](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="cde93-112">Please see [Getting Started with ASP.NET Core](xref:getting-started).</span></span>

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a><span data-ttu-id="cde93-113">Обновление моникера целевой платформы (TFM)</span><span class="sxs-lookup"><span data-stu-id="cde93-113">Update Target Framework Moniker (TFM)</span></span>
<span data-ttu-id="cde93-114">Проекты, предназначенные для .NET Core, должны использовать [моникер целевой платформы](/dotnet/standard/frameworks#referring-to-frameworks) версии не ниже .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="cde93-114">Projects targeting .NET Core should use the [TFM](/dotnet/standard/frameworks#referring-to-frameworks) of a version greater than or equal to .NET Core 2.0.</span></span> <span data-ttu-id="cde93-115">Найдите в файле *CSPROJ* узел `<TargetFramework>` и замените его содержимое на `netcoreapp2.0`:</span><span class="sxs-lookup"><span data-stu-id="cde93-115">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `netcoreapp2.0`:</span></span>

<span data-ttu-id="cde93-116">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=3)]</span><span class="sxs-lookup"><span data-stu-id="cde93-116">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=3)]</span></span>

<span data-ttu-id="cde93-117">Проекты, предназначенные для .NET Framework, должны использовать моникер целевой платформы версии не ниже .NET Framework 4.6.1.</span><span class="sxs-lookup"><span data-stu-id="cde93-117">Projects targeting .NET Framework should use the TFM of a version greater than or equal to .NET Framework 4.6.1.</span></span> <span data-ttu-id="cde93-118">Найдите в файле *CSPROJ* узел `<TargetFramework>` и замените его содержимое на `net461`:</span><span class="sxs-lookup"><span data-stu-id="cde93-118">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `net461`:</span></span>

<span data-ttu-id="cde93-119">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]</span><span class="sxs-lookup"><span data-stu-id="cde93-119">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]</span></span>

> [!NOTE]
> <span data-ttu-id="cde93-120">.NET Core 2.0 обеспечивает гораздо большую контактную зону по сравнению с .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="cde93-120">.NET Core 2.0 offers a much larger surface area than .NET Core 1.x.</span></span> <span data-ttu-id="cde93-121">Если вы используете .NET Framework только из-за отсутствия нужных API в .NET Core 1.x, скорее всего, .NET Core 2.0 удовлетворит ваши потребности.</span><span class="sxs-lookup"><span data-stu-id="cde93-121">If you're targeting .NET Framework solely because of missing APIs in .NET Core 1.x, targeting .NET Core 2.0 is likely to work.</span></span>

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a><span data-ttu-id="cde93-122">Обновление версии пакета SDK для .NET Core в файле global.json</span><span class="sxs-lookup"><span data-stu-id="cde93-122">Update .NET Core SDK version in global.json</span></span>
<span data-ttu-id="cde93-123">Если ваше решение использует файл [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) для указания целевой версии пакета SDK для .NET Core, измените значение свойства `version` так, чтобы использовалась версия 2.0, установленная на компьютере:</span><span class="sxs-lookup"><span data-stu-id="cde93-123">If your solution relies upon a [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) file to target a specific .NET Core SDK version, update its `version` property to use the 2.0 version installed on your machine:</span></span>

<span data-ttu-id="cde93-124">[!code-json[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/global.json?highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="cde93-124">[!code-json[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/global.json?highlight=3)]</span></span>

<a name="package-reference"></a>

## <a name="update-package-references"></a><span data-ttu-id="cde93-125">Обновление ссылок на пакеты</span><span class="sxs-lookup"><span data-stu-id="cde93-125">Update package references</span></span>
<span data-ttu-id="cde93-126">В файле *CSPROJ* в проекте версии 1.x перечислены все проекты NuGet, используемые проектом.</span><span class="sxs-lookup"><span data-stu-id="cde93-126">The *.csproj* file in a 1.x project lists each NuGet package used by the project.</span></span>

<span data-ttu-id="cde93-127">В проекте ASP.NET Core 2.0, предназначенном для .NET Core 2.0, коллекция пакетов в файле *CSPROJ* заменяется ссылкой на один [метапакет](xref:fundamentals/metapackage):</span><span class="sxs-lookup"><span data-stu-id="cde93-127">In an ASP.NET Core 2.0 project targeting .NET Core 2.0, a single [metapackage](xref:fundamentals/metapackage) reference in the *.csproj* file replaces the collection of packages:</span></span>

<span data-ttu-id="cde93-128">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=9-11)]</span><span class="sxs-lookup"><span data-stu-id="cde93-128">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=9-11)]</span></span>

<span data-ttu-id="cde93-129">В метапакет входят все компоненты ASP.NET Core 2.0 и Entity Framework Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="cde93-129">All the features of ASP.NET Core 2.0 and Entity Framework Core 2.0 are included in the metapackage.</span></span>

<span data-ttu-id="cde93-130">В проектах ASP.NET Core 2.0, предназначенных для .NET Framework, по-прежнему должны использоваться ссылки на отдельные пакеты NuGet.</span><span class="sxs-lookup"><span data-stu-id="cde93-130">ASP.NET Core 2.0 projects targeting .NET Framework should continue to reference individual NuGet packages.</span></span> <span data-ttu-id="cde93-131">Измените значение атрибута `Version` каждого узла `<PackageReference />` на 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="cde93-131">Update the `Version` attribute of each `<PackageReference />` node to 2.0.0.</span></span>

<span data-ttu-id="cde93-132">Например, вот список узлов `<PackageReference />`, используемых в типичном проекте ASP.NET Core 2.0, предназначенном для .NET Framework:</span><span class="sxs-lookup"><span data-stu-id="cde93-132">For example, here's the list of `<PackageReference />` nodes used in a typical ASP.NET Core 2.0 project targeting .NET Framework:</span></span>

<span data-ttu-id="cde93-133">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]</span><span class="sxs-lookup"><span data-stu-id="cde93-133">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]</span></span>

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a><span data-ttu-id="cde93-134">Обновление средств CLI для .NET Core</span><span class="sxs-lookup"><span data-stu-id="cde93-134">Update .NET Core CLI tools</span></span>
<span data-ttu-id="cde93-135">Измените значение атрибута * каждого узла * в файле `Version`CSPROJ`<DotNetCliToolReference />` на 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="cde93-135">In the *.csproj* file, update the `Version` attribute of each `<DotNetCliToolReference />` node to 2.0.0.</span></span>

<span data-ttu-id="cde93-136">Например, вот список средств CLI, используемых в типичном проекте ASP.NET Core 2.0, предназначенном для .NET Core 2.0:</span><span class="sxs-lookup"><span data-stu-id="cde93-136">For example, here's the list of CLI tools used in a typical ASP.NET Core 2.0 project targeting .NET Core 2.0:</span></span>

<span data-ttu-id="cde93-137">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=13-17)]</span><span class="sxs-lookup"><span data-stu-id="cde93-137">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=13-17)]</span></span>

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a><span data-ttu-id="cde93-138">Переименование свойства PackageTargetFallback</span><span class="sxs-lookup"><span data-stu-id="cde93-138">Rename Package Target Fallback property</span></span>
<span data-ttu-id="cde93-139">В проектах версии 1.x в файлах *CSPROJ* использовался узел `PackageTargetFallback` и переменная:</span><span class="sxs-lookup"><span data-stu-id="cde93-139">The *.csproj* file of a 1.x project used a `PackageTargetFallback` node and variable:</span></span>

<span data-ttu-id="cde93-140">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=5)]</span><span class="sxs-lookup"><span data-stu-id="cde93-140">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=5)]</span></span>

<span data-ttu-id="cde93-141">Переименуйте этот узел и переменную в `AssetTargetFallback`:</span><span class="sxs-lookup"><span data-stu-id="cde93-141">Rename both the node and variable to `AssetTargetFallback`:</span></span>

<span data-ttu-id="cde93-142">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=5)]</span><span class="sxs-lookup"><span data-stu-id="cde93-142">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=5)]</span></span>

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a><span data-ttu-id="cde93-143">Обновление метода Main в файле Program.cs</span><span class="sxs-lookup"><span data-stu-id="cde93-143">Update Main method in Program.cs</span></span>
<span data-ttu-id="cde93-144">В проектах версии 1.x метод `Main` в файле *Program.cs* выглядел так:</span><span class="sxs-lookup"><span data-stu-id="cde93-144">In 1.x projects, the `Main` method of *Program.cs* looked like this:</span></span>

<span data-ttu-id="cde93-145">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]</span><span class="sxs-lookup"><span data-stu-id="cde93-145">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]</span></span>

<span data-ttu-id="cde93-146">В проектах версии 2.0 метод `Main` в файле *Program.cs* упрощен:</span><span class="sxs-lookup"><span data-stu-id="cde93-146">In 2.0 projects, the `Main` method of *Program.cs* has been simplified:</span></span>

<span data-ttu-id="cde93-147">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Program.cs?highlight=8-11)]</span><span class="sxs-lookup"><span data-stu-id="cde93-147">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Program.cs?highlight=8-11)]</span></span>

<span data-ttu-id="cde93-148">Настоятельно рекомендуется внедрить этот новый шаблон версии 2.0, так как он необходим для работы таких функций, как [миграция Entity Framework Core](xref:data/ef-mvc/migrations).</span><span class="sxs-lookup"><span data-stu-id="cde93-148">The adoption of this new 2.0 pattern is highly recommended and is required for product features like [Entity Framework Core Migrations](xref:data/ef-mvc/migrations) to work.</span></span> <span data-ttu-id="cde93-149">Например, при выполнении команды `Update-Database` из консоли диспетчера пакетов или команды `dotnet ef database update` в командной строке (для проектов, преобразованных в ASP.NET Core 2.0) возникает следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="cde93-149">For example, running `Update-Database` from the Package Manager Console window or `dotnet ef database update` from the command line (on projects converted to ASP.NET Core 2.0) generates the following error:</span></span>

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="view-compilation"></a>

## <a name="review-your-razor-view-compilation-setting"></a><span data-ttu-id="cde93-150">Проверка параметра компиляции представлений Razor</span><span class="sxs-lookup"><span data-stu-id="cde93-150">Review your Razor View Compilation setting</span></span>
<span data-ttu-id="cde93-151">Сокращение времени запуска приложений и уменьшение размеров публикуемых пакетов крайне важны.</span><span class="sxs-lookup"><span data-stu-id="cde93-151">Faster application startup time and smaller published bundles are of utmost importance to you.</span></span> <span data-ttu-id="cde93-152">По этой причине в ASP.NET Core 2.0 по умолчанию включена [компиляция представлений Razor](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="cde93-152">For these reasons, [Razor view compilation](xref:mvc/views/view-compilation) is enabled by default in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="cde93-153">Присваивать свойству `MvcRazorCompileOnPublish` значение true больше не нужно.</span><span class="sxs-lookup"><span data-stu-id="cde93-153">Setting the `MvcRazorCompileOnPublish` property to true is no longer required.</span></span> <span data-ttu-id="cde93-154">Если вы не собираетесь отключать компиляцию представлений, это свойство можно удалить из файла *CSPROJ*.</span><span class="sxs-lookup"><span data-stu-id="cde93-154">Unless you're disabling view compilation, the property may be removed from the *.csproj* file.</span></span>

<span data-ttu-id="cde93-155">Если проект предназначен для .NET Framework, необходимо по-прежнему явно указывать ссылку на пакет NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) в файле *CSPROJ*:</span><span class="sxs-lookup"><span data-stu-id="cde93-155">When targeting .NET Framework, you still need to explicitly reference the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet package in your *.csproj* file:</span></span>

<span data-ttu-id="cde93-156">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]</span><span class="sxs-lookup"><span data-stu-id="cde93-156">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]</span></span>

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a><span data-ttu-id="cde93-157">Использование функций подготовки Application Insights</span><span class="sxs-lookup"><span data-stu-id="cde93-157">Rely on Application Insights "Light-Up" features</span></span>
<span data-ttu-id="cde93-158">Простота настройки инструментария для обеспечения производительности приложений имеет большое значение.</span><span class="sxs-lookup"><span data-stu-id="cde93-158">Effortless setup of application performance instrumentation is important.</span></span> <span data-ttu-id="cde93-159">Теперь вам доступны новые функции подготовки [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) в составе средств Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="cde93-159">You can now rely on the new [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" features available in the Visual Studio 2017 tooling.</span></span>

<span data-ttu-id="cde93-160">При создании проектов ASP.NET Core 1.1 в Visual Studio 2017 служба Application Insights добавлялась по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cde93-160">ASP.NET Core 1.1 projects created in Visual Studio 2017 added Application Insights by default.</span></span> <span data-ttu-id="cde93-161">Если вы не используете пакет SDK для Application Insights напрямую вне файлов *Program.cs* и *Startup.cs*, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="cde93-161">If you're not using the Application Insights SDK directly, outside of *Program.cs* and *Startup.cs*, follow these steps:</span></span>

1. <span data-ttu-id="cde93-162">Удалите из файла *CSPROJ* следующий узел `<PackageReference />`:</span><span class="sxs-lookup"><span data-stu-id="cde93-162">Remove the following `<PackageReference />` node from the *.csproj* file:</span></span>
    
    <span data-ttu-id="cde93-163">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=10)]</span><span class="sxs-lookup"><span data-stu-id="cde93-163">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=10)]</span></span>

2. <span data-ttu-id="cde93-164">Удалите вызов метода расширения `UseApplicationInsights` из файла *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="cde93-164">Remove the `UseApplicationInsights` extension method invocation from *Program.cs*:</span></span>

    <span data-ttu-id="cde93-165">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]</span><span class="sxs-lookup"><span data-stu-id="cde93-165">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]</span></span>

3. <span data-ttu-id="cde93-166">Удалите вызов API на стороне клиента Application Insights из файла *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="cde93-166">Remove the Application Insights client-side API call from *_Layout.cshtml*.</span></span> <span data-ttu-id="cde93-167">Этот вызов включает две строки кода:</span><span class="sxs-lookup"><span data-stu-id="cde93-167">It comprises the following two lines of code:</span></span>

    <span data-ttu-id="cde93-168">[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Views/Shared/_Layout.cshtml?range=1,19)]</span><span class="sxs-lookup"><span data-stu-id="cde93-168">[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Views/Shared/_Layout.cshtml?range=1,19)]</span></span>

<span data-ttu-id="cde93-169">Если вы используете пакет SDK для Application Insights напрямую, продолжайте делать это.</span><span class="sxs-lookup"><span data-stu-id="cde93-169">If you are using the Application Insights SDK directly, continue to do so.</span></span> <span data-ttu-id="cde93-170">[Метапакет](xref:fundamentals/metapackage) версии 2.0 включает в себя последнюю версию Application Insights, поэтому при попытке сослаться на более старую версию возникает ошибка понижения уровня пакета.</span><span class="sxs-lookup"><span data-stu-id="cde93-170">The 2.0 [metapackage](xref:fundamentals/metapackage) includes the latest version of Application Insights, so a package downgrade error appears if you're referencing an older version.</span></span>

<a name="auth-and-identity"></a>

## <a name="adopt-authentication--identity-improvements"></a><span data-ttu-id="cde93-171">Внедрение улучшений, связанных с проверкой подлинности и удостоверениями</span><span class="sxs-lookup"><span data-stu-id="cde93-171">Adopt Authentication / Identity Improvements</span></span>
<span data-ttu-id="cde93-172">В ASP.NET Core 2.0 реализована новая модель проверки подлинности и внесен ряд важных изменений в удостоверение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cde93-172">ASP.NET Core 2.0 has a new authentication model and a number of significant changes to ASP.NET Core Identity.</span></span> <span data-ttu-id="cde93-173">Если при создании проекта вы включили отдельные учетные записи пользователей либо вручную добавили проверку подлинности или удостоверение, см. статью [Миграция на другой метод проверки подлинности и другие удостоверения в ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span><span class="sxs-lookup"><span data-stu-id="cde93-173">If you created your project with Individual User Accounts enabled, or if you have manually added authentication or Identity, see [Migrating Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cde93-174">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="cde93-174">Additional Resources</span></span>
- [<span data-ttu-id="cde93-175">Критические изменения в ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="cde93-175">Breaking Changes in ASP.NET Core 2.0</span></span>](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
