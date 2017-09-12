---
title: "Макет"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 29f12d1f-9734-48bd-bf1a-cee53a8ab700
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/layout
ms.openlocfilehash: 25aa5fc730d9076fdcf9d29cb5d9dfa75a246a1a
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="layout"></a><span data-ttu-id="2d66c-103">Макет</span><span class="sxs-lookup"><span data-stu-id="2d66c-103">Layout</span></span>

<span data-ttu-id="2d66c-104">По [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="2d66c-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="2d66c-105">Представлений часто используют visual и программных элементов.</span><span class="sxs-lookup"><span data-stu-id="2d66c-105">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="2d66c-106">В этой статье вы узнаете, как использовать распространенных макетов, совместное использование директивы и выполнения общих кода перед отображением представлений в приложении ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2d66c-106">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="2d66c-107">Что такое разметка</span><span class="sxs-lookup"><span data-stu-id="2d66c-107">What is a Layout</span></span>

<span data-ttu-id="2d66c-108">Большинство веб-приложений имеют общий макет, который предоставляет согласованный пользователю при переходе между страницами.</span><span class="sxs-lookup"><span data-stu-id="2d66c-108">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="2d66c-109">Макет обычно включает общие элементы пользовательского интерфейса, такие как заголовок приложения, навигации или элементы меню и нижний колонтитул.</span><span class="sxs-lookup"><span data-stu-id="2d66c-109">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Пример макета страницы](layout/_static/page-layout.png)

<span data-ttu-id="2d66c-111">Многие другие страницы в приложении также часто используются общие структуры HTML как скрипты и таблицы стилей.</span><span class="sxs-lookup"><span data-stu-id="2d66c-111">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="2d66c-112">Все эти общие элементы могут определяться в *макета* файл, который можно ссылаться представлением, используется в приложении.</span><span class="sxs-lookup"><span data-stu-id="2d66c-112">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="2d66c-113">Макеты уменьшить повторяющийся код в представлениях, помогая выполните [не повторять самостоятельно (ПРОБНОГО) принцип](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="2d66c-113">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="2d66c-114">По соглашению макет по умолчанию для приложения ASP.NET с именем `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="2d66c-114">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="2d66c-115">Шаблон проекта Visual Studio ASP.NET Core MVC включает в этот файл макета `Views/Shared` папки:</span><span class="sxs-lookup"><span data-stu-id="2d66c-115">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![Папка Views в обозревателе решений](layout/_static/web-project-views.png)

<span data-ttu-id="2d66c-117">Шаблон верхнего уровня для представления определяются в приложении.</span><span class="sxs-lookup"><span data-stu-id="2d66c-117">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="2d66c-118">Приложения не требуют макет и приложений можно определить несколько макетов с различными представлениями, указав различные макеты.</span><span class="sxs-lookup"><span data-stu-id="2d66c-118">Apps do not require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="2d66c-119">Пример `_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="2d66c-119">An example `_Layout.cshtml`:</span></span>

<span data-ttu-id="2d66c-120">[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]</span><span class="sxs-lookup"><span data-stu-id="2d66c-120">[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]</span></span>

## <a name="specifying-a-layout"></a><span data-ttu-id="2d66c-121">Задать макет</span><span class="sxs-lookup"><span data-stu-id="2d66c-121">Specifying a Layout</span></span>

<span data-ttu-id="2d66c-122">У представлений Razor `Layout` свойство.</span><span class="sxs-lookup"><span data-stu-id="2d66c-122">Razor views have a `Layout` property.</span></span> <span data-ttu-id="2d66c-123">Отдельные представления задать макет, присвоив этому свойству:</span><span class="sxs-lookup"><span data-stu-id="2d66c-123">Individual views specify a layout by setting this property:</span></span>

<span data-ttu-id="2d66c-124">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="2d66c-124">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]</span></span>

