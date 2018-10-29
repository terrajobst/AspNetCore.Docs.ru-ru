---
title: Многоразовый интерфейс Razor в библиотеках классов в ASP.NET Core
author: Rick-Anderson
description: Описание способов создания многоразового пользовательского интерфейса Razor в библиотеке классов.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/07/2018
uid: razor-pages/ui-class
ms.openlocfilehash: 4cbff1565fe82925d9196d8cd810651b97b293da
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206526"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="d3cbe-103">Создание многократно используемых пользовательским Интерфейсом, с использованием проекта библиотеки классов Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d3cbe-103">Create reusable UI using the Razor Class Library project in ASP.NET Core</span></span>

<span data-ttu-id="d3cbe-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="d3cbe-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d3cbe-105">Представления Razor, страницы, контроллеры, модели страниц, [Просмотр компонентов](xref:mvc/views/view-components) и модели данных можно создавать в библиотеке классов Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="d3cbe-105">Razor views, pages, controllers, page models, [View components](xref:mvc/views/view-components), and data models can be built into a Razor Class Library (RCL).</span></span> <span data-ttu-id="d3cbe-106">RCL можно упаковать и использовать повторно.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="d3cbe-107">Приложения могут включать RCL и переопределять содержащиеся в нем представления и страницы.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="d3cbe-108">При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в RCL приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="d3cbe-109">Эта функция требует [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="d3cbe-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="d3cbe-110">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d3cbe-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="d3cbe-111">Создание библиотеки классов с пользовательским интерфейсом Razor</span><span class="sxs-lookup"><span data-stu-id="d3cbe-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d3cbe-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3cbe-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d3cbe-113">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="d3cbe-114">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="d3cbe-115">Назовите библиотеку (например, "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="d3cbe-116">Чтобы избежать конфликта имени файла с созданной библиотекой представлений, проверьте, что имя библиотеки не заканчивается на `.Views`.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="d3cbe-117">Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="d3cbe-118">Выберите **Библиотека классов Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-118">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="d3cbe-119">Библиотеки классов Razor имеет следующий файл проекта:</span><span class="sxs-lookup"><span data-stu-id="d3cbe-119">A Razor Class Library has the following project file:</span></span>

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d3cbe-120">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="d3cbe-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d3cbe-121">Выполните из командной строки команду `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="d3cbe-122">Пример:</span><span class="sxs-lookup"><span data-stu-id="d3cbe-122">For example:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="d3cbe-123">Дополнительные сведения см. в разделе [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="d3cbe-123">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="d3cbe-124">Чтобы избежать конфликта имени файла с созданной библиотекой представлений, проверьте, что имя библиотеки не заканчивается на `.Views`.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-124">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

------
<span data-ttu-id="d3cbe-125">Добавление файлов Razor в RCL.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-125">Add Razor files to the RCL.</span></span>

<span data-ttu-id="d3cbe-126">Шаблоны ASP.NET Core считает содержимое RCL *областей* папки.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-126">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="d3cbe-127">См. в разделе [макет страниц RCL](#afs) для создания содержимого в RCL, который предоставляет `~/Pages` вместо `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-127">See [RCL Pages layout](#afs) to create a RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="d3cbe-128">Создание ссылок на содержимое библиотеки классов Razor</span><span class="sxs-lookup"><span data-stu-id="d3cbe-128">Referencing Razor Class Library content</span></span>

<span data-ttu-id="d3cbe-129">На RCL могут ссылаться:</span><span class="sxs-lookup"><span data-stu-id="d3cbe-129">The RCL can be referenced by:</span></span>

* <span data-ttu-id="d3cbe-130">Пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-130">NuGet package.</span></span> <span data-ttu-id="d3cbe-131">См. [Создание пакетов NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) и [Создание и публикация пакета NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="d3cbe-131">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="d3cbe-132">*{ProjectName}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-132">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="d3cbe-133">См. [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="d3cbe-133">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="d3cbe-134">Пошаговое руководство. Создание проекта библиотеки классов Razor и использование в проекте Razor Pages</span><span class="sxs-lookup"><span data-stu-id="d3cbe-134">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="d3cbe-135">Вы можете не создавать, а загрузить [целый проект](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) и протестировать его.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-135">You can download the [complete project](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="d3cbe-136">Образец загрузки содержит дополнительный код и ссылки, что упрощает тестирование проекта.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-136">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="d3cbe-137">Оставьте свой комментарий о сравнении образцов загрузки с пошаговыми инструкциями в [этой проблеме GitHub](https://github.com/aspnet/Docs/issues/6098).</span><span class="sxs-lookup"><span data-stu-id="d3cbe-137">You can leave feedback in [this GitHub issue](https://github.com/aspnet/Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="d3cbe-138">Тестирование приложения загрузки</span><span class="sxs-lookup"><span data-stu-id="d3cbe-138">Test the download app</span></span>

<span data-ttu-id="d3cbe-139">Если вы еще не загрузили завершенное приложение и вместо этого хотите создать проект пошагового руководства, перейдите к [следующему разделу](#create-a-razor-class-library).</span><span class="sxs-lookup"><span data-stu-id="d3cbe-139">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d3cbe-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3cbe-140">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d3cbe-141">Откройте *SLN*-файл в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-141">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="d3cbe-142">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-142">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d3cbe-143">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="d3cbe-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d3cbe-144">В командной строке в каталоге *cli* создайте RCL и веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-144">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```console
dotnet build
```

<span data-ttu-id="d3cbe-145">Перейдите в каталог *WebApp1* и запустите приложение:</span><span class="sxs-lookup"><span data-stu-id="d3cbe-145">Move to the *WebApp1* directory and run the app:</span></span>

```console
dotnet run
```

------

<span data-ttu-id="d3cbe-146">Следуйте инструкциям в разделе [Тестирование WebApp1](#test)</span><span class="sxs-lookup"><span data-stu-id="d3cbe-146">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="d3cbe-147">Создание библиотеки классов Razor</span><span class="sxs-lookup"><span data-stu-id="d3cbe-147">Create a Razor Class Library</span></span>

<span data-ttu-id="d3cbe-148">В этом разделе создается библиотека классов Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="d3cbe-148">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="d3cbe-149">Файлы Razor будут добавлены в RCL.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-149">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d3cbe-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3cbe-150">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d3cbe-151">Создание проекта RCL:</span><span class="sxs-lookup"><span data-stu-id="d3cbe-151">Create the RCL project:</span></span>

* <span data-ttu-id="d3cbe-152">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-152">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="d3cbe-153">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-153">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="d3cbe-154">Присвойте приложению имя **RazorUIClassLib** > **ОК**.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-154">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="d3cbe-155">Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-155">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="d3cbe-156">Выберите **Библиотека классов Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-156">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="d3cbe-157">Добавьте файл частичного представления Razor с именем *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-157">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d3cbe-158">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="d3cbe-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d3cbe-159">Выполните следующую команду в командной строке:</span><span class="sxs-lookup"><span data-stu-id="d3cbe-159">From the command line, run the following:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="d3cbe-160">Предыдущие команды:</span><span class="sxs-lookup"><span data-stu-id="d3cbe-160">The preceding commands:</span></span>

* <span data-ttu-id="d3cbe-161">Создает библиотеку классов Razor (RCL) `RazorUIClassLib`.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-161">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="d3cbe-162">Создает страницу Razor _Message и добавляет ее в RCL.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-162">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="d3cbe-163">Параметр `-np` создает страницу без `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-163">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="d3cbe-164">Создает [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) файл и добавляет его к RCL.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-164">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="d3cbe-165">*_ViewStart.cshtml* файл необходим для использования макет проекта Razor Pages (который добавляется в следующем разделе).</span><span class="sxs-lookup"><span data-stu-id="d3cbe-165">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

------

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="d3cbe-166">Добавьте в проект Razor файлов и папок</span><span class="sxs-lookup"><span data-stu-id="d3cbe-166">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="d3cbe-167">Замените разметку в *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="d3cbe-167">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="d3cbe-168">Замените разметку в *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="d3cbe-168">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="d3cbe-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` требуется для использования частичного представления (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="d3cbe-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="d3cbe-170">Вместо включения директивы `@addTagHelper` можно добавить файл *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-170">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="d3cbe-171">Пример:</span><span class="sxs-lookup"><span data-stu-id="d3cbe-171">For example:</span></span>

```console
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="d3cbe-172">Дополнительные сведения о *_ViewImports.cshtml*, см. в разделе [Импорт общих директив](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="d3cbe-172">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="d3cbe-173">Создайте библиотеку классов, чтобы убедиться в отсутствии ошибок компилятора:</span><span class="sxs-lookup"><span data-stu-id="d3cbe-173">Build the class library to verify there are no compiler errors:</span></span>

```console
dotnet build RazorUIClassLib
```

<span data-ttu-id="d3cbe-174">Выходные данные сборки содержат библиотеки *RazorUIClassLib.dll* и *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-174">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="d3cbe-175">В библиотеке *RazorUIClassLib.Views.dll* находится скомпилированное содержимое Razor.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-175">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="d3cbe-176">Использование библиотеки пользовательского интерфейса Razor в проекте Razor Pages</span><span class="sxs-lookup"><span data-stu-id="d3cbe-176">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d3cbe-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3cbe-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d3cbe-178">Создание веб-приложения Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="d3cbe-178">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="d3cbe-179">В **обозревателе решений** щелкните решение правой кнопкой мыши > **Добавить** >  **Новый проект**.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-179">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="d3cbe-180">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-180">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="d3cbe-181">Назовите приложение **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-181">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="d3cbe-182">Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-182">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="d3cbe-183">Выберите **Веб-приложение** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-183">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="d3cbe-184">В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Назначить запускаемым проектом**.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-184">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="d3cbe-185">В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Зависимости сборки** > **Зависимости проекта**.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-185">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="d3cbe-186">Отметьте **RazorUIClassLib** как зависимость от **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-186">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="d3cbe-187">В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Добавить** > **Ссылка**.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-187">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="d3cbe-188">В диалоговом окне **Диспетчер ссылок** нажмите **RazorUIClassLib** > **ОК**.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-188">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="d3cbe-189">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-189">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d3cbe-190">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="d3cbe-190">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d3cbe-191">Создайте веб-приложение Razor Pages и файл решения, содержащий приложение Razor Pages и библиотеку классов Razor:</span><span class="sxs-lookup"><span data-stu-id="d3cbe-191">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="d3cbe-192">Выполните сборку и запустите приложения:</span><span class="sxs-lookup"><span data-stu-id="d3cbe-192">Build and run the web app:</span></span>

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="d3cbe-193">Тестирование WebApp1</span><span class="sxs-lookup"><span data-stu-id="d3cbe-193">Test WebApp1</span></span>

<span data-ttu-id="d3cbe-194">Убедитесь, что используется библиотека классов пользовательского интерфейса Razor.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-194">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="d3cbe-195">Перейдите по адресу `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-195">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="d3cbe-196">Переопределение представлений, частичных представлений и страниц</span><span class="sxs-lookup"><span data-stu-id="d3cbe-196">Override views, partial views, and pages</span></span>

<span data-ttu-id="d3cbe-197">При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в библиотеке классов Razor приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-197">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="d3cbe-198">Например, добавить *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* для WebApp1, и страница Page1 в WebApp1 будет иметь приоритет над Page1 в библиотеке классов Razor.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-198">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the Razor Class Library.</span></span>

<span data-ttu-id="d3cbe-199">В примере загрузки переименуйте *WebApp1/Areas/MyFeature2* в *WebApp1/Areas/MyFeature*, чтобы протестировать приоритет.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-199">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="d3cbe-200">Скопируйте частичное представление *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* в *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-200">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="d3cbe-201">Обновите разметку, чтобы указать новое расположение.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-201">Update the markup to indicate the new location.</span></span> <span data-ttu-id="d3cbe-202">Скомпилируйте и запустите приложение, чтобы убедиться, что используется версия частичного представления из приложения.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-202">Build and run the app to verify the app's version of the partial is being used.</span></span>

<a name="afs"></a>

### <a name="rcl-pages-layout"></a><span data-ttu-id="d3cbe-203">Макет страниц RCL</span><span class="sxs-lookup"><span data-stu-id="d3cbe-203">RCL Pages layout</span></span>

<span data-ttu-id="d3cbe-204">Ссылка RCL содержимого, как если бы, если он является частью веб приложения *страниц* папки, создайте проект RCL с со следующей структурой файла:</span><span class="sxs-lookup"><span data-stu-id="d3cbe-204">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="d3cbe-205">*RazorUIClassLib/страниц*</span><span class="sxs-lookup"><span data-stu-id="d3cbe-205">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="d3cbe-206">*RazorUIClassLib/страниц/Shared*</span><span class="sxs-lookup"><span data-stu-id="d3cbe-206">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="d3cbe-207">Предположим, что *RazorUIClassLib/страниц/Shared* содержит два неполных файлов: *_Header.cshtml* и *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d3cbe-207">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="d3cbe-208">`<partial>` Удалось добавить теги *_Layout.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="d3cbe-208">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>
  
```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```
