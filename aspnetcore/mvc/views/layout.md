---
title: "Макет"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/layout
ms.openlocfilehash: f225e2a93edfc552961f9f16294bc0ace6eb0002
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="layout"></a><span data-ttu-id="5eb8c-102">Макет</span><span class="sxs-lookup"><span data-stu-id="5eb8c-102">Layout</span></span>

<span data-ttu-id="5eb8c-103">По [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="5eb8c-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="5eb8c-104">Представлений часто используют visual и программных элементов.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-104">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="5eb8c-105">В этой статье вы узнаете, как использовать распространенных макетов, совместное использование директивы и выполнения общих кода перед отображением представлений в приложении ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-105">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="5eb8c-106">Что такое разметка</span><span class="sxs-lookup"><span data-stu-id="5eb8c-106">What is a Layout</span></span>

<span data-ttu-id="5eb8c-107">Большинство веб-приложений имеют общий макет, который предоставляет согласованный пользователю при переходе между страницами.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-107">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="5eb8c-108">Макет обычно включает общие элементы пользовательского интерфейса, такие как заголовок приложения, навигации или элементы меню и нижний колонтитул.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-108">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Пример макета страницы](layout/_static/page-layout.png)

<span data-ttu-id="5eb8c-110">Многие другие страницы в приложении также часто используются общие структуры HTML как скрипты и таблицы стилей.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-110">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="5eb8c-111">Все эти общие элементы могут определяться в *макета* файл, который можно ссылаться представлением, используется в приложении.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-111">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="5eb8c-112">Макеты уменьшить повторяющийся код в представлениях, помогая выполните [не повторять самостоятельно (ПРОБНОГО) принцип](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="5eb8c-112">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="5eb8c-113">По соглашению макет по умолчанию для приложения ASP.NET с именем `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-113">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="5eb8c-114">Шаблон проекта Visual Studio ASP.NET Core MVC включает в этот файл макета `Views/Shared` папки:</span><span class="sxs-lookup"><span data-stu-id="5eb8c-114">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![Папка Views в обозревателе решений](layout/_static/web-project-views.png)

<span data-ttu-id="5eb8c-116">Шаблон верхнего уровня для представления определяются в приложении.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-116">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="5eb8c-117">Приложения не требуют макет и приложений можно определить несколько макетов с различными представлениями, указав различные макеты.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-117">Apps do not require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="5eb8c-118">Пример `_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="5eb8c-118">An example `_Layout.cshtml`:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="5eb8c-119">Задать макет</span><span class="sxs-lookup"><span data-stu-id="5eb8c-119">Specifying a Layout</span></span>

<span data-ttu-id="5eb8c-120">У представлений Razor `Layout` свойство.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-120">Razor views have a `Layout` property.</span></span> <span data-ttu-id="5eb8c-121">Отдельные представления задать макет, присвоив этому свойству:</span><span class="sxs-lookup"><span data-stu-id="5eb8c-121">Individual views specify a layout by setting this property:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="5eb8c-122">Макетом, указанным в можно использовать полный путь (пример: `/Views/Shared/_Layout.cshtml`) или часть имени (пример: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="5eb8c-122">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="5eb8c-123">Если часть имени, макет файла, используя его обнаружения стандартный процесс выполняет поиск представлений Razor.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-123">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="5eb8c-124">Папка связанный контроллер просматривается первой, за которым следует `Shared` папки.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-124">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="5eb8c-125">Процесс обнаружения идентичен тому, который используется для обнаружения [разделяемые представления](partial.md).</span><span class="sxs-lookup"><span data-stu-id="5eb8c-125">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="5eb8c-126">По умолчанию, необходимо вызвать метод в каждом макете `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-126">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="5eb8c-127">Везде, где вызов `RenderBody` — разместить, содержимое представления будет отображаться.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-127">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="5eb8c-128">Разделы</span><span class="sxs-lookup"><span data-stu-id="5eb8c-128">Sections</span></span>

<span data-ttu-id="5eb8c-129">Макет можно дополнительно сослаться на один или несколько *разделы*, путем вызова `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-129">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="5eb8c-130">Разделы предоставляют способ организации, где должны размещаться определенные элементы страницы.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-130">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="5eb8c-131">Каждый вызов `RenderSection` можно указать, является ли этот раздел обязательными или необязательными.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-131">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="5eb8c-132">Если набор необходимых разделов не найден, будет вызвано исключение.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-132">If a required section is not found, an exception will be thrown.</span></span> <span data-ttu-id="5eb8c-133">Содержимое для отображения в разделе с помощью указания отдельных представлений `@section` синтаксиса Razor.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-133">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="5eb8c-134">Если представление определяет раздел, должны быть помещены или завершится ошибкой.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-134">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="5eb8c-135">Пример `@section` определение в представлении:</span><span class="sxs-lookup"><span data-stu-id="5eb8c-135">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="5eb8c-136">В приведенном выше коде сценарии проверки добавляются в `scripts` раздел на представление, которое включает форму.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-136">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="5eb8c-137">Другие представления в одном приложении могут не требовать дополнительных сценариев и поэтому не требуется определение раздела скриптов.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-137">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="5eb8c-138">Разделы, определенные в представлении, доступны только в его интерпретации макета страницы.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-138">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="5eb8c-139">Они не могут ссылаться на частичными репликами, просмотр компонентов или других частей представления системы.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-139">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="5eb8c-140">Пропуск разделов</span><span class="sxs-lookup"><span data-stu-id="5eb8c-140">Ignoring sections</span></span>

<span data-ttu-id="5eb8c-141">По умолчанию текст и все разделы на странице содержимого должны все быть помещены на странице макета.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-141">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="5eb8c-142">Обработчик представлений Razor следит за этим путем отслеживания ли к просмотру в тексте и в каждом разделе.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-142">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="5eb8c-143">Чтобы указать обработчик представлений, чтобы игнорировать текст или разделов, вызовите `IgnoreBody` и `IgnoreSection` методы.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-143">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="5eb8c-144">В тексте и каждый раздел на странице Razor должна к просмотру или игнорируются.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-144">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="5eb8c-145">Импорт общих директивы</span><span class="sxs-lookup"><span data-stu-id="5eb8c-145">Importing Shared Directives</span></span>

<span data-ttu-id="5eb8c-146">Представления можно использовать директивы Razor разными способами, например, импорт пространства имен и выполнении [внедрения зависимостей](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="5eb8c-146">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="5eb8c-147">Директивы совместно используется несколько представлений, которые могут быть указаны в общий `_ViewImports.cshtml` файл.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-147">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="5eb8c-148">`_ViewImports` Файл поддерживает следующие директивы:</span><span class="sxs-lookup"><span data-stu-id="5eb8c-148">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="5eb8c-149">Файл не поддерживает другие функции Razor, например функций и определения разделов.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-149">The file does not support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="5eb8c-150">Образец `_ViewImports.cshtml` файла:</span><span class="sxs-lookup"><span data-stu-id="5eb8c-150">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="5eb8c-151">`_ViewImports.cshtml` -Файле для приложения ASP.NET Core MVC обычно помещается в `Views` папки.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-151">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="5eb8c-152">Объект `_ViewImports.cshtml` файл может быть помещен в любую папку, в которой будут применяться только в случае представления в этой папке и ее вложенных папках.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-152">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="5eb8c-153">`_ViewImports`файлы обрабатываются начиная с корневого уровня, и затем для каждой папки, ведущих к расположение собственно представлению, параметры, указанные на корневом уровне может быть переопределен на уровне папок.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-153">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="5eb8c-154">Например, если на корневом уровне `_ViewImports.cshtml` файл определяет `@model` и `@addTagHelper`и другой `_ViewImports.cshtml` указывает другой файл в папке контроллера связанные представления `@model` и добавляет еще один `@addTagHelper`, представление будет иметь доступ к обоих вспомогательных функций тегов и будет использовать последнее `@model`.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-154">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="5eb8c-155">При наличии нескольких `_ViewImports.cshtml` файлы запуска представления, в сочетании поведение директив, включенных в `ViewImports.cshtml` файлов будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="5eb8c-155">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="5eb8c-156">`@addTagHelper`, `@removeTagHelper`: выполнения все в порядке</span><span class="sxs-lookup"><span data-stu-id="5eb8c-156">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="5eb8c-157">`@tagHelperPrefix`: один, ближайший к представлению переопределяет все остальные</span><span class="sxs-lookup"><span data-stu-id="5eb8c-157">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="5eb8c-158">`@model`: один, ближайший к представлению переопределяет все остальные</span><span class="sxs-lookup"><span data-stu-id="5eb8c-158">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="5eb8c-159">`@inherits`: один, ближайший к представлению переопределяет все остальные</span><span class="sxs-lookup"><span data-stu-id="5eb8c-159">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="5eb8c-160">`@using`: все включаются; повторяющиеся значения игнорируются</span><span class="sxs-lookup"><span data-stu-id="5eb8c-160">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="5eb8c-161">`@inject`: для каждого свойства один, ближайший к представлению переопределяет все другие пользователи с таким же именем свойства</span><span class="sxs-lookup"><span data-stu-id="5eb8c-161">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="5eb8c-162">Выполнение кода до каждого представления</span><span class="sxs-lookup"><span data-stu-id="5eb8c-162">Running Code Before Each View</span></span>

<span data-ttu-id="5eb8c-163">Если имеется код, необходимо запустить перед каждого представления, это должен быть помещен в `_ViewStart.cshtml` файла.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-163">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="5eb8c-164">По соглашению `_ViewStart.cshtml` файл находится в `Views` папки.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-164">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="5eb8c-165">Операторы, перечисленные в `_ViewStart.cshtml` выполняются до каждого полного представления (не макеты и не разделяемые представления).</span><span class="sxs-lookup"><span data-stu-id="5eb8c-165">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="5eb8c-166">Как [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` являются иерархическими.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-166">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="5eb8c-167">Если `_ViewStart.cshtml` файл определяется в папке контроллера связанные представления, он будет выполняться после того, определен в корне `Views` папку (если таковые имеются).</span><span class="sxs-lookup"><span data-stu-id="5eb8c-167">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="5eb8c-168">Образец `_ViewStart.cshtml` файла:</span><span class="sxs-lookup"><span data-stu-id="5eb8c-168">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="5eb8c-169">Данный файл указывает, что все представления будут использовать `_Layout.cshtml` макет.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-169">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="5eb8c-170">Ни `_ViewStart.cshtml` , ни `_ViewImports.cshtml` обычно помещаются `/Views/Shared` папки.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-170">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="5eb8c-171">Версии этих файлов уровня приложения следует поместить непосредственно в `/Views` папки.</span><span class="sxs-lookup"><span data-stu-id="5eb8c-171">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
