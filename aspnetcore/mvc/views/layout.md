---
title: Макет в ASP.NET Core
author: ardalis
description: Узнайте, как использовать общие макеты, директивы и как выполнять общий код перед преобразованием представлений для просмотра в приложении ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/layout
ms.openlocfilehash: a99b239a0aeeb14492b1eee962dc1149f056f0eb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274122"
---
# <a name="layout-in-aspnet-core"></a><span data-ttu-id="efbcb-103">Макет в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="efbcb-103">Layout in ASP.NET Core</span></span>

<span data-ttu-id="efbcb-104">Автор: [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="efbcb-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="efbcb-105">В представлениях часто есть общие визуальные и программные элементы.</span><span class="sxs-lookup"><span data-stu-id="efbcb-105">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="efbcb-106">В этой статье вы узнаете, как использовать общие макеты, директивы и как выполнять общий код перед преобразованием представлений для просмотра в приложении ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="efbcb-106">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="efbcb-107">Что такое макет</span><span class="sxs-lookup"><span data-stu-id="efbcb-107">What is a Layout</span></span>

<span data-ttu-id="efbcb-108">Большинство веб-приложений имеют общий макет, который обеспечивает согласованный пользовательский интерфейс при переходе между страницами.</span><span class="sxs-lookup"><span data-stu-id="efbcb-108">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="efbcb-109">Макет, как правило, включает в себя общие элементы пользовательского интерфейса, такие как верхний и нижний колонтитулы, а также элементы навигации или меню.</span><span class="sxs-lookup"><span data-stu-id="efbcb-109">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Пример макета страницы](layout/_static/page-layout.png)

