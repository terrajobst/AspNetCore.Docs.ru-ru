---
title: Создание и использование компонентов ASP.NET Core Razor
author: guardrex
description: Узнайте, как создавать и использовать компоненты Razor, включая способы привязки к данным, обработки событий и управления жизненным циклом компонентов.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/components
ms.openlocfilehash: f9b4eab29fafe8113528062f57d28dadd0f57577
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447104"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="0a8f2-103">Создание и использование компонентов ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="0a8f2-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="0a8f2-104">[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="0a8f2-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="0a8f2-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0a8f2-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

Blazor<span data-ttu-id="0a8f2-106"> приложения создаются с помощью *компонентов*.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-106"> apps are built using *components*.</span></span> <span data-ttu-id="0a8f2-107">Компонент — это автономный фрагмент пользовательского интерфейса, например страница, диалоговое окно или форма.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="0a8f2-108">Компонент включает разметку HTML и логику обработки, необходимую для вставки данных или реагирования на события пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="0a8f2-109">Компоненты являются гибкими и легковесными.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="0a8f2-110">Они могут быть вложенными, повторно использованы и совместно использоваться проектами.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="0a8f2-111">Классы компонентов</span><span class="sxs-lookup"><span data-stu-id="0a8f2-111">Component classes</span></span>

<span data-ttu-id="0a8f2-112">Компоненты реализуются в файлах компонентов [Razor](xref:mvc/views/razor) ( *. Razor*) с помощью комбинации C# и HTML-разметки.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="0a8f2-113">Компонент в Blazor формально называется *компонентом Razor*.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="0a8f2-114">Имя компонента должно начинаться с символа верхнего регистра.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="0a8f2-115">Например, *микулкомпонент. Razor* является допустимым, а *микулкомпонент. Razor* является недопустимым.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="0a8f2-116">Пользовательский интерфейс для компонента определяется с помощью HTML.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="0a8f2-117">Логика динамического отображения (например выражения, циклы и условные выражения) добавляется с помощью встроенного синтаксиса C# под названием [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="0a8f2-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="0a8f2-118">При компиляции приложения логика разметки и C# отрисовки HTML преобразуется в класс компонента.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="0a8f2-119">Имя созданного класса соответствует имени файла.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="0a8f2-120">Элементы класса компонента определяются в блоке `@code`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="0a8f2-121">В блоке `@code` состояние компонента (свойства, поля) указывается с помощью методов обработки событий или для определения другой логики компонента.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="0a8f2-122">Допускается более одного блока `@code`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-122">More than one `@code` block is permissible.</span></span>

<span data-ttu-id="0a8f2-123">Члены компонента можно использовать как часть логики визуализации компонента с помощью C# выражений, начинающихся с `@`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-123">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="0a8f2-124">Например, C# поле подготавливается к просмотру путем добавления `@` к имени поля.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-124">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="0a8f2-125">В следующем примере вычисляется и выводится следующее:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-125">The following example evaluates and renders:</span></span>

* <span data-ttu-id="0a8f2-126">`_headingFontStyle` значение свойства CSS для `font-style`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-126">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="0a8f2-127">`_headingText` содержимое элемента `<h1>`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-127">`_headingText` to the content of the `<h1>` element.</span></span>

```razor
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="0a8f2-128">После первоначального отображения компонента Компонент повторно создает дерево рендеринга в ответ на события.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-128">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> Blazor<span data-ttu-id="0a8f2-129"> затем сравнивает новое дерево отрисовки с предыдущим и применяет любые изменения в модель DOM (DOM) браузера.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-129"> then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="0a8f2-130">Компоненты являются обычными C# классами и могут быть помещены в любое место внутри проекта.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-130">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="0a8f2-131">Компоненты, создающие веб-страницы, обычно находятся в папке *страниц* .</span><span class="sxs-lookup"><span data-stu-id="0a8f2-131">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="0a8f2-132">Компоненты, не являющиеся страницами, часто помещаются в *общую* папку или пользовательскую папку, добавленную в проект.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-132">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span>

<span data-ttu-id="0a8f2-133">Как правило, пространство имен компонента является производным от корневого пространства имен приложения и расположения компонента (папки) в приложении.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-133">Typically, a component's namespace is derived from the app's root namespace and the component's location (folder) within the app.</span></span> <span data-ttu-id="0a8f2-134">Если корневое пространство имен приложения `BlazorApp` и компонент `Counter` находится в папке *pages* :</span><span class="sxs-lookup"><span data-stu-id="0a8f2-134">If the app's root namespace is `BlazorApp` and the `Counter` component resides in the *Pages* folder:</span></span>

* <span data-ttu-id="0a8f2-135">Пространство имен компонента `Counter` `BlazorApp.Pages`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-135">The `Counter` component's namespace is `BlazorApp.Pages`.</span></span>
* <span data-ttu-id="0a8f2-136">Полное имя типа компонента — `BlazorApp.Pages.Counter`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-136">The fully qualified type name of the component is `BlazorApp.Pages.Counter`.</span></span>

<span data-ttu-id="0a8f2-137">Дополнительные сведения см. в разделе [Импорт компонентов](#import-components) .</span><span class="sxs-lookup"><span data-stu-id="0a8f2-137">For more information, see the [Import components](#import-components) section.</span></span>

<span data-ttu-id="0a8f2-138">Чтобы использовать пользовательскую папку, добавьте пространство имен пользовательской папки либо в родительский компонент, либо в файл *_Imports. Razor* приложения.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-138">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="0a8f2-139">Например, следующее пространство имен делает компоненты в папке *Components* доступной при `BlazorApp`корневого пространства имен приложения:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-139">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `BlazorApp`:</span></span>

```razor
@using BlazorApp.Components
```

## <a name="tag-helpers-arent-used-in-components"></a><span data-ttu-id="0a8f2-140">Вспомогательные функции тегов не используются в компонентах</span><span class="sxs-lookup"><span data-stu-id="0a8f2-140">Tag Helpers aren't used in components</span></span>

<span data-ttu-id="0a8f2-141">[Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) не поддерживаются в компонентах Razor (файлы *. Razor* ).</span><span class="sxs-lookup"><span data-stu-id="0a8f2-141">[Tag Helpers](xref:mvc/views/tag-helpers/intro) aren't supported in Razor components (*.razor* files).</span></span> <span data-ttu-id="0a8f2-142">Чтобы обеспечить функциональные возможности тегов в Blazor, создайте компонент с теми же функциональными возможностями, что и вспомогательная функция тега, и используйте вместо него компонент.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-142">To provide Tag Helper-like functionality in Blazor, create a component with the same functionality as the Tag Helper and use the component instead.</span></span>

## <a name="use-components"></a><span data-ttu-id="0a8f2-143">Использование компонентов</span><span class="sxs-lookup"><span data-stu-id="0a8f2-143">Use components</span></span>

<span data-ttu-id="0a8f2-144">Компоненты могут включать другие компоненты, объявляя их с помощью синтаксиса HTML-элементов.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-144">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="0a8f2-145">Разметка для использования компонента выглядит как тег HTML с именем, соответствующем типу компонента.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-145">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="0a8f2-146">Привязка атрибута чувствительна к регистру.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-146">Attribute binding is case sensitive.</span></span> <span data-ttu-id="0a8f2-147">Например, `@bind` является допустимым, а `@Bind` является недопустимым.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-147">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="0a8f2-148">Следующая разметка в *index. Razor* визуализирует `HeadingComponent` экземпляр:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-148">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

```razor
<HeadingComponent />
```

<span data-ttu-id="0a8f2-149">*Components/хеадингкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-149">*Components/HeadingComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

<span data-ttu-id="0a8f2-150">Если компонент содержит HTML-элемент с первой буквой в верхнем регистре, который не соответствует имени компонента, выдается предупреждение, указывающее, что элемент имеет непредвиденное имя.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-150">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="0a8f2-151">Добавление директивы `@using` для пространства имен компонента делает компонент доступным, что позволяет устранить это предупреждение.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-151">Adding an `@using` directive for the component's namespace makes the component available, which resolves the warning.</span></span>

## <a name="routing"></a><span data-ttu-id="0a8f2-152">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="0a8f2-152">Routing</span></span>

<span data-ttu-id="0a8f2-153">Маршрутизация в Blazor достигается путем предоставления шаблона маршрута для каждого доступного компонента в приложении.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-153">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="0a8f2-154">При компиляции файла Razor с директивой `@page` созданному классу присваивается <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> с указанием шаблона маршрута.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-154">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="0a8f2-155">Во время выполнения маршрутизатор ищет классы компонентов с `RouteAttribute` и отображает, какой из компонентов имеет шаблон маршрута, соответствующий запрошенному URL-адресу.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-155">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

```razor
@page "/ParentComponent"

...
```

<span data-ttu-id="0a8f2-156">Дополнительные сведения см. в разделе <xref:blazor/routing>.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-156">For more information, see <xref:blazor/routing>.</span></span>

## <a name="parameters"></a><span data-ttu-id="0a8f2-157">Параметры</span><span class="sxs-lookup"><span data-stu-id="0a8f2-157">Parameters</span></span>

### <a name="route-parameters"></a><span data-ttu-id="0a8f2-158">Параметры маршрута</span><span class="sxs-lookup"><span data-stu-id="0a8f2-158">Route parameters</span></span>

<span data-ttu-id="0a8f2-159">Компоненты могут принимать параметры маршрута из шаблона маршрута, указанного в директиве `@page`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-159">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="0a8f2-160">Маршрутизатор использует параметры маршрута для заполнения соответствующих параметров компонента.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-160">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="0a8f2-161">*Pages/раутепараметер. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-161">*Pages/RouteParameter.razor*:</span></span>

[!code-razor[](components/samples_snapshot/RouteParameter.razor?highlight=2,7-8)]

<span data-ttu-id="0a8f2-162">Необязательные параметры не поддерживаются, поэтому в предыдущем примере применяются две директивы `@page`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-162">Optional parameters aren't supported, so two `@page` directives are applied in the preceding example.</span></span> <span data-ttu-id="0a8f2-163">Первый позволяет переходить к компоненту без параметра.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-163">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="0a8f2-164">Вторая директива `@page` получает параметр `{text}` маршрута и присваивает значение свойству `Text`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-164">The second `@page` directive receives the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

<span data-ttu-id="0a8f2-165">Синтаксис параметра *Catch-All* (`*`/`**`), который захватывает путь для нескольких папок, **не** поддерживается в компонентах Razor ( *. Razor*).</span><span class="sxs-lookup"><span data-stu-id="0a8f2-165">*Catch-all* parameter syntax (`*`/`**`), which captures the path across multiple folder boundaries, is **not** supported in Razor components (*.razor*).</span></span>

### <a name="component-parameters"></a><span data-ttu-id="0a8f2-166">Параметры компонентов</span><span class="sxs-lookup"><span data-stu-id="0a8f2-166">Component parameters</span></span>

<span data-ttu-id="0a8f2-167">Компоненты могут иметь *Параметры компонента*, которые определяются с помощью общедоступных свойств класса Component с атрибутом `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-167">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="0a8f2-168">Используйте атрибуты, чтобы указать аргументы для компонента в разметке.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-168">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="0a8f2-169">*Components/чилдкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-169">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=2,11-12)]

<span data-ttu-id="0a8f2-170">В следующем примере из примера приложения `ParentComponent` задает значение свойства `Title` `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-170">In the following example from the sample app, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="0a8f2-171">*Pages/паренткомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-171">*Pages/ParentComponent.razor*:</span></span>

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="0a8f2-172">Дочернее содержимое</span><span class="sxs-lookup"><span data-stu-id="0a8f2-172">Child content</span></span>

<span data-ttu-id="0a8f2-173">Компоненты могут устанавливать содержимое другого компонента.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-173">Components can set the content of another component.</span></span> <span data-ttu-id="0a8f2-174">Назначение компонента предоставляет содержимое между тегами, задающих принимающий компонент.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-174">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="0a8f2-175">В следующем примере `ChildComponent` имеет свойство `ChildContent`, представляющее `RenderFragment`, которое представляет сегмент пользовательского интерфейса для отрисовки.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-175">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="0a8f2-176">Значение `ChildContent` размещается в разметке компонента, где должно быть визуализировано содержимое.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-176">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="0a8f2-177">Значение `ChildContent` получено от родительского компонента и отображается внутри `panel-body`панели начальной загрузки.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-177">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="0a8f2-178">*Components/чилдкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-178">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="0a8f2-179">Свойству, получающему `RenderFragment`ное содержимое, необходимо присвоить имя `ChildContent` по соглашению.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-179">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="0a8f2-180">`ParentComponent` в примере приложения может предоставить содержимое для подготовки к просмотру `ChildComponent`, поместив содержимое в теги `<ChildComponent>`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-180">The `ParentComponent` in the sample app can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="0a8f2-181">*Pages/паренткомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-181">*Pages/ParentComponent.razor*:</span></span>

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="0a8f2-182">Атрибуты Сплаттинг и произвольные параметры</span><span class="sxs-lookup"><span data-stu-id="0a8f2-182">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="0a8f2-183">Компоненты могут записывать и отображать дополнительные атрибуты в дополнение к объявленным параметрам компонента.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-183">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="0a8f2-184">Дополнительные атрибуты можно записать в словарь, а затем *сплаттед* на элемент при подготовке компонента к просмотру с помощью директивы [`@attributes`](xref:mvc/views/razor#attributes) Razor.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-184">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [`@attributes`](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="0a8f2-185">Этот сценарий полезен при определении компонента, который создает элемент разметки, поддерживающий разнообразные настройки.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-185">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="0a8f2-186">Например, может быть утомительно определять атрибуты отдельно для `<input>`, который поддерживает много параметров.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-186">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="0a8f2-187">В следующем примере первый элемент `<input>` (`id="useIndividualParams"`) использует параметры отдельных компонентов, а второй элемент `<input>` (`id="useAttributesDict"`) использует атрибут Сплаттинг:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-187">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

```razor
<input id="useIndividualParams"
       maxlength="@Maxlength"
       placeholder="@Placeholder"
       required="@Required"
       size="@Size" />

<input id="useAttributesDict"
       @attributes="InputAttributes" />

@code {
    [Parameter]
    public string Maxlength { get; set; } = "10";

    [Parameter]
    public string Placeholder { get; set; } = "Input placeholder text";

    [Parameter]
    public string Required { get; set; } = "required";

    [Parameter]
    public string Size { get; set; } = "50";

    [Parameter]
    public Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "required" },
            { "size", "50" }
        };
}
```

<span data-ttu-id="0a8f2-188">Тип параметра должен реализовывать `IEnumerable<KeyValuePair<string, object>>` со строковыми ключами.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-188">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="0a8f2-189">Использование `IReadOnlyDictionary<string, object>` также является параметром в этом сценарии.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-189">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="0a8f2-190">Отображаемые `<input>` элементы, использующие оба подхода, идентичны:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-190">The rendered `<input>` elements using both approaches is identical:</span></span>

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">
```

