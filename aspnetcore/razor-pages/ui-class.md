---
title: Многоразовый интерфейс Razor в библиотеках классов в ASP.NET Core
author: Rick-Anderson
description: Описание способов создания многоразового пользовательского интерфейса Razor с помощью частичных представлений в библиотеке классов на платформе ASP.NET Core.
ms.author: riande
ms.date: 01/25/2020
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: f24dc62eba345a8a3d35143805b4966cb51832fa
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "78650986"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="adb69-103">Создание многоразового пользовательского интерфейса с помощью проекта библиотеки классов Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="adb69-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="adb69-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="adb69-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="adb69-105">Представления Razor, страницы, контроллеры, модели страниц, [компоненты Razor](xref:blazor/class-libraries), [Просмотр компонентов](xref:mvc/views/view-components) и модели данных можно создавать в библиотеке классов Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="adb69-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="adb69-106">RCL можно упаковать и использовать повторно.</span><span class="sxs-lookup"><span data-stu-id="adb69-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="adb69-107">Приложения могут включать RCL и переопределять содержащиеся в нем представления и страницы.</span><span class="sxs-lookup"><span data-stu-id="adb69-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="adb69-108">При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в RCL приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="adb69-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="adb69-109">[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="adb69-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="adb69-110">Создание библиотеки классов с пользовательским интерфейсом Razor</span><span class="sxs-lookup"><span data-stu-id="adb69-110">Create a class library containing Razor UI</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="adb69-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="adb69-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="adb69-112">В Visual Studio выберите **Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="adb69-112">From Visual Studio select **Create new a new project**.</span></span>
* <span data-ttu-id="adb69-113">Выберите **Библиотека классов Razor** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="adb69-113">Select **Razor Class Library** > **Next**.</span></span>
* <span data-ttu-id="adb69-114">Назовите библиотеку (например, "RazorClassLib") и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="adb69-114">Name the library (for example, "RazorClassLib"), > **Create**.</span></span> <span data-ttu-id="adb69-115">Чтобы избежать конфликта имени файла с созданной библиотекой представлений, проверьте, что имя библиотеки не заканчивается на `.Views`.</span><span class="sxs-lookup"><span data-stu-id="adb69-115">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="adb69-116">Если должны поддерживаться представления, установите флажок **Представления и страницы поддержки**.</span><span class="sxs-lookup"><span data-stu-id="adb69-116">Select **Support pages and views** if you need to support views.</span></span> <span data-ttu-id="adb69-117">По умолчанию поддерживаются только страницы Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="adb69-117">By default, only Razor Pages are supported.</span></span> <span data-ttu-id="adb69-118">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="adb69-118">Select **Create**.</span></span>

