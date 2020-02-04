---
title: Многоразовый интерфейс Razor в библиотеках классов в ASP.NET Core
author: Rick-Anderson
description: В этой статье описывается создание многократно используемых Razor пользовательского интерфейса, использование частичных представлений в библиотеку классов, в ASP.NET Core.
ms.author: riande
ms.date: 01/25/2020
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 1a20b136203921e6a147058eb34da6dcbb7863e7
ms.sourcegitcommit: 990a4c2e623c202a27f60bdf3902f250359c13be
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/03/2020
ms.locfileid: "76971985"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="bdf7b-103">Создание повторно используемого пользовательского интерфейса с помощью проекта библиотеки классов Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bdf7b-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="bdf7b-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="bdf7b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="bdf7b-105">Представления Razor, страницы, контроллеры, модели страниц, [компоненты Razor](xref:blazor/class-libraries), [Компоненты представления](xref:mvc/views/view-components)и модели данных могут быть встроены в БИБЛИОТЕКУ классов Razor (РКЛ).</span><span class="sxs-lookup"><span data-stu-id="bdf7b-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="bdf7b-106">RCL можно упаковать и использовать повторно.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="bdf7b-107">Приложения могут включать RCL и переопределять содержащиеся в нем представления и страницы.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="bdf7b-108">При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в RCL приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="bdf7b-109">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bdf7b-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="bdf7b-110">Создание библиотеки классов с пользовательским интерфейсом Razor</span><span class="sxs-lookup"><span data-stu-id="bdf7b-110">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bdf7b-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdf7b-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bdf7b-112">В Visual Studio выберите **создать новый проект**.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-112">From Visual Studio select **Create new a new project**.</span></span>
* <span data-ttu-id="bdf7b-113">Выберите **библиотеку классов Razor** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-113">Select **Razor Class Library** > **Next**.</span></span>
* <span data-ttu-id="bdf7b-114">Присвойте библиотеке имя (например, "Разоркласслиб"), > **создать**.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-114">Name the library (for example, "RazorClassLib"), > **Create**.</span></span> <span data-ttu-id="bdf7b-115">Чтобы избежать конфликта имени файла с созданной библиотекой представлений, проверьте, что имя библиотеки не заканчивается на `.Views`.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-115">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="bdf7b-116">Выберите пункт **поддержка страниц и представлений** , если требуется поддержка представлений.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-116">Select **Support pages and views** if you need to support views.</span></span> <span data-ttu-id="bdf7b-117">По умолчанию поддерживаются только Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-117">By default, only Razor Pages are supported.</span></span> <span data-ttu-id="bdf7b-118">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-118">Select **Create**.</span></span>

<span data-ttu-id="bdf7b-119">По умолчанию для шаблона библиотеки классов Razor (RCL) используется разработка компонентов Razor.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-119">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="bdf7b-120">Параметр **страницы поддержки и представления** поддерживает страницы и представления.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-120">The **Support pages and views** option supports pages and views.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bdf7b-121">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="bdf7b-121">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="bdf7b-122">Выполните из командной строки команду `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-122">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="bdf7b-123">Например:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-123">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="bdf7b-124">По умолчанию для шаблона библиотеки классов Razor (RCL) используется разработка компонентов Razor.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-124">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="bdf7b-125">Передайте параметр `--support-pages-and-views` (`dotnet new razorclasslib --support-pages-and-views`), чтобы обеспечить поддержку страниц и представлений.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-125">Pass the `--support-pages-and-views` option (`dotnet new razorclasslib --support-pages-and-views`) to provide support for pages and views.</span></span>

