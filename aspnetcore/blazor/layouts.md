---
title: ASP.NET Core макеты Blazor
author: guardrex
description: Узнайте, как создавать многократно используемые компоненты макета для Blazor приложений.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/layouts
ms.openlocfilehash: 90acfb0d4e9daadb12be79de6bd0c99fc545697a
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944061"
---
# <a name="aspnet-core-opno-locblazor-layouts"></a><span data-ttu-id="476a8-103">ASP.NET Core макеты Blazor</span><span class="sxs-lookup"><span data-stu-id="476a8-103">ASP.NET Core Blazor layouts</span></span>

<span data-ttu-id="476a8-104">[Раинер стропек](https://www.timecockpit.com) и [Люк ЛаСаМ](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="476a8-104">By [Rainer Stropek](https://www.timecockpit.com) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="476a8-105">Некоторые элементы приложения, такие как меню, сообщения об авторских правах и логотипы компании, обычно являются частью общего макета приложения и используются каждым компонентом в приложении.</span><span class="sxs-lookup"><span data-stu-id="476a8-105">Some app elements, such as menus, copyright messages, and company logos, are usually part of app's overall layout and used by every component in the app.</span></span> <span data-ttu-id="476a8-106">Копирование кода этих элементов во все компоненты приложения не является эффективным подходом&mdash;каждый раз, когда одному из элементов требуется обновление, каждый компонент должен быть обновлен.</span><span class="sxs-lookup"><span data-stu-id="476a8-106">Copying the code of these elements into all of the components of an app isn't an efficient approach&mdash;every time one of the elements requires an update, every component must be updated.</span></span> <span data-ttu-id="476a8-107">Такое дублирование сложно поддерживать и может привести к нестабильному содержимому с течением времени.</span><span class="sxs-lookup"><span data-stu-id="476a8-107">Such duplication is difficult to maintain and can lead to inconsistent content over time.</span></span> <span data-ttu-id="476a8-108">*Разметки* решают эту проблему.</span><span class="sxs-lookup"><span data-stu-id="476a8-108">*Layouts* solve this problem.</span></span>

<span data-ttu-id="476a8-109">Технически, макет — это просто другой компонент.</span><span class="sxs-lookup"><span data-stu-id="476a8-109">Technically, a layout is just another component.</span></span> <span data-ttu-id="476a8-110">Макет определяется в шаблоне Razor или в C# коде и может использовать [привязку данных](xref:blazor/components#data-binding), [внедрение зависимостей](xref:blazor/dependency-injection)и сценарии других компонентов.</span><span class="sxs-lookup"><span data-stu-id="476a8-110">A layout is defined in a Razor template or in C# code and can use [data binding](xref:blazor/components#data-binding), [dependency injection](xref:blazor/dependency-injection), and other component scenarios.</span></span>

<span data-ttu-id="476a8-111">Чтобы превратить *компонент* в *Макет*, компонент:</span><span class="sxs-lookup"><span data-stu-id="476a8-111">To turn a *component* into a *layout*, the component:</span></span>

* <span data-ttu-id="476a8-112">Наследует от `LayoutComponentBase`, который определяет свойство `Body` для отображаемого содержимого внутри макета.</span><span class="sxs-lookup"><span data-stu-id="476a8-112">Inherits from `LayoutComponentBase`, which defines a `Body` property for the rendered content inside the layout.</span></span>
* <span data-ttu-id="476a8-113">Использует `@Body` синтаксис Razor, чтобы указать расположение в разметке макета, в которой отображается содержимое.</span><span class="sxs-lookup"><span data-stu-id="476a8-113">Uses the Razor syntax `@Body` to specify the location in the layout markup where the content is rendered.</span></span>

<span data-ttu-id="476a8-114">В следующем образце кода показан шаблон Razor компонента макета *маинлайаут. Razor*.</span><span class="sxs-lookup"><span data-stu-id="476a8-114">The following code sample shows the Razor template of a layout component, *MainLayout.razor*.</span></span> <span data-ttu-id="476a8-115">Макет наследует `LayoutComponentBase` и задает `@Body` между панелью навигации и нижним колонтитулом:</span><span class="sxs-lookup"><span data-stu-id="476a8-115">The layout inherits `LayoutComponentBase` and sets the `@Body` between the navigation bar and the footer:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

<span data-ttu-id="476a8-116">В приложении, основанном на одном из Blazor шаблонов приложений, компонент `MainLayout` (*маинлайаут. Razor*) находится в *общей* папке приложения.</span><span class="sxs-lookup"><span data-stu-id="476a8-116">In an app based on one of the Blazor app templates, the `MainLayout` component (*MainLayout.razor*) is in the app's *Shared* folder.</span></span>

## <a name="default-layout"></a><span data-ttu-id="476a8-117">Макет по умолчанию</span><span class="sxs-lookup"><span data-stu-id="476a8-117">Default layout</span></span>

<span data-ttu-id="476a8-118">Укажите макет приложения по умолчанию в компоненте `Router` в файле *app. Razor* приложения.</span><span class="sxs-lookup"><span data-stu-id="476a8-118">Specify the default app layout in the `Router` component in the app's *App.razor* file.</span></span> <span data-ttu-id="476a8-119">Следующий `Router` компонент, предоставляемый шаблонами Blazor по умолчанию, задает для макета по умолчанию компонент `MainLayout`:</span><span class="sxs-lookup"><span data-stu-id="476a8-119">The following `Router` component, which is provided by the default Blazor templates, sets the default layout to the `MainLayout` component:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/App1.razor?highlight=3)]

<span data-ttu-id="476a8-120">Чтобы предоставить макет по умолчанию для `NotFound` содержимого, укажите `LayoutView` для `NotFound` содержимого:</span><span class="sxs-lookup"><span data-stu-id="476a8-120">To supply a default layout for `NotFound` content, specify a `LayoutView` for `NotFound` content:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/App2.razor?highlight=6-9)]