<span data-ttu-id="0a8f2-191">Чтобы принять произвольные атрибуты, определите параметр компонента с помощью атрибута `[Parameter]` со свойством `CaptureUnmatchedValues`, для которого задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-191">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="0a8f2-192">Свойство `CaptureUnmatchedValues` в `[Parameter]` позволяет параметру сопоставлять все атрибуты, не соответствующие ни одному другому параметру.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-192">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="0a8f2-193">Компонент может определять только один параметр с `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-193">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="0a8f2-194">Тип свойства, используемый с `CaptureUnmatchedValues`, должен быть назначен из `Dictionary<string, object>` с строковыми ключами.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-194">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="0a8f2-195">в этом сценарии также используются параметры `IEnumerable<KeyValuePair<string, object>>` и `IReadOnlyDictionary<string, object>`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-195">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="0a8f2-196">Расположение `@attributes` относительно положения атрибутов элемента важно.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-196">The position of `@attributes` relative to the position of element attributes is important.</span></span> <span data-ttu-id="0a8f2-197">Когда `@attributes` сплаттед для элемента, атрибуты обрабатываются справа налево (последний — первый).</span><span class="sxs-lookup"><span data-stu-id="0a8f2-197">When `@attributes` are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="0a8f2-198">Рассмотрим следующий пример компонента, использующего компонент `Child`:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-198">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="0a8f2-199">*Паренткомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-199">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="0a8f2-200">*Чилдкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-200">*ChildComponent.razor*:</span></span>

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="0a8f2-201">Атрибут `extra` `Child` компонента установлен справа от `@attributes`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-201">The `Child` component's `extra` attribute is set to the right of `@attributes`.</span></span> <span data-ttu-id="0a8f2-202">`<div>`, отображаемый компонентом `Parent`, содержит `extra="5"` при передаче через дополнительный атрибут, так как атрибуты обрабатываются справа налево (последний — первый):</span><span class="sxs-lookup"><span data-stu-id="0a8f2-202">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="0a8f2-203">В следующем примере порядок `extra` и `@attributes` отменяется в `<div>`е `Child` компонента:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-203">In the following example, the order of `extra` and `@attributes` is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="0a8f2-204">*Паренткомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-204">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="0a8f2-205">*Чилдкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-205">*ChildComponent.razor*:</span></span>

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="0a8f2-206">Отображаемые `<div>` в компоненте `Parent` содержат `extra="10"` при передаче через дополнительный атрибут:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-206">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="capture-references-to-components"></a><span data-ttu-id="0a8f2-207">Запись ссылок на компоненты</span><span class="sxs-lookup"><span data-stu-id="0a8f2-207">Capture references to components</span></span>

<span data-ttu-id="0a8f2-208">Ссылки на компоненты предоставляют способ ссылки на экземпляр компонента, чтобы можно было выполнять команды для этого экземпляра, например `Show` или `Reset`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-208">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="0a8f2-209">Чтобы записать ссылку на компонент, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-209">To capture a component reference:</span></span>

* <span data-ttu-id="0a8f2-210">Добавьте атрибут [`@ref`](xref:mvc/views/razor#ref) в дочерний компонент.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-210">Add an [`@ref`](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="0a8f2-211">Определите поле с тем же типом, что и у дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-211">Define a field with the same type as the child component.</span></span>

```razor
<MyLoginDialog @ref="_loginDialog" ... />

