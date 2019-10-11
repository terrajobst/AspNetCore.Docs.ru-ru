---
title: Создание и использование компонентов ASP.NET Core Razor
author: guardrex
description: Узнайте, как создавать и использовать компоненты Razor, включая способы привязки к данным, обработки событий и управления жизненным циклом компонентов.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/05/2019
uid: blazor/components
ms.openlocfilehash: 3e0966bf978c99fc00db7682bea3292306cbb03c
ms.sourcegitcommit: d81912782a8b0bd164f30a516ad80f8defb5d020
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72179028"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="a30f7-103">Создание и использование компонентов ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="a30f7-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="a30f7-104">[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="a30f7-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="a30f7-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a30f7-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a30f7-106">Приложения блазор создаются с помощью *компонентов*.</span><span class="sxs-lookup"><span data-stu-id="a30f7-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="a30f7-107">Компонент — это автономный фрагмент пользовательского интерфейса, например страница, диалоговое окно или форма.</span><span class="sxs-lookup"><span data-stu-id="a30f7-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="a30f7-108">Компонент включает разметку HTML и логику обработки, необходимую для вставки данных или реагирования на события пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="a30f7-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="a30f7-109">Компоненты являются гибкими и легковесными.</span><span class="sxs-lookup"><span data-stu-id="a30f7-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="a30f7-110">Они могут быть вложенными, повторно использованы и совместно использоваться проектами.</span><span class="sxs-lookup"><span data-stu-id="a30f7-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="a30f7-111">Классы компонентов</span><span class="sxs-lookup"><span data-stu-id="a30f7-111">Component classes</span></span>

<span data-ttu-id="a30f7-112">Компоненты реализуются в файлах компонентов [Razor](xref:mvc/views/razor) ( *. Razor*) с помощью комбинации C# и HTML-разметки.</span><span class="sxs-lookup"><span data-stu-id="a30f7-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="a30f7-113">Компонент в Блазор формально называется *компонентом Razor*.</span><span class="sxs-lookup"><span data-stu-id="a30f7-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="a30f7-114">Имя компонента должно начинаться с символа верхнего регистра.</span><span class="sxs-lookup"><span data-stu-id="a30f7-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="a30f7-115">Например, *микулкомпонент. Razor* является допустимым, а *микулкомпонент. Razor* является недопустимым.</span><span class="sxs-lookup"><span data-stu-id="a30f7-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="a30f7-116">Пользовательский интерфейс для компонента определяется с помощью HTML.</span><span class="sxs-lookup"><span data-stu-id="a30f7-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="a30f7-117">Логика динамического отображения (например выражения, циклы и условные выражения) добавляется с помощью встроенного синтаксиса C# под названием [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="a30f7-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="a30f7-118">При компиляции приложения логика разметки и C# отрисовки HTML преобразуется в класс компонента.</span><span class="sxs-lookup"><span data-stu-id="a30f7-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="a30f7-119">Имя созданного класса соответствует имени файла.</span><span class="sxs-lookup"><span data-stu-id="a30f7-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="a30f7-120">Элементы класса компонента определяются в блоке `@code`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="a30f7-121">В блоке `@code` состояние компонента (свойства, поля) указывается с помощью методов обработки событий или для определения другой логики компонента.</span><span class="sxs-lookup"><span data-stu-id="a30f7-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="a30f7-122">Допускается более одного блока `@code`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-122">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="a30f7-123">В предыдущих предварительных версиях ASP.NET Core 3,0 блоки `@functions` использовались для той же цели, что и блоки `@code` в компонентах Razor.</span><span class="sxs-lookup"><span data-stu-id="a30f7-123">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="a30f7-124">блоки `@functions` продолжают работать в компонентах Razor, но рекомендуется использовать блок `@code` в ASP.NET Core 3,0 Preview 6 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="a30f7-124">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="a30f7-125">Члены компонента можно использовать как часть логики визуализации компонента с помощью C# выражений, начинающихся с `@`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-125">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="a30f7-126">Например, C# поле отображается путем добавления префикса `@` к имени поля.</span><span class="sxs-lookup"><span data-stu-id="a30f7-126">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="a30f7-127">В следующем примере вычисляется и выводится следующее:</span><span class="sxs-lookup"><span data-stu-id="a30f7-127">The following example evaluates and renders:</span></span>

* <span data-ttu-id="a30f7-128">`_headingFontStyle` в значение свойства CSS для `font-style`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-128">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="a30f7-129">`_headingText` на содержимое элемента `<h1>`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-129">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="a30f7-130">После первоначального отображения компонента Компонент повторно создает дерево рендеринга в ответ на события.</span><span class="sxs-lookup"><span data-stu-id="a30f7-130">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="a30f7-131">Затем блазор сравнивает новое дерево отрисовки с предыдущим и применяет любые изменения к модель DOMу (DOM) браузера.</span><span class="sxs-lookup"><span data-stu-id="a30f7-131">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="a30f7-132">Компоненты являются обычными C# классами и могут быть помещены в любое место внутри проекта.</span><span class="sxs-lookup"><span data-stu-id="a30f7-132">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="a30f7-133">Компоненты, создающие веб-страницы, обычно находятся в папке *страниц* .</span><span class="sxs-lookup"><span data-stu-id="a30f7-133">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="a30f7-134">Компоненты, не являющиеся страницами, часто помещаются в *общую* папку или пользовательскую папку, добавленную в проект.</span><span class="sxs-lookup"><span data-stu-id="a30f7-134">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="a30f7-135">Чтобы использовать пользовательскую папку, добавьте пространство имен пользовательской папки либо в родительский компонент, либо в файл *_Imports. Razor* приложения.</span><span class="sxs-lookup"><span data-stu-id="a30f7-135">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="a30f7-136">Например, следующее пространство имен делает компоненты в папке *Components* доступной, когда корневое пространство имен приложения имеет значение `WebApplication`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-136">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="a30f7-137">Интеграция компонентов в Razor Pages и приложения MVC</span><span class="sxs-lookup"><span data-stu-id="a30f7-137">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="a30f7-138">Используйте компоненты с существующими Razor Pages и приложениями MVC.</span><span class="sxs-lookup"><span data-stu-id="a30f7-138">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="a30f7-139">Нет необходимости переписывать существующие страницы или представления для использования компонентов Razor.</span><span class="sxs-lookup"><span data-stu-id="a30f7-139">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="a30f7-140">При отображении страницы или представления компоненты предварительно отображаются.</span><span class="sxs-lookup"><span data-stu-id="a30f7-140">When the page or view is rendered, components are prerendered at the same time.</span></span>

<span data-ttu-id="a30f7-141">Чтобы отобразить компонент из страницы или представления, используйте вспомогательный метод HTML `RenderComponentAsync<TComponent>`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-141">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="MyComponent">
    @(await Html.RenderComponentAsync<MyComponent>(RenderMode.ServerPrerendered))
</div>
```

<span data-ttu-id="a30f7-142">Хотя страницы и представления могут использовать компоненты, наоборот это не так.</span><span class="sxs-lookup"><span data-stu-id="a30f7-142">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="a30f7-143">Компоненты не могут использовать сценарии представления и страницы, такие как частичные представления и разделы.</span><span class="sxs-lookup"><span data-stu-id="a30f7-143">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="a30f7-144">Чтобы использовать логику из частичного представления в компоненте, разнесите логику частичного представления в компонент.</span><span class="sxs-lookup"><span data-stu-id="a30f7-144">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="a30f7-145">Дополнительные сведения о подготовке компонентов к просмотру и управлении состоянием компонентов в Блазор Server Apps см. в статье <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="a30f7-145">For more information on how components are rendered and component state is managed in Blazor Server apps, see the <xref:blazor/hosting-models> article.</span></span>

## <a name="use-components"></a><span data-ttu-id="a30f7-146">Использование компонентов</span><span class="sxs-lookup"><span data-stu-id="a30f7-146">Use components</span></span>

<span data-ttu-id="a30f7-147">Компоненты могут включать другие компоненты, объявляя их с помощью синтаксиса HTML-элементов.</span><span class="sxs-lookup"><span data-stu-id="a30f7-147">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="a30f7-148">Разметка для использования компонента выглядит как тег HTML с именем, соответствующем типу компонента.</span><span class="sxs-lookup"><span data-stu-id="a30f7-148">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="a30f7-149">Привязка атрибута чувствительна к регистру.</span><span class="sxs-lookup"><span data-stu-id="a30f7-149">Attribute binding is case sensitive.</span></span> <span data-ttu-id="a30f7-150">Например, `@bind` является допустимым, а `@Bind` является недопустимым.</span><span class="sxs-lookup"><span data-stu-id="a30f7-150">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="a30f7-151">Следующая разметка в *index. Razor* визуализирует экземпляр `HeadingComponent`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-151">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="a30f7-152">*Components/хеадингкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a30f7-152">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

<span data-ttu-id="a30f7-153">Если компонент содержит HTML-элемент с первой буквой в верхнем регистре, который не соответствует имени компонента, выдается предупреждение, указывающее, что элемент имеет непредвиденное имя.</span><span class="sxs-lookup"><span data-stu-id="a30f7-153">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="a30f7-154">Добавление оператора `@using` для пространства имен компонента делает компонент доступным, что приводит к удалению предупреждения.</span><span class="sxs-lookup"><span data-stu-id="a30f7-154">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="a30f7-155">Параметры компонентов</span><span class="sxs-lookup"><span data-stu-id="a30f7-155">Component parameters</span></span>

<span data-ttu-id="a30f7-156">Компоненты могут иметь *Параметры компонента*, которые определяются с помощью общедоступных свойств класса Component с атрибутом `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-156">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="a30f7-157">Используйте атрибуты, чтобы указать аргументы для компонента в разметке.</span><span class="sxs-lookup"><span data-stu-id="a30f7-157">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="a30f7-158">*Components/чилдкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a30f7-158">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="a30f7-159">В следующем примере `ParentComponent` задает значение свойства `Title` для `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-159">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="a30f7-160">*Pages/паренткомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a30f7-160">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="a30f7-161">Дочернее содержимое</span><span class="sxs-lookup"><span data-stu-id="a30f7-161">Child content</span></span>

<span data-ttu-id="a30f7-162">Компоненты могут устанавливать содержимое другого компонента.</span><span class="sxs-lookup"><span data-stu-id="a30f7-162">Components can set the content of another component.</span></span> <span data-ttu-id="a30f7-163">Назначение компонента предоставляет содержимое между тегами, задающих принимающий компонент.</span><span class="sxs-lookup"><span data-stu-id="a30f7-163">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="a30f7-164">В следующем примере `ChildComponent` имеет свойство `ChildContent`, представляющее `RenderFragment`, которое представляет сегмент пользовательского интерфейса для отрисовки.</span><span class="sxs-lookup"><span data-stu-id="a30f7-164">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="a30f7-165">Значение `ChildContent` размещается в разметке компонента, где должно быть визуализировано содержимое.</span><span class="sxs-lookup"><span data-stu-id="a30f7-165">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="a30f7-166">Значение `ChildContent` получено от родительского компонента и отображается в `panel-body` панели начальной загрузки.</span><span class="sxs-lookup"><span data-stu-id="a30f7-166">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="a30f7-167">*Components/чилдкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a30f7-167">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="a30f7-168">Свойство, получающее содержимое `RenderFragment`, должно иметь имя `ChildContent` по соглашению.</span><span class="sxs-lookup"><span data-stu-id="a30f7-168">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="a30f7-169">Следующий `ParentComponent` может предоставить содержимое для отрисовки `ChildComponent`, поместив содержимое в теги `<ChildComponent>`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-169">The following `ParentComponent` can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="a30f7-170">*Pages/паренткомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a30f7-170">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="a30f7-171">Атрибуты Сплаттинг и произвольные параметры</span><span class="sxs-lookup"><span data-stu-id="a30f7-171">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="a30f7-172">Компоненты могут записывать и отображать дополнительные атрибуты в дополнение к объявленным параметрам компонента.</span><span class="sxs-lookup"><span data-stu-id="a30f7-172">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="a30f7-173">Дополнительные атрибуты могут быть записаны в словарь, а затем *сплаттед* на элемент при подготовке компонента к просмотру с помощью директивы Razor [@attributes](xref:mvc/views/razor#attributes) .</span><span class="sxs-lookup"><span data-stu-id="a30f7-173">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [@attributes](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="a30f7-174">Этот сценарий полезен при определении компонента, который создает элемент разметки, поддерживающий разнообразные настройки.</span><span class="sxs-lookup"><span data-stu-id="a30f7-174">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="a30f7-175">Например, может быть утомительно определять атрибуты отдельно для `<input>`, который поддерживает много параметров.</span><span class="sxs-lookup"><span data-stu-id="a30f7-175">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="a30f7-176">В следующем примере первый элемент `<input>` (`id="useIndividualParams"`) использует параметры отдельных компонентов, а второй элемент `<input>` (`id="useAttributesDict"`) использует атрибут Сплаттинг:</span><span class="sxs-lookup"><span data-stu-id="a30f7-176">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

```cshtml
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

<span data-ttu-id="a30f7-177">Тип параметра должен реализовывать `IEnumerable<KeyValuePair<string, object>>` со строковыми ключами.</span><span class="sxs-lookup"><span data-stu-id="a30f7-177">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="a30f7-178">Использование `IReadOnlyDictionary<string, object>` также является параметром в этом сценарии.</span><span class="sxs-lookup"><span data-stu-id="a30f7-178">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="a30f7-179">Отображаемые элементы `<input>`, использующие оба подхода, идентичны:</span><span class="sxs-lookup"><span data-stu-id="a30f7-179">The rendered `<input>` elements using both approaches is identical:</span></span>

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

<span data-ttu-id="a30f7-180">Чтобы принять произвольные атрибуты, определите параметр компонента с помощью атрибута `[Parameter]` со свойством `CaptureUnmatchedValues`, установленным в значение `true`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-180">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```cshtml
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="a30f7-181">Свойство `CaptureUnmatchedValues` в `[Parameter]` позволяет параметру сопоставлять все атрибуты, не соответствующие какому-либо другому параметру.</span><span class="sxs-lookup"><span data-stu-id="a30f7-181">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="a30f7-182">Компонент может определять только один параметр с `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-182">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="a30f7-183">Тип свойства, используемый с `CaptureUnmatchedValues`, должен быть назначен из `Dictionary<string, object>` со строковыми ключами.</span><span class="sxs-lookup"><span data-stu-id="a30f7-183">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="a30f7-184">в этом сценарии также используются параметры `IEnumerable<KeyValuePair<string, object>>` или `IReadOnlyDictionary<string, object>`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-184">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

## <a name="data-binding"></a><span data-ttu-id="a30f7-185">Привязка данных</span><span class="sxs-lookup"><span data-stu-id="a30f7-185">Data binding</span></span>

<span data-ttu-id="a30f7-186">Привязка данных как к компонентам, так и к элементам DOM осуществляется с помощью атрибута [@bind](xref:mvc/views/razor#bind) .</span><span class="sxs-lookup"><span data-stu-id="a30f7-186">Data binding to both components and DOM elements is accomplished with the [@bind](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="a30f7-187">В следующем примере свойство `CurrentValue` привязывается к значению текстового поля:</span><span class="sxs-lookup"><span data-stu-id="a30f7-187">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```cshtml
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="a30f7-188">Когда текстовое поле теряет фокус, значение свойства обновляется.</span><span class="sxs-lookup"><span data-stu-id="a30f7-188">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="a30f7-189">Текстовое поле обновляется в пользовательском интерфейсе только при подготовке компонента к просмотру, а не в ответ на изменение значения свойства.</span><span class="sxs-lookup"><span data-stu-id="a30f7-189">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="a30f7-190">Поскольку компоненты отображаются после выполнения кода обработчика событий, обновления свойств *обычно* отражаются в пользовательском интерфейсе сразу после запуска обработчика событий.</span><span class="sxs-lookup"><span data-stu-id="a30f7-190">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="a30f7-191">Использование `@bind` со свойством `CurrentValue` (`<input @bind="CurrentValue" />`) в основном эквивалентно следующему:</span><span class="sxs-lookup"><span data-stu-id="a30f7-191">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="a30f7-192">При подготовке к просмотру компонента `value` элемента input берется из свойства `CurrentValue`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-192">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="a30f7-193">Когда пользователь вводит текст в текстовое поле и изменяет фокус элемента, возникает событие `onchange`, а свойству `CurrentValue` присваивается измененное значение.</span><span class="sxs-lookup"><span data-stu-id="a30f7-193">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="a30f7-194">На практике создание кода более сложно, так как `@bind` обрабатывает случаи, когда выполняются преобразования типов.</span><span class="sxs-lookup"><span data-stu-id="a30f7-194">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="a30f7-195">В принципе, `@bind` связывает текущее значение выражения с атрибутом `value` и обрабатывает изменения с помощью зарегистрированного обработчика.</span><span class="sxs-lookup"><span data-stu-id="a30f7-195">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="a30f7-196">Помимо обработки событий `onchange` с синтаксисом `@bind`, свойство или поле можно привязать с помощью других событий, указав атрибут [@bind-value](xref:mvc/views/razor#bind) с параметром `event` ([@bind-value:event](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="a30f7-196">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [@bind-value](xref:mvc/views/razor#bind) attribute with an `event` parameter ([@bind-value:event](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="a30f7-197">В следующем примере привязывается свойство `CurrentValue` для события `oninput`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-197">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="a30f7-198">В отличие от `onchange`, которое срабатывает при потере фокуса элементом, `oninput` срабатывает при изменении значения текстового поля.</span><span class="sxs-lookup"><span data-stu-id="a30f7-198">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="a30f7-199">**Неанализируемые значения**</span><span class="sxs-lookup"><span data-stu-id="a30f7-199">**Unparsable values**</span></span>

<span data-ttu-id="a30f7-200">Когда пользователь предоставляет неинтерпретируемое значение для элемента с привязкой к данным, неанализируемое значение автоматически возвращается к предыдущему значению при срабатывании события привязки.</span><span class="sxs-lookup"><span data-stu-id="a30f7-200">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="a30f7-201">Рассмотрим следующий сценарий.</span><span class="sxs-lookup"><span data-stu-id="a30f7-201">Consider the following scenario:</span></span>

* <span data-ttu-id="a30f7-202">Элемент `<input>` привязан к типу `int` с начальным значением `123`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-202">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```cshtml
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="a30f7-203">Пользователь обновляет значение элемента на `123.45` на странице и изменяет фокус элемента.</span><span class="sxs-lookup"><span data-stu-id="a30f7-203">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="a30f7-204">В приведенном выше сценарии значение элемента возвращается к `123`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-204">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="a30f7-205">Если значение `123.45` отклоняется в пользу исходного значения `123`, пользователь понимает, что их значение не принято.</span><span class="sxs-lookup"><span data-stu-id="a30f7-205">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="a30f7-206">По умолчанию привязка применяется к событию `onchange` элемента (`@bind="{PROPERTY OR FIELD}"`).</span><span class="sxs-lookup"><span data-stu-id="a30f7-206">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="a30f7-207">Чтобы задать другое событие, используйте `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-207">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="a30f7-208">Для события `oninput` (`@bind-value:event="oninput"`) переверсия происходит после любого нажатия клавиши, которая вводит неинтерпретируемое значение.</span><span class="sxs-lookup"><span data-stu-id="a30f7-208">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="a30f7-209">При нацеливании на событие `oninput` с типом, связанным с @no__t -1, пользователю запрещено вводить символ `.`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-209">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="a30f7-210">Символ `.` немедленно удаляется, поэтому пользователь получает немедленный отзыв о том, что разрешены только целые числа.</span><span class="sxs-lookup"><span data-stu-id="a30f7-210">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="a30f7-211">Существуют сценарии, в которых возвращение значения события `oninput` не является идеальным решением, например, когда пользователю нужно удалить неинтерпретируемое значение `<input>`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-211">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="a30f7-212">К альтернативам относятся:</span><span class="sxs-lookup"><span data-stu-id="a30f7-212">Alternatives include:</span></span>

* <span data-ttu-id="a30f7-213">Не используйте событие `oninput`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-213">Don't use the `oninput` event.</span></span> <span data-ttu-id="a30f7-214">Используйте событие `onchange` по умолчанию (`@bind="{PROPERTY OR FIELD}"`), где недопустимое значение не возвращается, пока элемент не теряет фокус.</span><span class="sxs-lookup"><span data-stu-id="a30f7-214">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="a30f7-215">Выполните привязку к типу, допускающему значение null, например `int?` или `string`, и предоставьте настраиваемую логику для обработки недопустимых записей.</span><span class="sxs-lookup"><span data-stu-id="a30f7-215">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="a30f7-216">Используйте [компонент проверки формы](xref:blazor/forms-validation), например `InputNumber` или `InputDate`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-216">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="a30f7-217">Компоненты проверки форм имеют встроенную поддержку для управления недопустимыми входными данными.</span><span class="sxs-lookup"><span data-stu-id="a30f7-217">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="a30f7-218">Компоненты проверки формы:</span><span class="sxs-lookup"><span data-stu-id="a30f7-218">Form validation components:</span></span>
  * <span data-ttu-id="a30f7-219">Позволяет пользователю указать недопустимые входные данные и получить ошибки проверки для связанного `EditContext`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-219">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="a30f7-220">Отображает ошибки проверки в пользовательском интерфейсе, не мешая пользователю вводить дополнительные данные из формы.</span><span class="sxs-lookup"><span data-stu-id="a30f7-220">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

<span data-ttu-id="a30f7-221">**Глобализация**</span><span class="sxs-lookup"><span data-stu-id="a30f7-221">**Globalization**</span></span>

<span data-ttu-id="a30f7-222">значения `@bind` форматируются для вывода и анализируются с использованием правил текущего языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="a30f7-222">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="a30f7-223">Доступ к текущему языку и региональным параметрам можно получить из свойства <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="a30f7-223">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="a30f7-224">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) используется для следующих типов полей (`<input type="{TYPE}" />`):</span><span class="sxs-lookup"><span data-stu-id="a30f7-224">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="a30f7-225">Предыдущие типы полей:</span><span class="sxs-lookup"><span data-stu-id="a30f7-225">The preceding field types:</span></span>