<span data-ttu-id="2d66c-125">Макетом, указанным в можно использовать полный путь (пример: `/Views/Shared/_Layout.cshtml`) или часть имени (пример: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="2d66c-125">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="2d66c-126">Если часть имени, макет файла, используя его обнаружения стандартный процесс выполняет поиск представлений Razor.</span><span class="sxs-lookup"><span data-stu-id="2d66c-126">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="2d66c-127">Папка связанный контроллер просматривается первой, за которым следует `Shared` папки.</span><span class="sxs-lookup"><span data-stu-id="2d66c-127">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="2d66c-128">Процесс обнаружения идентичен тому, который используется для обнаружения [разделяемые представления](partial.md).</span><span class="sxs-lookup"><span data-stu-id="2d66c-128">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="2d66c-129">По умолчанию, необходимо вызвать метод в каждом макете `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="2d66c-129">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="2d66c-130">Везде, где вызов `RenderBody` — разместить, содержимое представления будет отображаться.</span><span class="sxs-lookup"><span data-stu-id="2d66c-130">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name=layout-sections-label></a>

### <a name="sections"></a><span data-ttu-id="2d66c-131">Разделы</span><span class="sxs-lookup"><span data-stu-id="2d66c-131">Sections</span></span>

<span data-ttu-id="2d66c-132">Макет можно дополнительно сослаться на один или несколько *разделы*, путем вызова `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="2d66c-132">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="2d66c-133">Разделы предоставляют способ организации, где должны размещаться определенные элементы страницы.</span><span class="sxs-lookup"><span data-stu-id="2d66c-133">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="2d66c-134">Каждый вызов `RenderSection` можно указать, является ли этот раздел обязательными или необязательными.</span><span class="sxs-lookup"><span data-stu-id="2d66c-134">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="2d66c-135">Если набор необходимых разделов не найден, будет вызвано исключение.</span><span class="sxs-lookup"><span data-stu-id="2d66c-135">If a required section is not found, an exception will be thrown.</span></span> <span data-ttu-id="2d66c-136">Содержимое для отображения в разделе с помощью указания отдельных представлений `@section` синтаксиса Razor.</span><span class="sxs-lookup"><span data-stu-id="2d66c-136">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="2d66c-137">Если представление определяет раздел, должны быть помещены или завершится ошибкой.</span><span class="sxs-lookup"><span data-stu-id="2d66c-137">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="2d66c-138">Пример `@section` определение в представлении:</span><span class="sxs-lookup"><span data-stu-id="2d66c-138">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="2d66c-139">В приведенном выше коде сценарии проверки добавляются в `scripts` раздел на представление, которое включает форму.</span><span class="sxs-lookup"><span data-stu-id="2d66c-139">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="2d66c-140">Другие представления в одном приложении могут не требовать дополнительных сценариев и поэтому не требуется определение раздела скриптов.</span><span class="sxs-lookup"><span data-stu-id="2d66c-140">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="2d66c-141">Разделы, определенные в представлении, доступны только в его интерпретации макета страницы.</span><span class="sxs-lookup"><span data-stu-id="2d66c-141">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="2d66c-142">Они не могут ссылаться на частичными репликами, просмотр компонентов или других частей представления системы.</span><span class="sxs-lookup"><span data-stu-id="2d66c-142">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="2d66c-143">Пропуск разделов</span><span class="sxs-lookup"><span data-stu-id="2d66c-143">Ignoring sections</span></span>

<span data-ttu-id="2d66c-144">По умолчанию текст и все разделы на странице содержимого должны все быть помещены на странице макета.</span><span class="sxs-lookup"><span data-stu-id="2d66c-144">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="2d66c-145">Обработчик представлений Razor следит за этим путем отслеживания ли к просмотру в тексте и в каждом разделе.</span><span class="sxs-lookup"><span data-stu-id="2d66c-145">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="2d66c-146">Чтобы указать обработчик представлений, чтобы игнорировать текст или разделов, вызовите `IgnoreBody` и `IgnoreSection` методы.</span><span class="sxs-lookup"><span data-stu-id="2d66c-146">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="2d66c-147">В тексте и каждый раздел на странице Razor должна к просмотру или игнорируются.</span><span class="sxs-lookup"><span data-stu-id="2d66c-147">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name=viewimports></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="2d66c-148">Импорт общих директивы</span><span class="sxs-lookup"><span data-stu-id="2d66c-148">Importing Shared Directives</span></span>

<span data-ttu-id="2d66c-149">Представления можно использовать директивы Razor разными способами, например, импорт пространства имен и выполнении [внедрения зависимостей](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="2d66c-149">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="2d66c-150">Директивы совместно используется несколько представлений, которые могут быть указаны в общий `_ViewImports.cshtml` файл.</span><span class="sxs-lookup"><span data-stu-id="2d66c-150">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="2d66c-151">`_ViewImports` Файл поддерживает следующие директивы:</span><span class="sxs-lookup"><span data-stu-id="2d66c-151">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="2d66c-152">Файл не поддерживает другие функции Razor, например функций и определения разделов.</span><span class="sxs-lookup"><span data-stu-id="2d66c-152">The file does not support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="2d66c-153">Образец `_ViewImports.cshtml` файла:</span><span class="sxs-lookup"><span data-stu-id="2d66c-153">A sample `_ViewImports.cshtml` file:</span></span>

<span data-ttu-id="2d66c-154">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="2d66c-154">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]</span></span>

<span data-ttu-id="2d66c-155">`_ViewImports.cshtml` -Файле для приложения ASP.NET Core MVC обычно помещается в `Views` папки.</span><span class="sxs-lookup"><span data-stu-id="2d66c-155">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="2d66c-156">Объект `_ViewImports.cshtml` файл может быть помещен в любую папку, в которой будут применяться только в случае представления в этой папке и ее вложенных папках.</span><span class="sxs-lookup"><span data-stu-id="2d66c-156">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="2d66c-157">`_ViewImports`файлы обрабатываются начиная с корневого уровня, и затем для каждой папки, ведущих к расположение собственно представлению, параметры, указанные на корневом уровне может быть переопределен на уровне папок.</span><span class="sxs-lookup"><span data-stu-id="2d66c-157">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="2d66c-158">Например, если на корневом уровне `_ViewImports.cshtml` файл определяет `@model` и `@addTagHelper`и другой `_ViewImports.cshtml` указывает другой файл в папке контроллера связанные представления `@model` и добавляет еще один `@addTagHelper`, представление будет иметь доступ к обоих вспомогательных функций тегов и будет использовать последнее `@model`.</span><span class="sxs-lookup"><span data-stu-id="2d66c-158">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="2d66c-159">При наличии нескольких `_ViewImports.cshtml` файлы запуска представления, в сочетании поведение директив, включенных в `ViewImports.cshtml` файлов будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2d66c-159">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="2d66c-160">`@addTagHelper`, `@removeTagHelper`: выполнения все в порядке</span><span class="sxs-lookup"><span data-stu-id="2d66c-160">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="2d66c-161">`@tagHelperPrefix`: один, ближайший к представлению переопределяет все остальные</span><span class="sxs-lookup"><span data-stu-id="2d66c-161">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="2d66c-162">`@model`: один, ближайший к представлению переопределяет все остальные</span><span class="sxs-lookup"><span data-stu-id="2d66c-162">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="2d66c-163">`@inherits`: один, ближайший к представлению переопределяет все остальные</span><span class="sxs-lookup"><span data-stu-id="2d66c-163">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="2d66c-164">`@using`: все включаются; повторяющиеся значения игнорируются</span><span class="sxs-lookup"><span data-stu-id="2d66c-164">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="2d66c-165">`@inject`: для каждого свойства один, ближайший к представлению переопределяет все другие пользователи с таким же именем свойства</span><span class="sxs-lookup"><span data-stu-id="2d66c-165">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name=viewstart></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="2d66c-166">Выполнение кода до каждого представления</span><span class="sxs-lookup"><span data-stu-id="2d66c-166">Running Code Before Each View</span></span>

<span data-ttu-id="2d66c-167">Если имеется код, необходимо запустить перед каждого представления, это должен быть помещен в `_ViewStart.cshtml` файла.</span><span class="sxs-lookup"><span data-stu-id="2d66c-167">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="2d66c-168">По соглашению `_ViewStart.cshtml` файл находится в `Views` папки.</span><span class="sxs-lookup"><span data-stu-id="2d66c-168">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="2d66c-169">Операторы, перечисленные в `_ViewStart.cshtml` выполняются до каждого полного представления (не макеты и не разделяемые представления).</span><span class="sxs-lookup"><span data-stu-id="2d66c-169">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="2d66c-170">Как [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` являются иерархическими.</span><span class="sxs-lookup"><span data-stu-id="2d66c-170">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="2d66c-171">Если `_ViewStart.cshtml` файл определяется в папке контроллера связанные представления, он будет выполняться после того, определен в корне `Views` папку (если таковые имеются).</span><span class="sxs-lookup"><span data-stu-id="2d66c-171">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="2d66c-172">Образец `_ViewStart.cshtml` файла:</span><span class="sxs-lookup"><span data-stu-id="2d66c-172">A sample `_ViewStart.cshtml` file:</span></span>

<span data-ttu-id="2d66c-173">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="2d66c-173">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]</span></span>

<span data-ttu-id="2d66c-174">Данный файл указывает, что все представления будут использовать `_Layout.cshtml` макет.</span><span class="sxs-lookup"><span data-stu-id="2d66c-174">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="2d66c-175">Ни `_ViewStart.cshtml` , ни `_ViewImports.cshtml` обычно помещаются `/Views/Shared` папки.</span><span class="sxs-lookup"><span data-stu-id="2d66c-175">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="2d66c-176">Версии этих файлов уровня приложения следует поместить непосредственно в `/Views` папки.</span><span class="sxs-lookup"><span data-stu-id="2d66c-176">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