@code {
    private MyLoginDialog _loginDialog;

    private void OnSomething()
    {
        _loginDialog.Show();
    }
}
```

<span data-ttu-id="0a8f2-212">При подготовке компонента к просмотру `_loginDialog` поле заполняется экземпляром `MyLoginDialog` дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-212">When the component is rendered, the `_loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="0a8f2-213">Затем можно вызывать методы .NET в экземпляре компонента.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-213">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0a8f2-214">`_loginDialog` переменная заполняется только после подготовки компонента, а ее выходные данные включают элемент `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-214">The `_loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="0a8f2-215">До этого момента нет никаких ссылок на.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-215">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="0a8f2-216">Чтобы управлять ссылками на компоненты после завершения подготовки компонента к просмотру, используйте [методы онафтеррендерасинк или онафтеррендер](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="0a8f2-216">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="0a8f2-217">При захвате ссылок на компоненты используется аналогичный синтаксис для [записи ссылок на элементы](xref:blazor/javascript-interop#capture-references-to-elements), но это не функция [взаимодействия JavaScript](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="0a8f2-217">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="0a8f2-218">Ссылки на компоненты не передаются в код JavaScript&mdash;они используются только в коде .NET.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-218">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="0a8f2-219">**Не** используйте ссылки на компоненты для изменения состояния дочерних компонентов.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-219">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="0a8f2-220">Вместо этого используйте обычные декларативные параметры для передачи данных дочерним компонентам.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-220">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="0a8f2-221">Использование обычных декларативных параметров приводит к тому, что дочерние компоненты автоматически визуализируются в нужное время.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-221">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="0a8f2-222">Вызывать методы компонента извне для обновления состояния</span><span class="sxs-lookup"><span data-stu-id="0a8f2-222">Invoke component methods externally to update state</span></span>