<span data-ttu-id="476a8-121">Дополнительные сведения о компоненте `Router` см. в разделе <xref:blazor/routing>.</span><span class="sxs-lookup"><span data-stu-id="476a8-121">For more information on the `Router` component, see <xref:blazor/routing>.</span></span>

<span data-ttu-id="476a8-122">Выбор макета в качестве макета по умолчанию в маршрутизаторе является полезной практикой, так как он может быть переопределен отдельно для каждого компонента или папки.</span><span class="sxs-lookup"><span data-stu-id="476a8-122">Specifying the layout as a default layout in the router is a useful practice because it can be overridden on a per-component or per-folder basis.</span></span> <span data-ttu-id="476a8-123">Предпочитать использование маршрутизатора для настройки макета приложения по умолчанию, так как это наиболее общий способ.</span><span class="sxs-lookup"><span data-stu-id="476a8-123">Prefer using the router to set the app's default layout because it's the most general technique.</span></span>

## <a name="specify-a-layout-in-a-component"></a><span data-ttu-id="476a8-124">Указание макета в компоненте</span><span class="sxs-lookup"><span data-stu-id="476a8-124">Specify a layout in a component</span></span>

<span data-ttu-id="476a8-125">Используйте директиву Razor `@layout`, чтобы применить макет к компоненту.</span><span class="sxs-lookup"><span data-stu-id="476a8-125">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="476a8-126">Компилятор преобразует `@layout` в `LayoutAttribute`, который применяется к классу Component.</span><span class="sxs-lookup"><span data-stu-id="476a8-126">The compiler converts `@layout` into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="476a8-127">Содержимое следующего `MasterList` компонента вставляется в `MasterLayout` в позиции `@Body`:</span><span class="sxs-lookup"><span data-stu-id="476a8-127">The content of the following `MasterList` component is inserted into the `MasterLayout` at the position of `@Body`:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

<span data-ttu-id="476a8-128">Указание макета непосредственно в компоненте переопределяет набор *макетов по умолчанию* в маршрутизаторе или директиву `@layout`, импортированную из *_Imports. Razor*.</span><span class="sxs-lookup"><span data-stu-id="476a8-128">Specifying the layout directly in a component overrides a *default layout* set in the router or an `@layout` directive imported from *_Imports.razor*.</span></span>

## <a name="centralized-layout-selection"></a><span data-ttu-id="476a8-129">Выбор централизованного макета</span><span class="sxs-lookup"><span data-stu-id="476a8-129">Centralized layout selection</span></span>

<span data-ttu-id="476a8-130">В каждой папке приложения может дополнительно содержаться файл шаблона с именем *_Imports. Razor*.</span><span class="sxs-lookup"><span data-stu-id="476a8-130">Every folder of an app can optionally contain a template file named *_Imports.razor*.</span></span> <span data-ttu-id="476a8-131">Компилятор включает директивы, указанные в файле импорта, во всех шаблонах Razor в одной и той же папке и рекурсивно во всех ее вложенных папках.</span><span class="sxs-lookup"><span data-stu-id="476a8-131">The compiler includes the directives specified in the imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="476a8-132">Таким образом, файл *_Imports. Razor* , содержащий `@layout MyCoolLayout`, гарантирует, что все компоненты в папке используют `MyCoolLayout`.</span><span class="sxs-lookup"><span data-stu-id="476a8-132">Therefore, an *_Imports.razor* file containing `@layout MyCoolLayout` ensures that all of the components in a folder use `MyCoolLayout`.</span></span> <span data-ttu-id="476a8-133">Нет необходимости многократно добавлять `@layout MyCoolLayout` во все файлы *. Razor* в папке и вложенных папках.</span><span class="sxs-lookup"><span data-stu-id="476a8-133">There's no need to repeatedly add `@layout MyCoolLayout` to all of the *.razor* files within the folder and subfolders.</span></span> <span data-ttu-id="476a8-134">директивы `@using` также применяются к компонентам таким же образом.</span><span class="sxs-lookup"><span data-stu-id="476a8-134">`@using` directives are also applied to components in the same way.</span></span>