<span data-ttu-id="adb69-119">По умолчанию для шаблона библиотеки классов Razor (RCL) используется разработка компонентов Razor.</span><span class="sxs-lookup"><span data-stu-id="adb69-119">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="adb69-120">При выборе параметра **Представления и страницы поддержки** поддерживаются страницы и представления.</span><span class="sxs-lookup"><span data-stu-id="adb69-120">The **Support pages and views** option supports pages and views.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="adb69-121">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="adb69-121">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="adb69-122">Выполните из командной строки команду `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="adb69-122">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="adb69-123">Пример:</span><span class="sxs-lookup"><span data-stu-id="adb69-123">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="adb69-124">По умолчанию для шаблона библиотеки классов Razor (RCL) используется разработка компонентов Razor.</span><span class="sxs-lookup"><span data-stu-id="adb69-124">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="adb69-125">Передайте параметр `--support-pages-and-views` (`dotnet new razorclasslib --support-pages-and-views`) для включения поддержки страниц и представлений.</span><span class="sxs-lookup"><span data-stu-id="adb69-125">Pass the `--support-pages-and-views` option (`dotnet new razorclasslib --support-pages-and-views`) to provide support for pages and views.</span></span>

<span data-ttu-id="adb69-126">Дополнительные сведения см. в разделе [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="adb69-126">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="adb69-127">Чтобы избежать конфликта имени файла с созданной библиотекой представлений, проверьте, что имя библиотеки не заканчивается на `.Views`.</span><span class="sxs-lookup"><span data-stu-id="adb69-127">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="adb69-128">Добавление файлов Razor в RCL.</span><span class="sxs-lookup"><span data-stu-id="adb69-128">Add Razor files to the RCL.</span></span>

<span data-ttu-id="adb69-129">В шаблонах ASP.NET Core предполагается, что содержимое RCL находится в папке *Areas*.</span><span class="sxs-lookup"><span data-stu-id="adb69-129">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="adb69-130">Сведения о том, как создать библиотеку RCL с содержимым в папке `~/Pages`, а не `~/Areas/Pages`, см. в разделе [Макет страниц RCL](#rcl-pages-layout).</span><span class="sxs-lookup"><span data-stu-id="adb69-130">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="adb69-131">Ссылка на содержимое RCL</span><span class="sxs-lookup"><span data-stu-id="adb69-131">Reference RCL content</span></span>

<span data-ttu-id="adb69-132">На RCL могут ссылаться:</span><span class="sxs-lookup"><span data-stu-id="adb69-132">The RCL can be referenced by:</span></span>

* <span data-ttu-id="adb69-133">Пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="adb69-133">NuGet package.</span></span> <span data-ttu-id="adb69-134">См. [Создание пакетов NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) и [Создание и публикация пакета NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="adb69-134">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="adb69-135">*{ProjectName}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="adb69-135">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="adb69-136">См. [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="adb69-136">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="adb69-137">Переопределение представлений, частичных представлений и страниц</span><span class="sxs-lookup"><span data-stu-id="adb69-137">Override views, partial views, and pages</span></span>

<span data-ttu-id="adb69-138">При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в RCL приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="adb69-138">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="adb69-139">Например, если вы добавите *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* в WebApp1, Page1 в WebApp1 будет иметь приоритет над Page1 в RCL.</span><span class="sxs-lookup"><span data-stu-id="adb69-139">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="adb69-140">В примере загрузки переименуйте *WebApp1/Areas/MyFeature2* в *WebApp1/Areas/MyFeature*, чтобы протестировать приоритет.</span><span class="sxs-lookup"><span data-stu-id="adb69-140">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="adb69-141">Скопируйте частичное представление *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* в *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="adb69-141">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="adb69-142">Обновите разметку, чтобы указать новое расположение.</span><span class="sxs-lookup"><span data-stu-id="adb69-142">Update the markup to indicate the new location.</span></span> <span data-ttu-id="adb69-143">Скомпилируйте и запустите приложение, чтобы убедиться, что используется версия частичного представления из приложения.</span><span class="sxs-lookup"><span data-stu-id="adb69-143">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="adb69-144">Макет страниц RCL</span><span class="sxs-lookup"><span data-stu-id="adb69-144">RCL Pages layout</span></span>

<span data-ttu-id="adb69-145">Чтобы ссылаться на содержимое RCL так, как будто оно находится в папке *Pages* веб-приложения, создайте проект RCL со следующей структурой файлов:</span><span class="sxs-lookup"><span data-stu-id="adb69-145">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="adb69-146">*RazorUIClassLib/Pages*</span><span class="sxs-lookup"><span data-stu-id="adb69-146">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="adb69-147">*RazorUIClassLib/Pages/Shared*</span><span class="sxs-lookup"><span data-stu-id="adb69-147">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="adb69-148">Предположим, что папка *RazorUIClassLib/Pages/Shared* содержит два частичных файла: *_Header.cshtml* и *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="adb69-148">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="adb69-149">В файл *_Layout.cshtml* можно добавить теги `<partial>`:</span><span class="sxs-lookup"><span data-stu-id="adb69-149">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="adb69-150">Создание библиотеки RCL со статическими ресурсами</span><span class="sxs-lookup"><span data-stu-id="adb69-150">Create an RCL with static assets</span></span>

<span data-ttu-id="adb69-151">Для библиотеки RCL могут потребоваться сопутствующие статические ресурсы, на которые может ссылаться либо сама библиотека RCL, либо использующее ее приложение.</span><span class="sxs-lookup"><span data-stu-id="adb69-151">An RCL may require companion static assets that can be referenced by either the RCL or the consuming app of the RCL.</span></span> <span data-ttu-id="adb69-152">ASP.NET Core позволяет создавать библиотеки RCL со статическими ресурсами, которые доступны использующим библиотеки приложениям.</span><span class="sxs-lookup"><span data-stu-id="adb69-152">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="adb69-153">Чтобы включить сопутствующие ресурсы в библиотеку RCL, создайте папку *wwwroot* в библиотеке классов и добавьте в нее все необходимые файлы.</span><span class="sxs-lookup"><span data-stu-id="adb69-153">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="adb69-154">При упаковке RCL все сопутствующие ресурсы в папке *wwwroot* автоматически включаются в пакет.</span><span class="sxs-lookup"><span data-stu-id="adb69-154">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="exclude-static-assets"></a><span data-ttu-id="adb69-155">Исключение статических ресурсов</span><span class="sxs-lookup"><span data-stu-id="adb69-155">Exclude static assets</span></span>

<span data-ttu-id="adb69-156">Чтобы исключить статические ресурсы, добавьте путь исключения в группу свойств `$(DefaultItemExcludes)` в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="adb69-156">To exclude static assets, add the desired exclusion path to the `$(DefaultItemExcludes)` property group in the project file.</span></span> <span data-ttu-id="adb69-157">Записи следует разделять точкой с запятой (`;`).</span><span class="sxs-lookup"><span data-stu-id="adb69-157">Separate entries with a semicolon (`;`).</span></span>

<span data-ttu-id="adb69-158">В следующем примере таблица стилей *lib.css* в папке *wwwroot* не считается статическим ресурсом и не включается в опубликованную библиотеку RCL:</span><span class="sxs-lookup"><span data-stu-id="adb69-158">In the following example, the *lib.css* stylesheet in the *wwwroot* folder isn't considered a static asset and isn't included in the published RCL:</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a><span data-ttu-id="adb69-159">Интеграция с TypeScript</span><span class="sxs-lookup"><span data-stu-id="adb69-159">Typescript integration</span></span>

<span data-ttu-id="adb69-160">Чтобы включить файлы TypeScript в библиотеку RCL, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="adb69-160">To include TypeScript files in an RCL:</span></span>

1. <span data-ttu-id="adb69-161">Поместите файлы TypeScript (*TS*) в папку, отличную от *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="adb69-161">Place the TypeScript files (*.ts*) outside of the *wwwroot* folder.</span></span> <span data-ttu-id="adb69-162">Например, их можно поместить в папку *Client*.</span><span class="sxs-lookup"><span data-stu-id="adb69-162">For example, place the files in a *Client* folder.</span></span>

1. <span data-ttu-id="adb69-163">Укажите папку *wwwroot* в качестве выходного пути сборки TypeScript.</span><span class="sxs-lookup"><span data-stu-id="adb69-163">Configure the TypeScript build output for the *wwwroot* folder.</span></span> <span data-ttu-id="adb69-164">Задайте свойство `TypescriptOutDir` в группе `PropertyGroup` в файле проекта:</span><span class="sxs-lookup"><span data-stu-id="adb69-164">Set the `TypescriptOutDir` property inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. <span data-ttu-id="adb69-165">Включите целевой объект TypeScript в качестве зависимости целевого объекта `ResolveCurrentProjectStaticWebAssets`, добавив следующий целевой объект в группу `PropertyGroup` в файле проекта:</span><span class="sxs-lookup"><span data-stu-id="adb69-165">Include the TypeScript target as a dependency of the `ResolveCurrentProjectStaticWebAssets` target by adding the following target inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     CompileTypeScript;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="adb69-166">Использование содержимого из связанной библиотеки RCL</span><span class="sxs-lookup"><span data-stu-id="adb69-166">Consume content from a referenced RCL</span></span>

<span data-ttu-id="adb69-167">Файлы в папке *wwwroot* библиотеки RCL доступны самой библиотеке RCL или использующему ее приложению по пути `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="adb69-167">The files included in the *wwwroot* folder of the RCL are exposed to either the RCL or the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="adb69-168">Например, для библиотеки с именем *Razor.Class.Lib* путь к статическому содержимому имеет вид `_content/Razor.Class.Lib/`.</span><span class="sxs-lookup"><span data-stu-id="adb69-168">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span> <span data-ttu-id="adb69-169">Если вы создаете пакет NuGet и имя сборки не совпадает с идентификатором пакета, используйте идентификатор пакета в качестве `{LIBRARY NAME}`.</span><span class="sxs-lookup"><span data-stu-id="adb69-169">When producing a NuGet package and the assembly name isn't the same as the package ID, use the package ID for `{LIBRARY NAME}`.</span></span>