Blazor<span data-ttu-id="0a8f2-223"> использует `SynchronizationContext` для принудительного применения одного логического потока выполнения.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-223"> uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="0a8f2-224">В этом `SynchronizationContext`выполняются [методы жизненного цикла](xref:blazor/lifecycle) компонента и любые обратные вызовы событий, вызванные Blazor.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-224">A component's [lifecycle methods](xref:blazor/lifecycle) and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="0a8f2-225">В случае, если компонент должен быть обновлен на основе внешнего события, такого как таймер или другие уведомления, используйте метод `InvokeAsync`, который будет отправляться в `SynchronizationContext`Blazor.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-225">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="0a8f2-226">Например, рассмотрим *службу уведомления* , которая может уведомлять любой компонент, выполняющий прослушивание, в обновленном состоянии:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-226">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

```csharp
public class NotifierService
{
    // Can be called from anywhere
    public async Task Update(string key, int value)
    {
        if (Notify != null)
        {
            await Notify.Invoke(key, value);
        }
    }

    public event Func<string, int, Task> Notify;
}
```

<span data-ttu-id="0a8f2-227">Зарегистрируйте `NotifierService` как синглетион:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-227">Register the `NotifierService` as a singletion:</span></span>

* <span data-ttu-id="0a8f2-228">В Blazor сборки Зарегистрируйте службу в `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-228">In Blazor WebAssembly, register the service in `Program.Main`:</span></span>

  ```csharp
  builder.Services.AddSingleton<NotifierService>();
  ```

* <span data-ttu-id="0a8f2-229">В Blazor Server Зарегистрируйте службу в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-229">In Blazor Server, register the service in `Startup.ConfigureServices`:</span></span>

  ```csharp
  services.AddSingleton<NotifierService>();
  ```

<span data-ttu-id="0a8f2-230">Используйте `NotifierService` для обновления компонента:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-230">Use the `NotifierService` to update a component:</span></span>

```razor
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @_lastNotification.key = @_lastNotification.value</p>

