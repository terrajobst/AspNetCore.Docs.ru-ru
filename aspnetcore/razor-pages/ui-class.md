---
title: Многоразовый интерфейс Razor в библиотеках классов в ASP.NET Core
author: Rick-Anderson
description: В этой статье описывается создание многократно используемых Razor пользовательского интерфейса, использование частичных представлений в библиотеку классов, в ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 06/28/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 77c7d4a318610fcd424da0485abd41d11e3fad6a
ms.sourcegitcommit: fbc66827e319d28bebed678ea5fd42f582fe3c34
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2019
ms.locfileid: "68493567"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="c4a3a-103">Создание повторно используемого пользовательского интерфейса с помощью проекта библиотеки классов Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c4a3a-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="c4a3a-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="c4a3a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c4a3a-105">Представления Razor, страницы, контроллеры, модели страниц, [компоненты Razor](xref:blazor/class-libraries), [Компоненты представления](xref:mvc/views/view-components)и модели данных могут быть встроены в БИБЛИОТЕКУ классов Razor (РКЛ).</span><span class="sxs-lookup"><span data-stu-id="c4a3a-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="c4a3a-106">RCL можно упаковать и использовать повторно.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="c4a3a-107">Приложения могут включать RCL и переопределять содержащиеся в нем представления и страницы.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="c4a3a-108">При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в RCL приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="c4a3a-109">Эта функция требует [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="c4a3a-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="c4a3a-110">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c4a3a-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="c4a3a-111">Создание библиотеки классов с пользовательским интерфейсом Razor</span><span class="sxs-lookup"><span data-stu-id="c4a3a-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4a3a-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4a3a-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c4a3a-113">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="c4a3a-114">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="c4a3a-115">Назовите библиотеку (например, "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="c4a3a-116">Чтобы избежать конфликта имени файла с созданной библиотекой представлений, проверьте, что имя библиотеки не заканчивается на `.Views`.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="c4a3a-117">Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="c4a3a-118">Выберите **Библиотека классов Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-118">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="c4a3a-119">РКЛ имеет следующий файл проекта:</span><span class="sxs-lookup"><span data-stu-id="c4a3a-119">An RCL has the following project file:</span></span>

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c4a3a-120">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="c4a3a-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c4a3a-121">Выполните из командной строки команду `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="c4a3a-122">Пример:</span><span class="sxs-lookup"><span data-stu-id="c4a3a-122">For example:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="c4a3a-123">Дополнительные сведения см. в разделе [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="c4a3a-123">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="c4a3a-124">Чтобы избежать конфликта имени файла с созданной библиотекой представлений, проверьте, что имя библиотеки не заканчивается на `.Views`.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-124">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="c4a3a-125">Добавление файлов Razor в RCL.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-125">Add Razor files to the RCL.</span></span>

<span data-ttu-id="c4a3a-126">Шаблоны ASP.NET Core считает содержимое RCL *областей* папки.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-126">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="c4a3a-127">Чтобы создать РКЛ, который предоставляет содержимое в, а не `~/Areas/Pages`в `~/Pages` , см. раздел [РКЛ Pages Layout](#afs) .</span><span class="sxs-lookup"><span data-stu-id="c4a3a-127">See [RCL Pages layout](#afs) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="referencing-rcl-content"></a><span data-ttu-id="c4a3a-128">Ссылка на содержимое РКЛ</span><span class="sxs-lookup"><span data-stu-id="c4a3a-128">Referencing RCL content</span></span>

<span data-ttu-id="c4a3a-129">На RCL могут ссылаться:</span><span class="sxs-lookup"><span data-stu-id="c4a3a-129">The RCL can be referenced by:</span></span>

* <span data-ttu-id="c4a3a-130">Пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-130">NuGet package.</span></span> <span data-ttu-id="c4a3a-131">См. [Создание пакетов NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) и [Создание и публикация пакета NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="c4a3a-131">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="c4a3a-132">*{ProjectName}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-132">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="c4a3a-133">См. [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="c4a3a-133">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="c4a3a-134">Пошаговое руководство. Создание проекта РКЛ и использование из проекта Razor Pages</span><span class="sxs-lookup"><span data-stu-id="c4a3a-134">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="c4a3a-135">Вы можете не создавать, а загрузить [целый проект](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) и протестировать его.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-135">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="c4a3a-136">Образец загрузки содержит дополнительный код и ссылки, что упрощает тестирование проекта.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-136">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="c4a3a-137">Оставьте свой комментарий о сравнении образцов загрузки с пошаговыми инструкциями в [этой проблеме GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6098).</span><span class="sxs-lookup"><span data-stu-id="c4a3a-137">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="c4a3a-138">Тестирование приложения загрузки</span><span class="sxs-lookup"><span data-stu-id="c4a3a-138">Test the download app</span></span>

<span data-ttu-id="c4a3a-139">Если вы еще не загрузили завершенное приложение и вместо этого хотите создать проект пошагового руководства, перейдите к [следующему разделу](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="c4a3a-139">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4a3a-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4a3a-140">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c4a3a-141">Откройте *SLN*-файл в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-141">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="c4a3a-142">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-142">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c4a3a-143">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="c4a3a-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c4a3a-144">В командной строке в каталоге *cli* создайте RCL и веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-144">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```console
dotnet build
```

<span data-ttu-id="c4a3a-145">Перейдите в каталог *WebApp1* и запустите приложение:</span><span class="sxs-lookup"><span data-stu-id="c4a3a-145">Move to the *WebApp1* directory and run the app:</span></span>

```console
dotnet run
```

---

<span data-ttu-id="c4a3a-146">Следуйте инструкциям в разделе [Тестирование WebApp1](#test)</span><span class="sxs-lookup"><span data-stu-id="c4a3a-146">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="c4a3a-147">Создание РКЛ</span><span class="sxs-lookup"><span data-stu-id="c4a3a-147">Create an RCL</span></span>

<span data-ttu-id="c4a3a-148">В этом разделе создается РКЛ.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-148">In this section, an RCL is created.</span></span> <span data-ttu-id="c4a3a-149">Файлы Razor будут добавлены в RCL.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-149">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4a3a-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4a3a-150">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c4a3a-151">Создание проекта RCL:</span><span class="sxs-lookup"><span data-stu-id="c4a3a-151">Create the RCL project:</span></span>

* <span data-ttu-id="c4a3a-152">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-152">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="c4a3a-153">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-153">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="c4a3a-154">Присвойте приложению имя **RazorUIClassLib** > **ОК**.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-154">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="c4a3a-155">Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-155">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="c4a3a-156">Выберите **Библиотека классов Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-156">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="c4a3a-157">Добавьте файл частичного представления Razor с именем *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-157">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c4a3a-158">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="c4a3a-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c4a3a-159">Выполните следующую команду в командной строке:</span><span class="sxs-lookup"><span data-stu-id="c4a3a-159">From the command line, run the following:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="c4a3a-160">Предыдущие команды:</span><span class="sxs-lookup"><span data-stu-id="c4a3a-160">The preceding commands:</span></span>

* <span data-ttu-id="c4a3a-161">`RazorUIClassLib` Создает РКЛ.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-161">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="c4a3a-162">Создает страницу Razor _Message и добавляет ее в RCL.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-162">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="c4a3a-163">Параметр `-np` создает страницу без `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-163">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="c4a3a-164">Создает [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) файл и добавляет его к RCL.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-164">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="c4a3a-165">*_ViewStart.cshtml* файл необходим для использования макет проекта Razor Pages (который добавляется в следующем разделе).</span><span class="sxs-lookup"><span data-stu-id="c4a3a-165">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="c4a3a-166">Добавьте в проект Razor файлов и папок</span><span class="sxs-lookup"><span data-stu-id="c4a3a-166">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="c4a3a-167">Замените разметку в *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="c4a3a-167">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="c4a3a-168">Замените разметку в *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="c4a3a-168">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="c4a3a-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` требуется для использования частичного представления (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="c4a3a-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="c4a3a-170">Вместо включения директивы `@addTagHelper` можно добавить файл *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-170">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="c4a3a-171">Пример:</span><span class="sxs-lookup"><span data-stu-id="c4a3a-171">For example:</span></span>

```console
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="c4a3a-172">Дополнительные сведения о *_ViewImports.cshtml*, см. в разделе [Импорт общих директив](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="c4a3a-172">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="c4a3a-173">Создайте библиотеку классов, чтобы убедиться в отсутствии ошибок компилятора:</span><span class="sxs-lookup"><span data-stu-id="c4a3a-173">Build the class library to verify there are no compiler errors:</span></span>

```console
dotnet build RazorUIClassLib
```

<span data-ttu-id="c4a3a-174">Выходные данные сборки содержат библиотеки *RazorUIClassLib.dll* и *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-174">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="c4a3a-175">В библиотеке *RazorUIClassLib.Views.dll* находится скомпилированное содержимое Razor.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-175">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="c4a3a-176">Использование библиотеки пользовательского интерфейса Razor в проекте Razor Pages</span><span class="sxs-lookup"><span data-stu-id="c4a3a-176">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4a3a-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4a3a-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c4a3a-178">Создание веб-приложения Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="c4a3a-178">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="c4a3a-179">В **обозревателе решений** щелкните решение правой кнопкой мыши > **Добавить** >  **Новый проект**.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-179">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="c4a3a-180">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-180">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="c4a3a-181">Назовите приложение **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-181">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="c4a3a-182">Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-182">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="c4a3a-183">Выберите **Веб-приложение** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-183">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="c4a3a-184">В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Назначить запускаемым проектом**.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-184">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="c4a3a-185">В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Зависимости сборки** > **Зависимости проекта**.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-185">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="c4a3a-186">Отметьте **RazorUIClassLib** как зависимость от **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-186">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="c4a3a-187">В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Добавить** > **Ссылка**.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-187">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="c4a3a-188">В диалоговом окне **Диспетчер ссылок** нажмите **RazorUIClassLib** > **ОК**.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-188">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="c4a3a-189">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-189">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c4a3a-190">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="c4a3a-190">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c4a3a-191">Создайте Razor Pages веб-приложение и файл решения, содержащий приложение Razor Pages и РКЛ:</span><span class="sxs-lookup"><span data-stu-id="c4a3a-191">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="c4a3a-192">Выполните сборку и запустите приложения:</span><span class="sxs-lookup"><span data-stu-id="c4a3a-192">Build and run the web app:</span></span>

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="c4a3a-193">Тестирование WebApp1</span><span class="sxs-lookup"><span data-stu-id="c4a3a-193">Test WebApp1</span></span>

<span data-ttu-id="c4a3a-194">Убедитесь, что библиотека классов пользовательского интерфейса Razor используется:</span><span class="sxs-lookup"><span data-stu-id="c4a3a-194">Verify the Razor UI class library is in use:</span></span>

* <span data-ttu-id="c4a3a-195">Перейдите по адресу `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-195">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="c4a3a-196">Переопределение представлений, частичных представлений и страниц</span><span class="sxs-lookup"><span data-stu-id="c4a3a-196">Override views, partial views, and pages</span></span>

<span data-ttu-id="c4a3a-197">При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в RCL приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-197">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="c4a3a-198">Например, добавьте в РКЛ элемент " *APP1/Areas/мифеатуре/Pages/* ", а также "стр. cshtml".</span><span class="sxs-lookup"><span data-stu-id="c4a3a-198">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="c4a3a-199">В примере загрузки переименуйте *WebApp1/Areas/MyFeature2* в *WebApp1/Areas/MyFeature*, чтобы протестировать приоритет.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-199">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="c4a3a-200">Скопируйте частичное представление *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* в *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-200">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="c4a3a-201">Обновите разметку, чтобы указать новое расположение.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-201">Update the markup to indicate the new location.</span></span> <span data-ttu-id="c4a3a-202">Скомпилируйте и запустите приложение, чтобы убедиться, что используется версия частичного представления из приложения.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-202">Build and run the app to verify the app's version of the partial is being used.</span></span>

<a name="afs"></a>

### <a name="rcl-pages-layout"></a><span data-ttu-id="c4a3a-203">Макет страниц RCL</span><span class="sxs-lookup"><span data-stu-id="c4a3a-203">RCL Pages layout</span></span>

<span data-ttu-id="c4a3a-204">Ссылка RCL содержимого, как если бы, если он является частью веб приложения *страниц* папки, создайте проект RCL с со следующей структурой файла:</span><span class="sxs-lookup"><span data-stu-id="c4a3a-204">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="c4a3a-205">*RazorUIClassLib/страниц*</span><span class="sxs-lookup"><span data-stu-id="c4a3a-205">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="c4a3a-206">*RazorUIClassLib/страниц/Shared*</span><span class="sxs-lookup"><span data-stu-id="c4a3a-206">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="c4a3a-207">Предположим, что *RazorUIClassLib/страниц/Shared* содержит два неполных файлов: *_Header.cshtml* и *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-207">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="c4a3a-208">`<partial>` Удалось добавить теги *_Layout.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="c4a3a-208">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker range=">= aspnetcore-3.0"

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="c4a3a-209">Создание РКЛ со статическими ресурсами</span><span class="sxs-lookup"><span data-stu-id="c4a3a-209">Create an RCL with static assets</span></span>

<span data-ttu-id="c4a3a-210">РКЛ может потребовать сопутствующих статических ресурсов, на которые может ссылаться приложение, использующее РКЛ.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-210">An RCL may require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="c4a3a-211">ASP.NET Core позволяет создавать Рклс, которые включают статические ресурсы, доступные для использования в приложении.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-211">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="c4a3a-212">Чтобы включить сопутствующие активы в состав РКЛ, создайте папку *wwwroot* в библиотеке классов и включите в нее все необходимые файлы.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-212">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="c4a3a-213">При упаковке РКЛ все сопутствующие ресурсы в папке *wwwroot* автоматически включаются в пакет и становятся доступными для приложений, ссылающихся на пакет.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-213">When packing an RCL, all companion assets in the *wwwroot* folder are included in the package automatically and are made available to apps referencing the package.</span></span>

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="c4a3a-214">Использовать содержимое из РКЛ, на которое указывает ссылка</span><span class="sxs-lookup"><span data-stu-id="c4a3a-214">Consume content from a referenced RCL</span></span>

<span data-ttu-id="c4a3a-215">Файлы, содержащиеся в папке *wwwroot* РКЛ, предоставляются приложению, использующему префикс `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-215">The files included in the *wwwroot* folder of the RCL are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="c4a3a-216">Например, Библиотека с именем *Razor. class. lib* приводит к получению пути к статическому содержимому `_content/Razor.Class.Lib/`в.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-216">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span>

<span data-ttu-id="c4a3a-217">Используемое приложение ссылается на статические ресурсы `<script>` `<img>`, предоставляемые библиотекой `<style>`, с помощью,, и других HTML-тегов.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-217">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="c4a3a-218">Для использования в приложении должна быть включена [Поддержка статических файлов](xref:fundamentals/static-files) .</span><span class="sxs-lookup"><span data-stu-id="c4a3a-218">The consuming app must have [static file support](xref:fundamentals/static-files) enabled.</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="c4a3a-219">Последовательность разработки нескольких проектов</span><span class="sxs-lookup"><span data-stu-id="c4a3a-219">Multi-project development flow</span></span>

<span data-ttu-id="c4a3a-220">При запуске работающего приложения:</span><span class="sxs-lookup"><span data-stu-id="c4a3a-220">When the consuming app runs:</span></span>

* <span data-ttu-id="c4a3a-221">Ресурсы в РКЛ остаются в исходных папках.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-221">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="c4a3a-222">Ресурсы не перемещаются в приложение, использующее использование.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-222">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="c4a3a-223">Любые изменения в папке *wwwroot* РКЛ отражаются в приложении после перестроения РКЛ и без перестроения приложения, использующего приложение.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-223">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="c4a3a-224">При построении РКЛ создается манифест, описывающий статические расположения веб-ресурсов.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-224">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="c4a3a-225">Использующее приложение считывает манифест во время выполнения для использования ресурсов из проектов и пакетов, на которые имеются ссылки.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-225">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="c4a3a-226">Когда новый ресурс добавляется в РКЛ, РКЛ необходимо перестраивать, чтобы обновить его манифест, прежде чем использование приложения сможет получить доступ к новому ресурсу.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-226">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="c4a3a-227">Публикация</span><span class="sxs-lookup"><span data-stu-id="c4a3a-227">Publish</span></span>

<span data-ttu-id="c4a3a-228">При публикации приложения сопутствующие ресурсы из всех упоминаемых проектов и пакетов копируются в папку *wwwroot* опубликованного приложения в разделе `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="c4a3a-228">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end
