---
title: Многоразовый интерфейс Razor в библиотеках классов в ASP.NET Core
author: Rick-Anderson
description: В этой статье описывается создание многократно используемых Razor пользовательского интерфейса, использование частичных представлений в библиотеку классов, в ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 06/24/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 96ef8fc055a6b92cd0808d02031d917b8446f305
ms.sourcegitcommit: 763af2cbdab0da62d1f1cfef4bcf787f251dfb5c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394744"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="7cbe6-103">Создание многократно используемых пользовательским Интерфейсом, с использованием проекта библиотеки классов Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7cbe6-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="7cbe6-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="7cbe6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7cbe6-105">Представления Razor, страницы, контроллеры, модели страниц [компоненты Razor](xref:blazor/class-libraries), [компоненты представлений](xref:mvc/views/view-components), и модели данных может быть встроен в библиотеку классов Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="7cbe6-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="7cbe6-106">RCL можно упаковать и использовать повторно.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="7cbe6-107">Приложения могут включать RCL и переопределять содержащиеся в нем представления и страницы.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="7cbe6-108">При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в RCL приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="7cbe6-109">Эта функция требует [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="7cbe6-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="7cbe6-110">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7cbe6-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="7cbe6-111">Создание библиотеки классов с пользовательским интерфейсом Razor</span><span class="sxs-lookup"><span data-stu-id="7cbe6-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7cbe6-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7cbe6-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7cbe6-113">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="7cbe6-114">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="7cbe6-115">Назовите библиотеку (например, "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="7cbe6-116">Чтобы избежать конфликта имени файла с созданной библиотекой представлений, проверьте, что имя библиотеки не заканчивается на `.Views`.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="7cbe6-117">Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="7cbe6-118">Выберите **Библиотека классов Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-118">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="7cbe6-119">RCL имеет следующий файл проекта:</span><span class="sxs-lookup"><span data-stu-id="7cbe6-119">An RCL has the following project file:</span></span>

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7cbe6-120">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="7cbe6-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="7cbe6-121">Выполните из командной строки команду `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="7cbe6-122">Пример:</span><span class="sxs-lookup"><span data-stu-id="7cbe6-122">For example:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="7cbe6-123">Дополнительные сведения см. в разделе [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="7cbe6-123">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="7cbe6-124">Чтобы избежать конфликта имени файла с созданной библиотекой представлений, проверьте, что имя библиотеки не заканчивается на `.Views`.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-124">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="7cbe6-125">Добавление файлов Razor в RCL.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-125">Add Razor files to the RCL.</span></span>

<span data-ttu-id="7cbe6-126">Шаблоны ASP.NET Core считает содержимое RCL *областей* папки.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-126">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="7cbe6-127">См. в разделе [макет страниц RCL](#afs) для создания содержимого в RCL, который предоставляет `~/Pages` вместо `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-127">See [RCL Pages layout](#afs) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="referencing-rcl-content"></a><span data-ttu-id="7cbe6-128">Ссылки на содержимое RCL</span><span class="sxs-lookup"><span data-stu-id="7cbe6-128">Referencing RCL content</span></span>

<span data-ttu-id="7cbe6-129">На RCL могут ссылаться:</span><span class="sxs-lookup"><span data-stu-id="7cbe6-129">The RCL can be referenced by:</span></span>

* <span data-ttu-id="7cbe6-130">Пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-130">NuGet package.</span></span> <span data-ttu-id="7cbe6-131">См. [Создание пакетов NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) и [Создание и публикация пакета NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="7cbe6-131">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="7cbe6-132">*{ProjectName}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-132">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="7cbe6-133">См. [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="7cbe6-133">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="7cbe6-134">Пошаговое руководство. Создание проекта RCL и использовать из проекта Razor Pages</span><span class="sxs-lookup"><span data-stu-id="7cbe6-134">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="7cbe6-135">Вы можете не создавать, а загрузить [целый проект](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) и протестировать его.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-135">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="7cbe6-136">Образец загрузки содержит дополнительный код и ссылки, что упрощает тестирование проекта.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-136">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="7cbe6-137">Оставьте свой комментарий о сравнении образцов загрузки с пошаговыми инструкциями в [этой проблеме GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6098).</span><span class="sxs-lookup"><span data-stu-id="7cbe6-137">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="7cbe6-138">Тестирование приложения загрузки</span><span class="sxs-lookup"><span data-stu-id="7cbe6-138">Test the download app</span></span>

<span data-ttu-id="7cbe6-139">Если вы еще не загрузили завершенное приложение и вместо этого хотите создать проект пошагового руководства, перейдите к [следующему разделу](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="7cbe6-139">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7cbe6-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7cbe6-140">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7cbe6-141">Откройте *SLN*-файл в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-141">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="7cbe6-142">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-142">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7cbe6-143">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="7cbe6-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="7cbe6-144">В командной строке в каталоге *cli* создайте RCL и веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-144">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```console
dotnet build
```

<span data-ttu-id="7cbe6-145">Перейдите в каталог *WebApp1* и запустите приложение:</span><span class="sxs-lookup"><span data-stu-id="7cbe6-145">Move to the *WebApp1* directory and run the app:</span></span>

```console
dotnet run
```

---

<span data-ttu-id="7cbe6-146">Следуйте инструкциям в разделе [Тестирование WebApp1](#test)</span><span class="sxs-lookup"><span data-stu-id="7cbe6-146">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="7cbe6-147">Создание RCL</span><span class="sxs-lookup"><span data-stu-id="7cbe6-147">Create an RCL</span></span>

<span data-ttu-id="7cbe6-148">В этом разделе создается RCL.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-148">In this section, an RCL is created.</span></span> <span data-ttu-id="7cbe6-149">Файлы Razor будут добавлены в RCL.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-149">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7cbe6-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7cbe6-150">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7cbe6-151">Создание проекта RCL:</span><span class="sxs-lookup"><span data-stu-id="7cbe6-151">Create the RCL project:</span></span>

* <span data-ttu-id="7cbe6-152">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-152">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="7cbe6-153">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-153">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="7cbe6-154">Присвойте приложению имя **RazorUIClassLib** > **ОК**.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-154">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="7cbe6-155">Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-155">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="7cbe6-156">Выберите **Библиотека классов Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-156">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="7cbe6-157">Добавьте файл частичного представления Razor с именем *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-157">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7cbe6-158">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="7cbe6-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="7cbe6-159">Выполните следующую команду в командной строке:</span><span class="sxs-lookup"><span data-stu-id="7cbe6-159">From the command line, run the following:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="7cbe6-160">Предыдущие команды:</span><span class="sxs-lookup"><span data-stu-id="7cbe6-160">The preceding commands:</span></span>

* <span data-ttu-id="7cbe6-161">Создает `RazorUIClassLib` RCL.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-161">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="7cbe6-162">Создает страницу Razor _Message и добавляет ее в RCL.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-162">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="7cbe6-163">Параметр `-np` создает страницу без `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-163">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="7cbe6-164">Создает [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) файл и добавляет его к RCL.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-164">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="7cbe6-165">*_ViewStart.cshtml* файл необходим для использования макет проекта Razor Pages (который добавляется в следующем разделе).</span><span class="sxs-lookup"><span data-stu-id="7cbe6-165">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="7cbe6-166">Добавьте в проект Razor файлов и папок</span><span class="sxs-lookup"><span data-stu-id="7cbe6-166">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="7cbe6-167">Замените разметку в *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="7cbe6-167">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="7cbe6-168">Замените разметку в *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="7cbe6-168">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="7cbe6-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` требуется для использования частичного представления (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="7cbe6-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="7cbe6-170">Вместо включения директивы `@addTagHelper` можно добавить файл *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-170">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="7cbe6-171">Пример:</span><span class="sxs-lookup"><span data-stu-id="7cbe6-171">For example:</span></span>

```console
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="7cbe6-172">Дополнительные сведения о *_ViewImports.cshtml*, см. в разделе [Импорт общих директив](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="7cbe6-172">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="7cbe6-173">Создайте библиотеку классов, чтобы убедиться в отсутствии ошибок компилятора:</span><span class="sxs-lookup"><span data-stu-id="7cbe6-173">Build the class library to verify there are no compiler errors:</span></span>

```console
dotnet build RazorUIClassLib
```

<span data-ttu-id="7cbe6-174">Выходные данные сборки содержат библиотеки *RazorUIClassLib.dll* и *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-174">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="7cbe6-175">В библиотеке *RazorUIClassLib.Views.dll* находится скомпилированное содержимое Razor.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-175">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="7cbe6-176">Использование библиотеки пользовательского интерфейса Razor в проекте Razor Pages</span><span class="sxs-lookup"><span data-stu-id="7cbe6-176">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7cbe6-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7cbe6-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7cbe6-178">Создание веб-приложения Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="7cbe6-178">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="7cbe6-179">В **обозревателе решений** щелкните решение правой кнопкой мыши > **Добавить** >  **Новый проект**.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-179">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="7cbe6-180">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-180">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="7cbe6-181">Назовите приложение **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-181">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="7cbe6-182">Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-182">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="7cbe6-183">Выберите **Веб-приложение** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-183">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="7cbe6-184">В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Назначить запускаемым проектом**.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-184">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="7cbe6-185">В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Зависимости сборки** > **Зависимости проекта**.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-185">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="7cbe6-186">Отметьте **RazorUIClassLib** как зависимость от **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-186">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="7cbe6-187">В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Добавить** > **Ссылка**.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-187">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="7cbe6-188">В диалоговом окне **Диспетчер ссылок** нажмите **RazorUIClassLib** > **ОК**.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-188">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="7cbe6-189">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-189">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7cbe6-190">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="7cbe6-190">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="7cbe6-191">Создание веб-приложения Razor Pages и файл решения, содержащего приложение Razor Pages и RCL:</span><span class="sxs-lookup"><span data-stu-id="7cbe6-191">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="7cbe6-192">Выполните сборку и запустите приложения:</span><span class="sxs-lookup"><span data-stu-id="7cbe6-192">Build and run the web app:</span></span>

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="7cbe6-193">Тестирование WebApp1</span><span class="sxs-lookup"><span data-stu-id="7cbe6-193">Test WebApp1</span></span>

<span data-ttu-id="7cbe6-194">Убедитесь, что библиотеки классов пользовательского интерфейса Razor используется:</span><span class="sxs-lookup"><span data-stu-id="7cbe6-194">Verify the Razor UI class library is in use:</span></span>

* <span data-ttu-id="7cbe6-195">Перейдите по адресу `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-195">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="7cbe6-196">Переопределение представлений, частичных представлений и страниц</span><span class="sxs-lookup"><span data-stu-id="7cbe6-196">Override views, partial views, and pages</span></span>

<span data-ttu-id="7cbe6-197">При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в RCL приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-197">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="7cbe6-198">Например, добавить *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* для WebApp1, и страница Page1 в WebApp1 будет иметь приоритет над Page1 в RCL.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-198">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="7cbe6-199">В примере загрузки переименуйте *WebApp1/Areas/MyFeature2* в *WebApp1/Areas/MyFeature*, чтобы протестировать приоритет.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-199">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="7cbe6-200">Скопируйте частичное представление *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* в *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-200">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="7cbe6-201">Обновите разметку, чтобы указать новое расположение.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-201">Update the markup to indicate the new location.</span></span> <span data-ttu-id="7cbe6-202">Скомпилируйте и запустите приложение, чтобы убедиться, что используется версия частичного представления из приложения.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-202">Build and run the app to verify the app's version of the partial is being used.</span></span>

<a name="afs"></a>

### <a name="rcl-pages-layout"></a><span data-ttu-id="7cbe6-203">Макет страниц RCL</span><span class="sxs-lookup"><span data-stu-id="7cbe6-203">RCL Pages layout</span></span>

<span data-ttu-id="7cbe6-204">Ссылка RCL содержимого, как если бы, если он является частью веб приложения *страниц* папки, создайте проект RCL с со следующей структурой файла:</span><span class="sxs-lookup"><span data-stu-id="7cbe6-204">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="7cbe6-205">*RazorUIClassLib/страниц*</span><span class="sxs-lookup"><span data-stu-id="7cbe6-205">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="7cbe6-206">*RazorUIClassLib/страниц/Shared*</span><span class="sxs-lookup"><span data-stu-id="7cbe6-206">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="7cbe6-207">Предположим, что *RazorUIClassLib/страниц/Shared* содержит два неполных файлов: *_Header.cshtml* и *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-207">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="7cbe6-208">`<partial>` Удалось добавить теги *_Layout.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="7cbe6-208">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="7cbe6-209">Создание RCL с помощью статических ресурсов</span><span class="sxs-lookup"><span data-stu-id="7cbe6-209">Create an RCL with static assets</span></span>

<span data-ttu-id="7cbe6-210">RCL может потребоваться дополнительное статических ресурсов, которые могут ссылаться принимающего приложения RCL.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-210">An RCL may require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="7cbe6-211">ASP.NET Core позволяет создавать RCLs, включающие статических ресурсов, которые доступны для принимающего приложения.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-211">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="7cbe6-212">Чтобы включить дополнительное ресурсов как часть RCL, создать *wwwroot* папку в библиотеке классов и включают все необходимые файлы в этой папке.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-212">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="7cbe6-213">При упаковке RCL, всех сопутствующих средств в *wwwroot* папки, автоматически включаются в пакет и становятся доступными для приложения, ссылающиеся на пакет.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-213">When packing an RCL, all companion assets in the *wwwroot* folder are included in the package automatically and are made available to apps referencing the package.</span></span>

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="7cbe6-214">Использовать содержимое из упоминаемой RCL</span><span class="sxs-lookup"><span data-stu-id="7cbe6-214">Consume content from a referenced RCL</span></span>

<span data-ttu-id="7cbe6-215">Файлы, включенные в *wwwroot* папке RCL предоставляются в приложении по префиксу `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-215">The files included in the *wwwroot* folder of the RCL are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="7cbe6-216">`{LIBRARY NAME}` имя проекта библиотеки преобразуется в нижний регистр с периодами (`.`) удалены.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-216">`{LIBRARY NAME}` is the library project name converted to lowercase with periods (`.`) removed.</span></span> <span data-ttu-id="7cbe6-217">Например, библиотеку *Razor.Class.Lib* приводит путь для статического контента на `_content/razorclasslib/`.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-217">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/razorclasslib/`.</span></span>

<span data-ttu-id="7cbe6-218">Много приложение ссылается на статические ресурсы, предоставляемые библиотекой с `<script>`, `<style>`, `<img>`и другие теги HTML.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-218">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="7cbe6-219">Много приложение должно иметь [статических файлов поддержки](xref:fundamentals/static-files) включена.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-219">The consuming app must have [static file support](xref:fundamentals/static-files) enabled.</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="7cbe6-220">Последовательностью разработки для нескольких проектов</span><span class="sxs-lookup"><span data-stu-id="7cbe6-220">Multi-project development flow</span></span>

<span data-ttu-id="7cbe6-221">Использование приложения при запуске:</span><span class="sxs-lookup"><span data-stu-id="7cbe6-221">When the consuming app runs:</span></span>

* <span data-ttu-id="7cbe6-222">Ресурсы в RCL остаются в исходных папках.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-222">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="7cbe6-223">Ресурсы не перемещаются в приложении.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-223">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="7cbe6-224">Любые изменения в RCL *wwwroot* папку отражается в приложении после RCL перестраивается и без перестроения принимающего приложения.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-224">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="7cbe6-225">При построении RCL создается манифест, описывающий расположения средств статического веб.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-225">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="7cbe6-226">Много приложение читает манифест во время выполнения, чтобы использовать ресурсы из указанных проектов и пакетов.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-226">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="7cbe6-227">При добавлении нового актива для RCL RCL необходимо перестроить, чтобы обновить его манифест, прежде чем много приложению доступ к нового средства.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-227">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="7cbe6-228">Публикация</span><span class="sxs-lookup"><span data-stu-id="7cbe6-228">Publish</span></span>

<span data-ttu-id="7cbe6-229">При публикации приложения, сопровождающий ресурсы из всех указанных проектов и пакетов копируется в *wwwroot* папки опубликованного приложения в разделе `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="7cbe6-229">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>