<span data-ttu-id="bdf7b-126">Дополнительные сведения см. в разделе [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="bdf7b-126">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="bdf7b-127">Чтобы избежать конфликта имени файла с созданной библиотекой представлений, проверьте, что имя библиотеки не заканчивается на `.Views`.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-127">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="bdf7b-128">Добавление файлов Razor в RCL.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-128">Add Razor files to the RCL.</span></span>

<span data-ttu-id="bdf7b-129">Шаблоны ASP.NET Core считает содержимое RCL *областей* папки.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-129">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="bdf7b-130">Чтобы создать РКЛ, который предоставляет содержимое в `~/Pages`, а не `~/Areas/Pages`, см. раздел [макет страниц РКЛ](#rcl-pages-layout) .</span><span class="sxs-lookup"><span data-stu-id="bdf7b-130">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="bdf7b-131">Справочные материалы по РКЛ</span><span class="sxs-lookup"><span data-stu-id="bdf7b-131">Reference RCL content</span></span>

<span data-ttu-id="bdf7b-132">На RCL могут ссылаться:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-132">The RCL can be referenced by:</span></span>

* <span data-ttu-id="bdf7b-133">Пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-133">NuGet package.</span></span> <span data-ttu-id="bdf7b-134">См. [Создание пакетов NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) и [Создание и публикация пакета NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="bdf7b-134">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="bdf7b-135">*{ProjectName}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-135">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="bdf7b-136">См. [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="bdf7b-136">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="bdf7b-137">Переопределение представлений, частичных представлений и страниц</span><span class="sxs-lookup"><span data-stu-id="bdf7b-137">Override views, partial views, and pages</span></span>

<span data-ttu-id="bdf7b-138">При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в RCL приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-138">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="bdf7b-139">Например, добавьте в РКЛ элемент " *APP1/Areas/мифеатуре/Pages/* ", а также "стр. cshtml".</span><span class="sxs-lookup"><span data-stu-id="bdf7b-139">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="bdf7b-140">В примере загрузки переименуйте *WebApp1/Areas/MyFeature2* в *WebApp1/Areas/MyFeature*, чтобы протестировать приоритет.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-140">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="bdf7b-141">Скопируйте частичное представление *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* в *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-141">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="bdf7b-142">Обновите разметку, чтобы указать новое расположение.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-142">Update the markup to indicate the new location.</span></span> <span data-ttu-id="bdf7b-143">Скомпилируйте и запустите приложение, чтобы убедиться, что используется версия частичного представления из приложения.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-143">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="bdf7b-144">Макет страниц RCL</span><span class="sxs-lookup"><span data-stu-id="bdf7b-144">RCL Pages layout</span></span>

<span data-ttu-id="bdf7b-145">Ссылка RCL содержимого, как если бы, если он является частью веб приложения *страниц* папки, создайте проект RCL с со следующей структурой файла:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-145">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="bdf7b-146">*RazorUIClassLib/страниц*</span><span class="sxs-lookup"><span data-stu-id="bdf7b-146">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="bdf7b-147">*RazorUIClassLib/страниц/Shared*</span><span class="sxs-lookup"><span data-stu-id="bdf7b-147">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="bdf7b-148">Предположим, что *RazorUIClassLib/страниц/Shared* содержит два неполных файлов: *_Header.cshtml* и *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-148">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="bdf7b-149">`<partial>` Удалось добавить теги *_Layout.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-149">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="bdf7b-150">Создание РКЛ со статическими ресурсами</span><span class="sxs-lookup"><span data-stu-id="bdf7b-150">Create an RCL with static assets</span></span>

<span data-ttu-id="bdf7b-151">РКЛ может потребовать сопутствующих статических ресурсов, на которые может ссылаться либо РКЛ, либо приложение, использующее РКЛ.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-151">An RCL may require companion static assets that can be referenced by either the RCL or the consuming app of the RCL.</span></span> <span data-ttu-id="bdf7b-152">ASP.NET Core позволяет создавать Рклс, которые включают статические ресурсы, доступные для использования в приложении.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-152">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="bdf7b-153">Чтобы включить сопутствующие активы в состав РКЛ, создайте папку *wwwroot* в библиотеке классов и включите в нее все необходимые файлы.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-153">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="bdf7b-154">При упаковке РКЛ все сопутствующие ресурсы в папке *wwwroot* автоматически включаются в пакет.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-154">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="exclude-static-assets"></a><span data-ttu-id="bdf7b-155">Исключить статические активы</span><span class="sxs-lookup"><span data-stu-id="bdf7b-155">Exclude static assets</span></span>

<span data-ttu-id="bdf7b-156">Чтобы исключить статические ресурсы, добавьте нужный путь исключения в группу свойств `$(DefaultItemExcludes)` в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-156">To exclude static assets, add the desired exclusion path to the `$(DefaultItemExcludes)` property group in the project file.</span></span> <span data-ttu-id="bdf7b-157">Разделяйте записи точкой с запятой (`;`).</span><span class="sxs-lookup"><span data-stu-id="bdf7b-157">Separate entries with a semicolon (`;`).</span></span>

<span data-ttu-id="bdf7b-158">В следующем примере таблица " *библиотека lib. CSS* " в папке *wwwroot* не считается статическим ресурсом и не включается в опубликованные РКЛ:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-158">In the following example, the *lib.css* stylesheet in the *wwwroot* folder isn't considered a static asset and isn't included in the published RCL:</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a><span data-ttu-id="bdf7b-159">Интеграция с typescript</span><span class="sxs-lookup"><span data-stu-id="bdf7b-159">Typescript integration</span></span>

<span data-ttu-id="bdf7b-160">Включение файлов TypeScript в РКЛ:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-160">To include TypeScript files in an RCL:</span></span>

1. <span data-ttu-id="bdf7b-161">Поместите файлы TypeScript ( *. TS*) за пределы папки *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="bdf7b-161">Place the TypeScript files (*.ts*) outside of the *wwwroot* folder.</span></span> <span data-ttu-id="bdf7b-162">Например, поместите файлы в *клиентскую* папку.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-162">For example, place the files in a *Client* folder.</span></span>

1. <span data-ttu-id="bdf7b-163">Настройте выходные данные сборки TypeScript для папки *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="bdf7b-163">Configure the TypeScript build output for the *wwwroot* folder.</span></span> <span data-ttu-id="bdf7b-164">Задайте свойство `TypescriptOutDir` в `PropertyGroup` в файле проекта:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-164">Set the `TypescriptOutDir` property inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. <span data-ttu-id="bdf7b-165">Включите целевой объект TypeScript в качестве зависимости от `ResolveCurrentProjectStaticWebAssets` целевого объекта, добавив следующий целевой объект в `PropertyGroup` в файле проекта:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-165">Include the TypeScript target as a dependency of the `ResolveCurrentProjectStaticWebAssets` target by adding the following target inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     CompileTypeScript;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="bdf7b-166">Использовать содержимое из РКЛ, на которое указывает ссылка</span><span class="sxs-lookup"><span data-stu-id="bdf7b-166">Consume content from a referenced RCL</span></span>

<span data-ttu-id="bdf7b-167">Файлы, содержащиеся в папке *wwwroot* РКЛ, предоставляются либо для РКЛ, либо для приложения, использующего префикс `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-167">The files included in the *wwwroot* folder of the RCL are exposed to either the RCL or the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="bdf7b-168">Например, Библиотека с именем *Razor. class. lib* приводит к получению пути к статическому содержимому на `_content/Razor.Class.Lib/`.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-168">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span> <span data-ttu-id="bdf7b-169">При создании пакета NuGet имя сборки не совпадает с ИДЕНТИФИКАТОРом пакета, используйте идентификатор пакета для `{LIBRARY NAME}`.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-169">When producing a NuGet package and the assembly name isn't the same as the package ID, use the package ID for `{LIBRARY NAME}`.</span></span>

<span data-ttu-id="bdf7b-170">Приложение, использующее ресурсы, ссылается на статические ресурсы, предоставляемые библиотекой, с `<script>`, `<style>`, `<img>`и другими тегами HTML.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-170">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="bdf7b-171">Для использования в приложении необходимо включить [поддержку статических файлов](xref:fundamentals/static-files) в `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-171">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="bdf7b-172">При запуске приложения из выходных данных сборки (`dotnet run`) статические веб-ресурсы по умолчанию включены в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-172">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="bdf7b-173">Для поддержки ресурсов в других средах при выполнении из выходных данных сборки вызовите `UseStaticWebAssets` в построителе узлов в *Program.CS*:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-173">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

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

<span data-ttu-id="bdf7b-174">Вызов `UseStaticWebAssets` не требуется при запуске приложения из опубликованных выходных данных (`dotnet publish`).</span><span class="sxs-lookup"><span data-stu-id="bdf7b-174">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="bdf7b-175">Последовательность разработки нескольких проектов</span><span class="sxs-lookup"><span data-stu-id="bdf7b-175">Multi-project development flow</span></span>

<span data-ttu-id="bdf7b-176">При запуске работающего приложения:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-176">When the consuming app runs:</span></span>

* <span data-ttu-id="bdf7b-177">Ресурсы в РКЛ остаются в исходных папках.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-177">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="bdf7b-178">Ресурсы не перемещаются в приложение, использующее использование.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-178">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="bdf7b-179">Любые изменения в папке *wwwroot* РКЛ отражаются в приложении после перестроения РКЛ и без перестроения приложения, использующего приложение.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-179">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="bdf7b-180">При построении РКЛ создается манифест, описывающий статические расположения веб-ресурсов.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-180">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="bdf7b-181">Использующее приложение считывает манифест во время выполнения для использования ресурсов из проектов и пакетов, на которые имеются ссылки.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-181">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="bdf7b-182">Когда новый ресурс добавляется в РКЛ, РКЛ необходимо перестраивать, чтобы обновить его манифест, прежде чем использование приложения сможет получить доступ к новому ресурсу.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-182">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="bdf7b-183">Опубликовать</span><span class="sxs-lookup"><span data-stu-id="bdf7b-183">Publish</span></span>

<span data-ttu-id="bdf7b-184">При публикации приложения сопутствующие ресурсы из всех упоминаемых проектов и пакетов копируются в папку *wwwroot* опубликованного приложения в разделе `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-184">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="bdf7b-185">Представления Razor, страницы, контроллеры, модели страниц, [компоненты Razor](xref:blazor/class-libraries), [Компоненты представления](xref:mvc/views/view-components)и модели данных могут быть встроены в БИБЛИОТЕКУ классов Razor (РКЛ).</span><span class="sxs-lookup"><span data-stu-id="bdf7b-185">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="bdf7b-186">RCL можно упаковать и использовать повторно.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-186">The RCL can be packaged and reused.</span></span> <span data-ttu-id="bdf7b-187">Приложения могут включать RCL и переопределять содержащиеся в нем представления и страницы.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-187">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="bdf7b-188">При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в RCL приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-188">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="bdf7b-189">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bdf7b-189">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="bdf7b-190">Создание библиотеки классов с пользовательским интерфейсом Razor</span><span class="sxs-lookup"><span data-stu-id="bdf7b-190">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bdf7b-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdf7b-191">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bdf7b-192">В Visual Studio в меню **Файл** щелкните **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-192">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="bdf7b-193">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-193">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="bdf7b-194">Назовите библиотеку (например, "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-194">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="bdf7b-195">Чтобы избежать конфликта имени файла с созданной библиотекой представлений, проверьте, что имя библиотеки не заканчивается на `.Views`.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-195">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="bdf7b-196">Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-196">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="bdf7b-197">Выберите > **ОК** **Библиотека классов Razor** .</span><span class="sxs-lookup"><span data-stu-id="bdf7b-197">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="bdf7b-198">РКЛ имеет следующий файл проекта:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-198">An RCL has the following project file:</span></span>

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bdf7b-199">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="bdf7b-199">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="bdf7b-200">Выполните из командной строки команду `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-200">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="bdf7b-201">Например:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-201">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="bdf7b-202">Дополнительные сведения см. в разделе [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="bdf7b-202">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="bdf7b-203">Чтобы избежать конфликта имени файла с созданной библиотекой представлений, проверьте, что имя библиотеки не заканчивается на `.Views`.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-203">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="bdf7b-204">Добавление файлов Razor в RCL.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-204">Add Razor files to the RCL.</span></span>

<span data-ttu-id="bdf7b-205">Шаблоны ASP.NET Core считает содержимое RCL *областей* папки.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-205">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="bdf7b-206">Чтобы создать РКЛ, который предоставляет содержимое в `~/Pages`, а не `~/Areas/Pages`, см. раздел [макет страниц РКЛ](#rcl-pages-layout) .</span><span class="sxs-lookup"><span data-stu-id="bdf7b-206">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="bdf7b-207">Справочные материалы по РКЛ</span><span class="sxs-lookup"><span data-stu-id="bdf7b-207">Reference RCL content</span></span>

<span data-ttu-id="bdf7b-208">На RCL могут ссылаться:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-208">The RCL can be referenced by:</span></span>

* <span data-ttu-id="bdf7b-209">Пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-209">NuGet package.</span></span> <span data-ttu-id="bdf7b-210">См. [Создание пакетов NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) и [Создание и публикация пакета NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="bdf7b-210">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="bdf7b-211">*{ProjectName}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-211">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="bdf7b-212">См. [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="bdf7b-212">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="bdf7b-213">Пошаговое руководство. Создание проекта РКЛ и использование из проекта Razor Pages</span><span class="sxs-lookup"><span data-stu-id="bdf7b-213">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="bdf7b-214">Вы можете не создавать, а загрузить [целый проект](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) и протестировать его.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-214">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="bdf7b-215">Образец загрузки содержит дополнительный код и ссылки, что упрощает тестирование проекта.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-215">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="bdf7b-216">Оставьте свой комментарий о сравнении образцов загрузки с пошаговыми инструкциями в [этой проблеме GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6098).</span><span class="sxs-lookup"><span data-stu-id="bdf7b-216">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="bdf7b-217">Тестирование приложения загрузки</span><span class="sxs-lookup"><span data-stu-id="bdf7b-217">Test the download app</span></span>

<span data-ttu-id="bdf7b-218">Если вы еще не загрузили завершенное приложение и вместо этого хотите создать проект пошагового руководства, перейдите к [следующему разделу](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="bdf7b-218">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bdf7b-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdf7b-219">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bdf7b-220">Откройте *SLN*-файл в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-220">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="bdf7b-221">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-221">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bdf7b-222">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="bdf7b-222">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="bdf7b-223">В командной строке в каталоге *cli* создайте RCL и веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-223">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="bdf7b-224">Перейдите в каталог *WebApp1* и запустите приложение:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-224">Move to the *WebApp1* directory and run the app:</span></span>

```dotnetcli
dotnet run
```

---

<span data-ttu-id="bdf7b-225">Следуйте инструкциям в разделе [Тестирование WebApp1](#test-webapp1)</span><span class="sxs-lookup"><span data-stu-id="bdf7b-225">Follow the instructions in [Test WebApp1](#test-webapp1)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="bdf7b-226">Создание РКЛ</span><span class="sxs-lookup"><span data-stu-id="bdf7b-226">Create an RCL</span></span>

<span data-ttu-id="bdf7b-227">В этом разделе создается РКЛ.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-227">In this section, an RCL is created.</span></span> <span data-ttu-id="bdf7b-228">Файлы Razor будут добавлены в RCL.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-228">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bdf7b-229">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdf7b-229">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bdf7b-230">Создание проекта RCL:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-230">Create the RCL project:</span></span>

* <span data-ttu-id="bdf7b-231">В Visual Studio в меню **Файл** щелкните **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-231">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="bdf7b-232">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-232">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="bdf7b-233">Присвойте приложению имя **разоруикласслиб** > **ОК**.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-233">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="bdf7b-234">Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-234">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="bdf7b-235">Выберите > **ОК** **Библиотека классов Razor** .</span><span class="sxs-lookup"><span data-stu-id="bdf7b-235">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="bdf7b-236">Добавьте файл частичного представления Razor с именем *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-236">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bdf7b-237">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="bdf7b-237">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="bdf7b-238">Выполните следующую команду в командной строке:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-238">From the command line, run the following:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="bdf7b-239">Предыдущие команды:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-239">The preceding commands:</span></span>

* <span data-ttu-id="bdf7b-240">Создает `RazorUIClassLib` РКЛ.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-240">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="bdf7b-241">Создает страницу Razor _Message и добавляет ее в RCL.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-241">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="bdf7b-242">Параметр `-np` создает страницу без `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-242">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="bdf7b-243">Создает [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) файл и добавляет его к RCL.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-243">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="bdf7b-244">*_ViewStart.cshtml* файл необходим для использования макет проекта Razor Pages (который добавляется в следующем разделе).</span><span class="sxs-lookup"><span data-stu-id="bdf7b-244">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="bdf7b-245">Добавьте в проект Razor файлов и папок</span><span class="sxs-lookup"><span data-stu-id="bdf7b-245">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="bdf7b-246">Замените разметку в *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-246">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="bdf7b-247">Замените разметку в *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-247">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  <span data-ttu-id="bdf7b-248">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` требуется для использования частичного представления (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="bdf7b-248">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="bdf7b-249">Вместо включения директивы `@addTagHelper` можно добавить файл *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-249">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="bdf7b-250">Например:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-250">For example:</span></span>

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  <span data-ttu-id="bdf7b-251">Дополнительные сведения о *_ViewImports.cshtml*, см. в разделе [Импорт общих директив](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="bdf7b-251">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="bdf7b-252">Создайте библиотеку классов, чтобы убедиться в отсутствии ошибок компилятора:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-252">Build the class library to verify there are no compiler errors:</span></span>

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

<span data-ttu-id="bdf7b-253">Выходные данные сборки содержат библиотеки *RazorUIClassLib.dll* и *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-253">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="bdf7b-254">В библиотеке *RazorUIClassLib.Views.dll* находится скомпилированное содержимое Razor.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-254">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="bdf7b-255">Использование библиотеки пользовательского интерфейса Razor в проекте Razor Pages</span><span class="sxs-lookup"><span data-stu-id="bdf7b-255">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bdf7b-256">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdf7b-256">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bdf7b-257">Создание веб-приложения Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-257">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="bdf7b-258">В **Обозреватель решений**щелкните правой кнопкой мыши решение > **Добавить** >**Новый проект**.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-258">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="bdf7b-259">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-259">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="bdf7b-260">Назовите приложение **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-260">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="bdf7b-261">Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-261">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="bdf7b-262">Выберите **веб-приложение** > **ОК**.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-262">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="bdf7b-263">В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Назначить запускаемым проектом**.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-263">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="bdf7b-264">В **Обозреватель решений**щелкните правой кнопкой мыши элемент **APP1** и выберите **зависимости сборки** > **зависимости проекта**.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-264">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="bdf7b-265">Отметьте **RazorUIClassLib** как зависимость от **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-265">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="bdf7b-266">В **Обозреватель решений**щелкните правой кнопкой мыши элемент **APP1** и выберите пункт **добавить** > **ссылку**.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-266">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="bdf7b-267">В диалоговом окне **Диспетчер ссылок** проверьте **разоруикласслиб** > **ОК**.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-267">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="bdf7b-268">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-268">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bdf7b-269">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="bdf7b-269">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="bdf7b-270">Создайте Razor Pages веб-приложение и файл решения, содержащий приложение Razor Pages и РКЛ:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-270">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="bdf7b-271">Выполните сборку и запустите приложения:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-271">Build and run the web app:</span></span>

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a><span data-ttu-id="bdf7b-272">Тестирование WebApp1</span><span class="sxs-lookup"><span data-stu-id="bdf7b-272">Test WebApp1</span></span>

<span data-ttu-id="bdf7b-273">Перейдите к `/MyFeature/Page1`, чтобы убедиться в том, что библиотека классов пользовательского интерфейса Razor используется.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-273">Browse to `/MyFeature/Page1` to verify that the Razor UI class library is in use.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="bdf7b-274">Переопределение представлений, частичных представлений и страниц</span><span class="sxs-lookup"><span data-stu-id="bdf7b-274">Override views, partial views, and pages</span></span>

<span data-ttu-id="bdf7b-275">При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в RCL приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-275">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="bdf7b-276">Например, добавьте в РКЛ элемент " *APP1/Areas/мифеатуре/Pages/* ", а также "стр. cshtml".</span><span class="sxs-lookup"><span data-stu-id="bdf7b-276">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="bdf7b-277">В примере загрузки переименуйте *WebApp1/Areas/MyFeature2* в *WebApp1/Areas/MyFeature*, чтобы протестировать приоритет.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-277">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="bdf7b-278">Скопируйте частичное представление *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* в *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-278">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="bdf7b-279">Обновите разметку, чтобы указать новое расположение.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-279">Update the markup to indicate the new location.</span></span> <span data-ttu-id="bdf7b-280">Скомпилируйте и запустите приложение, чтобы убедиться, что используется версия частичного представления из приложения.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-280">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="bdf7b-281">Макет страниц RCL</span><span class="sxs-lookup"><span data-stu-id="bdf7b-281">RCL Pages layout</span></span>

<span data-ttu-id="bdf7b-282">Ссылка RCL содержимого, как если бы, если он является частью веб приложения *страниц* папки, создайте проект RCL с со следующей структурой файла:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-282">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="bdf7b-283">*RazorUIClassLib/страниц*</span><span class="sxs-lookup"><span data-stu-id="bdf7b-283">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="bdf7b-284">*RazorUIClassLib/страниц/Shared*</span><span class="sxs-lookup"><span data-stu-id="bdf7b-284">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="bdf7b-285">Предположим, что *RazorUIClassLib/страниц/Shared* содержит два неполных файлов: *_Header.cshtml* и *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bdf7b-285">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="bdf7b-286">`<partial>` Удалось добавить теги *_Layout.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="bdf7b-286">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end
