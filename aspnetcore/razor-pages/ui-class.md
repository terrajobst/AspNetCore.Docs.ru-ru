---
title: Многоразовый интерфейс Razor в библиотеках классов в ASP.NET Core
author: Rick-Anderson
description: В этой статье описывается создание многократно используемых Razor пользовательского интерфейса, использование частичных представлений в библиотеку классов, в ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 10/08/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: d656e924033f1b217cdd8c86f7d00411c5d71beb
ms.sourcegitcommit: 73a451e9a58ac7102f90b608d661d8c23dd9bbaf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/08/2019
ms.locfileid: "72037583"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="93057-103">Создание повторно используемого пользовательского интерфейса с помощью проекта библиотеки классов Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="93057-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="93057-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="93057-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="93057-105">Представления Razor, страницы, контроллеры, модели страниц, [компоненты Razor](xref:blazor/class-libraries), [Компоненты представления](xref:mvc/views/view-components)и модели данных могут быть встроены в БИБЛИОТЕКУ классов Razor (РКЛ).</span><span class="sxs-lookup"><span data-stu-id="93057-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="93057-106">RCL можно упаковать и использовать повторно.</span><span class="sxs-lookup"><span data-stu-id="93057-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="93057-107">Приложения могут включать RCL и переопределять содержащиеся в нем представления и страницы.</span><span class="sxs-lookup"><span data-stu-id="93057-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="93057-108">При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в RCL приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="93057-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="93057-109">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="93057-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="93057-110">Создание библиотеки классов с пользовательским интерфейсом Razor</span><span class="sxs-lookup"><span data-stu-id="93057-110">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="93057-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93057-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="93057-112">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="93057-112">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="93057-113">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="93057-113">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="93057-114">Назовите библиотеку (например, "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="93057-114">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="93057-115">Чтобы избежать конфликта имени файла с созданной библиотекой представлений, проверьте, что имя библиотеки не заканчивается на `.Views`.</span><span class="sxs-lookup"><span data-stu-id="93057-115">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="93057-116">Убедитесь, что выбран параметр **ASP.NET Core 3,0** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="93057-116">Verify **ASP.NET Core 3.0** or later is selected.</span></span>
* <span data-ttu-id="93057-117">Выберите **Razor Class Library** > **ОК**.</span><span class="sxs-lookup"><span data-stu-id="93057-117">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="93057-118">По умолчанию для шаблона библиотеки классов Razor (RCL) используется разработка компонентов Razor.</span><span class="sxs-lookup"><span data-stu-id="93057-118">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="93057-119">Параметр шаблона в Visual Studio обеспечивает поддержку шаблонов для страниц и представлений.</span><span class="sxs-lookup"><span data-stu-id="93057-119">A template option in Visual Studio provides template support for pages and views.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="93057-120">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="93057-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="93057-121">Выполните из командной строки команду `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="93057-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="93057-122">Пример:</span><span class="sxs-lookup"><span data-stu-id="93057-122">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="93057-123">По умолчанию для шаблона библиотеки классов Razor (RCL) используется разработка компонентов Razor.</span><span class="sxs-lookup"><span data-stu-id="93057-123">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="93057-124">Передайте параметр `--support-pages-and-views` (`dotnet new razorclasslib --support-pages-and-views`), чтобы обеспечить поддержку страниц и представлений.</span><span class="sxs-lookup"><span data-stu-id="93057-124">Pass the `--support-pages-and-views` option (`dotnet new razorclasslib --support-pages-and-views`) to provide support for pages and views.</span></span>