<span data-ttu-id="efbcb-111">Общие структуры HTML, такие как скрипты и таблицы стилей, также часто используются разными страницами приложения.</span><span class="sxs-lookup"><span data-stu-id="efbcb-111">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="efbcb-112">Все эти общие элементы могут определяться в файле *макета*, на который затем может ссылаться на любое представление в приложении.</span><span class="sxs-lookup"><span data-stu-id="efbcb-112">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="efbcb-113">Макеты сокращают повторы кода в представлениях, помогая им следовать [принципу "не повторяйся" (DRY)](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="efbcb-113">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="efbcb-114">В соответствии с соглашением макет по умолчанию для приложения ASP.NET имеет имя `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="efbcb-114">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="efbcb-115">Шаблон проекта ASP.NET Core MVC в Visual Studio содержит этот файл макета в папке `Views/Shared`.</span><span class="sxs-lookup"><span data-stu-id="efbcb-115">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![Папка Views в обозревателе решений](layout/_static/web-project-views.png)

<span data-ttu-id="efbcb-117">Этот макет определяет шаблон верхнего уровня для представлений в приложении.</span><span class="sxs-lookup"><span data-stu-id="efbcb-117">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="efbcb-118">В приложении может не быть макета либо могут определяться несколько макетов для разных представлений.</span><span class="sxs-lookup"><span data-stu-id="efbcb-118">Apps don't require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="efbcb-119">Пример макета `_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="efbcb-119">An example `_Layout.cshtml`:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="efbcb-120">Указание макета</span><span class="sxs-lookup"><span data-stu-id="efbcb-120">Specifying a Layout</span></span>

<span data-ttu-id="efbcb-121">Представления Razor имеют свойство `Layout`.</span><span class="sxs-lookup"><span data-stu-id="efbcb-121">Razor views have a `Layout` property.</span></span> <span data-ttu-id="efbcb-122">С его помощью указывается макет в отдельных представлениях:</span><span class="sxs-lookup"><span data-stu-id="efbcb-122">Individual views specify a layout by setting this property:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="efbcb-123">Макет может указываться в виде полного пути (пример: `/Views/Shared/_Layout.cshtml`) или частичного имени (пример: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="efbcb-123">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="efbcb-124">Если указано частичное имя, подсистема представлений Razor ищет файл макета, используя стандартный процесс обнаружения.</span><span class="sxs-lookup"><span data-stu-id="efbcb-124">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="efbcb-125">Сначала поиск производится в папке, связанной с контроллером, а затем в папке `Shared`.</span><span class="sxs-lookup"><span data-stu-id="efbcb-125">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="efbcb-126">Процесс обнаружения аналогичен тому, который применяется для поиска [частичных представлений](partial.md).</span><span class="sxs-lookup"><span data-stu-id="efbcb-126">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="efbcb-127">По умолчанию каждый макет должен вызывать метод `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="efbcb-127">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="efbcb-128">При каждом вызове `RenderBody` содержимое представления будет преобразовываться для просмотра.</span><span class="sxs-lookup"><span data-stu-id="efbcb-128">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="efbcb-129">Разделы</span><span class="sxs-lookup"><span data-stu-id="efbcb-129">Sections</span></span>

<span data-ttu-id="efbcb-130">Макет может при необходимости ссылаться на один или несколько *разделов*, вызывая метод `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="efbcb-130">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="efbcb-131">Разделы — это средство для упорядочения размещения определенных элементов на странице.</span><span class="sxs-lookup"><span data-stu-id="efbcb-131">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="efbcb-132">В каждом вызове `RenderSection` можно указывать, является ли раздел обязательным или необязательным.</span><span class="sxs-lookup"><span data-stu-id="efbcb-132">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="efbcb-133">Если обязательный раздел не найден, создается исключение.</span><span class="sxs-lookup"><span data-stu-id="efbcb-133">If a required section isn't found, an exception will be thrown.</span></span> <span data-ttu-id="efbcb-134">В отдельных представлениях содержимое раздела, которое необходимо преобразовать для просмотра, указывается с помощью синтаксиса Razor `@section`.</span><span class="sxs-lookup"><span data-stu-id="efbcb-134">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="efbcb-135">Если в представлении определяется раздел, он должен быть преобразован для просмотра (в противном случае произойдет ошибка).</span><span class="sxs-lookup"><span data-stu-id="efbcb-135">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="efbcb-136">Пример определения `@section` в представлении:</span><span class="sxs-lookup"><span data-stu-id="efbcb-136">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="efbcb-137">В приведенном выше коде в раздел `scripts` представления, включающего в себя форму, добавляются скрипты проверки.</span><span class="sxs-lookup"><span data-stu-id="efbcb-137">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="efbcb-138">В других представлениях в этом же приложении потребность в дополнительных скриптах может отсутствовать, и поэтому в них не нужно определять раздел скриптов.</span><span class="sxs-lookup"><span data-stu-id="efbcb-138">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="efbcb-139">Разделы, определенные в представлении, доступны только непосредственно на странице макета.</span><span class="sxs-lookup"><span data-stu-id="efbcb-139">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="efbcb-140">На них нельзя ссылаться из частичных представлений, компонентов представлений или других частей системы представлений.</span><span class="sxs-lookup"><span data-stu-id="efbcb-140">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="efbcb-141">Пропуск разделов</span><span class="sxs-lookup"><span data-stu-id="efbcb-141">Ignoring sections</span></span>

<span data-ttu-id="efbcb-142">По умолчанию тело и все разделы страницы содержимого должны преобразовываться для просмотра страницей макета.</span><span class="sxs-lookup"><span data-stu-id="efbcb-142">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="efbcb-143">Подсистема представлений Razor обеспечивает выполнение этого требования, следя за тем, были ли преобразованы для просмотра тело и каждый раздел.</span><span class="sxs-lookup"><span data-stu-id="efbcb-143">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="efbcb-144">Чтобы подсистема представлений пропустила тело или разделы, вызовите методы `IgnoreBody` и `IgnoreSection`.</span><span class="sxs-lookup"><span data-stu-id="efbcb-144">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="efbcb-145">Тело и каждый раздел на странице Razor должны либо преобразовываться для просмотра, либо пропускаться.</span><span class="sxs-lookup"><span data-stu-id="efbcb-145">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="efbcb-146">Импорт общих директив</span><span class="sxs-lookup"><span data-stu-id="efbcb-146">Importing Shared Directives</span></span>

<span data-ttu-id="efbcb-147">Представления могут использовать директивы Razor для выполнения различных задач, таких как импорт пространств имен или [внедрение зависимостей](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="efbcb-147">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="efbcb-148">Директивы, используемые несколькими представлениями, можно указать в общем файле `_ViewImports.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="efbcb-148">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="efbcb-149">Файл `_ViewImports` поддерживает следующие директивы:</span><span class="sxs-lookup"><span data-stu-id="efbcb-149">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="efbcb-150">Этот файл не поддерживает другие возможности Razor, такие как функции и определения разделов.</span><span class="sxs-lookup"><span data-stu-id="efbcb-150">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="efbcb-151">Пример файла `_ViewImports.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="efbcb-151">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="efbcb-152">Файл `_ViewImports.cshtml` для приложения ASP.NET Core MVC обычно находится в папке `Views`.</span><span class="sxs-lookup"><span data-stu-id="efbcb-152">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="efbcb-153">Файл `_ViewImports.cshtml` можно поместить в любую папку, но в этом случае он будет применяться только к представлениям в этой папке и вложенных в нее папках.</span><span class="sxs-lookup"><span data-stu-id="efbcb-153">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="efbcb-154">Файлы `_ViewImports` обрабатываются начиная с корневого уровня, а затем в каждой папке вплоть до расположения самого представления, поэтому параметры, заданные на корневом уровне, могут переопределяться на уровне папки.</span><span class="sxs-lookup"><span data-stu-id="efbcb-154">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="efbcb-155">Например, если в файле `_ViewImports.cshtml` корневого уровня определены директивы `@model` и `@addTagHelper`, а в другом файле `_ViewImports.cshtml` в папке представления, связанной с контроллером, определяется другая директива `@model` и добавляется еще одна директива `@addTagHelper`, представление будет иметь доступ к обеим вспомогательным функциям тегов и использовать вторую директиву `@model`.</span><span class="sxs-lookup"><span data-stu-id="efbcb-155">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="efbcb-156">Если для представления выполняются несколько файлов `_ViewImports.cshtml`, директивы, включенные в файлы `ViewImports.cshtml`, будут комбинироваться следующим образом:</span><span class="sxs-lookup"><span data-stu-id="efbcb-156">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="efbcb-157">`@addTagHelper`, `@removeTagHelper`: выполняются все директивы по порядку;</span><span class="sxs-lookup"><span data-stu-id="efbcb-157">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="efbcb-158">`@tagHelperPrefix`: ближайшая к представлению директива переопределяет все остальные;</span><span class="sxs-lookup"><span data-stu-id="efbcb-158">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="efbcb-159">`@model`: ближайшая к представлению директива переопределяет все остальные;</span><span class="sxs-lookup"><span data-stu-id="efbcb-159">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="efbcb-160">`@inherits`: ближайшая к представлению директива переопределяет все остальные;</span><span class="sxs-lookup"><span data-stu-id="efbcb-160">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="efbcb-161">`@using`: включаются все директивы, повторяющиеся пропускаются;</span><span class="sxs-lookup"><span data-stu-id="efbcb-161">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="efbcb-162">`@inject`: для каждого свойства ближайшая к представлению директива переопределяет все остальные директивы с тем же именем свойства.</span><span class="sxs-lookup"><span data-stu-id="efbcb-162">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="efbcb-163">Выполнение кода перед каждым представлением</span><span class="sxs-lookup"><span data-stu-id="efbcb-163">Running Code Before Each View</span></span>

<span data-ttu-id="efbcb-164">Если есть код, который должен выполняться перед каждым представлением, его следует поместить в файл `_ViewStart.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="efbcb-164">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="efbcb-165">В соответствии с соглашением файл `_ViewStart.cshtml` находится в папке `Views`.</span><span class="sxs-lookup"><span data-stu-id="efbcb-165">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="efbcb-166">Операторы, перечисленные в файле `_ViewStart.cshtml`, выполняются перед каждым полным представлением (но не перед макетами и не перед частичными представлениями).</span><span class="sxs-lookup"><span data-stu-id="efbcb-166">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="efbcb-167">Так же как файлы [ViewImports.cshtml](xref:mvc/views/layout#viewimports), файлы `_ViewStart.cshtml` являются иерархическими.</span><span class="sxs-lookup"><span data-stu-id="efbcb-167">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="efbcb-168">Если файл `_ViewStart.cshtml` определен в папке представления, связанной с контроллером, он будет применяться после определенного в корне папки `Views` (при его наличии).</span><span class="sxs-lookup"><span data-stu-id="efbcb-168">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="efbcb-169">Пример файла `_ViewStart.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="efbcb-169">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="efbcb-170">Приведенный файл предписывает всем представлениям использовать макет `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="efbcb-170">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="efbcb-171">Файлы `_ViewStart.cshtml` и `_ViewImports.cshtml`, как правило, не помещаются в папку `/Views/Shared`.</span><span class="sxs-lookup"><span data-stu-id="efbcb-171">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="efbcb-172">Версии этих файлов, которые должны действовать на уровне приложения, следует помещать непосредственно в папку `/Views`.</span><span class="sxs-lookup"><span data-stu-id="efbcb-172">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