* <span data-ttu-id="a30f7-226">Отображаются с использованием соответствующих правил форматирования на основе браузера.</span><span class="sxs-lookup"><span data-stu-id="a30f7-226">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="a30f7-227">Не может содержать текст в свободной форме.</span><span class="sxs-lookup"><span data-stu-id="a30f7-227">Can't contain free-form text.</span></span>
* <span data-ttu-id="a30f7-228">Предоставление характеристик взаимодействия с пользователем в зависимости от реализации браузера.</span><span class="sxs-lookup"><span data-stu-id="a30f7-228">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="a30f7-229">Следующие типы полей имеют особые требования к форматированию, которые в настоящее время не поддерживаются Блазор, так как они не поддерживаются всеми основными браузерами.</span><span class="sxs-lookup"><span data-stu-id="a30f7-229">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="a30f7-230">`@bind` поддерживает параметр `@bind:culture`, чтобы предоставить <xref:System.Globalization.CultureInfo?displayProperty=fullName> для синтаксического анализа и форматирования значения.</span><span class="sxs-lookup"><span data-stu-id="a30f7-230">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="a30f7-231">Указание языка и региональных параметров не рекомендуется при использовании типов полей `date` и `number`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-231">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="a30f7-232">`date` и `number` имеют встроенную поддержку Блазор, которая предоставляет требуемый язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="a30f7-232">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="a30f7-233">Сведения о том, как задать язык и региональные параметры пользователя, см. в разделе [локализация](#localization).</span><span class="sxs-lookup"><span data-stu-id="a30f7-233">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="a30f7-234">**Строки формата**</span><span class="sxs-lookup"><span data-stu-id="a30f7-234">**Format strings**</span></span>