@code {
    private (string key, int value) _lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            _lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

<span data-ttu-id="0a8f2-231">В предыдущем примере `NotifierService` вызывает метод `OnNotify` компонента за пределами Blazor`SynchronizationContext`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-231">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="0a8f2-232">`InvokeAsync` используется для переключения на правильный контекст и постановка в очередь визуализации.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-232">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="0a8f2-233">Использование ключа \@для управления сохранением элементов и компонентов</span><span class="sxs-lookup"><span data-stu-id="0a8f2-233">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="0a8f2-234">При последующем изменении списка элементов или компонентов, а также элементов или компонентов алгоритм сравнения Blazorдолжен решить, какие из предыдущих элементов или компонентов могут быть сохранены и как объекты модели должны сопоставляться с ними.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-234">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="0a8f2-235">Обычно этот процесс выполняется автоматически и его можно игнорировать, но в некоторых случаях может потребоваться управление процессом.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-235">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="0a8f2-236">Рассмотрим следующий пример:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-236">Consider the following example:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="0a8f2-237">Содержимое коллекции `People` может изменяться при вставке, удалении или повторном упорядочении записей.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-237">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="0a8f2-238">Когда компонент перерисовывается, компонент `<DetailsEditor>` может измениться на получение различных значений параметров `Details`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-238">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="0a8f2-239">Это может привести к более сложной визуализации, чем ожидалось.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-239">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="0a8f2-240">В некоторых случаях перерисовка может привести к появлению видимых различий в поведении, таких как потеря фокуса элементов.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-240">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="0a8f2-241">Процесс сопоставления можно контролировать с помощью атрибута директивы `@key`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-241">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="0a8f2-242">`@key` заставляет алгоритм сравнения гарантировать сохранение элементов или компонентов на основе значения ключа:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-242">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="person" Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="0a8f2-243">При изменении коллекции `People` алгоритм сравнения удерживает связи между экземплярами `<DetailsEditor>` и экземплярами `person`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-243">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="0a8f2-244">Если `Person` удаляется из списка `People`, то из пользовательского интерфейса удаляется только соответствующий экземпляр `<DetailsEditor>`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-244">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="0a8f2-245">Другие экземпляры остаются без изменений.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-245">Other instances are left unchanged.</span></span>
* <span data-ttu-id="0a8f2-246">Если в какой-либо позиции в списке вставляется `Person`, то в соответствующей позиции вставляется один новый экземпляр `<DetailsEditor>`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-246">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="0a8f2-247">Другие экземпляры остаются без изменений.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-247">Other instances are left unchanged.</span></span>
* <span data-ttu-id="0a8f2-248">При повторном упорядочении записей `Person` соответствующие экземпляры `<DetailsEditor>` сохраняются и переупорядочиваются в пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-248">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="0a8f2-249">В некоторых сценариях использование `@key` уменьшает сложность повторного отображения и позволяет избежать потенциальных проблем с элементами, изменяющимися с отслеживанием состояния DOM, например положением фокуса.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-249">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0a8f2-250">Ключи являются локальными для каждого элемента или компонента контейнера.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-250">Keys are local to each container element or component.</span></span> <span data-ttu-id="0a8f2-251">Ключи не сравниваются глобально по всему документу.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-251">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="0a8f2-252">Когда следует использовать ключ \@</span><span class="sxs-lookup"><span data-stu-id="0a8f2-252">When to use \@key</span></span>

<span data-ttu-id="0a8f2-253">Как правило, имеет смысл использовать `@key` всякий раз при подготовке списка (например, в блоке `@foreach`) и при наличии подходящего значения для определения `@key`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-253">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="0a8f2-254">Можно также использовать `@key`, чтобы запретить Blazor сохранение поддерева элемента или компонента при изменении объекта.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-254">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="0a8f2-255">Если `@currentPerson` меняется, директива атрибута `@key` заставляет Blazor отбросить все `<div>` и его потомков и перестроить поддерево в пользовательском интерфейсе с помощью новых элементов и компонентов.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-255">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="0a8f2-256">Это может быть полезно, если необходимо гарантировать, что при `@currentPerson` изменений состояние пользовательского интерфейса не сохраняется.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-256">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="0a8f2-257">Когда не следует использовать ключ \@</span><span class="sxs-lookup"><span data-stu-id="0a8f2-257">When not to use \@key</span></span>

<span data-ttu-id="0a8f2-258">При использовании сравнения с `@key`ми возникают затраты на производительность.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-258">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="0a8f2-259">Затраты на производительность не слишком велики, но указываются только `@key` если управление правилами сохранения элементов или компонентов выгодно для приложения.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-259">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="0a8f2-260">Даже если `@key` не используется, Blazor сохраняет как можно больше экземпляров дочернего элемента и компонента.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-260">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="0a8f2-261">Единственным преимуществом использования `@key` является управление *тем, как* экземпляры модели сопоставляются с сохраненными экземплярами компонента, а не алгоритмом сравнения, который выбирает сопоставление.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-261">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="0a8f2-262">Какие значения следует использовать для ключа \@</span><span class="sxs-lookup"><span data-stu-id="0a8f2-262">What values to use for \@key</span></span>

<span data-ttu-id="0a8f2-263">Как правило, имеет смысл указать одно из следующих значений для `@key`:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-263">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="0a8f2-264">Экземпляры объектов Model (например, экземпляр `Person`, как в предыдущем примере).</span><span class="sxs-lookup"><span data-stu-id="0a8f2-264">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="0a8f2-265">Это гарантирует сохранение на основе равенства ссылок на объекты.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-265">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="0a8f2-266">Уникальные идентификаторы (например, значения первичного ключа типа `int`, `string`или `Guid`).</span><span class="sxs-lookup"><span data-stu-id="0a8f2-266">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="0a8f2-267">Убедитесь, что значения, используемые для `@key`, не конфликтуют.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-267">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="0a8f2-268">Если конфликтные значения обнаруживаются в одном родительском элементе, Blazor создает исключение, поскольку оно не может детерминированно сопоставлять старые элементы или компоненты с новыми элементами или компонентами.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-268">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="0a8f2-269">Используйте только уникальные значения, такие как экземпляры объекта или значения первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-269">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="partial-class-support"></a><span data-ttu-id="0a8f2-270">Поддержка разделяемых классов</span><span class="sxs-lookup"><span data-stu-id="0a8f2-270">Partial class support</span></span>

<span data-ttu-id="0a8f2-271">Компоненты Razor создаются как разделяемые классы.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-271">Razor components are generated as partial classes.</span></span> <span data-ttu-id="0a8f2-272">Компоненты Razor создаются с помощью любого из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-272">Razor components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="0a8f2-273">C#код определяется в [`@code`](xref:mvc/views/razor#code) блоке с разметкой HTML и кодом Razor в одном файле.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-273">C# code is defined in an [`@code`](xref:mvc/views/razor#code) block with HTML markup and Razor code in a single file.</span></span> <span data-ttu-id="0a8f2-274">с помощью этого подхода шаблоны Blazor определяют свои компоненты Razor.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-274">Blazor templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="0a8f2-275">C#код помещается в файл кода программной части, определенный как разделяемый класс.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-275">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="0a8f2-276">В следующем примере показан компонент `Counter` по умолчанию с блоком `@code` в приложении, созданном из шаблона Blazor.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-276">The following example shows the default `Counter` component with an `@code` block in an app generated from a Blazor template.</span></span> <span data-ttu-id="0a8f2-277">Разметка HTML, код Razor и C# код находятся в одном файле:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-277">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="0a8f2-278">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-278">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int _currentCount = 0;

    void IncrementCount()
    {
        _currentCount++;
    }
}
```

<span data-ttu-id="0a8f2-279">`Counter` компонент можно также создать с помощью файла кода программной части с разделяемым классом:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-279">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="0a8f2-280">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-280">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="0a8f2-281">*Counter.Razor.CS*:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-281">*Counter.razor.cs*:</span></span>

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        private int _currentCount = 0;

        void IncrementCount()
        {
            _currentCount++;
        }
    }
}
```