<span data-ttu-id="476a8-135">Следующий файл *_Imports. Razor* импортирует:</span><span class="sxs-lookup"><span data-stu-id="476a8-135">The following *_Imports.razor* file imports:</span></span>

* <span data-ttu-id="476a8-136">`MyCoolLayout`.</span><span class="sxs-lookup"><span data-stu-id="476a8-136">`MyCoolLayout`.</span></span>
* <span data-ttu-id="476a8-137">Все компоненты Razor в одной и той же папке и во всех вложенных папках.</span><span class="sxs-lookup"><span data-stu-id="476a8-137">All Razor components in the same folder and any subfolders.</span></span>
* <span data-ttu-id="476a8-138">Пространство имен `BlazorApp1.Data` .</span><span class="sxs-lookup"><span data-stu-id="476a8-138">The `BlazorApp1.Data` namespace.</span></span>
 
[!code-razor[](layouts/sample_snapshot/3.x/_Imports.razor)]

<span data-ttu-id="476a8-139">Файл *_Imports. Razor* аналогичен [файлу _ViewImports. cshtml для представлений и страниц Razor,](xref:mvc/views/layout#importing-shared-directives) но применяется специально к файлам компонентов Razor.</span><span class="sxs-lookup"><span data-stu-id="476a8-139">The *_Imports.razor* file is similar to the [_ViewImports.cshtml file for Razor views and pages](xref:mvc/views/layout#importing-shared-directives) but applied specifically to Razor component files.</span></span>

<span data-ttu-id="476a8-140">Указание макета в *_Imports. Razor* переопределяет макет, указанный в качестве макета маршрутизатора *по умолчанию*.</span><span class="sxs-lookup"><span data-stu-id="476a8-140">Specifying a layout in *_Imports.razor* overrides a layout specified as the router's *default layout*.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="476a8-141">Вложенные макеты</span><span class="sxs-lookup"><span data-stu-id="476a8-141">Nested layouts</span></span>

<span data-ttu-id="476a8-142">Приложения могут состоять из вложенных макетов.</span><span class="sxs-lookup"><span data-stu-id="476a8-142">Apps can consist of nested layouts.</span></span> <span data-ttu-id="476a8-143">Компонент может ссылаться на макет, который, в свою очередь, ссылается на другой макет.</span><span class="sxs-lookup"><span data-stu-id="476a8-143">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="476a8-144">Например, для создания структуры многоуровневого меню используются вложенные макеты.</span><span class="sxs-lookup"><span data-stu-id="476a8-144">For example, nesting layouts are used to create a multi-level menu structure.</span></span>

<span data-ttu-id="476a8-145">В следующем примере показано, как использовать вложенные макеты.</span><span class="sxs-lookup"><span data-stu-id="476a8-145">The following example shows how to use nested layouts.</span></span> <span data-ttu-id="476a8-146">Файл *еписодескомпонент. Razor* — это компонент для вывода.</span><span class="sxs-lookup"><span data-stu-id="476a8-146">The *EpisodesComponent.razor* file is the component to display.</span></span> <span data-ttu-id="476a8-147">Компонент ссылается на `MasterListLayout`:</span><span class="sxs-lookup"><span data-stu-id="476a8-147">The component references the `MasterListLayout`:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

<span data-ttu-id="476a8-148">Файл *мастерлистлайаут. Razor* предоставляет `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="476a8-148">The *MasterListLayout.razor* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="476a8-149">Макет ссылается на другой макет, `MasterLayout`, где он готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="476a8-149">The layout references another layout, `MasterLayout`, where it's rendered.</span></span> <span data-ttu-id="476a8-150">`EpisodesComponent` отображается, где отображается `@Body`:</span><span class="sxs-lookup"><span data-stu-id="476a8-150">`EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

<span data-ttu-id="476a8-151">Наконец, `MasterLayout` в *мастерлайаут. Razor* содержит элементы макета верхнего уровня, такие как заголовок, главное меню и нижний колонтитул.</span><span class="sxs-lookup"><span data-stu-id="476a8-151">Finally, `MasterLayout` in *MasterLayout.razor* contains the top-level layout elements, such as the header, main menu, and footer.</span></span> <span data-ttu-id="476a8-152">`MasterListLayout` с `EpisodesComponent` отображается там, где `@Body` отображается:</span><span class="sxs-lookup"><span data-stu-id="476a8-152">`MasterListLayout` with the `EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="additional-resources"></a><span data-ttu-id="476a8-153">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="476a8-153">Additional resources</span></span>

* <xref:mvc/views/layout>
