---
title: Многоразовый интерфейс Razor в библиотеках классов в ASP.NET Core
author: Rick-Anderson
description: Описание способов создания многоразового пользовательского интерфейса Razor в библиотеке классов.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: advanced
uid: mvc/razor-pages/ui-class
ms.openlocfilehash: 731d37a8f4983b18ded114f05470f8a408deb7cd
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="ee5e3-103">Создание многоразового пользовательского интерфейса с помощью проекта библиотеки классов Razor в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-103">Create reusable UI using the Razor Class Library project in ASP.NET Core.</span></span>

<span data-ttu-id="ee5e3-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="ee5e3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ee5e3-105">Представления Razor, страницы, контроллеры, модели страниц и модели данных можно создавать в библиотеке классов Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="ee5e3-105">Razor views, pages, controllers, page models, and data models can be built into a Razor Class Library(RCL).</span></span> <span data-ttu-id="ee5e3-106">RCL можно упаковать и использовать повторно.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-106">The RCL can be and packaged and reused.</span></span> <span data-ttu-id="ee5e3-107">Приложения могут включать RCL и переопределять содержащиеся в нем представления и страницы.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="ee5e3-108">При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в RCL приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="ee5e3-109">Для этой функции требуется [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="ee5e3-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

[!INCLUDE[](~/includes/2.1.md)]

<span data-ttu-id="ee5e3-110">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ee5e3-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="ee5e3-111">Создание библиотеки классов с пользовательским интерфейсом Razor</span><span class="sxs-lookup"><span data-stu-id="ee5e3-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ee5e3-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ee5e3-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ee5e3-113">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="ee5e3-114">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="ee5e3-115">Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-115">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="ee5e3-116">Выберите **Библиотека классов Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-116">Select **Razor Class Library** > **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ee5e3-117">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="ee5e3-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ee5e3-118">Выполните из командной строки команду `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-118">From the commandline, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="ee5e3-119">Пример:</span><span class="sxs-lookup"><span data-stu-id="ee5e3-119">For example:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="ee5e3-120">Дополнительные сведения см. в разделе [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="ee5e3-120">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span>

------
<span data-ttu-id="ee5e3-121">Добавление файлов Razor в RCL.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-121">Add Razor files to the RCL.</span></span>

<span data-ttu-id="ee5e3-122">Мы рекомендуем размещать содержимое RCL в папке *Areas*.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-122">We recommend RCL content go in the *Areas* folder.</span></span> 


## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="ee5e3-123">Создание ссылок на содержимое библиотеки классов Razor</span><span class="sxs-lookup"><span data-stu-id="ee5e3-123">Referencing Razor Class Library content</span></span>

<span data-ttu-id="ee5e3-124">На RCL могут ссылаться:</span><span class="sxs-lookup"><span data-stu-id="ee5e3-124">The RCL can be referenced by:</span></span>

* <span data-ttu-id="ee5e3-125">Пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-125">NuGet package.</span></span> <span data-ttu-id="ee5e3-126">См. [Создание пакетов NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) и [Создание и публикация пакета NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="ee5e3-126">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="ee5e3-127">*{ProjectName}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-127">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="ee5e3-128">См. [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="ee5e3-128">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

### <a name="partial-files-access-in-the-rcl"></a><span data-ttu-id="ee5e3-129">Доступ к частичным файлам в RCL</span><span class="sxs-lookup"><span data-stu-id="ee5e3-129">Partial files access in the RCL</span></span>

<span data-ttu-id="ee5e3-130">Если содержимое находится за пределами RCL, среда выполнения ASP.NET Core не ищет частичные файлы в RCL.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-130">For content outside the RCL, the ASP.NET Core runtime does not search for partial files in the RCL.</span></span>

<span data-ttu-id="ee5e3-131">Например, в образце загрузки **невозможно** ссылаться на частичное представление *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* в *WebApp1\Pages\About.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-131">For example, in the sample download, the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view can **not** be referenced in *WebApp1\Pages\About.cshtml*.</span></span> <span data-ttu-id="ee5e3-132">Однако страницы в RCL ( *RazorUIClassLib /* **могут** иметь доступ к *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-132">However, pages in the RCL ( *RazorUIClassLib/* **can** access *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="ee5e3-133">Пошаговое руководство. Создание проекта библиотеки классов Razor и использование в проекте Razor Pages</span><span class="sxs-lookup"><span data-stu-id="ee5e3-133">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="ee5e3-134">Вы можете не создавать, а загрузить [целый проект](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) и протестировать его.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-134">You can download the [complete project](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="ee5e3-135">Образец загрузки содержит дополнительный код и ссылки, что упрощает тестирование проекта.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-135">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="ee5e3-136">Оставьте свой комментарий о сравнении образцов загрузки с пошаговыми инструкциями в [этой проблеме GitHub](https://github.com/aspnet/Docs/issues/6098).</span><span class="sxs-lookup"><span data-stu-id="ee5e3-136">You can leave feedback in [this GitHub issue](https://github.com/aspnet/Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="ee5e3-137">Тестирование приложения загрузки</span><span class="sxs-lookup"><span data-stu-id="ee5e3-137">Test the download app</span></span>

<span data-ttu-id="ee5e3-138">Если вы еще не загрузили завершенное приложение и вместо этого хотите создать проект пошагового руководства, перейдите к [следующему разделу](#create-a-razor-class-library).</span><span class="sxs-lookup"><span data-stu-id="ee5e3-138">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ee5e3-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ee5e3-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ee5e3-140">Откройте *SLN*-файл в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-140">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="ee5e3-141">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-141">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ee5e3-142">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="ee5e3-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ee5e3-143">В командной строке в каталоге *cli* создайте RCL и веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-143">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

``` CLI
dotnet build
```

<span data-ttu-id="ee5e3-144">Перейдите в каталог *WebApp1* и запустите приложение:</span><span class="sxs-lookup"><span data-stu-id="ee5e3-144">Move to the *WebApp1* directory and run the app:</span></span>

``` CLI
dotnet run
```
------

<span data-ttu-id="ee5e3-145">Следуйте инструкциям в разделе [Тестирование WebApp1](#test)</span><span class="sxs-lookup"><span data-stu-id="ee5e3-145">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="ee5e3-146">Создание библиотеки классов Razor</span><span class="sxs-lookup"><span data-stu-id="ee5e3-146">Create a Razor Class Library</span></span>

<span data-ttu-id="ee5e3-147">В этом разделе создается библиотека классов Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="ee5e3-147">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="ee5e3-148">Файлы Razor будут добавлены в RCL.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-148">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ee5e3-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ee5e3-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ee5e3-150">Создание проекта RCL:</span><span class="sxs-lookup"><span data-stu-id="ee5e3-150">Create the RCL project:</span></span>

* <span data-ttu-id="ee5e3-151">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-151">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="ee5e3-152">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-152">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="ee5e3-153">Назовите приложение **RazorUIClassLib**.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-153">Name the app **RazorUIClassLib**.</span></span>
* <span data-ttu-id="ee5e3-154">Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-154">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="ee5e3-155">Выберите **Библиотека классов Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-155">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="ee5e3-156">Создание веб-приложения Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="ee5e3-156">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="ee5e3-157">В **обозревателе решений** щелкните решение правой кнопкой мыши > **Добавить** >  **Новый проект**.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-157">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="ee5e3-158">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-158">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="ee5e3-159">Назовите приложение **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-159">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="ee5e3-160">Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-160">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="ee5e3-161">Выберите **Веб-приложение** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-161">Select **Web Application** > **OK**.</span></span>

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="ee5e3-162">Добавление файлов и папок Razor в проект.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-162">Add Razor files and folders to the project.</span></span>

* <span data-ttu-id="ee5e3-163">Добавьте файл частичного представления Razor с именем *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-163">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>
* <span data-ttu-id="ee5e3-164">Замените разметку в *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="ee5e3-164">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="ee5e3-165">Скопируйте файл *_ViewStart.cshtml* из проекта WebApp1 в *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-165">Copy the *_ViewStart.cshtml* file from the WebApp1 project to  *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span></span>

  <span data-ttu-id="ee5e3-166">Для использования макета проекта Razor Pages требуется файл [viewstart](xref:mvc/views/layout#running-code-before-each-view).</span><span class="sxs-lookup"><span data-stu-id="ee5e3-166">The [viewstart](xref:mvc/views/layout#running-code-before-each-view) file is required to use the layout of the Razor Pages project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ee5e3-167">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="ee5e3-167">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ee5e3-168">Выполните следующую команду в командной строке:</span><span class="sxs-lookup"><span data-stu-id="ee5e3-168">From the command line, run the following:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="ee5e3-169">Предыдущие команды:</span><span class="sxs-lookup"><span data-stu-id="ee5e3-169">The preceding commands:</span></span>

* <span data-ttu-id="ee5e3-170">Создает библиотеку классов Razor (RCL) `RazorUIClassLib`.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-170">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="ee5e3-171">Создает страницу Razor _Message и добавляет ее в RCL.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-171">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="ee5e3-172">Параметр `-np` создает страницу без `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-172">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="ee5e3-173">Создает файл [viewstart](xref:mvc/views/layout#running-code-before-each-view) и добавляет его в RCL.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-173">Creates a [viewstart](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="ee5e3-174">Файл viewstart требуется для использования макета проекта Razor Pages (который добавляется в следующем разделе).</span><span class="sxs-lookup"><span data-stu-id="ee5e3-174">The viewstart file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

<span data-ttu-id="ee5e3-175">Обновление Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="ee5e3-175">Update the Razor Pages:</span></span>

* <span data-ttu-id="ee5e3-176">Замените разметку в *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="ee5e3-176">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="ee5e3-177">Замените разметку в *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="ee5e3-177">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="ee5e3-178">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` требуется для использования частичного представления (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="ee5e3-178">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="ee5e3-179">Вместо включения директивы `@addTagHelper` можно добавить файл *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-179">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="ee5e3-180">Пример:</span><span class="sxs-lookup"><span data-stu-id="ee5e3-180">For example:</span></span>

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="ee5e3-181">Дополнительные сведения о файле viewimports см. в разделе [Импорт общих директив](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="ee5e3-181">For more information on viewimports, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="ee5e3-182">Создайте библиотеку классов, чтобы убедиться в отсутствии ошибок компилятора:</span><span class="sxs-lookup"><span data-stu-id="ee5e3-182">Build the class library to verify there are no compiler errors:</span></span>

``` CLI
dotnet build RazorUIClassLib
```

<span data-ttu-id="ee5e3-183">Выходные данные сборки содержат библиотеки *RazorUIClassLib.dll* и *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-183">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="ee5e3-184">В библиотеке *RazorUIClassLib.Views.dll* находится скомпилированное содержимое Razor.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-184">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

------

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="ee5e3-185">Использование библиотеки пользовательского интерфейса Razor в проекте Razor Pages</span><span class="sxs-lookup"><span data-stu-id="ee5e3-185">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ee5e3-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ee5e3-186">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ee5e3-187">В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Назначить запускаемым проектом**.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-187">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="ee5e3-188">В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Зависимости сборки** > **Зависимости проекта**.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-188">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="ee5e3-189">Отметьте **RazorUIClassLib** как зависимость от **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-189">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="ee5e3-190">В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Добавить** > **Ссылка**.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-190">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="ee5e3-191">В диалоговом окне **Диспетчер ссылок** нажмите **RazorUIClassLib** > **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-191">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="ee5e3-192">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-192">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ee5e3-193">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="ee5e3-193">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ee5e3-194">Создайте веб-приложение Razor Pages и файл решения, содержащий приложение Razor Pages и библиотеку классов Razor:</span><span class="sxs-lookup"><span data-stu-id="ee5e3-194">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

``` CLI
dotnet new razor -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="ee5e3-195">Выполните сборку и запустите приложения:</span><span class="sxs-lookup"><span data-stu-id="ee5e3-195">Build and run the web app:</span></span>

``` CLI
cd WebApp1
dotnet run
```

------

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="ee5e3-196">Тестирование WebApp1</span><span class="sxs-lookup"><span data-stu-id="ee5e3-196">Test WebApp1</span></span>

<span data-ttu-id="ee5e3-197">Убедитесь, что используется библиотека классов пользовательского интерфейса Razor.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-197">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="ee5e3-198">Перейдите по адресу `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-198">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="ee5e3-199">Переопределение представлений, частичных представлений и страниц</span><span class="sxs-lookup"><span data-stu-id="ee5e3-199">Override views, partial views, and pages</span></span>

<span data-ttu-id="ee5e3-200">При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в библиотеке классов Razor приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-200">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="ee5e3-201">Например, если вы добавите *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* в WebApp1, Page1 в WebApp1 будет иметь приоритет над Page1 в библиотеке классов Razor.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-201">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1in the Razor Class Library.</span></span>

<span data-ttu-id="ee5e3-202">В примере загрузки переименуйте *WebApp1/Areas/MyFeature2* в *WebApp1/Areas/MyFeature*, чтобы протестировать приоритет.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-202">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="ee5e3-203">Скопируйте частичное представление *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* в *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-203">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="ee5e3-204">Обновите разметку, чтобы указать новое расположение.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-204">Update the markup to indicate the new location.</span></span> <span data-ttu-id="ee5e3-205">Скомпилируйте и запустите приложение, чтобы убедиться, что используется версия частичного представления из приложения.</span><span class="sxs-lookup"><span data-stu-id="ee5e3-205">Build and run the app to verify the app's version of the partial is being used.</span></span>