<span data-ttu-id="93057-125">Дополнительные сведения см. в разделе [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="93057-125">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="93057-126">Чтобы избежать конфликта имени файла с созданной библиотекой представлений, проверьте, что имя библиотеки не заканчивается на `.Views`.</span><span class="sxs-lookup"><span data-stu-id="93057-126">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="93057-127">Добавление файлов Razor в RCL.</span><span class="sxs-lookup"><span data-stu-id="93057-127">Add Razor files to the RCL.</span></span>

<span data-ttu-id="93057-128">Шаблоны ASP.NET Core считает содержимое RCL *областей* папки.</span><span class="sxs-lookup"><span data-stu-id="93057-128">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="93057-129">Чтобы создать РКЛ, который предоставляет содержимое `~/Pages`, а не `~/Areas/Pages`, см. раздел [макет страниц РКЛ](#rcl-pages-layout) .</span><span class="sxs-lookup"><span data-stu-id="93057-129">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="93057-130">Справочные материалы по РКЛ</span><span class="sxs-lookup"><span data-stu-id="93057-130">Reference RCL content</span></span>

<span data-ttu-id="93057-131">На RCL могут ссылаться:</span><span class="sxs-lookup"><span data-stu-id="93057-131">The RCL can be referenced by:</span></span>

* <span data-ttu-id="93057-132">Пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="93057-132">NuGet package.</span></span> <span data-ttu-id="93057-133">См. [Создание пакетов NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) и [Создание и публикация пакета NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="93057-133">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="93057-134">*{ProjectName}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="93057-134">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="93057-135">См. [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="93057-135">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="93057-136">Переопределение представлений, частичных представлений и страниц</span><span class="sxs-lookup"><span data-stu-id="93057-136">Override views, partial views, and pages</span></span>

<span data-ttu-id="93057-137">При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в RCL приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="93057-137">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="93057-138">Например, добавьте в РКЛ элемент " *APP1/Areas/мифеатуре/Pages/* ", а также "стр. cshtml".</span><span class="sxs-lookup"><span data-stu-id="93057-138">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="93057-139">В примере загрузки переименуйте *WebApp1/Areas/MyFeature2* в *WebApp1/Areas/MyFeature*, чтобы протестировать приоритет.</span><span class="sxs-lookup"><span data-stu-id="93057-139">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="93057-140">Скопируйте частичное представление *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* в *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="93057-140">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="93057-141">Обновите разметку, чтобы указать новое расположение.</span><span class="sxs-lookup"><span data-stu-id="93057-141">Update the markup to indicate the new location.</span></span> <span data-ttu-id="93057-142">Скомпилируйте и запустите приложение, чтобы убедиться, что используется версия частичного представления из приложения.</span><span class="sxs-lookup"><span data-stu-id="93057-142">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="93057-143">Макет страниц RCL</span><span class="sxs-lookup"><span data-stu-id="93057-143">RCL Pages layout</span></span>

<span data-ttu-id="93057-144">Ссылка RCL содержимого, как если бы, если он является частью веб приложения *страниц* папки, создайте проект RCL с со следующей структурой файла:</span><span class="sxs-lookup"><span data-stu-id="93057-144">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="93057-145">*RazorUIClassLib/страниц*</span><span class="sxs-lookup"><span data-stu-id="93057-145">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="93057-146">*RazorUIClassLib/страниц/Shared*</span><span class="sxs-lookup"><span data-stu-id="93057-146">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="93057-147">Предположим, что *RazorUIClassLib/страниц/Shared* содержит два неполных файлов: *_Header.cshtml* и *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="93057-147">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="93057-148">`<partial>` Удалось добавить теги *_Layout.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="93057-148">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="93057-149">Создание РКЛ со статическими ресурсами</span><span class="sxs-lookup"><span data-stu-id="93057-149">Create an RCL with static assets</span></span>

<span data-ttu-id="93057-150">РКЛ может потребовать сопутствующих статических ресурсов, на которые может ссылаться приложение, использующее РКЛ.</span><span class="sxs-lookup"><span data-stu-id="93057-150">An RCL may require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="93057-151">ASP.NET Core позволяет создавать Рклс, которые включают статические ресурсы, доступные для использования в приложении.</span><span class="sxs-lookup"><span data-stu-id="93057-151">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="93057-152">Чтобы включить сопутствующие активы в состав РКЛ, создайте папку *wwwroot* в библиотеке классов и включите в нее все необходимые файлы.</span><span class="sxs-lookup"><span data-stu-id="93057-152">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="93057-153">При упаковке РКЛ все сопутствующие ресурсы в папке *wwwroot* автоматически включаются в пакет.</span><span class="sxs-lookup"><span data-stu-id="93057-153">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="exclude-static-assets"></a><span data-ttu-id="93057-154">Исключить статические активы</span><span class="sxs-lookup"><span data-stu-id="93057-154">Exclude static assets</span></span>

<span data-ttu-id="93057-155">Чтобы исключить статические ресурсы, добавьте нужный путь исключения в группу свойств `$(DefaultItemExcludes)` в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="93057-155">To exclude static assets, add the desired exclusion path to the `$(DefaultItemExcludes)` property group in the project file.</span></span> <span data-ttu-id="93057-156">Разделяйте записи точкой с запятой (`;`).</span><span class="sxs-lookup"><span data-stu-id="93057-156">Separate entries with a semicolon (`;`).</span></span>

<span data-ttu-id="93057-157">В следующем примере таблица " *библиотека lib. CSS* " в папке *wwwroot* не считается статическим ресурсом и не включается в опубликованные РКЛ:</span><span class="sxs-lookup"><span data-stu-id="93057-157">In the following example, the *lib.css* stylesheet in the *wwwroot* folder isn't considered a static asset and isn't included in the published RCL:</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a><span data-ttu-id="93057-158">Интеграция с typescript</span><span class="sxs-lookup"><span data-stu-id="93057-158">Typescript integration</span></span>

<span data-ttu-id="93057-159">Включение файлов TypeScript в РКЛ:</span><span class="sxs-lookup"><span data-stu-id="93057-159">To include TypeScript files in an RCL:</span></span>

1. <span data-ttu-id="93057-160">Поместите файлы TypeScript ( *. TS*) за пределы папки *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="93057-160">Place the TypeScript files (*.ts*) outside of the *wwwroot* folder.</span></span> <span data-ttu-id="93057-161">Например, поместите файлы в *клиентскую* папку.</span><span class="sxs-lookup"><span data-stu-id="93057-161">For example, place the files in a *Client* folder.</span></span>

1. <span data-ttu-id="93057-162">Настройте выходные данные сборки TypeScript для папки *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="93057-162">Configure the TypeScript build output for the *wwwroot* folder.</span></span> <span data-ttu-id="93057-163">Задайте свойство `TypescriptOutDir` в `PropertyGroup` в файле проекта:</span><span class="sxs-lookup"><span data-stu-id="93057-163">Set the `TypescriptOutDir` property inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. <span data-ttu-id="93057-164">Включите целевой объект TypeScript в качестве зависимости от целевого объекта `ResolveCurrentProjectStaticWebAssets`, добавив следующий целевой объект в `PropertyGroup` в файле проекта:</span><span class="sxs-lookup"><span data-stu-id="93057-164">Include the TypeScript target as a dependency of the `ResolveCurrentProjectStaticWebAssets` target by adding the following target inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     TypeScriptCompile;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="93057-165">Использовать содержимое из РКЛ, на которое указывает ссылка</span><span class="sxs-lookup"><span data-stu-id="93057-165">Consume content from a referenced RCL</span></span>

<span data-ttu-id="93057-166">Файлы, входящие в папку *wwwroot* РКЛ, становятся доступными для приложения с префиксом `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="93057-166">The files included in the *wwwroot* folder of the RCL are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="93057-167">Например, Библиотека с именем *Razor. class. lib* приводит к получению пути к статическому содержимому в `_content/Razor.Class.Lib/`.</span><span class="sxs-lookup"><span data-stu-id="93057-167">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span>

<span data-ttu-id="93057-168">Использующее приложение ссылается на статические ресурсы, предоставляемые библиотекой, с `<script>`, `<style>`, `<img>` и другими тегами HTML.</span><span class="sxs-lookup"><span data-stu-id="93057-168">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="93057-169">В приложении, использующем приложение, должна быть включена [Поддержка статических файлов](xref:fundamentals/static-files) в `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="93057-169">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="93057-170">При запуске приложения из выходных данных сборки (`dotnet run`) статические веб-ресурсы по умолчанию включены в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="93057-170">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="93057-171">Для поддержки ресурсов в других средах при выполнении из выходных данных сборки вызовите `UseStaticWebAssets` в построителе узлов в *Program.CS*:</span><span class="sxs-lookup"><span data-stu-id="93057-171">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStaticWebAssets();
                webBuilder.UseStartup<Startup>();
            });
}
```

<span data-ttu-id="93057-172">Вызов `UseStaticWebAssets` не требуется при запуске приложения из опубликованных выходных данных (`dotnet publish`).</span><span class="sxs-lookup"><span data-stu-id="93057-172">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="93057-173">Последовательность разработки нескольких проектов</span><span class="sxs-lookup"><span data-stu-id="93057-173">Multi-project development flow</span></span>

<span data-ttu-id="93057-174">При запуске работающего приложения:</span><span class="sxs-lookup"><span data-stu-id="93057-174">When the consuming app runs:</span></span>

* <span data-ttu-id="93057-175">Ресурсы в РКЛ остаются в исходных папках.</span><span class="sxs-lookup"><span data-stu-id="93057-175">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="93057-176">Ресурсы не перемещаются в приложение, использующее использование.</span><span class="sxs-lookup"><span data-stu-id="93057-176">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="93057-177">Любые изменения в папке *wwwroot* РКЛ отражаются в приложении после перестроения РКЛ и без перестроения приложения, использующего приложение.</span><span class="sxs-lookup"><span data-stu-id="93057-177">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="93057-178">При построении РКЛ создается манифест, описывающий статические расположения веб-ресурсов.</span><span class="sxs-lookup"><span data-stu-id="93057-178">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="93057-179">Использующее приложение считывает манифест во время выполнения для использования ресурсов из проектов и пакетов, на которые имеются ссылки.</span><span class="sxs-lookup"><span data-stu-id="93057-179">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="93057-180">Когда новый ресурс добавляется в РКЛ, РКЛ необходимо перестраивать, чтобы обновить его манифест, прежде чем использование приложения сможет получить доступ к новому ресурсу.</span><span class="sxs-lookup"><span data-stu-id="93057-180">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="93057-181">Публикация</span><span class="sxs-lookup"><span data-stu-id="93057-181">Publish</span></span>

<span data-ttu-id="93057-182">При публикации приложения сопутствующие ресурсы из всех упоминаемых проектов и пакетов копируются в папку *wwwroot* опубликованного приложения в разделе `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="93057-182">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="93057-183">Представления Razor, страницы, контроллеры, модели страниц, [компоненты Razor](xref:blazor/class-libraries), [Компоненты представления](xref:mvc/views/view-components)и модели данных могут быть встроены в БИБЛИОТЕКУ классов Razor (РКЛ).</span><span class="sxs-lookup"><span data-stu-id="93057-183">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="93057-184">RCL можно упаковать и использовать повторно.</span><span class="sxs-lookup"><span data-stu-id="93057-184">The RCL can be packaged and reused.</span></span> <span data-ttu-id="93057-185">Приложения могут включать RCL и переопределять содержащиеся в нем представления и страницы.</span><span class="sxs-lookup"><span data-stu-id="93057-185">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="93057-186">При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в RCL приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="93057-186">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="93057-187">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="93057-187">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="93057-188">Создание библиотеки классов с пользовательским интерфейсом Razor</span><span class="sxs-lookup"><span data-stu-id="93057-188">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="93057-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93057-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="93057-190">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="93057-190">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="93057-191">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="93057-191">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="93057-192">Назовите библиотеку (например, "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="93057-192">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="93057-193">Чтобы избежать конфликта имени файла с созданной библиотекой представлений, проверьте, что имя библиотеки не заканчивается на `.Views`.</span><span class="sxs-lookup"><span data-stu-id="93057-193">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="93057-194">Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="93057-194">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="93057-195">Выберите **Razor Class Library** > **ОК**.</span><span class="sxs-lookup"><span data-stu-id="93057-195">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="93057-196">РКЛ имеет следующий файл проекта:</span><span class="sxs-lookup"><span data-stu-id="93057-196">An RCL has the following project file:</span></span>

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="93057-197">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="93057-197">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="93057-198">Выполните из командной строки команду `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="93057-198">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="93057-199">Пример:</span><span class="sxs-lookup"><span data-stu-id="93057-199">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="93057-200">Дополнительные сведения см. в разделе [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="93057-200">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="93057-201">Чтобы избежать конфликта имени файла с созданной библиотекой представлений, проверьте, что имя библиотеки не заканчивается на `.Views`.</span><span class="sxs-lookup"><span data-stu-id="93057-201">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="93057-202">Добавление файлов Razor в RCL.</span><span class="sxs-lookup"><span data-stu-id="93057-202">Add Razor files to the RCL.</span></span>

<span data-ttu-id="93057-203">Шаблоны ASP.NET Core считает содержимое RCL *областей* папки.</span><span class="sxs-lookup"><span data-stu-id="93057-203">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="93057-204">Чтобы создать РКЛ, который предоставляет содержимое `~/Pages`, а не `~/Areas/Pages`, см. раздел [макет страниц РКЛ](#rcl-pages-layout) .</span><span class="sxs-lookup"><span data-stu-id="93057-204">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="93057-205">Справочные материалы по РКЛ</span><span class="sxs-lookup"><span data-stu-id="93057-205">Reference RCL content</span></span>

<span data-ttu-id="93057-206">На RCL могут ссылаться:</span><span class="sxs-lookup"><span data-stu-id="93057-206">The RCL can be referenced by:</span></span>

* <span data-ttu-id="93057-207">Пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="93057-207">NuGet package.</span></span> <span data-ttu-id="93057-208">См. [Создание пакетов NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) и [Создание и публикация пакета NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="93057-208">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="93057-209">*{ProjectName}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="93057-209">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="93057-210">См. [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="93057-210">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="93057-211">Пошаговое руководство. Создание проекта РКЛ и использование из проекта Razor Pages</span><span class="sxs-lookup"><span data-stu-id="93057-211">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="93057-212">Вы можете не создавать, а загрузить [целый проект](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) и протестировать его.</span><span class="sxs-lookup"><span data-stu-id="93057-212">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="93057-213">Образец загрузки содержит дополнительный код и ссылки, что упрощает тестирование проекта.</span><span class="sxs-lookup"><span data-stu-id="93057-213">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="93057-214">Оставьте свой комментарий о сравнении образцов загрузки с пошаговыми инструкциями в [этой проблеме GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6098).</span><span class="sxs-lookup"><span data-stu-id="93057-214">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="93057-215">Тестирование приложения загрузки</span><span class="sxs-lookup"><span data-stu-id="93057-215">Test the download app</span></span>

<span data-ttu-id="93057-216">Если вы еще не загрузили завершенное приложение и вместо этого хотите создать проект пошагового руководства, перейдите к [следующему разделу](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="93057-216">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="93057-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93057-217">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="93057-218">Откройте *SLN*-файл в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="93057-218">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="93057-219">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="93057-219">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="93057-220">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="93057-220">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="93057-221">В командной строке в каталоге *cli* создайте RCL и веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="93057-221">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="93057-222">Перейдите в каталог *WebApp1* и запустите приложение:</span><span class="sxs-lookup"><span data-stu-id="93057-222">Move to the *WebApp1* directory and run the app:</span></span>

```dotnetcli
dotnet run
```

---

<span data-ttu-id="93057-223">Следуйте инструкциям в разделе [Тестирование WebApp1](#test-webapp1)</span><span class="sxs-lookup"><span data-stu-id="93057-223">Follow the instructions in [Test WebApp1](#test-webapp1)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="93057-224">Создание РКЛ</span><span class="sxs-lookup"><span data-stu-id="93057-224">Create an RCL</span></span>

<span data-ttu-id="93057-225">В этом разделе создается РКЛ.</span><span class="sxs-lookup"><span data-stu-id="93057-225">In this section, an RCL is created.</span></span> <span data-ttu-id="93057-226">Файлы Razor будут добавлены в RCL.</span><span class="sxs-lookup"><span data-stu-id="93057-226">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="93057-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93057-227">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="93057-228">Создание проекта RCL:</span><span class="sxs-lookup"><span data-stu-id="93057-228">Create the RCL project:</span></span>

* <span data-ttu-id="93057-229">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="93057-229">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="93057-230">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="93057-230">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="93057-231">Назовите приложение **разоруикласслиб** > **ОК**.</span><span class="sxs-lookup"><span data-stu-id="93057-231">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="93057-232">Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="93057-232">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="93057-233">Выберите **Razor Class Library** > **ОК**.</span><span class="sxs-lookup"><span data-stu-id="93057-233">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="93057-234">Добавьте файл частичного представления Razor с именем *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="93057-234">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="93057-235">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="93057-235">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="93057-236">Выполните следующую команду в командной строке:</span><span class="sxs-lookup"><span data-stu-id="93057-236">From the command line, run the following:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="93057-237">Предыдущие команды:</span><span class="sxs-lookup"><span data-stu-id="93057-237">The preceding commands:</span></span>

* <span data-ttu-id="93057-238">Создает `RazorUIClassLib` РКЛ.</span><span class="sxs-lookup"><span data-stu-id="93057-238">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="93057-239">Создает страницу Razor _Message и добавляет ее в RCL.</span><span class="sxs-lookup"><span data-stu-id="93057-239">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="93057-240">Параметр `-np` создает страницу без `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="93057-240">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="93057-241">Создает [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) файл и добавляет его к RCL.</span><span class="sxs-lookup"><span data-stu-id="93057-241">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="93057-242">*_ViewStart.cshtml* файл необходим для использования макет проекта Razor Pages (который добавляется в следующем разделе).</span><span class="sxs-lookup"><span data-stu-id="93057-242">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="93057-243">Добавьте в проект Razor файлов и папок</span><span class="sxs-lookup"><span data-stu-id="93057-243">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="93057-244">Замените разметку в *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="93057-244">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="93057-245">Замените разметку в *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="93057-245">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  <span data-ttu-id="93057-246">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` требуется для использования частичного представления (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="93057-246">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="93057-247">Вместо включения директивы `@addTagHelper` можно добавить файл *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="93057-247">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="93057-248">Пример:</span><span class="sxs-lookup"><span data-stu-id="93057-248">For example:</span></span>

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  <span data-ttu-id="93057-249">Дополнительные сведения о *_ViewImports.cshtml*, см. в разделе [Импорт общих директив](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="93057-249">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="93057-250">Создайте библиотеку классов, чтобы убедиться в отсутствии ошибок компилятора:</span><span class="sxs-lookup"><span data-stu-id="93057-250">Build the class library to verify there are no compiler errors:</span></span>

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

<span data-ttu-id="93057-251">Выходные данные сборки содержат библиотеки *RazorUIClassLib.dll* и *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="93057-251">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="93057-252">В библиотеке *RazorUIClassLib.Views.dll* находится скомпилированное содержимое Razor.</span><span class="sxs-lookup"><span data-stu-id="93057-252">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="93057-253">Использование библиотеки пользовательского интерфейса Razor в проекте Razor Pages</span><span class="sxs-lookup"><span data-stu-id="93057-253">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="93057-254">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93057-254">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="93057-255">Создание веб-приложения Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="93057-255">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="93057-256">В **Обозреватель решений**щелкните правой кнопкой мыши решение > **Добавить** >  **Новый проект**.</span><span class="sxs-lookup"><span data-stu-id="93057-256">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="93057-257">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="93057-257">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="93057-258">Назовите приложение **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="93057-258">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="93057-259">Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="93057-259">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="93057-260">Выберите **веб-приложение** > **ОК**.</span><span class="sxs-lookup"><span data-stu-id="93057-260">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="93057-261">В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Назначить запускаемым проектом**.</span><span class="sxs-lookup"><span data-stu-id="93057-261">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="93057-262">В **Обозреватель решений**щелкните правой кнопкой мыши элемент **APP1** и выберите **зависимости сборки** > **зависимости проекта**.</span><span class="sxs-lookup"><span data-stu-id="93057-262">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="93057-263">Отметьте **RazorUIClassLib** как зависимость от **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="93057-263">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="93057-264">В **Обозреватель решений**щелкните правой кнопкой мыши элемент **APP1** и выберите пункт **добавить** > **ссылку**.</span><span class="sxs-lookup"><span data-stu-id="93057-264">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="93057-265">В диалоговом окне **Диспетчер ссылок** проверьте **разоруикласслиб** > **ОК**.</span><span class="sxs-lookup"><span data-stu-id="93057-265">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="93057-266">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="93057-266">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="93057-267">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="93057-267">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="93057-268">Создайте Razor Pages веб-приложение и файл решения, содержащий приложение Razor Pages и РКЛ:</span><span class="sxs-lookup"><span data-stu-id="93057-268">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="93057-269">Выполните сборку и запустите приложения:</span><span class="sxs-lookup"><span data-stu-id="93057-269">Build and run the web app:</span></span>

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a><span data-ttu-id="93057-270">Тестирование WebApp1</span><span class="sxs-lookup"><span data-stu-id="93057-270">Test WebApp1</span></span>

<span data-ttu-id="93057-271">Перейдите в `/MyFeature/Page1`, чтобы убедиться в том, что библиотека классов пользовательского интерфейса Razor используется.</span><span class="sxs-lookup"><span data-stu-id="93057-271">Browse to `/MyFeature/Page1` to verify that the Razor UI class library is in use.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="93057-272">Переопределение представлений, частичных представлений и страниц</span><span class="sxs-lookup"><span data-stu-id="93057-272">Override views, partial views, and pages</span></span>

<span data-ttu-id="93057-273">При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в RCL приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="93057-273">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="93057-274">Например, добавьте в РКЛ элемент " *APP1/Areas/мифеатуре/Pages/* ", а также "стр. cshtml".</span><span class="sxs-lookup"><span data-stu-id="93057-274">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="93057-275">В примере загрузки переименуйте *WebApp1/Areas/MyFeature2* в *WebApp1/Areas/MyFeature*, чтобы протестировать приоритет.</span><span class="sxs-lookup"><span data-stu-id="93057-275">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="93057-276">Скопируйте частичное представление *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* в *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="93057-276">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="93057-277">Обновите разметку, чтобы указать новое расположение.</span><span class="sxs-lookup"><span data-stu-id="93057-277">Update the markup to indicate the new location.</span></span> <span data-ttu-id="93057-278">Скомпилируйте и запустите приложение, чтобы убедиться, что используется версия частичного представления из приложения.</span><span class="sxs-lookup"><span data-stu-id="93057-278">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="93057-279">Макет страниц RCL</span><span class="sxs-lookup"><span data-stu-id="93057-279">RCL Pages layout</span></span>

<span data-ttu-id="93057-280">Ссылка RCL содержимого, как если бы, если он является частью веб приложения *страниц* папки, создайте проект RCL с со следующей структурой файла:</span><span class="sxs-lookup"><span data-stu-id="93057-280">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="93057-281">*RazorUIClassLib/страниц*</span><span class="sxs-lookup"><span data-stu-id="93057-281">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="93057-282">*RazorUIClassLib/страниц/Shared*</span><span class="sxs-lookup"><span data-stu-id="93057-282">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="93057-283">Предположим, что *RazorUIClassLib/страниц/Shared* содержит два неполных файлов: *_Header.cshtml* и *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="93057-283">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="93057-284">`<partial>` Удалось добавить теги *_Layout.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="93057-284">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end