<span data-ttu-id="adb69-170">Использующее библиотеку приложение ссылается на предоставляемые ею статические ресурсы с помощью тегов HTML `<script>`, `<style>`, `<img>` и других.</span><span class="sxs-lookup"><span data-stu-id="adb69-170">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="adb69-171">Для приложения необходимо включить [поддержку статических файлов](xref:fundamentals/static-files) в `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="adb69-171">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="adb69-172">Когда приложение запускается из выходного пути сборки (`dotnet run`), в среде разработки статические веб-ресурсы включены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="adb69-172">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="adb69-173">Для поддержки ресурсов в других средах при запуске из выходного пути сборки вызовите метод `UseStaticWebAssets` построителя узла в файле *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="adb69-173">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

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

<span data-ttu-id="adb69-174">При запуске приложения из пути публикации (`dotnet publish`) вызывать `UseStaticWebAssets` не нужно.</span><span class="sxs-lookup"><span data-stu-id="adb69-174">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="adb69-175">Процесс разработки нескольких проектов</span><span class="sxs-lookup"><span data-stu-id="adb69-175">Multi-project development flow</span></span>

<span data-ttu-id="adb69-176">При запуске использующего библиотеку приложения происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="adb69-176">When the consuming app runs:</span></span>

* <span data-ttu-id="adb69-177">Ресурсы RCL остаются в исходных папках.</span><span class="sxs-lookup"><span data-stu-id="adb69-177">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="adb69-178">Они не перемещаются в приложение.</span><span class="sxs-lookup"><span data-stu-id="adb69-178">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="adb69-179">Любые изменения в папке *wwwroot* библиотеки RCL отражаются в приложении после перестроения RCL, причем приложение перестраивать не требуется.</span><span class="sxs-lookup"><span data-stu-id="adb69-179">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="adb69-180">При сборке библиотеки RCL создается манифест, в котором описывается расположение статических веб-ресурсов.</span><span class="sxs-lookup"><span data-stu-id="adb69-180">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="adb69-181">Использующее библиотеку приложение считывает манифест во время выполнения для получения ресурсов из связанных проектов и пакетов.</span><span class="sxs-lookup"><span data-stu-id="adb69-181">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="adb69-182">При добавлении нового ресурса в библиотеку RCL ее необходимо перестроить, чтобы обновить манифест. Только после этого приложение сможет получить доступ к новому ресурсу.</span><span class="sxs-lookup"><span data-stu-id="adb69-182">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="adb69-183">Публикация</span><span class="sxs-lookup"><span data-stu-id="adb69-183">Publish</span></span>

<span data-ttu-id="adb69-184">При публикации приложения сопутствующие ресурсы из всех связанных проектов и пакетов копируются в папку *wwwroot* опубликованного приложения в папке `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="adb69-184">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="adb69-185">Представления Razor, страницы, контроллеры, модели страниц, [компоненты Razor](xref:blazor/class-libraries), [Просмотр компонентов](xref:mvc/views/view-components) и модели данных можно создавать в библиотеке классов Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="adb69-185">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="adb69-186">RCL можно упаковать и использовать повторно.</span><span class="sxs-lookup"><span data-stu-id="adb69-186">The RCL can be packaged and reused.</span></span> <span data-ttu-id="adb69-187">Приложения могут включать RCL и переопределять содержащиеся в нем представления и страницы.</span><span class="sxs-lookup"><span data-stu-id="adb69-187">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="adb69-188">При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в RCL приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="adb69-188">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="adb69-189">[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="adb69-189">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="adb69-190">Создание библиотеки классов с пользовательским интерфейсом Razor</span><span class="sxs-lookup"><span data-stu-id="adb69-190">Create a class library containing Razor UI</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="adb69-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="adb69-191">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="adb69-192">В Visual Studio в меню **Файл** щелкните **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="adb69-192">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="adb69-193">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="adb69-193">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="adb69-194">Назовите библиотеку (например, "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="adb69-194">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="adb69-195">Чтобы избежать конфликта имени файла с созданной библиотекой представлений, проверьте, что имя библиотеки не заканчивается на `.Views`.</span><span class="sxs-lookup"><span data-stu-id="adb69-195">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="adb69-196">Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="adb69-196">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="adb69-197">Выберите **Библиотека классов Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="adb69-197">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="adb69-198">Библиотека RCL имеет следующий файл проекта:</span><span class="sxs-lookup"><span data-stu-id="adb69-198">An RCL has the following project file:</span></span>

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-cli"></a>[<span data-ttu-id="adb69-199">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="adb69-199">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="adb69-200">Выполните из командной строки команду `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="adb69-200">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="adb69-201">Пример:</span><span class="sxs-lookup"><span data-stu-id="adb69-201">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="adb69-202">Дополнительные сведения см. в разделе [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="adb69-202">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="adb69-203">Чтобы избежать конфликта имени файла с созданной библиотекой представлений, проверьте, что имя библиотеки не заканчивается на `.Views`.</span><span class="sxs-lookup"><span data-stu-id="adb69-203">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="adb69-204">Добавление файлов Razor в RCL.</span><span class="sxs-lookup"><span data-stu-id="adb69-204">Add Razor files to the RCL.</span></span>

<span data-ttu-id="adb69-205">В шаблонах ASP.NET Core предполагается, что содержимое RCL находится в папке *Areas*.</span><span class="sxs-lookup"><span data-stu-id="adb69-205">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="adb69-206">Сведения о том, как создать библиотеку RCL с содержимым в папке `~/Pages`, а не `~/Areas/Pages`, см. в разделе [Макет страниц RCL](#rcl-pages-layout).</span><span class="sxs-lookup"><span data-stu-id="adb69-206">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="adb69-207">Ссылка на содержимое RCL</span><span class="sxs-lookup"><span data-stu-id="adb69-207">Reference RCL content</span></span>

<span data-ttu-id="adb69-208">На RCL могут ссылаться:</span><span class="sxs-lookup"><span data-stu-id="adb69-208">The RCL can be referenced by:</span></span>

* <span data-ttu-id="adb69-209">Пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="adb69-209">NuGet package.</span></span> <span data-ttu-id="adb69-210">См. [Создание пакетов NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) и [Создание и публикация пакета NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="adb69-210">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="adb69-211">*{ProjectName}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="adb69-211">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="adb69-212">См. [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="adb69-212">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="adb69-213">Пошаговое руководство. Создание проекта RCL и его использование в проекте Razor Pages</span><span class="sxs-lookup"><span data-stu-id="adb69-213">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="adb69-214">Вы можете не создавать, а загрузить [целый проект](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) и протестировать его.</span><span class="sxs-lookup"><span data-stu-id="adb69-214">You can download the [complete project](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="adb69-215">Образец загрузки содержит дополнительный код и ссылки, что упрощает тестирование проекта.</span><span class="sxs-lookup"><span data-stu-id="adb69-215">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="adb69-216">Оставьте свой комментарий о сравнении образцов загрузки с пошаговыми инструкциями в [этой проблеме GitHub](https://github.com/dotnet/AspNetCore.Docs/issues/6098).</span><span class="sxs-lookup"><span data-stu-id="adb69-216">You can leave feedback in [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="adb69-217">Тестирование приложения загрузки</span><span class="sxs-lookup"><span data-stu-id="adb69-217">Test the download app</span></span>

<span data-ttu-id="adb69-218">Если вы еще не загрузили завершенное приложение и вместо этого хотите создать проект пошагового руководства, перейдите к [следующему разделу](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="adb69-218">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="adb69-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="adb69-219">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="adb69-220">Откройте *SLN*-файл в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="adb69-220">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="adb69-221">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="adb69-221">Run the app.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="adb69-222">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="adb69-222">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="adb69-223">В командной строке в каталоге *cli* создайте RCL и веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="adb69-223">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="adb69-224">Перейдите в каталог *WebApp1* и запустите приложение:</span><span class="sxs-lookup"><span data-stu-id="adb69-224">Move to the *WebApp1* directory and run the app:</span></span>

```dotnetcli
dotnet run
```

---

<span data-ttu-id="adb69-225">Следуйте инструкциям в разделе [Тестирование WebApp1](#test-webapp1)</span><span class="sxs-lookup"><span data-stu-id="adb69-225">Follow the instructions in [Test WebApp1](#test-webapp1)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="adb69-226">Создание библиотеки RCL</span><span class="sxs-lookup"><span data-stu-id="adb69-226">Create an RCL</span></span>

<span data-ttu-id="adb69-227">В этом разделе создается библиотека RCL.</span><span class="sxs-lookup"><span data-stu-id="adb69-227">In this section, an RCL is created.</span></span> <span data-ttu-id="adb69-228">Файлы Razor будут добавлены в RCL.</span><span class="sxs-lookup"><span data-stu-id="adb69-228">Razor files are added to the RCL.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="adb69-229">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="adb69-229">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="adb69-230">Создание проекта RCL:</span><span class="sxs-lookup"><span data-stu-id="adb69-230">Create the RCL project:</span></span>

* <span data-ttu-id="adb69-231">В Visual Studio в меню **Файл** щелкните **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="adb69-231">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="adb69-232">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="adb69-232">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="adb69-233">Назовите приложение **RazorUIClassLib** > **ОК**.</span><span class="sxs-lookup"><span data-stu-id="adb69-233">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="adb69-234">Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="adb69-234">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="adb69-235">Выберите **Библиотека классов Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="adb69-235">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="adb69-236">Добавьте файл частичного представления Razor с именем *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="adb69-236">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="adb69-237">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="adb69-237">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="adb69-238">Выполните следующую команду в командной строке:</span><span class="sxs-lookup"><span data-stu-id="adb69-238">From the command line, run the following:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="adb69-239">Предыдущие команды:</span><span class="sxs-lookup"><span data-stu-id="adb69-239">The preceding commands:</span></span>

* <span data-ttu-id="adb69-240">Создают библиотеку RCL `RazorUIClassLib`.</span><span class="sxs-lookup"><span data-stu-id="adb69-240">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="adb69-241">Создает страницу Razor _Message и добавляет ее в RCL.</span><span class="sxs-lookup"><span data-stu-id="adb69-241">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="adb69-242">Параметр `-np` создает страницу без `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="adb69-242">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="adb69-243">Создают файл [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) и добавляют его в RCL.</span><span class="sxs-lookup"><span data-stu-id="adb69-243">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="adb69-244">Файл *_ViewStart.cshtml* требуется для использования макета проекта Razor Pages (который добавляется в следующем разделе).</span><span class="sxs-lookup"><span data-stu-id="adb69-244">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="adb69-245">Добавление файлов и папок Razor в проект</span><span class="sxs-lookup"><span data-stu-id="adb69-245">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="adb69-246">Замените разметку в *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="adb69-246">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="adb69-247">Замените разметку в *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="adb69-247">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  <span data-ttu-id="adb69-248">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` требуется для использования частичного представления (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="adb69-248">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="adb69-249">Вместо включения директивы `@addTagHelper` можно добавить файл *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="adb69-249">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="adb69-250">Пример:</span><span class="sxs-lookup"><span data-stu-id="adb69-250">For example:</span></span>

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  <span data-ttu-id="adb69-251">Дополнительные сведения о файле *_ViewImports.cshtml* см. в разделе [Импорт общих директив](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="adb69-251">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="adb69-252">Создайте библиотеку классов, чтобы убедиться в отсутствии ошибок компилятора:</span><span class="sxs-lookup"><span data-stu-id="adb69-252">Build the class library to verify there are no compiler errors:</span></span>

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

<span data-ttu-id="adb69-253">Выходные данные сборки содержат библиотеки *RazorUIClassLib.dll* и *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="adb69-253">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="adb69-254">В библиотеке *RazorUIClassLib.Views.dll* находится скомпилированное содержимое Razor.</span><span class="sxs-lookup"><span data-stu-id="adb69-254">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="adb69-255">Использование библиотеки пользовательского интерфейса Razor в проекте Razor Pages</span><span class="sxs-lookup"><span data-stu-id="adb69-255">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="adb69-256">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="adb69-256">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="adb69-257">Создание веб-приложения Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="adb69-257">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="adb69-258">В **обозревателе решений** щелкните решение правой кнопкой мыши и выберите **Добавить** > **Новый проект**.</span><span class="sxs-lookup"><span data-stu-id="adb69-258">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="adb69-259">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="adb69-259">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="adb69-260">Назовите приложение **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="adb69-260">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="adb69-261">Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="adb69-261">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="adb69-262">Выберите **Веб-приложение** > **ОК**.</span><span class="sxs-lookup"><span data-stu-id="adb69-262">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="adb69-263">В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Назначить запускаемым проектом**.</span><span class="sxs-lookup"><span data-stu-id="adb69-263">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="adb69-264">В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Зависимости сборки** > **Зависимости проекта**.</span><span class="sxs-lookup"><span data-stu-id="adb69-264">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="adb69-265">Отметьте **RazorUIClassLib** как зависимость от **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="adb69-265">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="adb69-266">В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Добавить** > **Ссылка**.</span><span class="sxs-lookup"><span data-stu-id="adb69-266">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="adb69-267">В диалоговом окне **Диспетчер ссылок** нажмите **RazorUIClassLib** > **ОК**.</span><span class="sxs-lookup"><span data-stu-id="adb69-267">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="adb69-268">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="adb69-268">Run the app.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="adb69-269">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="adb69-269">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="adb69-270">Создайте веб-приложение Razor Pages и файл решения, содержащий приложение Razor Pages и библиотеку RCL:</span><span class="sxs-lookup"><span data-stu-id="adb69-270">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="adb69-271">Выполните сборку и запустите приложения:</span><span class="sxs-lookup"><span data-stu-id="adb69-271">Build and run the web app:</span></span>

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a><span data-ttu-id="adb69-272">Тестирование WebApp1</span><span class="sxs-lookup"><span data-stu-id="adb69-272">Test WebApp1</span></span>

<span data-ttu-id="adb69-273">Перейдите по адресу `/MyFeature/Page1`, чтобы убедиться в том, что используется библиотека классов пользовательского интерфейса Razor.</span><span class="sxs-lookup"><span data-stu-id="adb69-273">Browse to `/MyFeature/Page1` to verify that the Razor UI class library is in use.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="adb69-274">Переопределение представлений, частичных представлений и страниц</span><span class="sxs-lookup"><span data-stu-id="adb69-274">Override views, partial views, and pages</span></span>

<span data-ttu-id="adb69-275">При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в RCL приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="adb69-275">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="adb69-276">Например, если вы добавите *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* в WebApp1, Page1 в WebApp1 будет иметь приоритет над Page1 в RCL.</span><span class="sxs-lookup"><span data-stu-id="adb69-276">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="adb69-277">В примере загрузки переименуйте *WebApp1/Areas/MyFeature2* в *WebApp1/Areas/MyFeature*, чтобы протестировать приоритет.</span><span class="sxs-lookup"><span data-stu-id="adb69-277">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="adb69-278">Скопируйте частичное представление *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* в *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="adb69-278">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="adb69-279">Обновите разметку, чтобы указать новое расположение.</span><span class="sxs-lookup"><span data-stu-id="adb69-279">Update the markup to indicate the new location.</span></span> <span data-ttu-id="adb69-280">Скомпилируйте и запустите приложение, чтобы убедиться, что используется версия частичного представления из приложения.</span><span class="sxs-lookup"><span data-stu-id="adb69-280">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="adb69-281">Макет страниц RCL</span><span class="sxs-lookup"><span data-stu-id="adb69-281">RCL Pages layout</span></span>

<span data-ttu-id="adb69-282">Чтобы ссылаться на содержимое RCL так, как будто оно находится в папке *Pages* веб-приложения, создайте проект RCL со следующей структурой файлов:</span><span class="sxs-lookup"><span data-stu-id="adb69-282">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="adb69-283">*RazorUIClassLib/Pages*</span><span class="sxs-lookup"><span data-stu-id="adb69-283">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="adb69-284">*RazorUIClassLib/Pages/Shared*</span><span class="sxs-lookup"><span data-stu-id="adb69-284">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="adb69-285">Предположим, что папка *RazorUIClassLib/Pages/Shared* содержит два частичных файла: *_Header.cshtml* и *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="adb69-285">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="adb69-286">В файл *_Layout.cshtml* можно добавить теги `<partial>`:</span><span class="sxs-lookup"><span data-stu-id="adb69-286">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="adb69-287">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="adb69-287">Additional resources</span></span>

* <xref:blazor/class-libraries>