<span data-ttu-id="a30f7-235">Привязка данных работает со строками формата <xref:System.DateTime> с помощью [@bind:format](xref:mvc/views/razor#bind).</span><span class="sxs-lookup"><span data-stu-id="a30f7-235">Data binding works with <xref:System.DateTime> format strings using [@bind:format](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="a30f7-236">Другие выражения форматирования, такие как денежные или числовые форматы, в настоящее время недоступны.</span><span class="sxs-lookup"><span data-stu-id="a30f7-236">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="a30f7-237">В приведенном выше коде тип поля элемента `<input>` (`type`) по умолчанию имеет значение `text`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-237">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="a30f7-238">`@bind:format` поддерживается для привязки следующих типов .NET:</span><span class="sxs-lookup"><span data-stu-id="a30f7-238">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="a30f7-239"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="a30f7-239"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="a30f7-240"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="a30f7-240"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="a30f7-241">Атрибут `@bind:format` задает формат даты, применяемый к `value` элемента `<input>`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-241">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="a30f7-242">Формат также используется для анализа значения при возникновении события `onchange`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-242">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="a30f7-243">Указание формата для типа поля `date` не рекомендуется, так как Блазор имеет встроенную поддержку для форматирования дат.</span><span class="sxs-lookup"><span data-stu-id="a30f7-243">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span>

<span data-ttu-id="a30f7-244">**Параметры компонента**</span><span class="sxs-lookup"><span data-stu-id="a30f7-244">**Component parameters**</span></span>

<span data-ttu-id="a30f7-245">Привязка распознает параметры компонента, где `@bind-{property}` может привязать значение свойства к нескольким компонентам.</span><span class="sxs-lookup"><span data-stu-id="a30f7-245">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="a30f7-246">Следующий дочерний компонент (`ChildComponent`) имеет параметр компонента `Year` и обратный вызов `YearChanged`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-246">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="a30f7-247">`EventCallback<T>` объясняется в разделе [вложенный EventCallback](#eventcallback) .</span><span class="sxs-lookup"><span data-stu-id="a30f7-247">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="a30f7-248">Следующий родительский компонент использует `ChildComponent` и привязывает параметр `ParentYear` от родительского к параметру `Year` в дочернем компоненте:</span><span class="sxs-lookup"><span data-stu-id="a30f7-248">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent @bind-Year="ParentYear" />

<button class="btn btn-primary" @onclick="ChangeTheYear">
    Change Year to 1986
</button>

@code {
    [Parameter]
    public int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="a30f7-249">При загрузке `ParentComponent` создается следующая разметка:</span><span class="sxs-lookup"><span data-stu-id="a30f7-249">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="a30f7-250">Если значение свойства `ParentYear` было изменено нажатием кнопки в `ParentComponent`, то обновляется свойство `Year` объекта `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-250">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="a30f7-251">Новое значение `Year` отображается в пользовательском интерфейсе при повторной отрисовке `ParentComponent`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-251">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="a30f7-252">Параметр `Year` доступен для привязки, так как имеет сопутствующее событие `YearChanged`, соответствующее типу параметра `Year`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-252">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="a30f7-253">По соглашению `<ChildComponent @bind-Year="ParentYear" />` по сути является аналогом записи:</span><span class="sxs-lookup"><span data-stu-id="a30f7-253">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="a30f7-254">Как правило, свойство может быть привязано к соответствующему обработчику событий с помощью атрибута `@bind-property:event`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-254">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="a30f7-255">Например, свойство `MyProp` можно привязать к `MyEventHandler` с помощью следующих двух атрибутов:</span><span class="sxs-lookup"><span data-stu-id="a30f7-255">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="a30f7-256">Обработка событий</span><span class="sxs-lookup"><span data-stu-id="a30f7-256">Event handling</span></span>

<span data-ttu-id="a30f7-257">Компоненты Razor предоставляют функции обработки событий.</span><span class="sxs-lookup"><span data-stu-id="a30f7-257">Razor components provide event handling features.</span></span> <span data-ttu-id="a30f7-258">Для атрибута элемента HTML с именем `on{event}` (например, `onclick` и `onsubmit`) со значением, определяемым делегатом, компоненты Razor рассматривают значение атрибута как обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="a30f7-258">For an HTML element attribute named `on{event}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="a30f7-259">Имя атрибута всегда отформатировано [@on {Event}](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="a30f7-259">The attribute's name is always formatted [@on{event}](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="a30f7-260">Следующий код вызывает метод `UpdateHeading`, когда в пользовательском интерфейсе выбрана кнопка:</span><span class="sxs-lookup"><span data-stu-id="a30f7-260">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="a30f7-261">Следующий код вызывает метод `CheckChanged`, когда флажок изменяется в пользовательском интерфейсе:</span><span class="sxs-lookup"><span data-stu-id="a30f7-261">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="a30f7-262">Обработчики событий также могут быть асинхронными и возвращать <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="a30f7-262">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="a30f7-263">Нет необходимости вручную вызывать `StateHasChanged()`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-263">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="a30f7-264">Исключения регистрируются при их возникновении.</span><span class="sxs-lookup"><span data-stu-id="a30f7-264">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="a30f7-265">В следующем примере `UpdateHeading` вызывается асинхронно при выборе этой кнопки:</span><span class="sxs-lookup"><span data-stu-id="a30f7-265">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

### <a name="event-argument-types"></a><span data-ttu-id="a30f7-266">Типы аргументов событий</span><span class="sxs-lookup"><span data-stu-id="a30f7-266">Event argument types</span></span>

<span data-ttu-id="a30f7-267">Для некоторых событий разрешены типы аргументов событий.</span><span class="sxs-lookup"><span data-stu-id="a30f7-267">For some events, event argument types are permitted.</span></span> <span data-ttu-id="a30f7-268">Если доступ к одному из этих типов событий не нужен, в вызове метода это не требуется.</span><span class="sxs-lookup"><span data-stu-id="a30f7-268">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="a30f7-269">Поддерживаемые `EventArgs` приведены в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="a30f7-269">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="a30f7-270">событие</span><span class="sxs-lookup"><span data-stu-id="a30f7-270">Event</span></span> | <span data-ttu-id="a30f7-271">Класс</span><span class="sxs-lookup"><span data-stu-id="a30f7-271">Class</span></span> |
| ----- | ----- |
| <span data-ttu-id="a30f7-272">буфер обмена</span><span class="sxs-lookup"><span data-stu-id="a30f7-272">Clipboard</span></span>        | `ClipboardEventArgs` |
| <span data-ttu-id="a30f7-273">Переместить</span><span class="sxs-lookup"><span data-stu-id="a30f7-273">Drag</span></span>             | <span data-ttu-id="a30f7-274">`DragEventArgs` &ndash; `DataTransfer` и `DataTransferItem` содержат перетаскиваемые данные элемента.</span><span class="sxs-lookup"><span data-stu-id="a30f7-274">`DragEventArgs` &ndash; `DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="a30f7-275">Ошибка</span><span class="sxs-lookup"><span data-stu-id="a30f7-275">Error</span></span>            | `ErrorEventArgs` |
| <span data-ttu-id="a30f7-276">Фокус</span><span class="sxs-lookup"><span data-stu-id="a30f7-276">Focus</span></span>            | <span data-ttu-id="a30f7-277">`FocusEventArgs` &ndash; не включает поддержку `relatedTarget`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-277">`FocusEventArgs` &ndash; Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="a30f7-278">Изменение`<input>`</span><span class="sxs-lookup"><span data-stu-id="a30f7-278">`<input>` change</span></span> | `ChangeEventArgs` |
| <span data-ttu-id="a30f7-279">Клавиатура</span><span class="sxs-lookup"><span data-stu-id="a30f7-279">Keyboard</span></span>         | `KeyboardEventArgs` |
| <span data-ttu-id="a30f7-280">Мышь</span><span class="sxs-lookup"><span data-stu-id="a30f7-280">Mouse</span></span>            | `MouseEventArgs` |
| <span data-ttu-id="a30f7-281">Указатель мыши</span><span class="sxs-lookup"><span data-stu-id="a30f7-281">Mouse pointer</span></span>    | `PointerEventArgs` |
| <span data-ttu-id="a30f7-282">Колесо мыши</span><span class="sxs-lookup"><span data-stu-id="a30f7-282">Mouse wheel</span></span>      | `WheelEventArgs` |
| <span data-ttu-id="a30f7-283">Ход выполнения</span><span class="sxs-lookup"><span data-stu-id="a30f7-283">Progress</span></span>         | `ProgressEventArgs` |
| <span data-ttu-id="a30f7-284">Сенсорные технологии</span><span class="sxs-lookup"><span data-stu-id="a30f7-284">Touch</span></span>            | <span data-ttu-id="a30f7-285">`TouchEventArgs` &ndash; `TouchPoint` представляет одну точку контакта на сенсорном устройстве.</span><span class="sxs-lookup"><span data-stu-id="a30f7-285">`TouchEventArgs` &ndash; `TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="a30f7-286">Сведения о свойствах и поведении событий, описанных в приведенной выше таблице, см. в разделе [классы EventArgs в источнике ссылки (ветвь ASPNET/AspNetCore Release/3.0)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="a30f7-286">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source (aspnet/AspNetCore release/3.0 branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="a30f7-287">Лямбда-выражения</span><span class="sxs-lookup"><span data-stu-id="a30f7-287">Lambda expressions</span></span>

<span data-ttu-id="a30f7-288">Лямбда-выражения также могут использоваться:</span><span class="sxs-lookup"><span data-stu-id="a30f7-288">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="a30f7-289">Часто бывает удобно закрывать дополнительные значения, например при переборе набора элементов.</span><span class="sxs-lookup"><span data-stu-id="a30f7-289">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="a30f7-290">В следующем примере создаются три кнопки, каждый из которых вызывает `UpdateHeading`, передавая аргумент события (`MouseEventArgs`) и номер кнопки (`buttonNumber`) при выборе в пользовательском интерфейсе:</span><span class="sxs-lookup"><span data-stu-id="a30f7-290">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="a30f7-291">**Не** используйте переменную цикла (`i`) в цикле `for` непосредственно в лямбда-выражении.</span><span class="sxs-lookup"><span data-stu-id="a30f7-291">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="a30f7-292">В противном случае одна и та же переменная используется всеми лямбда-выражениями, в результате чего значение `i` будет одинаковым для всех лямбда-выражений.</span><span class="sxs-lookup"><span data-stu-id="a30f7-292">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="a30f7-293">Всегда запишите свое значение в локальную переменную (`buttonNumber` в предыдущем примере), а затем используйте ее.</span><span class="sxs-lookup"><span data-stu-id="a30f7-293">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="a30f7-294">Вложенный EventCallback</span><span class="sxs-lookup"><span data-stu-id="a30f7-294">EventCallback</span></span>

<span data-ttu-id="a30f7-295">Распространенным сценарием с вложенными компонентами является желание запускать метод родительского компонента при возникновении события дочернего компонента @ no__t-0for, когда в дочернем элементе возникает событие `onclick`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-295">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="a30f7-296">Чтобы обеспечить доступ к событиям по компонентам, используйте `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-296">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="a30f7-297">Родительский компонент может назначить метод обратного вызова `EventCallback` дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="a30f7-297">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="a30f7-298">@No__t-0 в примере приложения демонстрирует настройку обработчика `onclick` кнопки на получение делегата `EventCallback` из примера `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-298">The `ChildComponent` in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="a30f7-299">@No__t-0 вводится `MouseEventArgs`, что подходит для события `onclick` на периферийном устройстве:</span><span class="sxs-lookup"><span data-stu-id="a30f7-299">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="a30f7-300">@No__t-0 устанавливает `EventCallback<T>` дочернего элемента в метод `ShowMessage`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-300">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="a30f7-301">При выборе кнопки в `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-301">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="a30f7-302">Вызывается метод `ParentComponent` `ShowMessage`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-302">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="a30f7-303">`messageText` обновляется и отображается в `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-303">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="a30f7-304">Вызов `StateHasChanged` не требуется в методе обратного вызова (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="a30f7-304">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="a30f7-305">`StateHasChanged` вызывается автоматически для повторной визуализации `ParentComponent`, как и дочерние события. компонент активируется в обработчиках событий, которые выполняются внутри дочернего элемента.</span><span class="sxs-lookup"><span data-stu-id="a30f7-305">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="a30f7-306">`EventCallback` и `EventCallback<T>` допускают использование асинхронных делегатов.</span><span class="sxs-lookup"><span data-stu-id="a30f7-306">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="a30f7-307">`EventCallback<T>` является строго типизированным и требует определенного типа аргумента.</span><span class="sxs-lookup"><span data-stu-id="a30f7-307">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="a30f7-308">`EventCallback` слабо типизирован и допускает любой тип аргумента.</span><span class="sxs-lookup"><span data-stu-id="a30f7-308">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="a30f7-309">Вызовите `EventCallback` или `EventCallback<T>` с `InvokeAsync` и ожидайте <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="a30f7-309">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="a30f7-310">Используйте `EventCallback` и `EventCallback<T>` для обработки событий и параметров компонента привязки.</span><span class="sxs-lookup"><span data-stu-id="a30f7-310">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="a30f7-311">Предпочитать строго типизированный `EventCallback<T>` на `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-311">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="a30f7-312">`EventCallback<T>` обеспечивает лучшую реакцию на ошибки для пользователей компонента.</span><span class="sxs-lookup"><span data-stu-id="a30f7-312">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="a30f7-313">Как и в случае с другими обработчиками событий пользовательского интерфейса, указание параметра события является необязательным.</span><span class="sxs-lookup"><span data-stu-id="a30f7-313">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="a30f7-314">Используйте `EventCallback`, если не передается значение обратного вызова.</span><span class="sxs-lookup"><span data-stu-id="a30f7-314">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="chained-bind"></a><span data-ttu-id="a30f7-315">Привязка к цепочке</span><span class="sxs-lookup"><span data-stu-id="a30f7-315">Chained bind</span></span>

<span data-ttu-id="a30f7-316">Распространенным сценарием является привязка привязанного к данным параметра к элементу страницы в выходных данных компонента.</span><span class="sxs-lookup"><span data-stu-id="a30f7-316">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="a30f7-317">Этот сценарий называется *связанной* привязкой, так как несколько уровней привязки происходят одновременно.</span><span class="sxs-lookup"><span data-stu-id="a30f7-317">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="a30f7-318">Связанную с цепочкой привязку нельзя реализовать с помощью синтаксиса `@bind` в элементе страницы.</span><span class="sxs-lookup"><span data-stu-id="a30f7-318">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="a30f7-319">Обработчик событий и значение должны быть указаны отдельно.</span><span class="sxs-lookup"><span data-stu-id="a30f7-319">The event handler and value must be specified separately.</span></span> <span data-ttu-id="a30f7-320">Однако родительский компонент может использовать синтаксис `@bind` с параметром компонента.</span><span class="sxs-lookup"><span data-stu-id="a30f7-320">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="a30f7-321">Следующий компонент `PasswordField` (*пассвордфиелд. Razor*):</span><span class="sxs-lookup"><span data-stu-id="a30f7-321">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="a30f7-322">Задает значение элемента `<input>` для свойства `Password`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-322">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="a30f7-323">Предоставляет изменения свойства `Password` в родительский компонент с помощью [вложенный EventCallback](#eventcallback).</span><span class="sxs-lookup"><span data-stu-id="a30f7-323">Exposes changes of the `Password` property to a parent component with an [EventCallback](#eventcallback).</span></span>

```cshtml
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool showPassword;

    [Parameter]
    public string Password { get; set; }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        showPassword = !showPassword;
    }
}
```

<span data-ttu-id="a30f7-324">Компонент `PasswordField` используется в другом компоненте:</span><span class="sxs-lookup"><span data-stu-id="a30f7-324">The `PasswordField` component is used in another component:</span></span>

```cshtml
<PasswordField @bind-Password="password" />

@code {
    private string password;
}
```

<span data-ttu-id="a30f7-325">Для выполнения проверок или перехвата ошибок в пароле в предыдущем примере:</span><span class="sxs-lookup"><span data-stu-id="a30f7-325">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="a30f7-326">Создайте резервное поле для `Password` (`password` в следующем примере кода).</span><span class="sxs-lookup"><span data-stu-id="a30f7-326">Create a backing field for `Password` (`password` in the following example code).</span></span>
* <span data-ttu-id="a30f7-327">Выполните проверки или ошибки ловушек в методе задания `Password`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-327">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="a30f7-328">В следующем примере представлена немедленная реакция пользователя, если в значении пароля используется пробел:</span><span class="sxs-lookup"><span data-stu-id="a30f7-328">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

```cshtml
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@validationMessage</span>

@code {
    private bool showPassword;
    private string password;
    private string validationMessage;

    [Parameter]
    public string Password
    {
        get { return password ?? string.Empty; }
        set
        {
            if (password != value)
            {
                if (value.Contains(' '))
                {
                    validationMessage = "Spaces not allowed!";
                }
                else
                {
                    password = value;
                    validationMessage = string.Empty;
                }
            }
        }
    }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        showPassword = !showPassword;
    }
}
```

## <a name="capture-references-to-components"></a><span data-ttu-id="a30f7-329">Запись ссылок на компоненты</span><span class="sxs-lookup"><span data-stu-id="a30f7-329">Capture references to components</span></span>

<span data-ttu-id="a30f7-330">Ссылки на компоненты предоставляют способ ссылки на экземпляр компонента, чтобы можно было выполнять команды для этого экземпляра, например `Show` или `Reset`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-330">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="a30f7-331">Чтобы записать ссылку на компонент, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="a30f7-331">To capture a component reference:</span></span>

* <span data-ttu-id="a30f7-332">Добавьте атрибут [@ref](xref:mvc/views/razor#ref) к дочернему компоненту.</span><span class="sxs-lookup"><span data-stu-id="a30f7-332">Add an [@ref](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="a30f7-333">Определите поле с тем же типом, что и у дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="a30f7-333">Define a field with the same type as the child component.</span></span>

```cshtml
<MyLoginDialog @ref="loginDialog" ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="a30f7-334">При подготовке компонента к просмотру поле `loginDialog` заполняется экземпляром дочернего компонента `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-334">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="a30f7-335">Затем можно вызывать методы .NET в экземпляре компонента.</span><span class="sxs-lookup"><span data-stu-id="a30f7-335">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a30f7-336">Переменная `loginDialog` заполняется только после подготовки компонента, а ее выходные данные включают элемент `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-336">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="a30f7-337">До этого момента нет никаких ссылок на.</span><span class="sxs-lookup"><span data-stu-id="a30f7-337">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="a30f7-338">Чтобы управлять ссылками на компоненты после завершения подготовки компонента к просмотру, используйте методы `OnAfterRenderAsync` или `OnAfterRender`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-338">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<span data-ttu-id="a30f7-339">При захвате ссылок на компоненты используется аналогичный синтаксис для [записи ссылок на элементы](xref:blazor/javascript-interop#capture-references-to-elements), но это не функция [взаимодействия JavaScript](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="a30f7-339">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="a30f7-340">Ссылки на компоненты не передаются в код JavaScript @ no__t-0they're используются только в коде .NET.</span><span class="sxs-lookup"><span data-stu-id="a30f7-340">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="a30f7-341">**Не** используйте ссылки на компоненты для изменения состояния дочерних компонентов.</span><span class="sxs-lookup"><span data-stu-id="a30f7-341">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="a30f7-342">Вместо этого используйте обычные декларативные параметры для передачи данных дочерним компонентам.</span><span class="sxs-lookup"><span data-stu-id="a30f7-342">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="a30f7-343">Использование обычных декларативных параметров приводит к тому, что дочерние компоненты автоматически визуализируются в нужное время.</span><span class="sxs-lookup"><span data-stu-id="a30f7-343">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="a30f7-344">Вызывать методы компонента извне для обновления состояния</span><span class="sxs-lookup"><span data-stu-id="a30f7-344">Invoke component methods externally to update state</span></span>

<span data-ttu-id="a30f7-345">Блазор использует `SynchronizationContext` для принудительного применения одного логического потока выполнения.</span><span class="sxs-lookup"><span data-stu-id="a30f7-345">Blazor uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="a30f7-346">Методы жизненного цикла компонента и любые обратные вызовы событий, вызванные Блазор, выполняются на этом `SynchronizationContext`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-346">A component's lifecycle methods and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="a30f7-347">В случае, если компонент должен быть обновлен на основе внешнего события, такого как таймер или другие уведомления, используйте метод `InvokeAsync`, который будет отправляться в Блазор `SynchronizationContext`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-347">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="a30f7-348">Например, рассмотрим *службу уведомления* , которая может уведомлять любой компонент, выполняющий прослушивание, в обновленном состоянии:</span><span class="sxs-lookup"><span data-stu-id="a30f7-348">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

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

<span data-ttu-id="a30f7-349">Использование `NotifierService` для обновления компонента:</span><span class="sxs-lookup"><span data-stu-id="a30f7-349">Usage of the `NotifierService` to update a component:</span></span>

```cshtml
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @lastNotification.key = @lastNotification.value</p>

@code {
    private (string key, int value) lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

<span data-ttu-id="a30f7-350">В предыдущем примере `NotifierService` вызывает метод `OnNotify` компонента за пределами Блазор `SynchronizationContext`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-350">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="a30f7-351">`InvokeAsync` используется для переключения на правильный контекст и постановку в очередь визуализации.</span><span class="sxs-lookup"><span data-stu-id="a30f7-351">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="a30f7-352">Использование \@key для управления сохранением элементов и компонентов</span><span class="sxs-lookup"><span data-stu-id="a30f7-352">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="a30f7-353">При последующем изменении списка элементов или компонентов, а также элементов или компонентов алгоритм сравнения Блазор должен решить, какие из предыдущих элементов или компонентов могут быть сохранены и как объекты модели должны сопоставляться с ними.</span><span class="sxs-lookup"><span data-stu-id="a30f7-353">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="a30f7-354">Обычно этот процесс выполняется автоматически и его можно игнорировать, но в некоторых случаях может потребоваться управление процессом.</span><span class="sxs-lookup"><span data-stu-id="a30f7-354">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="a30f7-355">Рассмотрим следующий пример.</span><span class="sxs-lookup"><span data-stu-id="a30f7-355">Consider the following example:</span></span>

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

<span data-ttu-id="a30f7-356">Содержимое коллекции `People` может изменяться при вставке, удалении или повторном упорядочении записей.</span><span class="sxs-lookup"><span data-stu-id="a30f7-356">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="a30f7-357">Когда компонент перерисовывается, компонент `<DetailsEditor>` может измениться на получение различных значений параметров `Details`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-357">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="a30f7-358">Это может привести к более сложной визуализации, чем ожидалось.</span><span class="sxs-lookup"><span data-stu-id="a30f7-358">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="a30f7-359">В некоторых случаях перерисовка может привести к появлению видимых различий в поведении, таких как потеря фокуса элементов.</span><span class="sxs-lookup"><span data-stu-id="a30f7-359">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="a30f7-360">Процесс сопоставления можно контролировать с помощью атрибута директивы `@key`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-360">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="a30f7-361">`@key` приводит к тому, что алгоритм сравнения гарантирует сохранение элементов или компонентов на основе значения ключа:</span><span class="sxs-lookup"><span data-stu-id="a30f7-361">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

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

<span data-ttu-id="a30f7-362">При изменении коллекции `People` алгоритм сравнения удерживает связь между экземплярами `<DetailsEditor>` и экземплярами `person`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-362">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="a30f7-363">Если `Person` удаляется из списка `People`, из пользовательского интерфейса удаляется только соответствующий экземпляр `<DetailsEditor>`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-363">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="a30f7-364">Другие экземпляры остаются без изменений.</span><span class="sxs-lookup"><span data-stu-id="a30f7-364">Other instances are left unchanged.</span></span>
* <span data-ttu-id="a30f7-365">Если в некоторой позиции в списке вставляется `Person`, то в соответствующей позиции вставляется один новый экземпляр `<DetailsEditor>`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-365">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="a30f7-366">Другие экземпляры остаются без изменений.</span><span class="sxs-lookup"><span data-stu-id="a30f7-366">Other instances are left unchanged.</span></span>
* <span data-ttu-id="a30f7-367">При повторном упорядочении записей `Person` соответствующие экземпляры `<DetailsEditor>` сохраняются и повторно упорядочиваются в пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="a30f7-367">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="a30f7-368">В некоторых сценариях использование `@key` уменьшает сложность повторного отображения и позволяет избежать потенциальных проблем с элементами, изменяющимися с отслеживанием состояния DOM, например положением фокуса.</span><span class="sxs-lookup"><span data-stu-id="a30f7-368">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a30f7-369">Ключи являются локальными для каждого элемента или компонента контейнера.</span><span class="sxs-lookup"><span data-stu-id="a30f7-369">Keys are local to each container element or component.</span></span> <span data-ttu-id="a30f7-370">Ключи не сравниваются глобально по всему документу.</span><span class="sxs-lookup"><span data-stu-id="a30f7-370">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="a30f7-371">Когда следует использовать @no__t 0key</span><span class="sxs-lookup"><span data-stu-id="a30f7-371">When to use \@key</span></span>

<span data-ttu-id="a30f7-372">Как правило, имеет смысл использовать `@key` при подготовке списка (например, в блоке `@foreach`), а подходящее значение — для определения `@key`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-372">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="a30f7-373">Можно также использовать `@key`, чтобы предотвратить сохранение Блазор элемента или поддерева компонента при изменении объекта.</span><span class="sxs-lookup"><span data-stu-id="a30f7-373">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="a30f7-374">Если `@currentPerson` меняется, директива атрибута `@key` заставляет Блазор отбросить весь `<div>` и его потомков и перестроить поддерево в пользовательском интерфейсе с помощью новых элементов и компонентов.</span><span class="sxs-lookup"><span data-stu-id="a30f7-374">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="a30f7-375">Это может быть полезно, если необходимо гарантировать, что при изменении `@currentPerson` состояние пользовательского интерфейса не сохраняется.</span><span class="sxs-lookup"><span data-stu-id="a30f7-375">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="a30f7-376">Когда не следует использовать \@key</span><span class="sxs-lookup"><span data-stu-id="a30f7-376">When not to use \@key</span></span>

<span data-ttu-id="a30f7-377">При использовании сравнения с `@key` возникают затраты на производительность.</span><span class="sxs-lookup"><span data-stu-id="a30f7-377">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="a30f7-378">Затраты на производительность не велики, но при управлении правилами сохранения элементов или компонентов вместо них следует указывать только `@key`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-378">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="a30f7-379">Даже если `@key` не используется, Блазор сохраняет дочерние элементы и экземпляры компонентов насколько это возможно.</span><span class="sxs-lookup"><span data-stu-id="a30f7-379">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="a30f7-380">Единственным преимуществом использования `@key` является управление *тем, как* экземпляры модели сопоставляются с сохраненными экземплярами компонента, а не алгоритмом сравнения, который выбирает сопоставление.</span><span class="sxs-lookup"><span data-stu-id="a30f7-380">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="a30f7-381">Какие значения следует использовать для @no__t 0key</span><span class="sxs-lookup"><span data-stu-id="a30f7-381">What values to use for \@key</span></span>

<span data-ttu-id="a30f7-382">Как правило, имеет смысл указать один из следующих видов значений для `@key`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-382">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="a30f7-383">Экземпляры объектов Model (например, экземпляр `Person`, как в предыдущем примере).</span><span class="sxs-lookup"><span data-stu-id="a30f7-383">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="a30f7-384">Это гарантирует сохранение на основе равенства ссылок на объекты.</span><span class="sxs-lookup"><span data-stu-id="a30f7-384">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="a30f7-385">Уникальные идентификаторы (например, значения первичного ключа типа `int`, `string` или `Guid`).</span><span class="sxs-lookup"><span data-stu-id="a30f7-385">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="a30f7-386">Убедитесь, что значения, используемые для `@key`, не конфликтуют.</span><span class="sxs-lookup"><span data-stu-id="a30f7-386">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="a30f7-387">Если конфликтные значения обнаруживаются в одном родительском элементе, Блазор создает исключение, поскольку оно не может детерминированно сопоставлять старые элементы или компоненты с новыми элементами или компонентами.</span><span class="sxs-lookup"><span data-stu-id="a30f7-387">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="a30f7-388">Используйте только уникальные значения, такие как экземпляры объекта или значения первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="a30f7-388">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="a30f7-389">Методы жизненного цикла</span><span class="sxs-lookup"><span data-stu-id="a30f7-389">Lifecycle methods</span></span>

<span data-ttu-id="a30f7-390">`OnInitializedAsync` и `OnInitialized` выполнить код для инициализации компонента.</span><span class="sxs-lookup"><span data-stu-id="a30f7-390">`OnInitializedAsync` and `OnInitialized` execute code to initialize the component.</span></span> <span data-ttu-id="a30f7-391">Для выполнения асинхронной операции используйте `OnInitializedAsync` и ключевое слово `await` для операции:</span><span class="sxs-lookup"><span data-stu-id="a30f7-391">To perform an asynchronous operation, use `OnInitializedAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

<span data-ttu-id="a30f7-392">Для синхронной операции используйте `OnInitialized`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-392">For a synchronous operation, use `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="a30f7-393">`OnParametersSetAsync` и `OnParametersSet` вызываются, когда компонент получил параметры от родительского объекта и значения присваиваются свойствам.</span><span class="sxs-lookup"><span data-stu-id="a30f7-393">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="a30f7-394">Эти методы выполняются после инициализации компонента и при каждом отображении компонента:</span><span class="sxs-lookup"><span data-stu-id="a30f7-394">These methods are executed after component initialization and each time the component is rendered:</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

```csharp
protected override void OnParametersSet()
{
    ...
}
```

<span data-ttu-id="a30f7-395">`OnAfterRenderAsync` и `OnAfterRender` вызываются после завершения подготовки компонента к просмотру.</span><span class="sxs-lookup"><span data-stu-id="a30f7-395">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="a30f7-396">В этот момент заполнены ссылки на элементы и компоненты.</span><span class="sxs-lookup"><span data-stu-id="a30f7-396">Element and component references are populated at this point.</span></span> <span data-ttu-id="a30f7-397">Используйте этот этап для выполнения дополнительных шагов инициализации с помощью готового к просмотру содержимого, например для активации сторонних библиотек JavaScript, которые работают с визуализированными элементами DOM.</span><span class="sxs-lookup"><span data-stu-id="a30f7-397">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="a30f7-398">`OnAfterRender` *не вызывается при предварительной отрисовке на сервере.*</span><span class="sxs-lookup"><span data-stu-id="a30f7-398">`OnAfterRender` *is not called when prerendering on the server.*</span></span>

<span data-ttu-id="a30f7-399">Параметр `firstRender` для `OnAfterRenderAsync` и `OnAfterRender`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-399">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender` is:</span></span>

* <span data-ttu-id="a30f7-400">Задайте значение `true` при первом вызове экземпляра компонента.</span><span class="sxs-lookup"><span data-stu-id="a30f7-400">Set to `true` the first time that the component instance is invoked.</span></span>
* <span data-ttu-id="a30f7-401">Гарантирует, что работа по инициализации выполняется только один раз.</span><span class="sxs-lookup"><span data-stu-id="a30f7-401">Ensures that initialization work is only performed once.</span></span>

```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

### <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="a30f7-402">Обработка незавершенных асинхронных действий при отрисовке</span><span class="sxs-lookup"><span data-stu-id="a30f7-402">Handle incomplete async actions at render</span></span>

<span data-ttu-id="a30f7-403">Асинхронные действия, выполняемые в событиях жизненного цикла, могут не завершиться до подготовки компонента к просмотру.</span><span class="sxs-lookup"><span data-stu-id="a30f7-403">Asynchronous actions performed in lifecycle events may not have completed before the component is rendered.</span></span> <span data-ttu-id="a30f7-404">Объекты могут быть `null` или не заполнены данными во время выполнения метода жизненного цикла.</span><span class="sxs-lookup"><span data-stu-id="a30f7-404">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="a30f7-405">Предоставьте логику отрисовки для подтверждения инициализации объектов.</span><span class="sxs-lookup"><span data-stu-id="a30f7-405">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="a30f7-406">Отрисовывает элементы пользовательского интерфейса заполнителя (например, сообщение загрузки), а объекты — `null`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-406">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="a30f7-407">В компоненте `FetchData` шаблонов Блазор `OnInitializedAsync` переопределяется в асичронаусли получения данных прогноза (`forecasts`).</span><span class="sxs-lookup"><span data-stu-id="a30f7-407">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="a30f7-408">Если `forecasts` равно `null`, пользователю выводится сообщение о загрузке.</span><span class="sxs-lookup"><span data-stu-id="a30f7-408">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="a30f7-409">После того, как `Task` возвращено `OnInitializedAsync`, компонент перерисовывается с обновленным состоянием.</span><span class="sxs-lookup"><span data-stu-id="a30f7-409">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="a30f7-410">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="a30f7-410">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a><span data-ttu-id="a30f7-411">Выполнить код до установки параметров</span><span class="sxs-lookup"><span data-stu-id="a30f7-411">Execute code before parameters are set</span></span>

<span data-ttu-id="a30f7-412">`SetParameters` можно переопределить для выполнения кода перед установкой параметров:</span><span class="sxs-lookup"><span data-stu-id="a30f7-412">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterView parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="a30f7-413">Если `base.SetParameters` не вызывается, Пользовательский код может интерпретировать значение входящих параметров любым необходимым образом.</span><span class="sxs-lookup"><span data-stu-id="a30f7-413">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="a30f7-414">Например, входящие параметры не обязательно должны быть назначены свойствам класса.</span><span class="sxs-lookup"><span data-stu-id="a30f7-414">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

### <a name="suppress-refreshing-of-the-ui"></a><span data-ttu-id="a30f7-415">Отключить обновление пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="a30f7-415">Suppress refreshing of the UI</span></span>

<span data-ttu-id="a30f7-416">`ShouldRender` можно переопределить, чтобы отключить обновление пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="a30f7-416">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="a30f7-417">Если реализация возвращает `true`, Пользовательский интерфейс обновляется.</span><span class="sxs-lookup"><span data-stu-id="a30f7-417">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="a30f7-418">Даже при переопределении `ShouldRender` компонент всегда первоначально готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="a30f7-418">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="a30f7-419">Освобождение компонентов с помощью IDisposable</span><span class="sxs-lookup"><span data-stu-id="a30f7-419">Component disposal with IDisposable</span></span>

<span data-ttu-id="a30f7-420">Если компонент реализует <xref:System.IDisposable>, [метод Dispose](/dotnet/standard/garbage-collection/implementing-dispose) вызывается при удалении компонента из пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="a30f7-420">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="a30f7-421">Следующий компонент использует `@implements IDisposable` и метод `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-421">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```csharp
@using System
@implements IDisposable

...

@code {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a><span data-ttu-id="a30f7-422">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="a30f7-422">Routing</span></span>

<span data-ttu-id="a30f7-423">Маршрутизация в Блазор достигается путем предоставления шаблона маршрута для каждого доступного компонента в приложении.</span><span class="sxs-lookup"><span data-stu-id="a30f7-423">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="a30f7-424">При компиляции файла Razor с директивой `@page` создаваемому классу присваивается значение <xref:Microsoft.AspNetCore.Mvc.RouteAttribute>, задающее шаблон маршрута.</span><span class="sxs-lookup"><span data-stu-id="a30f7-424">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="a30f7-425">Во время выполнения маршрутизатор ищет классы компонентов с `RouteAttribute` и отображает, какой из компонентов имеет шаблон маршрута, соответствующий запрошенному URL-адресу.</span><span class="sxs-lookup"><span data-stu-id="a30f7-425">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="a30f7-426">К компоненту можно применить несколько шаблонов маршрутов.</span><span class="sxs-lookup"><span data-stu-id="a30f7-426">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="a30f7-427">Следующий компонент отвечает на запросы для `/BlazorRoute` и `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-427">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="a30f7-428">Параметры маршрута</span><span class="sxs-lookup"><span data-stu-id="a30f7-428">Route parameters</span></span>

<span data-ttu-id="a30f7-429">Компоненты могут принимать параметры маршрута из шаблона маршрута, указанного в директиве `@page`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-429">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="a30f7-430">Маршрутизатор использует параметры маршрута для заполнения соответствующих параметров компонента.</span><span class="sxs-lookup"><span data-stu-id="a30f7-430">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="a30f7-431">*Компонент параметра маршрута*:</span><span class="sxs-lookup"><span data-stu-id="a30f7-431">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="a30f7-432">Необязательные параметры не поддерживаются, поэтому в приведенном выше примере применяются две директивы `@page`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-432">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="a30f7-433">Первый позволяет переходить к компоненту без параметра.</span><span class="sxs-lookup"><span data-stu-id="a30f7-433">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="a30f7-434">Вторая директива `@page` принимает параметр маршрута `{text}` и присваивает значение свойству `Text`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-434">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="a30f7-435">Наследование базового класса для взаимодействия с кодом программной части</span><span class="sxs-lookup"><span data-stu-id="a30f7-435">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="a30f7-436">Файлы компонентов сочетают разметку HTML C# и обрабатывают код в одном файле.</span><span class="sxs-lookup"><span data-stu-id="a30f7-436">Component files mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="a30f7-437">Директива `@inherits` может использоваться для предоставления Блазор приложений с помощью «кода программной части», который отделяет разметку компонентов от кода обработки.</span><span class="sxs-lookup"><span data-stu-id="a30f7-437">The `@inherits` directive can be used to provide Blazor apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="a30f7-438">[Пример приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) показывает, как компонент может наследовать базовый класс `BlazorRocksBase` для предоставления свойств и методов компонента.</span><span class="sxs-lookup"><span data-stu-id="a30f7-438">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="a30f7-439">*Pages/блазорроккс. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a30f7-439">*Pages/BlazorRocks.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

<span data-ttu-id="a30f7-440">*BlazorRocksBase.CS*:</span><span class="sxs-lookup"><span data-stu-id="a30f7-440">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="a30f7-441">Базовый класс должен быть производным от `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-441">The base class should derive from `ComponentBase`.</span></span>

## <a name="import-components"></a><span data-ttu-id="a30f7-442">Импорт компонентов</span><span class="sxs-lookup"><span data-stu-id="a30f7-442">Import components</span></span>

<span data-ttu-id="a30f7-443">Пространство имен компонента, созданного с помощью Razor, основано на (в порядке приоритета):</span><span class="sxs-lookup"><span data-stu-id="a30f7-443">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="a30f7-444">[@namespace](xref:mvc/views/razor#namespace) обозначение в разметке файла Razor ( *. Razor*) (`@namespace BlazorSample.MyNamespace`).</span><span class="sxs-lookup"><span data-stu-id="a30f7-444">[@namespace](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="a30f7-445">@No__t проекта-0 в файле проекта (`<RootNamespace>BlazorSample</RootNamespace>`).</span><span class="sxs-lookup"><span data-stu-id="a30f7-445">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="a30f7-446">Имя проекта, полученное из имени файла проекта (*CSPROJ*), и путь из корневого каталога проекта к компоненту.</span><span class="sxs-lookup"><span data-stu-id="a30f7-446">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="a30f7-447">Например, платформа разрешает *{root проекта}/Пажес/индекс.Разор* (*блазорсампле. csproj*) к пространству имен `BlazorSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-447">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="a30f7-448">Компоненты следуют C# правилам привязки имен.</span><span class="sxs-lookup"><span data-stu-id="a30f7-448">Components follow C# name binding rules.</span></span> <span data-ttu-id="a30f7-449">Для компонента `Index` в этом примере компоненты в области являются всеми компонентами:</span><span class="sxs-lookup"><span data-stu-id="a30f7-449">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="a30f7-450">В той же папке *страницы*.</span><span class="sxs-lookup"><span data-stu-id="a30f7-450">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="a30f7-451">Компоненты в корне проекта, которые не задают явно другое пространство имен.</span><span class="sxs-lookup"><span data-stu-id="a30f7-451">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="a30f7-452">Компоненты, определенные в другом пространстве имен, передаются в область с помощью директивы Razor [@using](xref:mvc/views/razor#using) .</span><span class="sxs-lookup"><span data-stu-id="a30f7-452">Components defined in a different namespace are brought into scope using Razor's [@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="a30f7-453">Если другой компонент, `NavMenu.razor`, существует в папке *блазорсампле/Shared/* Folder, компонент можно использовать в `Index.razor` со следующей инструкцией `@using`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-453">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="a30f7-454">На компоненты также можно ссылаться с помощью полных имен, для которых не требуется директива [@using](xref:mvc/views/razor#using) :</span><span class="sxs-lookup"><span data-stu-id="a30f7-454">Components can also be referenced using their fully qualified names, which doesn't require the [@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="a30f7-455">Квалификация `global::` не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="a30f7-455">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="a30f7-456">Импорт компонентов с псевдонимами `using` (например, `@using Foo = Bar`) не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="a30f7-456">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="a30f7-457">Частично определенные имена не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="a30f7-457">Partially qualified names aren't supported.</span></span> <span data-ttu-id="a30f7-458">Например, добавление `@using BlazorSample` и ссылка `NavMenu.razor` с помощью `<Shared.NavMenu></Shared.NavMenu>` не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="a30f7-458">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="a30f7-459">Атрибуты условного HTML-элемента</span><span class="sxs-lookup"><span data-stu-id="a30f7-459">Conditional HTML element attributes</span></span>

<span data-ttu-id="a30f7-460">Атрибуты HTML-элемента условно подготавливаются в зависимости от значения .NET.</span><span class="sxs-lookup"><span data-stu-id="a30f7-460">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="a30f7-461">Если значение равно `false` или `null`, то атрибут не отображается.</span><span class="sxs-lookup"><span data-stu-id="a30f7-461">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="a30f7-462">Если значение равно `true`, то атрибут отображается в виде сворачивания.</span><span class="sxs-lookup"><span data-stu-id="a30f7-462">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="a30f7-463">В следующем примере `IsCompleted` определяет, отображается ли `checked` в разметке элемента:</span><span class="sxs-lookup"><span data-stu-id="a30f7-463">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="a30f7-464">Если `IsCompleted` равно `true`, флажок отображается следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a30f7-464">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="a30f7-465">Если `IsCompleted` равно `false`, флажок отображается следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a30f7-465">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="a30f7-466">Дополнительные сведения см. в разделе <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="a30f7-466">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="a30f7-467">Некоторые атрибуты HTML, такие как [ARIA-Pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), не работают должным образом, если тип .net — `bool`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-467">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="a30f7-468">В этих случаях используйте тип `string` вместо `bool`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-468">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="a30f7-469">Необработанный HTML</span><span class="sxs-lookup"><span data-stu-id="a30f7-469">Raw HTML</span></span>

<span data-ttu-id="a30f7-470">Строки обычно подготавливаются с помощью текстовых узлов DOM, что означает, что любая разметка, которую они могут содержать, игнорируется и обрабатывается как литеральный текст.</span><span class="sxs-lookup"><span data-stu-id="a30f7-470">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="a30f7-471">Для отрисовки необработанного HTML-кода заключите содержимое HTML в значение `MarkupString`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-471">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="a30f7-472">Значение анализируется как HTML или SVG и вставляется в модель DOM.</span><span class="sxs-lookup"><span data-stu-id="a30f7-472">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="a30f7-473">Визуализация необработанного HTML-кода, созданного из любого ненадежного источника, является **угрозой безопасности** , и ее следует избегать.</span><span class="sxs-lookup"><span data-stu-id="a30f7-473">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="a30f7-474">В следующем примере показано использование типа `MarkupString` для добавления блока статического HTML-содержимого в отображаемые выходные данные компонента.</span><span class="sxs-lookup"><span data-stu-id="a30f7-474">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="a30f7-475">Шаблонные компоненты</span><span class="sxs-lookup"><span data-stu-id="a30f7-475">Templated components</span></span>

<span data-ttu-id="a30f7-476">Шаблонные компоненты — это компоненты, принимающие один или несколько шаблонов пользовательского интерфейса в качестве параметров, которые затем можно использовать как часть логики отрисовки компонента.</span><span class="sxs-lookup"><span data-stu-id="a30f7-476">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="a30f7-477">Шаблонные компоненты позволяют создавать более высокие компоненты более высокого уровня, чем обычные компоненты.</span><span class="sxs-lookup"><span data-stu-id="a30f7-477">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="a30f7-478">Ниже приведены несколько примеров.</span><span class="sxs-lookup"><span data-stu-id="a30f7-478">A couple of examples include:</span></span>

* <span data-ttu-id="a30f7-479">Компонент таблицы, позволяющий пользователю указывать шаблоны для заголовков, строк и нижних колонтитулов таблицы.</span><span class="sxs-lookup"><span data-stu-id="a30f7-479">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="a30f7-480">Компонент списка, позволяющий пользователю указать шаблон для отрисовки элементов в списке.</span><span class="sxs-lookup"><span data-stu-id="a30f7-480">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="a30f7-481">Параметры шаблона</span><span class="sxs-lookup"><span data-stu-id="a30f7-481">Template parameters</span></span>

<span data-ttu-id="a30f7-482">Шаблонный компонент определяется путем указания одного или нескольких параметров компонента типа `RenderFragment` или `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-482">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="a30f7-483">Фрагмент отрисовки представляет сегмент пользовательского интерфейса для отрисовки.</span><span class="sxs-lookup"><span data-stu-id="a30f7-483">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="a30f7-484">`RenderFragment<T>` принимает параметр типа, который может быть указан при вызове фрагмента прорисовки.</span><span class="sxs-lookup"><span data-stu-id="a30f7-484">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="a30f7-485">компонент `TableTemplate`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-485">`TableTemplate` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

<span data-ttu-id="a30f7-486">При использовании компонент-шаблона можно указать параметры шаблона с помощью дочерних элементов, совпадающих с именами параметров (`TableHeader` и `RowTemplate` в следующем примере):</span><span class="sxs-lookup"><span data-stu-id="a30f7-486">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```cshtml
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="template-context-parameters"></a><span data-ttu-id="a30f7-487">Параметры контекста шаблона</span><span class="sxs-lookup"><span data-stu-id="a30f7-487">Template context parameters</span></span>

<span data-ttu-id="a30f7-488">Аргументы компонента типа `RenderFragment<T>`, переданные как элементы, имеют неявный параметр с именем `context` (например, из предыдущего примера кода, `@context.PetId`), но можно изменить имя параметра с помощью атрибута `Context` в дочернем элементе.</span><span class="sxs-lookup"><span data-stu-id="a30f7-488">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="a30f7-489">В следующем примере атрибуту `Context` элемента `RowTemplate` задается параметр `pet`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-489">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```cshtml
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

<span data-ttu-id="a30f7-490">Кроме того, можно указать атрибут `Context` в элементе Component.</span><span class="sxs-lookup"><span data-stu-id="a30f7-490">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="a30f7-491">Указанный атрибут `Context` применяется ко всем указанным параметрам шаблона.</span><span class="sxs-lookup"><span data-stu-id="a30f7-491">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="a30f7-492">Это может быть полезно, если необходимо указать имя параметра содержимого для неявного дочернего содержимого (без обертывания дочернего элемента).</span><span class="sxs-lookup"><span data-stu-id="a30f7-492">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="a30f7-493">В следующем примере атрибут `Context` отображается в элементе `TableTemplate` и применяется ко всем параметрам шаблона:</span><span class="sxs-lookup"><span data-stu-id="a30f7-493">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```cshtml
<TableTemplate Items="pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="generic-typed-components"></a><span data-ttu-id="a30f7-494">Универсальные типы компонентов</span><span class="sxs-lookup"><span data-stu-id="a30f7-494">Generic-typed components</span></span>

<span data-ttu-id="a30f7-495">Шаблонные компоненты часто вводятся в универсальном виде.</span><span class="sxs-lookup"><span data-stu-id="a30f7-495">Templated components are often generically typed.</span></span> <span data-ttu-id="a30f7-496">Например, универсальный компонент `ListViewTemplate` можно использовать для отображения значений `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-496">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="a30f7-497">Чтобы определить универсальный компонент, используйте директиву [@typeparam](xref:mvc/views/razor#typeparam) для указания параметров типа:</span><span class="sxs-lookup"><span data-stu-id="a30f7-497">To define a generic component, use the [@typeparam](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

<span data-ttu-id="a30f7-498">При использовании компонентов универсальной типизации параметр типа выводится, если это возможно:</span><span class="sxs-lookup"><span data-stu-id="a30f7-498">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="a30f7-499">В противном случае параметр типа необходимо явно указать с помощью атрибута, совпадающего с именем параметра типа.</span><span class="sxs-lookup"><span data-stu-id="a30f7-499">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="a30f7-500">В следующем примере `TItem="Pet"` указывает тип:</span><span class="sxs-lookup"><span data-stu-id="a30f7-500">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="a30f7-501">Каскадные значения и параметры</span><span class="sxs-lookup"><span data-stu-id="a30f7-501">Cascading values and parameters</span></span>

<span data-ttu-id="a30f7-502">В некоторых сценариях неудобно передавать данные из компонента-предка в дочерний компонент с помощью [параметров компонента](#component-parameters), особенно при наличии нескольких слоев компонентов.</span><span class="sxs-lookup"><span data-stu-id="a30f7-502">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="a30f7-503">Каскадные значения и параметры позволяют решить эту проблему, предоставляя удобный способ, позволяющий компоненту-предку предоставить значение всем его дочерним компонентам.</span><span class="sxs-lookup"><span data-stu-id="a30f7-503">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="a30f7-504">Каскадные значения и параметры также предоставляют подход для координации компонентов.</span><span class="sxs-lookup"><span data-stu-id="a30f7-504">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="a30f7-505">Пример темы</span><span class="sxs-lookup"><span data-stu-id="a30f7-505">Theme example</span></span>

<span data-ttu-id="a30f7-506">В следующем примере из примера приложения класс `ThemeInfo` указывает сведения о теме для перетекания иерархии компонентов таким образом, чтобы все кнопки в пределах данной части приложения совместно совпадали с тем же стилем.</span><span class="sxs-lookup"><span data-stu-id="a30f7-506">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="a30f7-507">*Уисемеклассес/themeinfo указывает расположение. CS*:</span><span class="sxs-lookup"><span data-stu-id="a30f7-507">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="a30f7-508">Компонент-предок может предоставлять каскадное значение с помощью компонента каскадного значения.</span><span class="sxs-lookup"><span data-stu-id="a30f7-508">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="a30f7-509">Компонент `CascadingValue` заключает в оболочку поддерево иерархии компонентов и предоставляет одно значение всем компонентам в этом поддереве.</span><span class="sxs-lookup"><span data-stu-id="a30f7-509">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="a30f7-510">Например, в примере приложения указываются сведения о теме (`ThemeInfo`) в одном из макетов приложения в виде каскадного параметра для всех компонентов, составляющих тело макета свойства `@Body`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-510">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="a30f7-511">`ButtonClass` присвоено значение `btn-success` в компоненте макета.</span><span class="sxs-lookup"><span data-stu-id="a30f7-511">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="a30f7-512">Любой дочерний компонент может использовать это свойство через каскадный объект `ThemeInfo`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-512">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="a30f7-513">компонент `CascadingValuesParametersLayout`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-513">`CascadingValuesParametersLayout` component:</span></span>

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="a30f7-514">Чтобы использовать каскадные значения, компоненты объявляют каскадные параметры с помощью атрибута `[CascadingParameter]`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-514">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="a30f7-515">Каскадные значения привязываются к каскадным параметрам по типу.</span><span class="sxs-lookup"><span data-stu-id="a30f7-515">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="a30f7-516">В примере приложения компонент `CascadingValuesParametersTheme` привязывает каскадное значение `ThemeInfo` к каскадному параметру.</span><span class="sxs-lookup"><span data-stu-id="a30f7-516">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="a30f7-517">Параметр используется для задания класса CSS для одной из кнопок, отображаемых компонентом.</span><span class="sxs-lookup"><span data-stu-id="a30f7-517">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="a30f7-518">компонент `CascadingValuesParametersTheme`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-518">`CascadingValuesParametersTheme` component:</span></span>

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

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
    private int currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

<span data-ttu-id="a30f7-519">Чтобы вычислить несколько значений одного типа в одном поддереве, укажите уникальную строку `Name` для каждого компонента `CascadingValue` и соответствующего `CascadingParameter`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-519">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="a30f7-520">В следующем примере два компонента `CascadingValue` разворачивают разные экземпляры `MyCascadingType` по имени:</span><span class="sxs-lookup"><span data-stu-id="a30f7-520">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

```cshtml
<CascadingValue Value=@ParentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType ParentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

<span data-ttu-id="a30f7-521">В компоненте-потомках каскадные параметры получают значения из соответствующих каскадных значений в компоненте-предке по имени:</span><span class="sxs-lookup"><span data-stu-id="a30f7-521">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```cshtml
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="a30f7-522">Пример Табсет</span><span class="sxs-lookup"><span data-stu-id="a30f7-522">TabSet example</span></span>

<span data-ttu-id="a30f7-523">Каскадные параметры также позволяют компонентам работать в иерархии компонентов.</span><span class="sxs-lookup"><span data-stu-id="a30f7-523">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="a30f7-524">Например, рассмотрим следующий пример *табсет* в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="a30f7-524">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="a30f7-525">В примере приложения имеется интерфейс `ITab`, который реализуется вкладками:</span><span class="sxs-lookup"><span data-stu-id="a30f7-525">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="a30f7-526">Компонент `CascadingValuesParametersTabSet` использует компонент `TabSet`, который содержит несколько компонентов `Tab`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-526">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="a30f7-527">Дочерние компоненты `Tab` не передаются в качестве параметров в `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-527">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="a30f7-528">Вместо этого дочерние компоненты `Tab` являются частью дочернего содержимого `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-528">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="a30f7-529">Однако `TabSet` по-прежнему необходимо узнать о каждом компоненте `Tab`, чтобы он мог отобразить заголовки и активную вкладку. Чтобы обеспечить эту координацию, не требуя дополнительного кода, компонент `TabSet` *может предоставить себя как каскадное значение* , которое затем выбирается дочерними компонентами `Tab`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-529">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="a30f7-530">компонент `TabSet`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-530">`TabSet` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

<span data-ttu-id="a30f7-531">Дочерние компоненты `Tab` захватывают содержащий `TabSet` как каскадный параметр, поэтому компоненты `Tab` добавляют себя в `TabSet` и координирует, какая вкладка активна.</span><span class="sxs-lookup"><span data-stu-id="a30f7-531">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="a30f7-532">компонент `Tab`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-532">`Tab` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="a30f7-533">Шаблоны Razor</span><span class="sxs-lookup"><span data-stu-id="a30f7-533">Razor templates</span></span>

<span data-ttu-id="a30f7-534">Фрагменты визуализации можно определить с помощью синтаксиса шаблона Razor.</span><span class="sxs-lookup"><span data-stu-id="a30f7-534">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="a30f7-535">Шаблоны Razor позволяют определить фрагмент пользовательского интерфейса и принять следующий формат:</span><span class="sxs-lookup"><span data-stu-id="a30f7-535">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="a30f7-536">В следующем примере показано, как указать значения `RenderFragment` и `RenderFragment<T>` и визуализировать шаблоны непосредственно в компоненте.</span><span class="sxs-lookup"><span data-stu-id="a30f7-536">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="a30f7-537">Фрагменты рендеринга также могут передаваться в качестве аргументов в [шаблонные компоненты](#templated-components).</span><span class="sxs-lookup"><span data-stu-id="a30f7-537">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

```cshtml
@timeTemplate

@petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> petTemplate = 
        (pet) => @<p>Your pet's name is @pet.Name.</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="a30f7-538">Отображаемые выходные данные предыдущего кода:</span><span class="sxs-lookup"><span data-stu-id="a30f7-538">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="a30f7-539">Логика Рендертрибуилдер вручную</span><span class="sxs-lookup"><span data-stu-id="a30f7-539">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="a30f7-540">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` предоставляет методы для управления компонентами и элементами, включая создание компонентов вручную в C# коде.</span><span class="sxs-lookup"><span data-stu-id="a30f7-540">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="a30f7-541">Использование `RenderTreeBuilder` для создания компонентов является расширенным сценарием.</span><span class="sxs-lookup"><span data-stu-id="a30f7-541">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="a30f7-542">Неправильно сформированный компонент (например, незакрытый тег разметки) может привести к неопределенному поведению.</span><span class="sxs-lookup"><span data-stu-id="a30f7-542">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="a30f7-543">Рассмотрим следующий компонент `PetDetails`, который можно вручную встроить в другой компонент:</span><span class="sxs-lookup"><span data-stu-id="a30f7-543">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="a30f7-544">В следующем примере цикл в методе `CreateComponent` создает три компонента `PetDetails`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-544">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="a30f7-545">При вызове методов `RenderTreeBuilder` для создания компонентов (`OpenComponent` и `AddAttribute`) порядковые номера представляют собой номера строк исходного кода.</span><span class="sxs-lookup"><span data-stu-id="a30f7-545">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="a30f7-546">Алгоритм разницы Блазор зависит от порядковых номеров, соответствующих разным строкам кода, а не отдельных вызовов вызова.</span><span class="sxs-lookup"><span data-stu-id="a30f7-546">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="a30f7-547">При создании компонента с методами `RenderTreeBuilder` жестко указывайте аргументы для порядковых номеров.</span><span class="sxs-lookup"><span data-stu-id="a30f7-547">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="a30f7-548">**Использование вычисления или счетчика для формирования порядкового номера может привести к снижению производительности.**</span><span class="sxs-lookup"><span data-stu-id="a30f7-548">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="a30f7-549">Дополнительные сведения см. в разделе [порядковые номера относятся к разделу номера строк кода и порядок выполнения](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="a30f7-549">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="a30f7-550">компонент `BuiltContent`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-550">`BuiltContent` component:</span></span>

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
    Create three Pet Details components
</button>

@code {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```

> <span data-ttu-id="a30f7-551">! ! Типы в `Microsoft.AspNetCore.Components.RenderTree` позволяют обрабатывать *результаты* операций отрисовки.</span><span class="sxs-lookup"><span data-stu-id="a30f7-551">![WARNING] The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="a30f7-552">Это внутренние сведения о реализации Блазор Framework.</span><span class="sxs-lookup"><span data-stu-id="a30f7-552">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="a30f7-553">Эти типы следует считать *нестабильными* и могут быть изменены в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="a30f7-553">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="a30f7-554">Порядковые номера связаны с номерами строк кода, а не с порядком выполнения</span><span class="sxs-lookup"><span data-stu-id="a30f7-554">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="a30f7-555">Файлы блазор `.razor` всегда компилируются.</span><span class="sxs-lookup"><span data-stu-id="a30f7-555">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="a30f7-556">Это, вероятно, является отличным преимуществом для `.razor`, так как этап компиляции можно использовать для вставки сведений, повышающих производительность приложения во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="a30f7-556">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="a30f7-557">Ключевым примером этих улучшений являются *порядковые номера*.</span><span class="sxs-lookup"><span data-stu-id="a30f7-557">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="a30f7-558">Порядковые номера указывают среде выполнения, какие выходные данные поступили из разных и упорядоченных строк кода.</span><span class="sxs-lookup"><span data-stu-id="a30f7-558">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="a30f7-559">Среда выполнения использует эти сведения для создания эффективных различий дерева в линейное время, что является гораздо быстрее, чем обычно возможно для общего алгоритма различения дерева.</span><span class="sxs-lookup"><span data-stu-id="a30f7-559">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="a30f7-560">Рассмотрим следующий простой файл `.razor`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-560">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="a30f7-561">Предыдущий код компилируется примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a30f7-561">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="a30f7-562">Когда код выполняется впервые, если `someFlag` равно `true`, построитель получит следующее:</span><span class="sxs-lookup"><span data-stu-id="a30f7-562">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="a30f7-563">Sequence</span><span class="sxs-lookup"><span data-stu-id="a30f7-563">Sequence</span></span> | <span data-ttu-id="a30f7-564">Type</span><span class="sxs-lookup"><span data-stu-id="a30f7-564">Type</span></span>      | <span data-ttu-id="a30f7-565">Data</span><span class="sxs-lookup"><span data-stu-id="a30f7-565">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="a30f7-566">0</span><span class="sxs-lookup"><span data-stu-id="a30f7-566">0</span></span>        | <span data-ttu-id="a30f7-567">Текстовый узел</span><span class="sxs-lookup"><span data-stu-id="a30f7-567">Text node</span></span> | <span data-ttu-id="a30f7-568">Первая</span><span class="sxs-lookup"><span data-stu-id="a30f7-568">First</span></span>  |
| <span data-ttu-id="a30f7-569">1</span><span class="sxs-lookup"><span data-stu-id="a30f7-569">1</span></span>        | <span data-ttu-id="a30f7-570">Текстовый узел</span><span class="sxs-lookup"><span data-stu-id="a30f7-570">Text node</span></span> | <span data-ttu-id="a30f7-571">Вторая</span><span class="sxs-lookup"><span data-stu-id="a30f7-571">Second</span></span> |

<span data-ttu-id="a30f7-572">Представьте, что `someFlag` становится `false`, а разметка снова готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="a30f7-572">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="a30f7-573">На этот раз построитель получит:</span><span class="sxs-lookup"><span data-stu-id="a30f7-573">This time, the builder receives:</span></span>

| <span data-ttu-id="a30f7-574">Sequence</span><span class="sxs-lookup"><span data-stu-id="a30f7-574">Sequence</span></span> | <span data-ttu-id="a30f7-575">Type</span><span class="sxs-lookup"><span data-stu-id="a30f7-575">Type</span></span>       | <span data-ttu-id="a30f7-576">Data</span><span class="sxs-lookup"><span data-stu-id="a30f7-576">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="a30f7-577">1</span><span class="sxs-lookup"><span data-stu-id="a30f7-577">1</span></span>        | <span data-ttu-id="a30f7-578">Текстовый узел</span><span class="sxs-lookup"><span data-stu-id="a30f7-578">Text node</span></span>  | <span data-ttu-id="a30f7-579">Вторая</span><span class="sxs-lookup"><span data-stu-id="a30f7-579">Second</span></span> |

<span data-ttu-id="a30f7-580">Когда среда выполнения выполняет различие, она видит, что элемент в последовательности `0` был удален, поэтому он создает следующий тривиальный *сценарий редактирования*:</span><span class="sxs-lookup"><span data-stu-id="a30f7-580">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="a30f7-581">Удалите первый текстовый узел.</span><span class="sxs-lookup"><span data-stu-id="a30f7-581">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="a30f7-582">Что становится неправильным, если вы создаете порядковые номера программным способом</span><span class="sxs-lookup"><span data-stu-id="a30f7-582">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="a30f7-583">Вместо этого Представьте себе, что вы написали следующую логику конструктора деревьев визуализации:</span><span class="sxs-lookup"><span data-stu-id="a30f7-583">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="a30f7-584">Теперь первые выходные данные:</span><span class="sxs-lookup"><span data-stu-id="a30f7-584">Now, the first output is:</span></span>

| <span data-ttu-id="a30f7-585">Sequence</span><span class="sxs-lookup"><span data-stu-id="a30f7-585">Sequence</span></span> | <span data-ttu-id="a30f7-586">Type</span><span class="sxs-lookup"><span data-stu-id="a30f7-586">Type</span></span>      | <span data-ttu-id="a30f7-587">Data</span><span class="sxs-lookup"><span data-stu-id="a30f7-587">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="a30f7-588">0</span><span class="sxs-lookup"><span data-stu-id="a30f7-588">0</span></span>        | <span data-ttu-id="a30f7-589">Текстовый узел</span><span class="sxs-lookup"><span data-stu-id="a30f7-589">Text node</span></span> | <span data-ttu-id="a30f7-590">Первая</span><span class="sxs-lookup"><span data-stu-id="a30f7-590">First</span></span>  |
| <span data-ttu-id="a30f7-591">1</span><span class="sxs-lookup"><span data-stu-id="a30f7-591">1</span></span>        | <span data-ttu-id="a30f7-592">Текстовый узел</span><span class="sxs-lookup"><span data-stu-id="a30f7-592">Text node</span></span> | <span data-ttu-id="a30f7-593">Вторая</span><span class="sxs-lookup"><span data-stu-id="a30f7-593">Second</span></span> |

<span data-ttu-id="a30f7-594">Этот результат идентичен предыдущему случаю, поэтому отрицательные проблемы не возникают.</span><span class="sxs-lookup"><span data-stu-id="a30f7-594">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="a30f7-595">`someFlag` `false` при второй отрисовке, а выходные данные:</span><span class="sxs-lookup"><span data-stu-id="a30f7-595">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="a30f7-596">Sequence</span><span class="sxs-lookup"><span data-stu-id="a30f7-596">Sequence</span></span> | <span data-ttu-id="a30f7-597">Type</span><span class="sxs-lookup"><span data-stu-id="a30f7-597">Type</span></span>      | <span data-ttu-id="a30f7-598">Data</span><span class="sxs-lookup"><span data-stu-id="a30f7-598">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="a30f7-599">0</span><span class="sxs-lookup"><span data-stu-id="a30f7-599">0</span></span>        | <span data-ttu-id="a30f7-600">Текстовый узел</span><span class="sxs-lookup"><span data-stu-id="a30f7-600">Text node</span></span> | <span data-ttu-id="a30f7-601">Вторая</span><span class="sxs-lookup"><span data-stu-id="a30f7-601">Second</span></span> |

<span data-ttu-id="a30f7-602">На этот раз алгоритм diff видит, что были внесены *два* изменения, и алгоритм создает следующий сценарий редактирования:</span><span class="sxs-lookup"><span data-stu-id="a30f7-602">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="a30f7-603">Измените значение первого текстового узла на `Second`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-603">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="a30f7-604">Удалите второй текстовый узел.</span><span class="sxs-lookup"><span data-stu-id="a30f7-604">Remove the second text node.</span></span>

<span data-ttu-id="a30f7-605">При формировании порядковых номеров теряются все полезные сведения о том, где в исходном коде представлены ветви и циклы `if/else`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-605">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="a30f7-606">Это приводит к **удвоению в два раза больше** , чем раньше.</span><span class="sxs-lookup"><span data-stu-id="a30f7-606">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="a30f7-607">Это тривиальный пример.</span><span class="sxs-lookup"><span data-stu-id="a30f7-607">This is a trivial example.</span></span> <span data-ttu-id="a30f7-608">В более реалистичных случаях со сложными и глубокими вложенными структурами, особенно с циклами, затраты на производительность более серьезны.</span><span class="sxs-lookup"><span data-stu-id="a30f7-608">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="a30f7-609">Вместо того, чтобы сразу определять, какие блоки или ветви цикла были вставлены или удалены, алгоритм diff должен рекурсивно пребираться в деревьях отрисовки и, как правило, создает гораздо более длительные операции редактирования, так как он сообщает о том, как старую и новую структуры связь друг с другом.</span><span class="sxs-lookup"><span data-stu-id="a30f7-609">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="a30f7-610">Руководство и выводы</span><span class="sxs-lookup"><span data-stu-id="a30f7-610">Guidance and conclusions</span></span>

* <span data-ttu-id="a30f7-611">Производительность приложения снижается, если порядковые номера создаются динамически.</span><span class="sxs-lookup"><span data-stu-id="a30f7-611">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="a30f7-612">Платформа не может автоматически создавать собственные порядковые номера во время выполнения, поскольку необходимая информация не существует, если она не захвачена во время компиляции.</span><span class="sxs-lookup"><span data-stu-id="a30f7-612">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="a30f7-613">Не записывайте длинные блоки, реализованные вручную `RenderTreeBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-613">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="a30f7-614">Предпочитать файлы `.razor` и позволяют компилятору работать с порядковыми номерами.</span><span class="sxs-lookup"><span data-stu-id="a30f7-614">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="a30f7-615">Если не удается избежать @no__t логики вручную, разделите длинные блоки кода на более мелкие части, Упакованные в вызовы `OpenRegion` @ no__t-2 @ no__t-3.</span><span class="sxs-lookup"><span data-stu-id="a30f7-615">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="a30f7-616">Каждый регион имеет собственное отдельное пространство порядковых номеров, поэтому вы можете перезапускаться от нуля (или любого другого произвольного числа) внутри каждого региона.</span><span class="sxs-lookup"><span data-stu-id="a30f7-616">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="a30f7-617">Если порядковые номера задаются жестко, то для алгоритма diff требуется, чтобы только порядковые номера увеличиваются в значении.</span><span class="sxs-lookup"><span data-stu-id="a30f7-617">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="a30f7-618">Начальное значение и зазоры несущественны.</span><span class="sxs-lookup"><span data-stu-id="a30f7-618">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="a30f7-619">Один из этих вариантов — использовать номер строки кода в качестве порядкового номера или начать с нуля и увеличить на единицу или сотни (или любой другой интервал).</span><span class="sxs-lookup"><span data-stu-id="a30f7-619">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* <span data-ttu-id="a30f7-620">Блазор использует порядковые номера, а другие платформы пользовательского интерфейса для различения структуры не используют их.</span><span class="sxs-lookup"><span data-stu-id="a30f7-620">Blazor uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="a30f7-621">Сравнение выполняется гораздо быстрее, если используются порядковые номера, и Блазор имеет преимущество этапа компиляции, который автоматически обрабатывает порядковые номера для разработчиков, занимающихся созданием файлов `.razor`.</span><span class="sxs-lookup"><span data-stu-id="a30f7-621">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring `.razor` files.</span></span>

## <a name="localization"></a><span data-ttu-id="a30f7-622">Локализация</span><span class="sxs-lookup"><span data-stu-id="a30f7-622">Localization</span></span>

<span data-ttu-id="a30f7-623">Блазор серверные приложения локализованы по [промежуточного слоя локализации](xref:fundamentals/localization#localization-middleware).</span><span class="sxs-lookup"><span data-stu-id="a30f7-623">Blazor Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="a30f7-624">По промежуточного слоя выбирает соответствующие языки и региональные параметры для пользователей, запрашивающих ресурсы из приложения.</span><span class="sxs-lookup"><span data-stu-id="a30f7-624">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="a30f7-625">Язык и региональные параметры можно задать с помощью одного из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="a30f7-625">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="a30f7-626">Файлы "cookie"</span><span class="sxs-lookup"><span data-stu-id="a30f7-626">Cookies</span></span>](#cookies)
* [<span data-ttu-id="a30f7-627">Предоставление пользовательского интерфейса для выбора языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="a30f7-627">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="a30f7-628">Дополнительные сведения и примеры см. в разделе <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="a30f7-628">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="a30f7-629">Файлы cookie</span><span class="sxs-lookup"><span data-stu-id="a30f7-629">Cookies</span></span>

<span data-ttu-id="a30f7-630">Файл cookie культуры локализации может сохранять язык и региональные параметры пользователя.</span><span class="sxs-lookup"><span data-stu-id="a30f7-630">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="a30f7-631">Файл cookie создается методом `OnGet` страницы узла приложения (*pages/Host. cshtml. CS*).</span><span class="sxs-lookup"><span data-stu-id="a30f7-631">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="a30f7-632">По промежуточного слоя локализации считывает файл cookie при последующих запросах, чтобы задать язык и региональные параметры пользователя.</span><span class="sxs-lookup"><span data-stu-id="a30f7-632">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="a30f7-633">Использование файла cookie гарантирует, что соединение WebSocket сможет правильно распространить культуру.</span><span class="sxs-lookup"><span data-stu-id="a30f7-633">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="a30f7-634">Если схемы локализации основаны на пути URL-адреса или строке запроса, схема может не поддерживать работу с WebSockets, поэтому она не сохраняет культуру.</span><span class="sxs-lookup"><span data-stu-id="a30f7-634">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="a30f7-635">Поэтому рекомендуемым подходом является использование файла cookie языка и региональных параметров локализации.</span><span class="sxs-lookup"><span data-stu-id="a30f7-635">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="a30f7-636">Любой метод можно использовать для назначения языка и региональных параметров, если язык и региональные параметры сохраняются в файле cookie локализации.</span><span class="sxs-lookup"><span data-stu-id="a30f7-636">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="a30f7-637">Если у приложения уже есть установленная схема локализации для ASP.NET Core на стороне сервера, продолжайте использовать существующую инфраструктуру локализации приложения и задавайте файл cookie культуры локализации в схеме приложения.</span><span class="sxs-lookup"><span data-stu-id="a30f7-637">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="a30f7-638">В следующем примере показано, как задать текущий язык и региональные параметры в файле cookie, который может быть прочитан по промежуточного слоя локализации.</span><span class="sxs-lookup"><span data-stu-id="a30f7-638">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="a30f7-639">Создайте файл *pages/Host. cshtml. CS* со следующим содержимым в приложении блазор Server:</span><span class="sxs-lookup"><span data-stu-id="a30f7-639">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

```csharp
public class HostModel : PageModel
{
    public void OnGet()
    {
        HttpContext.Response.Cookies.Append(
            CookieRequestCultureProvider.DefaultCookieName,
            CookieRequestCultureProvider.MakeCookieValue(
                new RequestCulture(
                    CultureInfo.CurrentCulture,
                    CultureInfo.CurrentUICulture)));
    }
}
```

<span data-ttu-id="a30f7-640">Локализация обрабатывается в приложении:</span><span class="sxs-lookup"><span data-stu-id="a30f7-640">Localization is handled in the app:</span></span>

1. <span data-ttu-id="a30f7-641">Браузер отправляет в приложение исходный HTTP-запрос.</span><span class="sxs-lookup"><span data-stu-id="a30f7-641">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="a30f7-642">Язык и региональные параметры назначаются по промежуточного слоя локализации.</span><span class="sxs-lookup"><span data-stu-id="a30f7-642">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="a30f7-643">Метод `OnGet` в *_Host. cshtml. CS* сохраняет язык и региональные параметры в файле cookie как часть ответа.</span><span class="sxs-lookup"><span data-stu-id="a30f7-643">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="a30f7-644">Браузер открывает соединение WebSocket для создания интерактивного сеанса Блазор Server.</span><span class="sxs-lookup"><span data-stu-id="a30f7-644">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="a30f7-645">По промежуточного слоя для локализации считывает файл cookie и назначает язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="a30f7-645">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="a30f7-646">Сеанс сервера Блазор начинается с правильного языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="a30f7-646">The Blazor Server session begins with the correct culture.</span></span>

## <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="a30f7-647">Предоставление пользовательского интерфейса для выбора языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="a30f7-647">Provide UI to choose the culture</span></span>

<span data-ttu-id="a30f7-648">Для предоставления пользовательского интерфейса, позволяющего пользователю выбирать язык и региональные параметры, рекомендуется *подход на основе перенаправления* .</span><span class="sxs-lookup"><span data-stu-id="a30f7-648">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="a30f7-649">Процесс аналогичен тому, что происходит в веб-приложении, когда пользователь пытается получить доступ к защищенному ресурсу @ no__t-0the пользователь перенаправляется на страницу входа, а затем перенаправляется обратно к исходному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="a30f7-649">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="a30f7-650">Приложение сохраняет выбранный язык и региональные параметры пользователя с помощью перенаправления к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="a30f7-650">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="a30f7-651">Контроллер устанавливает выбранный язык и региональные параметры пользователя в файл cookie и перенаправляет пользователя обратно к исходному коду URI.</span><span class="sxs-lookup"><span data-stu-id="a30f7-651">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="a30f7-652">Установите конечную точку HTTP на сервере, чтобы задать выбранный язык и региональные параметры пользователя в файле cookie, и выполните перенаправление обратно в исходный URI:</span><span class="sxs-lookup"><span data-stu-id="a30f7-652">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

```csharp
[Route("[controller]/[action]")]
public class CultureController : Controller
{
    public IActionResult SetCulture(string culture, string redirectUri)
    {
        if (culture != null)
        {
            HttpContext.Response.Cookies.Append(
                CookieRequestCultureProvider.DefaultCookieName,
                CookieRequestCultureProvider.MakeCookieValue(
                    new RequestCulture(culture)));
        }

        return LocalRedirect(redirectUri);
    }
}
```

> [!WARNING]
> <span data-ttu-id="a30f7-653">Используйте результат действия `LocalRedirect`, чтобы предотвратить атаки с открытым перенаправлением.</span><span class="sxs-lookup"><span data-stu-id="a30f7-653">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="a30f7-654">Дополнительные сведения см. в разделе <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="a30f7-654">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="a30f7-655">В следующем компоненте показан пример выполнения начального перенаправления, когда пользователь выбирает язык и региональные параметры:</span><span class="sxs-lookup"><span data-stu-id="a30f7-655">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```cshtml
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private double textNumber;

    private void OnSelected(ChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(NavigationManager.Uri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        NavigationManager.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

### <a name="use-net-localization-scenarios-in-blazor-apps"></a><span data-ttu-id="a30f7-656">Использование сценариев локализации .NET в приложениях Блазор</span><span class="sxs-lookup"><span data-stu-id="a30f7-656">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="a30f7-657">В приложениях Блазор доступны следующие сценарии локализации и глобализации .NET:</span><span class="sxs-lookup"><span data-stu-id="a30f7-657">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="a30f7-658">. Система ресурсов NET</span><span class="sxs-lookup"><span data-stu-id="a30f7-658">.NET's resources system</span></span>
* <span data-ttu-id="a30f7-659">Форматирование чисел и дат, зависящих от языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="a30f7-659">Culture-specific number and date formatting</span></span>

<span data-ttu-id="a30f7-660">Функция `@bind` блазор выполняет глобализацию на основе текущего языка и региональных параметров пользователя.</span><span class="sxs-lookup"><span data-stu-id="a30f7-660">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="a30f7-661">Дополнительные сведения см. в разделе [Привязка данных](#data-binding) .</span><span class="sxs-lookup"><span data-stu-id="a30f7-661">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="a30f7-662">В настоящее время поддерживается ограниченный набор сценариев локализации ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="a30f7-662">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="a30f7-663">`IStringLocalizer<>` *поддерживается* в приложениях блазор.</span><span class="sxs-lookup"><span data-stu-id="a30f7-663">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="a30f7-664">`IHtmlLocalizer<>`, `IViewLocalizer<>` и локализация заметок к данным — ASP.NET Core сценарии MVC и **не поддерживаются** в приложениях блазор.</span><span class="sxs-lookup"><span data-stu-id="a30f7-664">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="a30f7-665">Дополнительные сведения см. в разделе <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="a30f7-665">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="a30f7-666">Масштабируемые изображения векторной графики (SVG)</span><span class="sxs-lookup"><span data-stu-id="a30f7-666">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="a30f7-667">Поскольку Блазор визуализирует HTML-файлы, поддерживаемые браузером изображения, включая масштабируемые векторные графики (SVG *),* поддерживаются с помощью тега `<img>`:</span><span class="sxs-lookup"><span data-stu-id="a30f7-667">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="a30f7-668">Аналогичным образом SVG-изображения поддерживаются в правилах CSS файла стилей ( *. CSS*):</span><span class="sxs-lookup"><span data-stu-id="a30f7-668">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="a30f7-669">Однако встроенная разметка SVG не поддерживается во всех сценариях.</span><span class="sxs-lookup"><span data-stu-id="a30f7-669">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="a30f7-670">Если поместить тег `<svg>` непосредственно в файл компонента (*Razor*), то поддерживается базовая визуализация изображений, но многие расширенные сценарии еще не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="a30f7-670">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="a30f7-671">Например, в настоящее время Теги `<use>` не учитываются, и `@bind` нельзя использовать с некоторыми тегами SVG.</span><span class="sxs-lookup"><span data-stu-id="a30f7-671">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="a30f7-672">В будущем выпуске мы планируем устранить эти ограничения.</span><span class="sxs-lookup"><span data-stu-id="a30f7-672">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a30f7-673">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a30f7-673">Additional resources</span></span>

* <span data-ttu-id="a30f7-674"><xref:security/blazor/server> &ndash; включает рекомендации по созданию серверных приложений Блазор, которые должны конкурировать с нехваткой ресурсов.</span><span class="sxs-lookup"><span data-stu-id="a30f7-674"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