<span data-ttu-id="0a8f2-282">При необходимости добавьте необходимые пространства имен в файл разделяемого класса.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-282">Add any required namespaces to the partial class file as needed.</span></span> <span data-ttu-id="0a8f2-283">К типичным пространствам имен, используемым компонентами Razor, относятся:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-283">Typical namespaces used by Razor components include:</span></span>

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

## <a name="specify-a-base-class"></a><span data-ttu-id="0a8f2-284">Укажите базовый класс</span><span class="sxs-lookup"><span data-stu-id="0a8f2-284">Specify a base class</span></span>

<span data-ttu-id="0a8f2-285">Директиву [`@inherits`](xref:mvc/views/razor#inherits) можно использовать, чтобы указать базовый класс для компонента.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-285">The [`@inherits`](xref:mvc/views/razor#inherits) directive can be used to specify a base class for a component.</span></span> <span data-ttu-id="0a8f2-286">В следующем примере показано, как компонент может наследовать базовый класс `BlazorRocksBase`, чтобы предоставить свойства и методы компонента.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-286">The following example shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span> <span data-ttu-id="0a8f2-287">Базовый класс должен быть производным от `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-287">The base class should derive from `ComponentBase`.</span></span>

<span data-ttu-id="0a8f2-288">*Pages/блазорроккс. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-288">*Pages/BlazorRocks.razor*:</span></span>

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="0a8f2-289">*BlazorRocksBase.CS*:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-289">*BlazorRocksBase.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Components;

namespace BlazorSample
{
    public class BlazorRocksBase : ComponentBase
    {
        public string BlazorRocksText { get; set; } = 
            "Blazor rocks the browser!";
    }
}
```

## <a name="specify-an-attribute"></a><span data-ttu-id="0a8f2-290">Укажите атрибут</span><span class="sxs-lookup"><span data-stu-id="0a8f2-290">Specify an attribute</span></span>

<span data-ttu-id="0a8f2-291">Атрибуты можно указать в компонентах Razor с помощью директивы [`@attribute`](xref:mvc/views/razor#attribute) .</span><span class="sxs-lookup"><span data-stu-id="0a8f2-291">Attributes can be specified in Razor components with the [`@attribute`](xref:mvc/views/razor#attribute) directive.</span></span> <span data-ttu-id="0a8f2-292">В следующем примере атрибут `[Authorize]` применяется к классу Component:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-292">The following example applies the `[Authorize]` attribute to the component class:</span></span>

```razor
@page "/"
@attribute [Authorize]
```

## <a name="import-components"></a><span data-ttu-id="0a8f2-293">Импорт компонентов</span><span class="sxs-lookup"><span data-stu-id="0a8f2-293">Import components</span></span>

<span data-ttu-id="0a8f2-294">Пространство имен компонента, созданного с помощью Razor, основано на (в порядке приоритета):</span><span class="sxs-lookup"><span data-stu-id="0a8f2-294">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="0a8f2-295">[`@namespace`е](xref:mvc/views/razor#namespace) обозначение в разметке файла Razor ( *. Razor*) (`@namespace BlazorSample.MyNamespace`).</span><span class="sxs-lookup"><span data-stu-id="0a8f2-295">[`@namespace`](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="0a8f2-296">`RootNamespace` проекта в файле проекта (`<RootNamespace>BlazorSample</RootNamespace>`).</span><span class="sxs-lookup"><span data-stu-id="0a8f2-296">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="0a8f2-297">Имя проекта, полученное из имени файла проекта (*CSPROJ*), и путь из корневого каталога проекта к компоненту.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-297">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="0a8f2-298">Например, платформа разрешает *{root Project}/Пажес/индекс.Разор* (*блазорсампле. csproj*) к пространству имен `BlazorSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-298">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="0a8f2-299">Компоненты следуют C# правилам привязки имен.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-299">Components follow C# name binding rules.</span></span> <span data-ttu-id="0a8f2-300">Для компонента `Index` в этом примере компоненты в области являются всеми компонентами:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-300">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="0a8f2-301">В той же папке *страницы*.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-301">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="0a8f2-302">Компоненты в корне проекта, которые не задают явно другое пространство имен.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-302">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="0a8f2-303">Компоненты, определенные в другом пространстве имен, помещаются в область с помощью директивы [`@using`](xref:mvc/views/razor#using) Razor.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-303">Components defined in a different namespace are brought into scope using Razor's [`@using`](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="0a8f2-304">Если другой компонент, `NavMenu.razor`, существует в папке *блазорсампле/Shared/* Folder, компонент можно использовать в `Index.razor` со следующей инструкцией `@using`:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-304">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="0a8f2-305">На компоненты также можно ссылаться с помощью полных имен, для которых не требуется директива [`@using`](xref:mvc/views/razor#using) :</span><span class="sxs-lookup"><span data-stu-id="0a8f2-305">Components can also be referenced using their fully qualified names, which doesn't require the [`@using`](xref:mvc/views/razor#using) directive:</span></span>

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="0a8f2-306">Квалификация `global::` не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-306">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="0a8f2-307">Импорт компонентов с псевдонимами `using` операторы (например, `@using Foo = Bar`) не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-307">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="0a8f2-308">Частично определенные имена не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-308">Partially qualified names aren't supported.</span></span> <span data-ttu-id="0a8f2-309">Например, добавление `@using BlazorSample` и ссылки на `NavMenu.razor` с `<Shared.NavMenu></Shared.NavMenu>` не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-309">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="0a8f2-310">Атрибуты условного HTML-элемента</span><span class="sxs-lookup"><span data-stu-id="0a8f2-310">Conditional HTML element attributes</span></span>

<span data-ttu-id="0a8f2-311">Атрибуты HTML-элемента условно подготавливаются в зависимости от значения .NET.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-311">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="0a8f2-312">Если значение равно `false` или `null`, то атрибут не отображается.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-312">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="0a8f2-313">Если значение равно `true`, атрибут отображается в виде сворачивания.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-313">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="0a8f2-314">В следующем примере `IsCompleted` определяет, отображается ли `checked` в разметке элемента:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-314">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="0a8f2-315">Если `IsCompleted` `true`, флажок отображается следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-315">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="0a8f2-316">Если `IsCompleted` `false`, флажок отображается следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-316">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="0a8f2-317">Дополнительные сведения см. в разделе <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-317">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="0a8f2-318">Некоторые атрибуты HTML, такие как [ARIA-Pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), не работают должным образом, если тип .net является `bool`ом.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-318">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="0a8f2-319">В этих случаях используйте `string` тип вместо `bool`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-319">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="0a8f2-320">Необработанный HTML</span><span class="sxs-lookup"><span data-stu-id="0a8f2-320">Raw HTML</span></span>

<span data-ttu-id="0a8f2-321">Строки обычно подготавливаются с помощью текстовых узлов DOM, что означает, что любая разметка, которую они могут содержать, игнорируется и обрабатывается как литеральный текст.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-321">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="0a8f2-322">Для отрисовки необработанного HTML-кода заключите содержимое HTML в `MarkupString` значение.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-322">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="0a8f2-323">Значение анализируется как HTML или SVG и вставляется в модель DOM.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-323">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="0a8f2-324">Визуализация необработанного HTML-кода, созданного из любого ненадежного источника, является **угрозой безопасности** , и ее следует избегать.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-324">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="0a8f2-325">В следующем примере показано использование типа `MarkupString` для добавления блока статического HTML-содержимого в отображаемые выходные данные компонента:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-325">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)_myMarkup)

@code {
    private string _myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="0a8f2-326">Каскадные значения и параметры</span><span class="sxs-lookup"><span data-stu-id="0a8f2-326">Cascading values and parameters</span></span>

<span data-ttu-id="0a8f2-327">В некоторых сценариях неудобно передавать данные из компонента-предка в дочерний компонент с помощью [параметров компонента](#component-parameters), особенно при наличии нескольких слоев компонентов.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-327">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="0a8f2-328">Каскадные значения и параметры позволяют решить эту проблему, предоставляя удобный способ, позволяющий компоненту-предку предоставить значение всем его дочерним компонентам.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-328">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="0a8f2-329">Каскадные значения и параметры также предоставляют подход для координации компонентов.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-329">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="0a8f2-330">Пример темы</span><span class="sxs-lookup"><span data-stu-id="0a8f2-330">Theme example</span></span>

<span data-ttu-id="0a8f2-331">В следующем примере из примера приложения класс `ThemeInfo` указывает сведения о теме для перетекания иерархии компонентов, чтобы все кнопки в пределах данной части приложения совпадали с тем же стилем.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-331">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="0a8f2-332">*Уисемеклассес/themeinfo указывает расположение. CS*:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-332">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="0a8f2-333">Компонент-предок может предоставлять каскадное значение с помощью компонента каскадного значения.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-333">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="0a8f2-334">Компонент `CascadingValue` упаковывает поддерево иерархии компонентов и предоставляет одно значение всем компонентам в этом поддереве.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-334">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="0a8f2-335">Например, в примере приложения указываются сведения о теме (`ThemeInfo`) в одном из макетов приложения в виде каскадного параметра для всех компонентов, составляющих текст макета свойства `@Body`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-335">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="0a8f2-336">`ButtonClass` присваивается значение `btn-success` в компоненте макета.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-336">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="0a8f2-337">Любой компонент-потомок может использовать это свойство через `ThemeInfo` каскадный объект.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-337">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="0a8f2-338">`CascadingValuesParametersLayout` компонент:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-338">`CascadingValuesParametersLayout` component:</span></span>

```razor
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="_theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo _theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="0a8f2-339">Чтобы использовать каскадные значения, компоненты объявляют каскадные параметры с помощью атрибута `[CascadingParameter]`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-339">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="0a8f2-340">Каскадные значения привязываются к каскадным параметрам по типу.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-340">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="0a8f2-341">В примере приложения компонент `CascadingValuesParametersTheme` привязывает `ThemeInfo` каскадное значение к каскадному параметру.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-341">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="0a8f2-342">Параметр используется для задания класса CSS для одной из кнопок, отображаемых компонентом.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-342">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="0a8f2-343">`CascadingValuesParametersTheme` компонент:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-343">`CascadingValuesParametersTheme` component:</span></span>

```razor
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @_currentCount</p>

<p>
    <button class="btn" @onclick="IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" @onclick="IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@code {
    private int _currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        _currentCount++;
    }
}
```

<span data-ttu-id="0a8f2-344">Чтобы вычислить несколько значений одного типа в одном поддереве, укажите уникальную строку `Name` для каждого компонента `CascadingValue` и соответствующего `CascadingParameter`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-344">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="0a8f2-345">В следующем примере два `CascadingValue` компонентов каскадом разворачивают разные экземпляры `MyCascadingType` по имени:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-345">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

```razor
<CascadingValue Value=@_parentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType _parentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

<span data-ttu-id="0a8f2-346">В компоненте-потомках каскадные параметры получают значения из соответствующих каскадных значений в компоненте-предке по имени:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-346">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="0a8f2-347">Пример Табсет</span><span class="sxs-lookup"><span data-stu-id="0a8f2-347">TabSet example</span></span>

<span data-ttu-id="0a8f2-348">Каскадные параметры также позволяют компонентам работать в иерархии компонентов.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-348">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="0a8f2-349">Например, рассмотрим следующий пример *табсет* в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-349">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="0a8f2-350">В примере приложения имеется интерфейс `ITab`, который реализуется вкладками:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-350">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="0a8f2-351">Компонент `CascadingValuesParametersTabSet` использует компонент `TabSet`, который содержит несколько `Tab` компонентов:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-351">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

```razor
<TabSet>
    <Tab Title="First tab">
        <h4>Greetings from the first tab!</h4>

        <label>
            <input type="checkbox" @bind="showThirdTab" />
            Toggle third tab
        </label>
    </Tab>
    <Tab Title="Second tab">
        <h4>The second tab says Hello World!</h4>
    </Tab>

    @if (showThirdTab)
    {
        <Tab Title="Third tab">
            <h4>Welcome to the disappearing third tab!</h4>
            <p>Toggle this tab from the first tab.</p>
        </Tab>
    }
</TabSet>
```

<span data-ttu-id="0a8f2-352">Дочерние `Tab` компоненты явно не передаются в качестве параметров в `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-352">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="0a8f2-353">Вместо этого дочерние `Tab` компоненты являются частью дочернего содержимого `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-353">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="0a8f2-354">Однако `TabSet` по-прежнему необходимо узнать о каждом компоненте `Tab`, чтобы он мог отобразить заголовки и активную вкладку. Чтобы обеспечить эту координацию без необходимости дополнительного кода, компонент `TabSet` *может предоставить себя как каскадное значение* , которое затем будет выбиралось компонентами-потомками `Tab`.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-354">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="0a8f2-355">`TabSet` компонент:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-355">`TabSet` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="0a8f2-356">Дочерние `Tab` компоненты захватывают содержащий `TabSet` в виде каскадного параметра, поэтому `Tab` компоненты добавляются в `TabSet` и координирует, какая вкладка активна.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-356">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="0a8f2-357">`Tab` компонент:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-357">`Tab` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="0a8f2-358">Шаблоны Razor</span><span class="sxs-lookup"><span data-stu-id="0a8f2-358">Razor templates</span></span>

<span data-ttu-id="0a8f2-359">Фрагменты визуализации можно определить с помощью синтаксиса шаблона Razor.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-359">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="0a8f2-360">Шаблоны Razor позволяют определить фрагмент пользовательского интерфейса и принять следующий формат:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-360">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```razor
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="0a8f2-361">В следующем примере показано, как указать значения `RenderFragment` и `RenderFragment<T>` и визуализировать шаблоны непосредственно в компоненте.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-361">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="0a8f2-362">Фрагменты рендеринга также могут передаваться в качестве аргументов в [шаблонные компоненты](xref:blazor/templated-components).</span><span class="sxs-lookup"><span data-stu-id="0a8f2-362">Render fragments can also be passed as arguments to [templated components](xref:blazor/templated-components).</span></span>

```razor
@_timeTemplate

@_petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment _timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> _petTemplate = (pet) => @<p>Pet: @pet.Name</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="0a8f2-363">Отображаемые выходные данные предыдущего кода:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-363">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Pet: Rex</p>
```

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="0a8f2-364">Масштабируемые изображения векторной графики (SVG)</span><span class="sxs-lookup"><span data-stu-id="0a8f2-364">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="0a8f2-365">Поскольку Blazor отображает HTML-файлы, поддерживаемые браузером изображения, включая масштабируемые векторные графики (SVG *),* поддерживаются с помощью тега `<img>`:</span><span class="sxs-lookup"><span data-stu-id="0a8f2-365">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="0a8f2-366">Аналогичным образом SVG-изображения поддерживаются в правилах CSS файла стилей ( *. CSS*):</span><span class="sxs-lookup"><span data-stu-id="0a8f2-366">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="0a8f2-367">Однако встроенная разметка SVG не поддерживается во всех сценариях.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-367">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="0a8f2-368">Если поместить тег `<svg>` непосредственно в файл компонента (*Razor*), то поддерживается базовая визуализация изображений, но многие расширенные сценарии пока не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-368">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="0a8f2-369">Например, `<use>` теги в настоящее время не учитываются, и `@bind` нельзя использовать с некоторыми тегами SVG.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-369">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="0a8f2-370">В будущем выпуске мы планируем устранить эти ограничения.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-370">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0a8f2-371">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0a8f2-371">Additional resources</span></span>

* <span data-ttu-id="0a8f2-372"><xref:security/blazor/server> &ndash; содержит рекомендации по созданию серверных приложений Blazor, которые должны конкурировать с нехваткой ресурсов.</span><span class="sxs-lookup"><span data-stu-id="0a8f2-372"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
