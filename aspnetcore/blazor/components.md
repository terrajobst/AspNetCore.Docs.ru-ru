---
title: Создание и использование компонентов ASP.NET Core Razor
author: guardrex
description: Узнайте, как создавать и использовать компоненты Razor, включая способы привязки к данным, обработки событий и управления жизненным циклом компонентов.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/components
ms.openlocfilehash: 8cb2dc4c3cd22fe71fe15c22762948f9dcd3c08f
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030360"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="1686a-103">Создание и использование компонентов ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="1686a-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="1686a-104">[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="1686a-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="1686a-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1686a-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1686a-106">Приложения блазор создаются с помощью *компонентов*.</span><span class="sxs-lookup"><span data-stu-id="1686a-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="1686a-107">Компонент — это автономный фрагмент пользовательского интерфейса, например страница, диалоговое окно или форма.</span><span class="sxs-lookup"><span data-stu-id="1686a-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="1686a-108">Компонент включает разметку HTML и логику обработки, необходимую для вставки данных или реагирования на события пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="1686a-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="1686a-109">Компоненты являются гибкими и легковесными.</span><span class="sxs-lookup"><span data-stu-id="1686a-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="1686a-110">Они могут быть вложенными, повторно использованы и совместно использоваться проектами.</span><span class="sxs-lookup"><span data-stu-id="1686a-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="1686a-111">Классы компонентов</span><span class="sxs-lookup"><span data-stu-id="1686a-111">Component classes</span></span>

<span data-ttu-id="1686a-112">Компоненты реализуются в файлах компонентов [Razor](xref:mvc/views/razor) ( *. Razor*) с помощью комбинации C# и HTML-разметки.</span><span class="sxs-lookup"><span data-stu-id="1686a-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="1686a-113">Компонент в Блазор формально называется *компонентом Razor*.</span><span class="sxs-lookup"><span data-stu-id="1686a-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="1686a-114">Имя компонента должно начинаться с символа верхнего регистра.</span><span class="sxs-lookup"><span data-stu-id="1686a-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="1686a-115">Например, *микулкомпонент. Razor* является допустимым, а *микулкомпонент. Razor* является недопустимым.</span><span class="sxs-lookup"><span data-stu-id="1686a-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="1686a-116">Компоненты можно создавать с помощью расширения файла *. cshtml* , если файлы определены как файлы компонентов Razor с помощью `_RazorComponentInclude` свойства MSBuild.</span><span class="sxs-lookup"><span data-stu-id="1686a-116">Components can be authored using the *.cshtml* file extension as long as the files are identified as Razor component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="1686a-117">Например, приложение, которое указывает, что все файлы *. cshtml* в папке *pages* должны рассматриваться как файлы компонентов Razor:</span><span class="sxs-lookup"><span data-stu-id="1686a-117">For example, an app that specifies that all *.cshtml* files under the *Pages* folder should be treated as Razor components files:</span></span>

```xml
<PropertyGroup>
  <_RazorComponentInclude>Pages\**\*.cshtml</_RazorComponentInclude>
</PropertyGroup>
```

<span data-ttu-id="1686a-118">Пользовательский интерфейс для компонента определяется с помощью HTML.</span><span class="sxs-lookup"><span data-stu-id="1686a-118">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="1686a-119">Логика динамического отображения (например выражения, циклы и условные выражения) добавляется с помощью встроенного синтаксиса C# под названием [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="1686a-119">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="1686a-120">При компиляции приложения логика разметки и C# отрисовки HTML преобразуется в класс компонента.</span><span class="sxs-lookup"><span data-stu-id="1686a-120">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="1686a-121">Имя созданного класса соответствует имени файла.</span><span class="sxs-lookup"><span data-stu-id="1686a-121">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="1686a-122">Элементы класса компонента определяются в блоке `@code`.</span><span class="sxs-lookup"><span data-stu-id="1686a-122">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="1686a-123">`@code` В блоке состояние компонента (свойства, поля) указывается с помощью методов обработки событий или для определения другой логики компонента.</span><span class="sxs-lookup"><span data-stu-id="1686a-123">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="1686a-124">Допускается более одного блока `@code`.</span><span class="sxs-lookup"><span data-stu-id="1686a-124">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="1686a-125">В предыдущих предварительных версиях ASP.NET Core 3,0 `@functions` блоки использовались для тех же целей, что `@code` и блоки в компонентах Razor.</span><span class="sxs-lookup"><span data-stu-id="1686a-125">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="1686a-126">`@functions`блоки продолжают функционировать в компонентах Razor, но мы рекомендуем использовать `@code` блок в ASP.NET Core 3,0 Preview 6 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="1686a-126">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="1686a-127">Члены компонента можно использовать как часть логики отрисовки компонента с помощью C# выражений, начинающихся с `@`.</span><span class="sxs-lookup"><span data-stu-id="1686a-127">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="1686a-128">Например, C# поле подготавливается к просмотру путем `@` добавления префикса к имени поля.</span><span class="sxs-lookup"><span data-stu-id="1686a-128">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="1686a-129">В следующем примере вычисляется и выводится следующее:</span><span class="sxs-lookup"><span data-stu-id="1686a-129">The following example evaluates and renders:</span></span>

* <span data-ttu-id="1686a-130">`_headingFontStyle`к значению свойства CSS для `font-style`.</span><span class="sxs-lookup"><span data-stu-id="1686a-130">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="1686a-131">`_headingText`к содержимому `<h1>` элемента.</span><span class="sxs-lookup"><span data-stu-id="1686a-131">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="1686a-132">После первоначального отображения компонента Компонент повторно создает дерево рендеринга в ответ на события.</span><span class="sxs-lookup"><span data-stu-id="1686a-132">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="1686a-133">Затем блазор сравнивает новое дерево отрисовки с предыдущим и применяет любые изменения к модель DOMу (DOM) браузера.</span><span class="sxs-lookup"><span data-stu-id="1686a-133">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="1686a-134">Компоненты являются обычными C# классами и могут быть помещены в любое место внутри проекта.</span><span class="sxs-lookup"><span data-stu-id="1686a-134">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="1686a-135">Компоненты, создающие веб-страницы, обычно находятся в папке *страниц* .</span><span class="sxs-lookup"><span data-stu-id="1686a-135">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="1686a-136">Компоненты, не являющиеся страницами, часто помещаются в *общую* папку или пользовательскую папку, добавленную в проект.</span><span class="sxs-lookup"><span data-stu-id="1686a-136">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="1686a-137">Чтобы использовать пользовательскую папку, добавьте пространство имен пользовательской папки либо в родительский компонент, либо в файл *_Imports. Razor* приложения.</span><span class="sxs-lookup"><span data-stu-id="1686a-137">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="1686a-138">Например, следующее пространство имен делает компоненты в папке *Components* доступной, когда корневое пространство имен приложения имеет `WebApplication`следующий уровень:</span><span class="sxs-lookup"><span data-stu-id="1686a-138">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="1686a-139">Интеграция компонентов в Razor Pages и приложения MVC</span><span class="sxs-lookup"><span data-stu-id="1686a-139">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="1686a-140">Используйте компоненты с существующими Razor Pages и приложениями MVC.</span><span class="sxs-lookup"><span data-stu-id="1686a-140">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="1686a-141">Нет необходимости переписывать существующие страницы или представления для использования компонентов Razor.</span><span class="sxs-lookup"><span data-stu-id="1686a-141">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="1686a-142">При отображении страницы или представления компоненты предварительно отображаются.</span><span class="sxs-lookup"><span data-stu-id="1686a-142">When the page or view is rendered, components are prerendered at the same time.</span></span>

<span data-ttu-id="1686a-143">Чтобы отобразить компонент из страницы или представления, используйте `RenderComponentAsync<TComponent>` вспомогательный метод HTML:</span><span class="sxs-lookup"><span data-stu-id="1686a-143">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

<span data-ttu-id="1686a-144">Хотя страницы и представления могут использовать компоненты, наоборот это не так.</span><span class="sxs-lookup"><span data-stu-id="1686a-144">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="1686a-145">Компоненты не могут использовать сценарии представления и страницы, такие как частичные представления и разделы.</span><span class="sxs-lookup"><span data-stu-id="1686a-145">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="1686a-146">Чтобы использовать логику из частичного представления в компоненте, разнесите логику частичного представления в компонент.</span><span class="sxs-lookup"><span data-stu-id="1686a-146">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="1686a-147">Дополнительные сведения о подготовке компонентов к просмотру и управлении состоянием компонентов в приложениях блазор на стороне сервера см. в <xref:blazor/hosting-models> статье.</span><span class="sxs-lookup"><span data-stu-id="1686a-147">For more information on how components are rendered and component state is managed in Blazor server-side apps, see the <xref:blazor/hosting-models> article.</span></span>

## <a name="use-components"></a><span data-ttu-id="1686a-148">Использование компонентов</span><span class="sxs-lookup"><span data-stu-id="1686a-148">Use components</span></span>

<span data-ttu-id="1686a-149">Компоненты могут включать другие компоненты, объявляя их с помощью синтаксиса HTML-элементов.</span><span class="sxs-lookup"><span data-stu-id="1686a-149">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="1686a-150">Разметка для использования компонента выглядит как тег HTML с именем, соответствующем типу компонента.</span><span class="sxs-lookup"><span data-stu-id="1686a-150">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="1686a-151">Привязка атрибута чувствительна к регистру.</span><span class="sxs-lookup"><span data-stu-id="1686a-151">Attribute binding is case sensitive.</span></span> <span data-ttu-id="1686a-152">Например, `@bind` является допустимым и `@Bind` является недопустимым.</span><span class="sxs-lookup"><span data-stu-id="1686a-152">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="1686a-153">Следующая разметка в *index. Razor* визуализирует `HeadingComponent` экземпляр:</span><span class="sxs-lookup"><span data-stu-id="1686a-153">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="1686a-154">*Components/хеадингкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="1686a-154">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

<span data-ttu-id="1686a-155">Если компонент содержит HTML-элемент с первой буквой в верхнем регистре, который не соответствует имени компонента, выдается предупреждение, указывающее, что элемент имеет непредвиденное имя.</span><span class="sxs-lookup"><span data-stu-id="1686a-155">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="1686a-156">`@using` Добавление инструкции для пространства имен компонента делает компонент доступным, что приводит к удалению предупреждения.</span><span class="sxs-lookup"><span data-stu-id="1686a-156">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="1686a-157">Параметры компонентов</span><span class="sxs-lookup"><span data-stu-id="1686a-157">Component parameters</span></span>

<span data-ttu-id="1686a-158">Компоненты могут иметь *Параметры компонента*, которые определяются с помощью общедоступных свойств класса Component с `[Parameter]` атрибутом.</span><span class="sxs-lookup"><span data-stu-id="1686a-158">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="1686a-159">Используйте атрибуты, чтобы указать аргументы для компонента в разметке.</span><span class="sxs-lookup"><span data-stu-id="1686a-159">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="1686a-160">*Components/чилдкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="1686a-160">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="1686a-161">В следующем примере `ParentComponent` `Title` свойство устанавливает значение свойства объекта `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="1686a-161">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="1686a-162">*Pages/паренткомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="1686a-162">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="1686a-163">Дочернее содержимое</span><span class="sxs-lookup"><span data-stu-id="1686a-163">Child content</span></span>

<span data-ttu-id="1686a-164">Компоненты могут устанавливать содержимое другого компонента.</span><span class="sxs-lookup"><span data-stu-id="1686a-164">Components can set the content of another component.</span></span> <span data-ttu-id="1686a-165">Назначение компонента предоставляет содержимое между тегами, задающих принимающий компонент.</span><span class="sxs-lookup"><span data-stu-id="1686a-165">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="1686a-166">В следующем примере объект `ChildComponent` `ChildContent` имеет свойство, представляющее объект `RenderFragment`, который представляет сегмент пользовательского интерфейса для отрисовки.</span><span class="sxs-lookup"><span data-stu-id="1686a-166">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="1686a-167">Значение `ChildContent` размещается в разметке компонента, где должно быть визуализировано содержимое.</span><span class="sxs-lookup"><span data-stu-id="1686a-167">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="1686a-168">Значение `ChildContent` получается от родительского компонента и отображается внутри `panel-body`панели начальной загрузки.</span><span class="sxs-lookup"><span data-stu-id="1686a-168">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="1686a-169">*Components/чилдкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="1686a-169">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="1686a-170">Свойству, получающему `RenderFragment` содержимое, необходимо `ChildContent` присвоить имя по соглашению.</span><span class="sxs-lookup"><span data-stu-id="1686a-170">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="1686a-171">Следующие `ParentComponent` данные могут предоставлять содержимое для подготовки к `ChildComponent` просмотру путем `<ChildComponent>` размещения содержимого внутри тегов.</span><span class="sxs-lookup"><span data-stu-id="1686a-171">The following `ParentComponent` can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="1686a-172">*Pages/паренткомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="1686a-172">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="1686a-173">Атрибуты Сплаттинг и произвольные параметры</span><span class="sxs-lookup"><span data-stu-id="1686a-173">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="1686a-174">Компоненты могут записывать и отображать дополнительные атрибуты в дополнение к объявленным параметрам компонента.</span><span class="sxs-lookup"><span data-stu-id="1686a-174">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="1686a-175">Дополнительные атрибуты можно записать в словарь, а затем *сплаттед* на элемент при подготовке компонента к просмотру с помощью [@attributes](xref:mvc/views/razor#attributes) директивы Razor.</span><span class="sxs-lookup"><span data-stu-id="1686a-175">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [@attributes](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="1686a-176">Этот сценарий полезен при определении компонента, который создает элемент разметки, поддерживающий разнообразные настройки.</span><span class="sxs-lookup"><span data-stu-id="1686a-176">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="1686a-177">Например, может быть утомительно определять атрибуты отдельно для `<input>` , который поддерживает множество параметров.</span><span class="sxs-lookup"><span data-stu-id="1686a-177">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="1686a-178">В `<input>` следующем примере первый элемент (`id="useIndividualParams"`) использует индивидуальные параметры компонента, а второй `<input>` элемент (`id="useAttributesDict"`) использует атрибут Сплаттинг:</span><span class="sxs-lookup"><span data-stu-id="1686a-178">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

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
            { "required", "true" },
            { "size", "50" }
        };
}
```

<span data-ttu-id="1686a-179">Тип параметра должен реализовываться `IEnumerable<KeyValuePair<string, object>>` со строковыми ключами.</span><span class="sxs-lookup"><span data-stu-id="1686a-179">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="1686a-180">В `IReadOnlyDictionary<string, object>` этом сценарии также используется параметр using.</span><span class="sxs-lookup"><span data-stu-id="1686a-180">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="1686a-181">Отображаемые `<input>` элементы, использующие оба подхода, идентичны:</span><span class="sxs-lookup"><span data-stu-id="1686a-181">The rendered `<input>` elements using both approaches is identical:</span></span>

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="true"
       size="50">
```

<span data-ttu-id="1686a-182">Чтобы принять произвольные атрибуты, определите параметр компонента с помощью `[Parameter]` атрибута `CaptureUnmatchedValues` со свойством, имеющим `true`значение:</span><span class="sxs-lookup"><span data-stu-id="1686a-182">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```cshtml
@code {
    [Parameter(CaptureUnmatchedAttributes = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="1686a-183">`CaptureUnmatchedValues` Свойство в`[Parameter]` позволяет параметру сопоставлять все атрибуты, не соответствующие ни одному другому параметру.</span><span class="sxs-lookup"><span data-stu-id="1686a-183">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="1686a-184">Компонент может определять только один параметр с помощью `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="1686a-184">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="1686a-185">Тип свойства, используемый с `CaptureUnmatchedValues` параметром, должен быть `Dictionary<string, object>` назначен с помощью строковых ключей.</span><span class="sxs-lookup"><span data-stu-id="1686a-185">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="1686a-186">`IEnumerable<KeyValuePair<string, object>>`Кроме `IReadOnlyDictionary<string, object>` того, в этом сценарии также есть параметры.</span><span class="sxs-lookup"><span data-stu-id="1686a-186">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

## <a name="data-binding"></a><span data-ttu-id="1686a-187">Привязка данных</span><span class="sxs-lookup"><span data-stu-id="1686a-187">Data binding</span></span>

<span data-ttu-id="1686a-188">Привязка данных как к компонентам, так и к элементам DOM [@bind](xref:mvc/views/razor#bind) осуществляется с помощью атрибута.</span><span class="sxs-lookup"><span data-stu-id="1686a-188">Data binding to both components and DOM elements is accomplished with the [@bind](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="1686a-189">В следующем примере `_italicsCheck` поле привязывается к установленному флажку.</span><span class="sxs-lookup"><span data-stu-id="1686a-189">The following example binds the `_italicsCheck` field to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    @bind="_italicsCheck" />
```

<span data-ttu-id="1686a-190">Если флажок установлен и снят, значение свойства обновляется до `true` и `false`соответственно.</span><span class="sxs-lookup"><span data-stu-id="1686a-190">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="1686a-191">Флажок обновляется в пользовательском интерфейсе только при подготовке компонента к просмотру, а не в ответ на изменение значения свойства.</span><span class="sxs-lookup"><span data-stu-id="1686a-191">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="1686a-192">Поскольку компоненты отображаются после выполнения кода обработчика событий, обновления свойств, как правило, немедленно отражаются в пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="1686a-192">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="1686a-193">Использование `@bind` `<input @bind="CurrentValue" />`со свойством (), по сути, эквивалентно следующему: `CurrentValue`</span><span class="sxs-lookup"><span data-stu-id="1686a-193">Using `@bind` with a `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue"
    @onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

<span data-ttu-id="1686a-194">При подготовке компонента к `value` просмотру элемент входного элемента берется `CurrentValue` из свойства.</span><span class="sxs-lookup"><span data-stu-id="1686a-194">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="1686a-195">Когда пользователь вводит текст в текстовое поле, `onchange` создается событие, `CurrentValue` а свойству присваивается измененное значение.</span><span class="sxs-lookup"><span data-stu-id="1686a-195">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="1686a-196">На практике создание кода немного сложнее, так как `@bind` обрабатывает несколько случаев, когда выполняются преобразования типов.</span><span class="sxs-lookup"><span data-stu-id="1686a-196">In reality, the code generation is a little more complex because `@bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="1686a-197">В принципе, `@bind` связывает текущее значение выражения `value` с атрибутом и обрабатывает изменения с помощью зарегистрированного обработчика.</span><span class="sxs-lookup"><span data-stu-id="1686a-197">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="1686a-198">`onchange` Помимо обработки[@bind-value:event](xref:mvc/views/razor#bind)событий с `@bind` синтаксисом, свойство или поле можно привязать с помощью [@bind-value](xref:mvc/views/razor#bind) других событий, `event` указав атрибут с параметром ().</span><span class="sxs-lookup"><span data-stu-id="1686a-198">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [@bind-value](xref:mvc/views/razor#bind) attribute with an `event` parameter ([@bind-value:event](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="1686a-199">В следующем примере показано, `CurrentValue` как привязать свойство `oninput` к событию:</span><span class="sxs-lookup"><span data-stu-id="1686a-199">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />
```

<span data-ttu-id="1686a-200">В отличие `onchange`от, которое срабатывает при потере фокуса элементом `oninput` , срабатывает при изменении значения текстового поля.</span><span class="sxs-lookup"><span data-stu-id="1686a-200">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="1686a-201">**Глобализация**</span><span class="sxs-lookup"><span data-stu-id="1686a-201">**Globalization**</span></span>

<span data-ttu-id="1686a-202">`@bind`значения форматируются для вывода и анализируются с использованием правил текущего языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="1686a-202">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="1686a-203">Доступ к текущему языку и региональным параметрам можно получить из <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> свойства.</span><span class="sxs-lookup"><span data-stu-id="1686a-203">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="1686a-204">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) используется для следующих типов полей (`<input type="{TYPE}" />`):</span><span class="sxs-lookup"><span data-stu-id="1686a-204">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="1686a-205">Предыдущие типы полей:</span><span class="sxs-lookup"><span data-stu-id="1686a-205">The preceding field types:</span></span>

* <span data-ttu-id="1686a-206">Отображаются с использованием соответствующих правил форматирования на основе браузера.</span><span class="sxs-lookup"><span data-stu-id="1686a-206">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="1686a-207">Не может содержать текст в свободной форме.</span><span class="sxs-lookup"><span data-stu-id="1686a-207">Can't contain free-form text.</span></span>
* <span data-ttu-id="1686a-208">Предоставление характеристик взаимодействия с пользователем в зависимости от реализации браузера.</span><span class="sxs-lookup"><span data-stu-id="1686a-208">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="1686a-209">Следующие типы полей имеют особые требования к форматированию, которые в настоящее время не поддерживаются Блазор, так как они не поддерживаются всеми основными браузерами.</span><span class="sxs-lookup"><span data-stu-id="1686a-209">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="1686a-210">`@bind`поддерживает параметр для <xref:System.Globalization.CultureInfo?displayProperty=fullName> обеспечения синтаксического анализа и форматирования значения. `@bind:culture`</span><span class="sxs-lookup"><span data-stu-id="1686a-210">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="1686a-211">Указание языка и региональных параметров не рекомендуется при `date` использовании `number` типов полей и.</span><span class="sxs-lookup"><span data-stu-id="1686a-211">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="1686a-212">`date`и `number` имеют встроенную поддержку блазор, которая предоставляет требуемый язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="1686a-212">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="1686a-213">Сведения о том, как задать язык и региональные параметры пользователя, см. в разделе [локализация](#localization).</span><span class="sxs-lookup"><span data-stu-id="1686a-213">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="1686a-214">**Строки формата**</span><span class="sxs-lookup"><span data-stu-id="1686a-214">**Format strings**</span></span>

<span data-ttu-id="1686a-215">Привязка данных работает со <xref:System.DateTime> строками формата [@bind:format](xref:mvc/views/razor#bind)с помощью.</span><span class="sxs-lookup"><span data-stu-id="1686a-215">Data binding works with <xref:System.DateTime> format strings using [@bind:format](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="1686a-216">Другие выражения форматирования, такие как денежные или числовые форматы, в настоящее время недоступны.</span><span class="sxs-lookup"><span data-stu-id="1686a-216">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="1686a-217">В приведенном выше коде `<input>` тип поля элемента (`type`) по умолчанию `text`имеет значение.</span><span class="sxs-lookup"><span data-stu-id="1686a-217">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="1686a-218">`@bind:format`поддерживается для привязки следующих типов .NET:</span><span class="sxs-lookup"><span data-stu-id="1686a-218">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="1686a-219"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="1686a-219"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="1686a-220"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="1686a-220"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="1686a-221">Атрибут задает формат даты, `value` `<input>` применяемый к элементу. `@bind:format`</span><span class="sxs-lookup"><span data-stu-id="1686a-221">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="1686a-222">Формат также используется для анализа значения при `onchange` возникновении события.</span><span class="sxs-lookup"><span data-stu-id="1686a-222">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="1686a-223">Указание формата для `date` типа поля не рекомендуется, так как блазор имеет встроенную поддержку для форматирования дат.</span><span class="sxs-lookup"><span data-stu-id="1686a-223">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span>

<span data-ttu-id="1686a-224">**Параметры компонента**</span><span class="sxs-lookup"><span data-stu-id="1686a-224">**Component parameters**</span></span>

<span data-ttu-id="1686a-225">Привязка распознает параметры компонента, `@bind-{property}` где можно привязать значение свойства к разным компонентам.</span><span class="sxs-lookup"><span data-stu-id="1686a-225">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="1686a-226">Следующий дочерний компонент`ChildComponent`() `Year` имеет параметр компонента и `YearChanged` обратный вызов:</span><span class="sxs-lookup"><span data-stu-id="1686a-226">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

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

<span data-ttu-id="1686a-227">`EventCallback<T>`описывается в разделе [вложенный EventCallback](#eventcallback) .</span><span class="sxs-lookup"><span data-stu-id="1686a-227">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="1686a-228">Следующий родительский компонент использует `ChildComponent` и связывает `ParentYear` параметр `Year` из родительского компонента с параметром в дочернем компоненте:</span><span class="sxs-lookup"><span data-stu-id="1686a-228">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

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

<span data-ttu-id="1686a-229">При загрузке `ParentComponent` создается следующая разметка:</span><span class="sxs-lookup"><span data-stu-id="1686a-229">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="1686a-230">Если значение `ParentYear` свойства изменяется путем нажатия кнопки `ParentComponent`в `ChildComponent` , `Year` свойство элемента обновляется.</span><span class="sxs-lookup"><span data-stu-id="1686a-230">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="1686a-231">Новое значение параметра `Year` отображается в пользовательском интерфейсе при его `ParentComponent` визуализации.</span><span class="sxs-lookup"><span data-stu-id="1686a-231">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="1686a-232">Параметр является связываемым, так как содержит сопутствующее `YearChanged` событие, `Year` соответствующее типу параметра. `Year`</span><span class="sxs-lookup"><span data-stu-id="1686a-232">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="1686a-233">По соглашению `<ChildComponent @bind-Year="ParentYear" />` , по сути, эквивалентно написанию:</span><span class="sxs-lookup"><span data-stu-id="1686a-233">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="1686a-234">Как правило, свойство может быть привязано к соответствующему обработчику событий `@bind-property:event` с помощью атрибута.</span><span class="sxs-lookup"><span data-stu-id="1686a-234">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="1686a-235">Например, свойство `MyProp` можно привязать к `MyEventHandler` с помощью следующих двух атрибутов:</span><span class="sxs-lookup"><span data-stu-id="1686a-235">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="1686a-236">Обработка событий</span><span class="sxs-lookup"><span data-stu-id="1686a-236">Event handling</span></span>

<span data-ttu-id="1686a-237">Компоненты Razor предоставляют функции обработки событий.</span><span class="sxs-lookup"><span data-stu-id="1686a-237">Razor components provide event handling features.</span></span> <span data-ttu-id="1686a-238">Для атрибута HTML-элемента с `on{event}` именем (например, `onclick` и `onsubmit`) со значением, вводимым с помощью делегата, компоненты Razor рассматривают значение атрибута как обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="1686a-238">For an HTML element attribute named `on{event}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="1686a-239">Имя атрибута всегда имеет формат [ @on{Event}](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="1686a-239">The attribute's name is always formatted [@on{event}](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="1686a-240">Следующий код вызывает метод, `UpdateHeading` когда кнопка выбрана в пользовательском интерфейсе:</span><span class="sxs-lookup"><span data-stu-id="1686a-240">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="1686a-241">Следующий код вызывает `CheckChanged` метод при изменении флажка в пользовательском интерфейсе:</span><span class="sxs-lookup"><span data-stu-id="1686a-241">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="1686a-242">Обработчики событий также могут быть асинхронными и <xref:System.Threading.Tasks.Task>возвращать.</span><span class="sxs-lookup"><span data-stu-id="1686a-242">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="1686a-243">Нет необходимости вручную вызывать `StateHasChanged()`.</span><span class="sxs-lookup"><span data-stu-id="1686a-243">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="1686a-244">Исключения регистрируются при их возникновении.</span><span class="sxs-lookup"><span data-stu-id="1686a-244">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="1686a-245">В следующем примере `UpdateHeading` метод вызывается асинхронно при выборе кнопки:</span><span class="sxs-lookup"><span data-stu-id="1686a-245">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

### <a name="event-argument-types"></a><span data-ttu-id="1686a-246">Типы аргументов событий</span><span class="sxs-lookup"><span data-stu-id="1686a-246">Event argument types</span></span>

<span data-ttu-id="1686a-247">Для некоторых событий разрешены типы аргументов событий.</span><span class="sxs-lookup"><span data-stu-id="1686a-247">For some events, event argument types are permitted.</span></span> <span data-ttu-id="1686a-248">Если доступ к одному из этих типов событий не нужен, в вызове метода это не требуется.</span><span class="sxs-lookup"><span data-stu-id="1686a-248">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="1686a-249">Поддерживаемые [уиевентаргс](https://github.com/aspnet/AspNetCore/blob/release/3.0-preview8/src/Components/Components/src/UIEventArgs.cs) показаны в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="1686a-249">Supported [UIEventArgs](https://github.com/aspnet/AspNetCore/blob/release/3.0-preview8/src/Components/Components/src/UIEventArgs.cs) are shown in the following table.</span></span>

| <span data-ttu-id="1686a-250">событие</span><span class="sxs-lookup"><span data-stu-id="1686a-250">Event</span></span> | <span data-ttu-id="1686a-251">Класс</span><span class="sxs-lookup"><span data-stu-id="1686a-251">Class</span></span> |
| ----- | ----- |
| <span data-ttu-id="1686a-252">буфер обмена</span><span class="sxs-lookup"><span data-stu-id="1686a-252">Clipboard</span></span> | `UIClipboardEventArgs` |
| <span data-ttu-id="1686a-253">Переместить</span><span class="sxs-lookup"><span data-stu-id="1686a-253">Drag</span></span>  | <span data-ttu-id="1686a-254">`UIDragEventArgs`используется для хранения перетаскиваемых данных во время операции перетаскивания и может содержать один или несколько `UIDataTransferItem`. &ndash; `DataTransfer`</span><span class="sxs-lookup"><span data-stu-id="1686a-254">`UIDragEventArgs` &ndash; `DataTransfer` is used to hold the dragged data during a drag and drop operation and may hold one or more `UIDataTransferItem`.</span></span> <span data-ttu-id="1686a-255">`UIDataTransferItem`представляет один элемент данных перетаскивания.</span><span class="sxs-lookup"><span data-stu-id="1686a-255">`UIDataTransferItem` represents one drag data item.</span></span> |
| <span data-ttu-id="1686a-256">Error</span><span class="sxs-lookup"><span data-stu-id="1686a-256">Error</span></span> | `UIErrorEventArgs` |
| <span data-ttu-id="1686a-257">Фокус</span><span class="sxs-lookup"><span data-stu-id="1686a-257">Focus</span></span> | <span data-ttu-id="1686a-258">`UIFocusEventArgs`Не включает поддержку для `relatedTarget`. &ndash;</span><span class="sxs-lookup"><span data-stu-id="1686a-258">`UIFocusEventArgs` &ndash; Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="1686a-259">Изменение`<input>`</span><span class="sxs-lookup"><span data-stu-id="1686a-259">`<input>` change</span></span> | `UIChangeEventArgs` |
| <span data-ttu-id="1686a-260">Клавиатура</span><span class="sxs-lookup"><span data-stu-id="1686a-260">Keyboard</span></span> | `UIKeyboardEventArgs` |
| <span data-ttu-id="1686a-261">Мышь</span><span class="sxs-lookup"><span data-stu-id="1686a-261">Mouse</span></span> | `UIMouseEventArgs` |
| <span data-ttu-id="1686a-262">Указатель мыши</span><span class="sxs-lookup"><span data-stu-id="1686a-262">Mouse pointer</span></span> | `UIPointerEventArgs` |
| <span data-ttu-id="1686a-263">Колесо мыши</span><span class="sxs-lookup"><span data-stu-id="1686a-263">Mouse wheel</span></span> | `UIWheelEventArgs` |
| <span data-ttu-id="1686a-264">Ход выполнения</span><span class="sxs-lookup"><span data-stu-id="1686a-264">Progress</span></span> | `UIProgressEventArgs` |
| <span data-ttu-id="1686a-265">Сенсорные технологии</span><span class="sxs-lookup"><span data-stu-id="1686a-265">Touch</span></span> | <span data-ttu-id="1686a-266">`UITouchEventArgs`&ndash; представляетоднуточкуконтактанаустройствес`UITouchPoint` сенсорным вводом.</span><span class="sxs-lookup"><span data-stu-id="1686a-266">`UITouchEventArgs` &ndash; `UITouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="1686a-267">Сведения о свойствах и поведении событий в приведенной выше таблице см. в разделе [классы EventArgs в исходном справочнике](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview8/src/Components/Web/src).</span><span class="sxs-lookup"><span data-stu-id="1686a-267">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview8/src/Components/Web/src).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="1686a-268">Лямбда-выражения</span><span class="sxs-lookup"><span data-stu-id="1686a-268">Lambda expressions</span></span>

<span data-ttu-id="1686a-269">Лямбда-выражения также могут использоваться:</span><span class="sxs-lookup"><span data-stu-id="1686a-269">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="1686a-270">Часто бывает удобно закрывать дополнительные значения, например при переборе набора элементов.</span><span class="sxs-lookup"><span data-stu-id="1686a-270">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="1686a-271">В следующем примере создаются три кнопки, каждый из которых вызывает `UpdateHeading` передачу аргумента события (`UIMouseEventArgs`) и номер кнопки (`buttonNumber`) при выборе в пользовательском интерфейсе:</span><span class="sxs-lookup"><span data-stu-id="1686a-271">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

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

    private void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="1686a-272">**Не** используйте переменную цикла (`i`) в `for` цикле непосредственно в лямбда-выражении.</span><span class="sxs-lookup"><span data-stu-id="1686a-272">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="1686a-273">В противном случае одна и та же переменная используется всеми `i`лямбда-выражениями, которые вызывают одинаковое значение во всех лямбдаах.</span><span class="sxs-lookup"><span data-stu-id="1686a-273">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="1686a-274">Всегда запишите свое значение в локальную переменную (`buttonNumber` в предыдущем примере), а затем используйте ее.</span><span class="sxs-lookup"><span data-stu-id="1686a-274">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="1686a-275">Вложенный EventCallback</span><span class="sxs-lookup"><span data-stu-id="1686a-275">EventCallback</span></span>

<span data-ttu-id="1686a-276">Распространенным сценарием с вложенными компонентами является желание запускать метод родительского компонента при возникновении&mdash;события дочернего компонента, например `onclick` при возникновении события в дочернем элементе.</span><span class="sxs-lookup"><span data-stu-id="1686a-276">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="1686a-277">Чтобы предоставить события для компонентов, используйте `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="1686a-277">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="1686a-278">Родительский компонент может назначить метод обратного вызова для дочернего `EventCallback`компонента.</span><span class="sxs-lookup"><span data-stu-id="1686a-278">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="1686a-279">В примере приложения `EventCallback` демонстрируется настройка `onclick` обработчика кнопки на получение делегата из примера `ParentComponent`. `ChildComponent`</span><span class="sxs-lookup"><span data-stu-id="1686a-279">The `ChildComponent` in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="1686a-280">Вводится с помощью `UIMouseEventArgs` ,`onclick` что подходит для события с периферийного устройства. `EventCallback`</span><span class="sxs-lookup"><span data-stu-id="1686a-280">The `EventCallback` is typed with `UIMouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="1686a-281">Задает для дочернего `EventCallback<T>` элемента метод:`ShowMessage` `ParentComponent`</span><span class="sxs-lookup"><span data-stu-id="1686a-281">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="1686a-282">При выборе кнопки в `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="1686a-282">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="1686a-283">`ParentComponent` Вызывается`ShowMessage` метод.</span><span class="sxs-lookup"><span data-stu-id="1686a-283">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="1686a-284">`messageText`обновляется и отображается в `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="1686a-284">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="1686a-285">Вызов `StateHasChanged` метода не требуется в методе обратного вызова (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="1686a-285">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="1686a-286">`StateHasChanged`вызывается автоматически для перевизуализации `ParentComponent`компонента, как и в случае, когда дочерние события активируют компонент в обработчиках событий, которые выполняются внутри дочернего элемента.</span><span class="sxs-lookup"><span data-stu-id="1686a-286">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="1686a-287">`EventCallback`и `EventCallback<T>` разрешите асинхронные делегаты.</span><span class="sxs-lookup"><span data-stu-id="1686a-287">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="1686a-288">`EventCallback<T>`является строго типизированным и требует определенного типа аргумента.</span><span class="sxs-lookup"><span data-stu-id="1686a-288">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="1686a-289">`EventCallback`слабо типизирован и допускает любой тип аргумента.</span><span class="sxs-lookup"><span data-stu-id="1686a-289">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="1686a-290">Вызовите `EventCallback<T>` `InvokeAsync` или с параметром и <xref:System.Threading.Tasks.Task>дождаться: `EventCallback`</span><span class="sxs-lookup"><span data-stu-id="1686a-290">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="1686a-291">Используйте `EventCallback` и`EventCallback<T>` для обработки событий и параметров компонента привязки.</span><span class="sxs-lookup"><span data-stu-id="1686a-291">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="1686a-292">Предпочитать строго типизированный `EventCallback<T>`. `EventCallback`</span><span class="sxs-lookup"><span data-stu-id="1686a-292">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="1686a-293">`EventCallback<T>`обеспечивает лучшую реакцию на ошибки для пользователей компонента.</span><span class="sxs-lookup"><span data-stu-id="1686a-293">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="1686a-294">Как и в случае с другими обработчиками событий пользовательского интерфейса, указание параметра события является необязательным.</span><span class="sxs-lookup"><span data-stu-id="1686a-294">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="1686a-295">Используется `EventCallback` при отсутствии значения, переданного обратному вызову.</span><span class="sxs-lookup"><span data-stu-id="1686a-295">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="capture-references-to-components"></a><span data-ttu-id="1686a-296">Запись ссылок на компоненты</span><span class="sxs-lookup"><span data-stu-id="1686a-296">Capture references to components</span></span>

<span data-ttu-id="1686a-297">Ссылки на компоненты предоставляют способ ссылки на экземпляр компонента, чтобы можно было выполнять команды для этого экземпляра, например `Show` или. `Reset`</span><span class="sxs-lookup"><span data-stu-id="1686a-297">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="1686a-298">Чтобы записать ссылку на компонент, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="1686a-298">To capture a component reference:</span></span>

* <span data-ttu-id="1686a-299">[@ref](xref:mvc/views/razor#ref) Добавьте атрибут к дочернему компоненту.</span><span class="sxs-lookup"><span data-stu-id="1686a-299">Add an [@ref](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="1686a-300">Определите поле с тем же типом, что и у дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="1686a-300">Define a field with the same type as the child component.</span></span>

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

<span data-ttu-id="1686a-301">При подготовке компонента к просмотру `loginDialog` поле заполняется `MyLoginDialog` экземпляром дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="1686a-301">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="1686a-302">Затем можно вызывать методы .NET в экземпляре компонента.</span><span class="sxs-lookup"><span data-stu-id="1686a-302">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1686a-303">Переменная заполняется только после подготовки компонента, и ее выходные данные `MyLoginDialog` включают элемент. `loginDialog`</span><span class="sxs-lookup"><span data-stu-id="1686a-303">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="1686a-304">До этого момента нет никаких ссылок на.</span><span class="sxs-lookup"><span data-stu-id="1686a-304">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="1686a-305">Чтобы управлять ссылками на компоненты после завершения подготовки компонента к просмотру `OnAfterRenderAsync` , `OnAfterRender` используйте методы или.</span><span class="sxs-lookup"><span data-stu-id="1686a-305">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<!-- HOLD https://github.com/aspnet/AspNetCore.Docs/pull/13818
Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.

The Razor compiler automatically generates a backing field for element and component references when using [@ref](xref:mvc/views/razor#ref). In the following example, there's no need to create a `myLoginDialog` field for the `LoginDialog` component:

```cshtml
<LoginDialog @ref="myLoginDialog" ... />

@code {
    private void OnSomething()
    {
        myLoginDialog.Show();
    }
}
```

When the component is rendered, the generated `myLoginDialog` field is populated with the `LoginDialog` component instance. You can then invoke .NET methods on the component instance.

In some cases, a backing field is required. For example, declare a backing field when referencing generic components. To suppress backing field generation, specify the `@ref:suppressField` parameter.

> [!IMPORTANT]
> The generated `myLoginDialog` variable is only populated after the component is rendered and its output includes the `LoginDialog` element. Until that point, there's nothing to reference. To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.
-->

<span data-ttu-id="1686a-306">При захвате ссылок на компоненты используется аналогичный синтаксис для [записи ссылок на элементы](xref:blazor/javascript-interop#capture-references-to-elements), но это не функция [взаимодействия JavaScript](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="1686a-306">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="1686a-307">Ссылки на компоненты не передаются&mdash;в код JavaScript, они используются только в коде .NET.</span><span class="sxs-lookup"><span data-stu-id="1686a-307">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="1686a-308">**Не** используйте ссылки на компоненты для изменения состояния дочерних компонентов.</span><span class="sxs-lookup"><span data-stu-id="1686a-308">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="1686a-309">Вместо этого используйте обычные декларативные параметры для передачи данных дочерним компонентам.</span><span class="sxs-lookup"><span data-stu-id="1686a-309">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="1686a-310">Использование обычных декларативных параметров приводит к тому, что дочерние компоненты автоматически визуализируются в нужное время.</span><span class="sxs-lookup"><span data-stu-id="1686a-310">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="1686a-311">Использование \@ключа для управления сохранением элементов и компонентов</span><span class="sxs-lookup"><span data-stu-id="1686a-311">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="1686a-312">При последующем изменении списка элементов или компонентов, а также элементов или компонентов алгоритм сравнения Блазор должен решить, какие из предыдущих элементов или компонентов могут быть сохранены и как объекты модели должны сопоставляться с ними.</span><span class="sxs-lookup"><span data-stu-id="1686a-312">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="1686a-313">Обычно этот процесс выполняется автоматически и его можно игнорировать, но в некоторых случаях может потребоваться управление процессом.</span><span class="sxs-lookup"><span data-stu-id="1686a-313">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="1686a-314">Рассмотрим следующий пример.</span><span class="sxs-lookup"><span data-stu-id="1686a-314">Consider the following example:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="1686a-315">Содержимое `People` коллекции может изменяться при вставке, удалении или повторном упорядочении записей.</span><span class="sxs-lookup"><span data-stu-id="1686a-315">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="1686a-316">Когда компонент перерисовывается, `<DetailsEditor>` компонент может измениться для получения различных `Details` значений параметров.</span><span class="sxs-lookup"><span data-stu-id="1686a-316">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="1686a-317">Это может привести к более сложной визуализации, чем ожидалось.</span><span class="sxs-lookup"><span data-stu-id="1686a-317">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="1686a-318">В некоторых случаях перерисовка может привести к появлению видимых различий в поведении, таких как потеря фокуса элементов.</span><span class="sxs-lookup"><span data-stu-id="1686a-318">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="1686a-319">Процесс сопоставления можно контролировать с помощью `@key` атрибута директивы.</span><span class="sxs-lookup"><span data-stu-id="1686a-319">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="1686a-320">`@key`заставляет алгоритм сравнения гарантировать сохранение элементов или компонентов на основе значения ключа:</span><span class="sxs-lookup"><span data-stu-id="1686a-320">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="@person" Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="1686a-321">При изменении `<DetailsEditor>` `person` коллекции алгоритм сравнения удерживает связи между экземплярами и экземплярами. `People`</span><span class="sxs-lookup"><span data-stu-id="1686a-321">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="1686a-322">Если объект `Person` удаляется `People` из списка, то из пользовательского интерфейса `<DetailsEditor>` удаляется только соответствующий экземпляр.</span><span class="sxs-lookup"><span data-stu-id="1686a-322">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="1686a-323">Другие экземпляры остаются без изменений.</span><span class="sxs-lookup"><span data-stu-id="1686a-323">Other instances are left unchanged.</span></span>
* <span data-ttu-id="1686a-324">Если в `Person` какой-либо позиции в списке вставляется элемент, то `<DetailsEditor>` в соответствующую позиции вставляется один новый экземпляр.</span><span class="sxs-lookup"><span data-stu-id="1686a-324">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="1686a-325">Другие экземпляры остаются без изменений.</span><span class="sxs-lookup"><span data-stu-id="1686a-325">Other instances are left unchanged.</span></span>
* <span data-ttu-id="1686a-326">При `Person` повторном упорядочении записей соответствующие `<DetailsEditor>` экземпляры сохраняются и повторно упорядочиваются в пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="1686a-326">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="1686a-327">В некоторых сценариях использование `@key` сводится к уменьшению сложности повторного отображения и позволяет избежать потенциальных проблем с элементами, изменяющимися с отслеживанием состояния DOM, например положением фокуса.</span><span class="sxs-lookup"><span data-stu-id="1686a-327">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1686a-328">Ключи являются локальными для каждого элемента или компонента контейнера.</span><span class="sxs-lookup"><span data-stu-id="1686a-328">Keys are local to each container element or component.</span></span> <span data-ttu-id="1686a-329">Ключи не сравниваются глобально по всему документу.</span><span class="sxs-lookup"><span data-stu-id="1686a-329">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="1686a-330">Когда следует использовать \@ключ</span><span class="sxs-lookup"><span data-stu-id="1686a-330">When to use \@key</span></span>

<span data-ttu-id="1686a-331">Обычно имеет смысл использовать `@key` каждый раз при подготовке списка (например, `@foreach` в блоке), а подходящее `@key`значение существует для определения.</span><span class="sxs-lookup"><span data-stu-id="1686a-331">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="1686a-332">Кроме того, можно `@key` использовать, чтобы предотвратить блазор с сохранением поддерева элемента или компонента при изменении объекта:</span><span class="sxs-lookup"><span data-stu-id="1686a-332">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="@currentPerson">
    ... content that depends on @currentPerson ...
</div>
```

<span data-ttu-id="1686a-333">Если `@currentPerson` `<div>` изменяется`@key` , директива атрибута заставляет блазор отбросить все и его потомки и перестроить поддерево в пользовательском интерфейсе с помощью новых элементов и компонентов.</span><span class="sxs-lookup"><span data-stu-id="1686a-333">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="1686a-334">Это может быть полезно, если необходимо гарантировать, что при `@currentPerson` изменении состояние пользовательского интерфейса не сохраняется.</span><span class="sxs-lookup"><span data-stu-id="1686a-334">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="1686a-335">Когда не следует использовать \@ключ</span><span class="sxs-lookup"><span data-stu-id="1686a-335">When not to use \@key</span></span>

<span data-ttu-id="1686a-336">При использовании различий в `@key`производительности возникают затраты на производительность.</span><span class="sxs-lookup"><span data-stu-id="1686a-336">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="1686a-337">Затраты на производительность не слишком велики, но указывают `@key` только в том случае, если управление правилами сохранения элементов или компонентов выгодно для приложения.</span><span class="sxs-lookup"><span data-stu-id="1686a-337">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="1686a-338">Даже если `@key` не используется, блазор сохраняет дочерние элементы и экземпляры компонентов насколько это возможно.</span><span class="sxs-lookup"><span data-stu-id="1686a-338">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="1686a-339">Единственным преимуществом использования `@key` является управление *тем, как* экземпляры модели сопоставляются с сохраненными экземплярами компонента, а не алгоритмом сравнения, который выбирает сопоставление.</span><span class="sxs-lookup"><span data-stu-id="1686a-339">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="1686a-340">Какие значения следует использовать для \@ключа</span><span class="sxs-lookup"><span data-stu-id="1686a-340">What values to use for \@key</span></span>

<span data-ttu-id="1686a-341">Как правило, имеет смысл указать одно из следующих видов значений для `@key`:</span><span class="sxs-lookup"><span data-stu-id="1686a-341">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="1686a-342">Экземпляры объектов Model (например, `Person` экземпляр, как в предыдущем примере).</span><span class="sxs-lookup"><span data-stu-id="1686a-342">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="1686a-343">Это гарантирует сохранение на основе равенства ссылок на объекты.</span><span class="sxs-lookup"><span data-stu-id="1686a-343">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="1686a-344">Уникальные идентификаторы (например, значения первичного ключа типа `int`, `string`или `Guid`).</span><span class="sxs-lookup"><span data-stu-id="1686a-344">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="1686a-345">Убедитесь, что значения, `@key` используемые для, не конфликтуют.</span><span class="sxs-lookup"><span data-stu-id="1686a-345">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="1686a-346">Если конфликтные значения обнаруживаются в одном родительском элементе, Блазор создает исключение, поскольку оно не может детерминированно сопоставлять старые элементы или компоненты с новыми элементами или компонентами.</span><span class="sxs-lookup"><span data-stu-id="1686a-346">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="1686a-347">Используйте только уникальные значения, такие как экземпляры объекта или значения первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="1686a-347">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="1686a-348">Методы жизненного цикла</span><span class="sxs-lookup"><span data-stu-id="1686a-348">Lifecycle methods</span></span>

<span data-ttu-id="1686a-349">`OnInitializedAsync`и `OnInitialized` выполняют код для инициализации компонента.</span><span class="sxs-lookup"><span data-stu-id="1686a-349">`OnInitializedAsync` and `OnInitialized` execute code to initialize the component.</span></span> <span data-ttu-id="1686a-350">Для выполнения асинхронной операции используйте `OnInitializedAsync` `await` и ключевое слово для операции:</span><span class="sxs-lookup"><span data-stu-id="1686a-350">To perform an asynchronous operation, use `OnInitializedAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

<span data-ttu-id="1686a-351">Для синхронной операции используйте `OnInitialized`:</span><span class="sxs-lookup"><span data-stu-id="1686a-351">For a synchronous operation, use `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="1686a-352">`OnParametersSetAsync`и `OnParametersSet` вызываются, когда компонент получил параметры от родительского элемента, и значения присваиваются свойствам.</span><span class="sxs-lookup"><span data-stu-id="1686a-352">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="1686a-353">Эти методы выполняются после инициализации компонента и при каждом отображении компонента:</span><span class="sxs-lookup"><span data-stu-id="1686a-353">These methods are executed after component initialization and each time the component is rendered:</span></span>

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

<span data-ttu-id="1686a-354">`OnAfterRenderAsync`и `OnAfterRender` вызываются после завершения подготовки компонента к просмотру.</span><span class="sxs-lookup"><span data-stu-id="1686a-354">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="1686a-355">В этот момент заполнены ссылки на элементы и компоненты.</span><span class="sxs-lookup"><span data-stu-id="1686a-355">Element and component references are populated at this point.</span></span> <span data-ttu-id="1686a-356">Используйте этот этап для выполнения дополнительных шагов инициализации с помощью готового к просмотру содержимого, например для активации сторонних библиотек JavaScript, которые работают с визуализированными элементами DOM.</span><span class="sxs-lookup"><span data-stu-id="1686a-356">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

```csharp
protected override async Task OnAfterRenderAsync()
{
    await ...
}
```

```csharp
protected override void OnAfterRender()
{
    ...
}
```

### <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="1686a-357">Обработка незавершенных асинхронных действий при отрисовке</span><span class="sxs-lookup"><span data-stu-id="1686a-357">Handle incomplete async actions at render</span></span>

<span data-ttu-id="1686a-358">Асинхронные действия, выполняемые в событиях жизненного цикла, могут не завершиться до подготовки компонента к просмотру.</span><span class="sxs-lookup"><span data-stu-id="1686a-358">Asynchronous actions performed in lifecycle events may not have completed before the component is rendered.</span></span> <span data-ttu-id="1686a-359">Объекты могут быть `null` заполнены или незаполнены данными во время выполнения метода жизненного цикла.</span><span class="sxs-lookup"><span data-stu-id="1686a-359">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="1686a-360">Предоставьте логику отрисовки для подтверждения инициализации объектов.</span><span class="sxs-lookup"><span data-stu-id="1686a-360">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="1686a-361">Отрисовывает элементы пользовательского интерфейса заполнителя (например, сообщение загрузки), пока объекты `null`являются объектами.</span><span class="sxs-lookup"><span data-stu-id="1686a-361">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="1686a-362">В компоненте `OnInitializedAsync` шаблонов блазор переопределяется в асичронаусли получения данных прогноза (`forecasts`). `FetchData`</span><span class="sxs-lookup"><span data-stu-id="1686a-362">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="1686a-363">Если `forecasts` параметр `null`имеет значение, пользователю выводится сообщение о загрузке.</span><span class="sxs-lookup"><span data-stu-id="1686a-363">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="1686a-364">После завершения `OnInitializedAsync` возврата по завершении компонент перерисовывается с обновленным состоянием. `Task`</span><span class="sxs-lookup"><span data-stu-id="1686a-364">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="1686a-365">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="1686a-365">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a><span data-ttu-id="1686a-366">Выполнить код до установки параметров</span><span class="sxs-lookup"><span data-stu-id="1686a-366">Execute code before parameters are set</span></span>

<span data-ttu-id="1686a-367">`SetParameters`можно переопределить для выполнения кода перед установкой параметров:</span><span class="sxs-lookup"><span data-stu-id="1686a-367">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterView parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="1686a-368">Если `base.SetParameters` не вызывается, Пользовательский код может интерпретировать значение входящих параметров любым необходимым образом.</span><span class="sxs-lookup"><span data-stu-id="1686a-368">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="1686a-369">Например, входящие параметры не обязательно должны быть назначены свойствам класса.</span><span class="sxs-lookup"><span data-stu-id="1686a-369">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

### <a name="suppress-refreshing-of-the-ui"></a><span data-ttu-id="1686a-370">Отключить обновление пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="1686a-370">Suppress refreshing of the UI</span></span>

<span data-ttu-id="1686a-371">`ShouldRender`можно переопределить, чтобы отключить обновление пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="1686a-371">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="1686a-372">Если реализация возвращает `true`, Пользовательский интерфейс обновляется.</span><span class="sxs-lookup"><span data-stu-id="1686a-372">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="1686a-373">Даже если `ShouldRender` переопределяется, компонент всегда первоначально готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="1686a-373">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="1686a-374">Освобождение компонентов с помощью IDisposable</span><span class="sxs-lookup"><span data-stu-id="1686a-374">Component disposal with IDisposable</span></span>

<span data-ttu-id="1686a-375">Если компонент реализует <xref:System.IDisposable>, [метод Dispose](/dotnet/standard/garbage-collection/implementing-dispose) вызывается при удалении компонента из пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="1686a-375">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="1686a-376">Следующий компонент использует `@implements IDisposable` `Dispose` и метод:</span><span class="sxs-lookup"><span data-stu-id="1686a-376">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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

## <a name="routing"></a><span data-ttu-id="1686a-377">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="1686a-377">Routing</span></span>

<span data-ttu-id="1686a-378">Маршрутизация в Блазор достигается путем предоставления шаблона маршрута для каждого доступного компонента в приложении.</span><span class="sxs-lookup"><span data-stu-id="1686a-378">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="1686a-379">При компиляции файла Razor с `@page` директивой созданному классу <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> присваивается Указание шаблона маршрута.</span><span class="sxs-lookup"><span data-stu-id="1686a-379">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="1686a-380">Во время выполнения маршрутизатор ищет классы компонентов с помощью `RouteAttribute` и отображает, какой из компонентов имеет шаблон маршрута, соответствующий запрошенному URL-адресу.</span><span class="sxs-lookup"><span data-stu-id="1686a-380">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="1686a-381">К компоненту можно применить несколько шаблонов маршрутов.</span><span class="sxs-lookup"><span data-stu-id="1686a-381">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="1686a-382">Следующий компонент отвечает на запросы `/BlazorRoute` и: `/DifferentBlazorRoute`</span><span class="sxs-lookup"><span data-stu-id="1686a-382">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="1686a-383">Параметры маршрута</span><span class="sxs-lookup"><span data-stu-id="1686a-383">Route parameters</span></span>

<span data-ttu-id="1686a-384">Компоненты могут принимать параметры маршрута из шаблона маршрута, указанного в `@page` директиве.</span><span class="sxs-lookup"><span data-stu-id="1686a-384">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="1686a-385">Маршрутизатор использует параметры маршрута для заполнения соответствующих параметров компонента.</span><span class="sxs-lookup"><span data-stu-id="1686a-385">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="1686a-386">*Компонент параметра маршрута*:</span><span class="sxs-lookup"><span data-stu-id="1686a-386">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="1686a-387">Необязательные параметры не поддерживаются `@page` , поэтому в приведенном выше примере применяются две директивы.</span><span class="sxs-lookup"><span data-stu-id="1686a-387">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="1686a-388">Первый позволяет переходить к компоненту без параметра.</span><span class="sxs-lookup"><span data-stu-id="1686a-388">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="1686a-389">Вторая `@page` директива `{text}` принимает параметр Route и присваивает значение `Text` свойству.</span><span class="sxs-lookup"><span data-stu-id="1686a-389">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="1686a-390">Наследование базового класса для взаимодействия с кодом программной части</span><span class="sxs-lookup"><span data-stu-id="1686a-390">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="1686a-391">Файлы компонентов сочетают разметку HTML C# и обрабатывают код в одном файле.</span><span class="sxs-lookup"><span data-stu-id="1686a-391">Component files mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="1686a-392">`@inherits` Директиву можно использовать для предоставления блазор приложений с «кодом программной части», который отделяет разметку компонента от кода обработки.</span><span class="sxs-lookup"><span data-stu-id="1686a-392">The `@inherits` directive can be used to provide Blazor apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="1686a-393">В [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) показано, как компонент может наследовать базовый класс, `BlazorRocksBase`чтобы предоставить свойства и методы компонента.</span><span class="sxs-lookup"><span data-stu-id="1686a-393">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="1686a-394">*Pages/блазорроккс. Razor*:</span><span class="sxs-lookup"><span data-stu-id="1686a-394">*Pages/BlazorRocks.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

<span data-ttu-id="1686a-395">*BlazorRocksBase.CS*:</span><span class="sxs-lookup"><span data-stu-id="1686a-395">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="1686a-396">Базовый класс должен быть производным от `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="1686a-396">The base class should derive from `ComponentBase`.</span></span>

## <a name="import-components"></a><span data-ttu-id="1686a-397">Импорт компонентов</span><span class="sxs-lookup"><span data-stu-id="1686a-397">Import components</span></span>

<span data-ttu-id="1686a-398">Пространство имен компонента, созданного с помощью Razor, основано на следующих:</span><span class="sxs-lookup"><span data-stu-id="1686a-398">The namespace of a component authored with Razor is based on:</span></span>

* <span data-ttu-id="1686a-399">Проект `RootNamespace`.</span><span class="sxs-lookup"><span data-stu-id="1686a-399">The project's `RootNamespace`.</span></span>
* <span data-ttu-id="1686a-400">Путь от корня проекта к компоненту.</span><span class="sxs-lookup"><span data-stu-id="1686a-400">The path from the project root to the component.</span></span> <span data-ttu-id="1686a-401">Например, `ComponentsSample/Pages/Index.razor` находится в пространстве имен `ComponentsSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="1686a-401">For example, `ComponentsSample/Pages/Index.razor` is in the namespace `ComponentsSample.Pages`.</span></span> <span data-ttu-id="1686a-402">Компоненты следуют C# правилам привязки имен.</span><span class="sxs-lookup"><span data-stu-id="1686a-402">Components follow C# name binding rules.</span></span> <span data-ttu-id="1686a-403">В случае *index. Razor*все компоненты в той же папке, *страницах*и родительской папке *компонентссампле*находятся в области действия.</span><span class="sxs-lookup"><span data-stu-id="1686a-403">In the case of *Index.razor*, all components in the same folder, *Pages*, and the parent folder, *ComponentsSample*, are in scope.</span></span>

<span data-ttu-id="1686a-404">Компоненты, определенные в другом пространстве имен, могут быть добавлены в область с помощью директивы [ \@using](xref:mvc/views/razor#using) Razor.</span><span class="sxs-lookup"><span data-stu-id="1686a-404">Components defined in a different namespace can be brought into scope using Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="1686a-405">Если в папке `ComponentsSample/Shared/`существует `NavMenu.razor`другой компонент, то этот компонент можно использовать в `Index.razor` со следующей `@using` инструкцией:</span><span class="sxs-lookup"><span data-stu-id="1686a-405">If another component, `NavMenu.razor`, exists in the folder `ComponentsSample/Shared/`, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using ComponentsSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="1686a-406">На компоненты также можно ссылаться с помощью полных имен, что устраняет необходимость в [ \@](xref:mvc/views/razor#using) директиве using:</span><span class="sxs-lookup"><span data-stu-id="1686a-406">Components can also be referenced using their fully qualified names, which removes the need for the [\@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<ComponentsSample.Shared.NavMenu></ComponentsSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="1686a-407">`global::` Квалификация не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="1686a-407">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="1686a-408">Импорт компонентов с псевдонимами `using` операторов (например, `@using Foo = Bar`) не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="1686a-408">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="1686a-409">Частично определенные имена не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="1686a-409">Partially qualified names aren't supported.</span></span> <span data-ttu-id="1686a-410">Например, добавление `@using ComponentsSample` и создание ссылок `NavMenu.razor` с `<Shared.NavMenu></Shared.NavMenu>` помощью не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="1686a-410">For example, adding `@using ComponentsSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="1686a-411">Атрибуты условного HTML-элемента</span><span class="sxs-lookup"><span data-stu-id="1686a-411">Conditional HTML element attributes</span></span>

<span data-ttu-id="1686a-412">Атрибуты HTML-элемента условно подготавливаются в зависимости от значения .NET.</span><span class="sxs-lookup"><span data-stu-id="1686a-412">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="1686a-413">Если значение равно `false` или `null`, то атрибут не подготавливается к просмотру.</span><span class="sxs-lookup"><span data-stu-id="1686a-413">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="1686a-414">Если значение равно `true`, атрибут отображается в виде сворачивания.</span><span class="sxs-lookup"><span data-stu-id="1686a-414">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="1686a-415">В следующем примере определяет, `IsCompleted` отображается ли `checked` в разметке элемента:</span><span class="sxs-lookup"><span data-stu-id="1686a-415">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="1686a-416">Если `IsCompleted` имеет `true`значение, флажок отображается следующим образом:</span><span class="sxs-lookup"><span data-stu-id="1686a-416">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="1686a-417">Если `IsCompleted` имеет `false`значение, флажок отображается следующим образом:</span><span class="sxs-lookup"><span data-stu-id="1686a-417">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="1686a-418">Дополнительные сведения см. в разделе <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="1686a-418">For more information, see <xref:mvc/views/razor>.</span></span>

## <a name="raw-html"></a><span data-ttu-id="1686a-419">Необработанный HTML</span><span class="sxs-lookup"><span data-stu-id="1686a-419">Raw HTML</span></span>

<span data-ttu-id="1686a-420">Строки обычно подготавливаются с помощью текстовых узлов DOM, что означает, что любая разметка, которую они могут содержать, игнорируется и обрабатывается как литеральный текст.</span><span class="sxs-lookup"><span data-stu-id="1686a-420">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="1686a-421">Для отрисовки необработанного HTML-кода заключите `MarkupString` содержимое HTML в значение.</span><span class="sxs-lookup"><span data-stu-id="1686a-421">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="1686a-422">Значение анализируется как HTML или SVG и вставляется в модель DOM.</span><span class="sxs-lookup"><span data-stu-id="1686a-422">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="1686a-423">Визуализация необработанного HTML-кода, созданного из любого ненадежного источника, является **угрозой безопасности** , и ее следует избегать.</span><span class="sxs-lookup"><span data-stu-id="1686a-423">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="1686a-424">В следующем примере показано использование `MarkupString` типа для добавления блока статического HTML-содержимого в отображаемые выходные данные компонента:</span><span class="sxs-lookup"><span data-stu-id="1686a-424">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="1686a-425">Шаблонные компоненты</span><span class="sxs-lookup"><span data-stu-id="1686a-425">Templated components</span></span>

<span data-ttu-id="1686a-426">Шаблонные компоненты — это компоненты, принимающие один или несколько шаблонов пользовательского интерфейса в качестве параметров, которые затем можно использовать как часть логики отрисовки компонента.</span><span class="sxs-lookup"><span data-stu-id="1686a-426">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="1686a-427">Шаблонные компоненты позволяют создавать более высокие компоненты более высокого уровня, чем обычные компоненты.</span><span class="sxs-lookup"><span data-stu-id="1686a-427">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="1686a-428">Ниже приведены несколько примеров.</span><span class="sxs-lookup"><span data-stu-id="1686a-428">A couple of examples include:</span></span>

* <span data-ttu-id="1686a-429">Компонент таблицы, позволяющий пользователю указывать шаблоны для заголовков, строк и нижних колонтитулов таблицы.</span><span class="sxs-lookup"><span data-stu-id="1686a-429">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="1686a-430">Компонент списка, позволяющий пользователю указать шаблон для отрисовки элементов в списке.</span><span class="sxs-lookup"><span data-stu-id="1686a-430">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="1686a-431">Параметры шаблона</span><span class="sxs-lookup"><span data-stu-id="1686a-431">Template parameters</span></span>

<span data-ttu-id="1686a-432">Шаблонный компонент определяется путем указания одного или нескольких параметров компонента типа `RenderFragment` или. `RenderFragment<T>`</span><span class="sxs-lookup"><span data-stu-id="1686a-432">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="1686a-433">Фрагмент отрисовки представляет сегмент пользовательского интерфейса для отрисовки.</span><span class="sxs-lookup"><span data-stu-id="1686a-433">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="1686a-434">`RenderFragment<T>`принимает параметр типа, который может быть указан при вызове фрагмента прорисовки.</span><span class="sxs-lookup"><span data-stu-id="1686a-434">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="1686a-435">`TableTemplate`см</span><span class="sxs-lookup"><span data-stu-id="1686a-435">`TableTemplate` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

<span data-ttu-id="1686a-436">При использовании компонента шаблона можно указать параметры шаблона с помощью дочерних элементов, совпадающих с именами параметров (`TableHeader` и `RowTemplate` в следующем примере):</span><span class="sxs-lookup"><span data-stu-id="1686a-436">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```cshtml
<TableTemplate Items="@pets">
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

### <a name="template-context-parameters"></a><span data-ttu-id="1686a-437">Параметры контекста шаблона</span><span class="sxs-lookup"><span data-stu-id="1686a-437">Template context parameters</span></span>

<span data-ttu-id="1686a-438">Аргументы компонента типа `RenderFragment<T>` , переданные как элементы, имеют неявный параметр с именем `context` (например, из предыдущего `@context.PetId`примера кода,), но можно изменить имя параметра с `Context` помощью атрибута дочернего элемента дерев.</span><span class="sxs-lookup"><span data-stu-id="1686a-438">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="1686a-439">В следующем примере `RowTemplate` `Context` атрибут элемента задает `pet` параметр:</span><span class="sxs-lookup"><span data-stu-id="1686a-439">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```cshtml
<TableTemplate Items="@pets">
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

<span data-ttu-id="1686a-440">Кроме того, можно указать `Context` атрибут для элемента Component.</span><span class="sxs-lookup"><span data-stu-id="1686a-440">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="1686a-441">Указанный `Context` атрибут применяется ко всем указанным параметрам шаблона.</span><span class="sxs-lookup"><span data-stu-id="1686a-441">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="1686a-442">Это может быть полезно, если необходимо указать имя параметра содержимого для неявного дочернего содержимого (без обертывания дочернего элемента).</span><span class="sxs-lookup"><span data-stu-id="1686a-442">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="1686a-443">В следующем примере `Context` атрибут отображается `TableTemplate` в элементе и применяется ко всем параметрам шаблона:</span><span class="sxs-lookup"><span data-stu-id="1686a-443">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```cshtml
<TableTemplate Items="@pets" Context="pet">
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

### <a name="generic-typed-components"></a><span data-ttu-id="1686a-444">Универсальные типы компонентов</span><span class="sxs-lookup"><span data-stu-id="1686a-444">Generic-typed components</span></span>

<span data-ttu-id="1686a-445">Шаблонные компоненты часто вводятся в универсальном виде.</span><span class="sxs-lookup"><span data-stu-id="1686a-445">Templated components are often generically typed.</span></span> <span data-ttu-id="1686a-446">Например, универсальный `ListViewTemplate` компонент можно использовать для отображения `IEnumerable<T>` значений.</span><span class="sxs-lookup"><span data-stu-id="1686a-446">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="1686a-447">Чтобы определить универсальный компонент, используйте `@typeparam` директиву для указания параметров типа:</span><span class="sxs-lookup"><span data-stu-id="1686a-447">To define a generic component, use the `@typeparam` directive to specify type parameters:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

<span data-ttu-id="1686a-448">При использовании компонентов универсальной типизации параметр типа выводится, если это возможно:</span><span class="sxs-lookup"><span data-stu-id="1686a-448">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="1686a-449">В противном случае параметр типа необходимо явно указать с помощью атрибута, совпадающего с именем параметра типа.</span><span class="sxs-lookup"><span data-stu-id="1686a-449">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="1686a-450">В следующем примере `TItem="Pet"` указывает тип:</span><span class="sxs-lookup"><span data-stu-id="1686a-450">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="1686a-451">Каскадные значения и параметры</span><span class="sxs-lookup"><span data-stu-id="1686a-451">Cascading values and parameters</span></span>

<span data-ttu-id="1686a-452">В некоторых сценариях неудобно передавать данные из компонента-предка в дочерний компонент с помощью [параметров компонента](#component-parameters), особенно при наличии нескольких слоев компонентов.</span><span class="sxs-lookup"><span data-stu-id="1686a-452">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="1686a-453">Каскадные значения и параметры позволяют решить эту проблему, предоставляя удобный способ, позволяющий компоненту-предку предоставить значение всем его дочерним компонентам.</span><span class="sxs-lookup"><span data-stu-id="1686a-453">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="1686a-454">Каскадные значения и параметры также предоставляют подход для координации компонентов.</span><span class="sxs-lookup"><span data-stu-id="1686a-454">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="1686a-455">Пример темы</span><span class="sxs-lookup"><span data-stu-id="1686a-455">Theme example</span></span>

<span data-ttu-id="1686a-456">В следующем примере из примера приложения `ThemeInfo` класс указывает сведения о теме для перетекания иерархии компонентов, чтобы все кнопки в пределах данной части приложения совпадали с тем же стилем.</span><span class="sxs-lookup"><span data-stu-id="1686a-456">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="1686a-457">*Уисемеклассес/themeinfo указывает расположение. CS*:</span><span class="sxs-lookup"><span data-stu-id="1686a-457">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="1686a-458">Компонент-предок может предоставлять каскадное значение с помощью компонента каскадного значения.</span><span class="sxs-lookup"><span data-stu-id="1686a-458">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="1686a-459">`CascadingValue` Компонент заключает поддерево иерархии компонентов и предоставляет одно значение всем компонентам в этом поддереве.</span><span class="sxs-lookup"><span data-stu-id="1686a-459">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="1686a-460">Например, в примере приложения указываются сведения о теме`ThemeInfo`() в одном из макетов приложения в виде каскадного параметра для всех компонентов, составляющих текст `@Body` макета свойства.</span><span class="sxs-lookup"><span data-stu-id="1686a-460">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="1686a-461">`ButtonClass`присваивается значение `btn-success` в компоненте макета.</span><span class="sxs-lookup"><span data-stu-id="1686a-461">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="1686a-462">Любой компонент-потомок может использовать это свойство через `ThemeInfo` каскадный объект.</span><span class="sxs-lookup"><span data-stu-id="1686a-462">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="1686a-463">`CascadingValuesParametersLayout`см</span><span class="sxs-lookup"><span data-stu-id="1686a-463">`CascadingValuesParametersLayout` component:</span></span>

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="@theme">
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

<span data-ttu-id="1686a-464">Чтобы использовать каскадные значения, компоненты объявляют каскадные параметры с помощью `[CascadingParameter]` атрибута или на основе значения имени строки:</span><span class="sxs-lookup"><span data-stu-id="1686a-464">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute or based on a string name value:</span></span>

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

<span data-ttu-id="1686a-465">Привязка со значением имени строки уместна, если имеется несколько каскадных значений одного типа и необходимо отличать их друг от друга в одном поддереве.</span><span class="sxs-lookup"><span data-stu-id="1686a-465">Binding with a string name value is relevant if you have multiple cascading values of the same type and need to differentiate them within the same subtree.</span></span>

<span data-ttu-id="1686a-466">Каскадные значения привязываются к каскадным параметрам по типу.</span><span class="sxs-lookup"><span data-stu-id="1686a-466">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="1686a-467">В примере приложения `CascadingValuesParametersTheme` компонент привязывает `ThemeInfo` каскадное значение к каскадному параметру.</span><span class="sxs-lookup"><span data-stu-id="1686a-467">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="1686a-468">Параметр используется для задания класса CSS для одной из кнопок, отображаемых компонентом.</span><span class="sxs-lookup"><span data-stu-id="1686a-468">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="1686a-469">`CascadingValuesParametersTheme`см</span><span class="sxs-lookup"><span data-stu-id="1686a-469">`CascadingValuesParametersTheme` component:</span></span>

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

### <a name="tabset-example"></a><span data-ttu-id="1686a-470">Пример Табсет</span><span class="sxs-lookup"><span data-stu-id="1686a-470">TabSet example</span></span>

<span data-ttu-id="1686a-471">Каскадные параметры также позволяют компонентам работать в иерархии компонентов.</span><span class="sxs-lookup"><span data-stu-id="1686a-471">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="1686a-472">Например, рассмотрим следующий пример *табсет* в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="1686a-472">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="1686a-473">Пример приложения имеет интерфейс, `ITab` который реализуется с помощью вкладок:</span><span class="sxs-lookup"><span data-stu-id="1686a-473">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="1686a-474">Компонент использует компонент, который содержит несколько `Tab` компонентов: `TabSet` `CascadingValuesParametersTabSet`</span><span class="sxs-lookup"><span data-stu-id="1686a-474">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="1686a-475">Дочерние `Tab` компоненты явно не передаются в `TabSet`качестве параметров в.</span><span class="sxs-lookup"><span data-stu-id="1686a-475">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="1686a-476">Вместо этого дочерние `Tab` компоненты являются частью дочернего содержимого `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="1686a-476">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="1686a-477">Тем не менее `TabSet` , по-прежнему необходимо узнать `Tab` о каждом компоненте, чтобы он мог отобразить заголовки и активную вкладку. Чтобы обеспечить эту координацию, не требуя дополнительного `TabSet` кода, компонент *может предоставить себя как каскадное значение* , которое затем будет использоваться компонентами- `Tab` потомками.</span><span class="sxs-lookup"><span data-stu-id="1686a-477">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="1686a-478">`TabSet`см</span><span class="sxs-lookup"><span data-stu-id="1686a-478">`TabSet` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

<span data-ttu-id="1686a-479">Компоненты- `Tab` потомки захватывают `TabSet` объект, содержащийся в виде каскадного `Tab` параметра, поэтому компоненты добавляются `TabSet` в и координаты, на которой активна вкладка.</span><span class="sxs-lookup"><span data-stu-id="1686a-479">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="1686a-480">`Tab`см</span><span class="sxs-lookup"><span data-stu-id="1686a-480">`Tab` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="1686a-481">Шаблоны Razor</span><span class="sxs-lookup"><span data-stu-id="1686a-481">Razor templates</span></span>

<span data-ttu-id="1686a-482">Фрагменты визуализации можно определить с помощью синтаксиса шаблона Razor.</span><span class="sxs-lookup"><span data-stu-id="1686a-482">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="1686a-483">Шаблоны Razor позволяют определить фрагмент пользовательского интерфейса и принять следующий формат:</span><span class="sxs-lookup"><span data-stu-id="1686a-483">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="1686a-484">В следующем примере показано, как задавать `RenderFragment` значения `RenderFragment<T>` и, а также подготавливать шаблоны непосредственно в компоненте.</span><span class="sxs-lookup"><span data-stu-id="1686a-484">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="1686a-485">Фрагменты рендеринга также могут передаваться в качестве аргументов в [шаблонные компоненты](#templated-components).</span><span class="sxs-lookup"><span data-stu-id="1686a-485">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

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

<span data-ttu-id="1686a-486">Отображаемые выходные данные предыдущего кода:</span><span class="sxs-lookup"><span data-stu-id="1686a-486">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="1686a-487">Логика Рендертрибуилдер вручную</span><span class="sxs-lookup"><span data-stu-id="1686a-487">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="1686a-488">`Microsoft.AspNetCore.Components.RenderTree`предоставляет методы для управления компонентами и элементами, включая создание компонентов вручную в C# коде.</span><span class="sxs-lookup"><span data-stu-id="1686a-488">`Microsoft.AspNetCore.Components.RenderTree` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="1686a-489">`RenderTreeBuilder` Использование для создания компонентов является расширенным сценарием.</span><span class="sxs-lookup"><span data-stu-id="1686a-489">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="1686a-490">Неправильно сформированный компонент (например, незакрытый тег разметки) может привести к неопределенному поведению.</span><span class="sxs-lookup"><span data-stu-id="1686a-490">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="1686a-491">Рассмотрим следующий `PetDetails` компонент, который можно вручную встроить в другой компонент:</span><span class="sxs-lookup"><span data-stu-id="1686a-491">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="1686a-492">В следующем примере цикл в `CreateComponent` методе создает три `PetDetails` компонента.</span><span class="sxs-lookup"><span data-stu-id="1686a-492">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="1686a-493">При вызове `RenderTreeBuilder` методов для создания компонентов (`OpenComponent` и `AddAttribute`) порядковые номера представляют собой номера строк исходного кода.</span><span class="sxs-lookup"><span data-stu-id="1686a-493">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="1686a-494">Алгоритм разницы Блазор зависит от порядковых номеров, соответствующих разным строкам кода, а не отдельных вызовов вызова.</span><span class="sxs-lookup"><span data-stu-id="1686a-494">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="1686a-495">При создании компонента с `RenderTreeBuilder` методами жестко указывайте аргументы для порядковых номеров.</span><span class="sxs-lookup"><span data-stu-id="1686a-495">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="1686a-496">**Использование вычисления или счетчика для формирования порядкового номера может привести к снижению производительности.**</span><span class="sxs-lookup"><span data-stu-id="1686a-496">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="1686a-497">Дополнительные сведения см. в разделе [порядковые номера относятся к разделу номера строк кода и порядок выполнения](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="1686a-497">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="1686a-498">`BuiltContent`см</span><span class="sxs-lookup"><span data-stu-id="1686a-498">`BuiltContent` component:</span></span>

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

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="1686a-499">Порядковые номера связаны с номерами строк кода, а не с порядком выполнения</span><span class="sxs-lookup"><span data-stu-id="1686a-499">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="1686a-500">Файлы `.razor` блазор всегда компилируются.</span><span class="sxs-lookup"><span data-stu-id="1686a-500">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="1686a-501">Это может быть отличным преимуществом `.razor` , поскольку этап компиляции можно использовать для вставки сведений, повышающих производительность приложения во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="1686a-501">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="1686a-502">Ключевым примером этих улучшений являются *порядковые номера*.</span><span class="sxs-lookup"><span data-stu-id="1686a-502">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="1686a-503">Порядковые номера указывают среде выполнения, какие выходные данные поступили из разных и упорядоченных строк кода.</span><span class="sxs-lookup"><span data-stu-id="1686a-503">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="1686a-504">Среда выполнения использует эти сведения для создания эффективных различий дерева в линейное время, что является гораздо быстрее, чем обычно возможно для общего алгоритма различения дерева.</span><span class="sxs-lookup"><span data-stu-id="1686a-504">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="1686a-505">Рассмотрим следующий простой `.razor` файл:</span><span class="sxs-lookup"><span data-stu-id="1686a-505">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="1686a-506">Предыдущий код компилируется примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="1686a-506">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="1686a-507">Когда код выполняется в первый раз, если `someFlag` имеет `true`значение, построитель получает:</span><span class="sxs-lookup"><span data-stu-id="1686a-507">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="1686a-508">Sequence</span><span class="sxs-lookup"><span data-stu-id="1686a-508">Sequence</span></span> | <span data-ttu-id="1686a-509">Тип</span><span class="sxs-lookup"><span data-stu-id="1686a-509">Type</span></span>      | <span data-ttu-id="1686a-510">Данные</span><span class="sxs-lookup"><span data-stu-id="1686a-510">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="1686a-511">0</span><span class="sxs-lookup"><span data-stu-id="1686a-511">0</span></span>        | <span data-ttu-id="1686a-512">Текстовый узел</span><span class="sxs-lookup"><span data-stu-id="1686a-512">Text node</span></span> | <span data-ttu-id="1686a-513">Первая</span><span class="sxs-lookup"><span data-stu-id="1686a-513">First</span></span>  |
| <span data-ttu-id="1686a-514">1</span><span class="sxs-lookup"><span data-stu-id="1686a-514">1</span></span>        | <span data-ttu-id="1686a-515">Текстовый узел</span><span class="sxs-lookup"><span data-stu-id="1686a-515">Text node</span></span> | <span data-ttu-id="1686a-516">Вторая</span><span class="sxs-lookup"><span data-stu-id="1686a-516">Second</span></span> |

<span data-ttu-id="1686a-517">Представьте, `someFlag` что `false`преобразуются, и разметка снова готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="1686a-517">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="1686a-518">На этот раз построитель получит:</span><span class="sxs-lookup"><span data-stu-id="1686a-518">This time, the builder receives:</span></span>

| <span data-ttu-id="1686a-519">Sequence</span><span class="sxs-lookup"><span data-stu-id="1686a-519">Sequence</span></span> | <span data-ttu-id="1686a-520">Тип</span><span class="sxs-lookup"><span data-stu-id="1686a-520">Type</span></span>       | <span data-ttu-id="1686a-521">Данные</span><span class="sxs-lookup"><span data-stu-id="1686a-521">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="1686a-522">1</span><span class="sxs-lookup"><span data-stu-id="1686a-522">1</span></span>        | <span data-ttu-id="1686a-523">Текстовый узел</span><span class="sxs-lookup"><span data-stu-id="1686a-523">Text node</span></span>  | <span data-ttu-id="1686a-524">Вторая</span><span class="sxs-lookup"><span data-stu-id="1686a-524">Second</span></span> |

<span data-ttu-id="1686a-525">Когда среда выполнения выполняет поиск различий, она видит, что элемент в `0` последовательности был удален, поэтому он создает следующий тривиальный *сценарий редактирования*:</span><span class="sxs-lookup"><span data-stu-id="1686a-525">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="1686a-526">Удалите первый текстовый узел.</span><span class="sxs-lookup"><span data-stu-id="1686a-526">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="1686a-527">Что становится неправильным, если вы создаете порядковые номера программным способом</span><span class="sxs-lookup"><span data-stu-id="1686a-527">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="1686a-528">Вместо этого Представьте себе, что вы написали следующую логику конструктора деревьев визуализации:</span><span class="sxs-lookup"><span data-stu-id="1686a-528">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="1686a-529">Теперь первые выходные данные:</span><span class="sxs-lookup"><span data-stu-id="1686a-529">Now, the first output is:</span></span>

| <span data-ttu-id="1686a-530">Sequence</span><span class="sxs-lookup"><span data-stu-id="1686a-530">Sequence</span></span> | <span data-ttu-id="1686a-531">Тип</span><span class="sxs-lookup"><span data-stu-id="1686a-531">Type</span></span>      | <span data-ttu-id="1686a-532">Данные</span><span class="sxs-lookup"><span data-stu-id="1686a-532">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="1686a-533">0</span><span class="sxs-lookup"><span data-stu-id="1686a-533">0</span></span>        | <span data-ttu-id="1686a-534">Текстовый узел</span><span class="sxs-lookup"><span data-stu-id="1686a-534">Text node</span></span> | <span data-ttu-id="1686a-535">Первая</span><span class="sxs-lookup"><span data-stu-id="1686a-535">First</span></span>  |
| <span data-ttu-id="1686a-536">1</span><span class="sxs-lookup"><span data-stu-id="1686a-536">1</span></span>        | <span data-ttu-id="1686a-537">Текстовый узел</span><span class="sxs-lookup"><span data-stu-id="1686a-537">Text node</span></span> | <span data-ttu-id="1686a-538">Вторая</span><span class="sxs-lookup"><span data-stu-id="1686a-538">Second</span></span> |

<span data-ttu-id="1686a-539">Этот результат идентичен предыдущему случаю, поэтому отрицательные проблемы не возникают.</span><span class="sxs-lookup"><span data-stu-id="1686a-539">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="1686a-540">`someFlag`находится `false` во второй визуализации, а выходные данные:</span><span class="sxs-lookup"><span data-stu-id="1686a-540">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="1686a-541">Sequence</span><span class="sxs-lookup"><span data-stu-id="1686a-541">Sequence</span></span> | <span data-ttu-id="1686a-542">Тип</span><span class="sxs-lookup"><span data-stu-id="1686a-542">Type</span></span>      | <span data-ttu-id="1686a-543">Данные</span><span class="sxs-lookup"><span data-stu-id="1686a-543">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="1686a-544">0</span><span class="sxs-lookup"><span data-stu-id="1686a-544">0</span></span>        | <span data-ttu-id="1686a-545">Текстовый узел</span><span class="sxs-lookup"><span data-stu-id="1686a-545">Text node</span></span> | <span data-ttu-id="1686a-546">Вторая</span><span class="sxs-lookup"><span data-stu-id="1686a-546">Second</span></span> |

<span data-ttu-id="1686a-547">На этот раз алгоритм diff видит, что были внесены *два* изменения, и алгоритм создает следующий сценарий редактирования:</span><span class="sxs-lookup"><span data-stu-id="1686a-547">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="1686a-548">Измените значение первого текстового узла на `Second`.</span><span class="sxs-lookup"><span data-stu-id="1686a-548">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="1686a-549">Удалите второй текстовый узел.</span><span class="sxs-lookup"><span data-stu-id="1686a-549">Remove the second text node.</span></span>

<span data-ttu-id="1686a-550">При формировании порядковых номеров теряются все полезные сведения о том `if/else` , где находятся ветви и циклы в исходном коде.</span><span class="sxs-lookup"><span data-stu-id="1686a-550">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="1686a-551">Это приводит к удвоению в **два раза больше** , чем раньше.</span><span class="sxs-lookup"><span data-stu-id="1686a-551">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="1686a-552">Это тривиальный пример.</span><span class="sxs-lookup"><span data-stu-id="1686a-552">This is a trivial example.</span></span> <span data-ttu-id="1686a-553">В более реалистичных случаях со сложными и глубокими вложенными структурами, особенно с циклами, затраты на производительность более серьезны.</span><span class="sxs-lookup"><span data-stu-id="1686a-553">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="1686a-554">Вместо того, чтобы сразу определять, какие блоки или ветви цикла были вставлены или удалены, алгоритм diff должен рекурсивно пребираться в деревьях отрисовки и, как правило, создает гораздо более длительные операции редактирования, так как он сообщает о том, как старую и новую структуры связь друг с другом.</span><span class="sxs-lookup"><span data-stu-id="1686a-554">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="1686a-555">Руководство и выводы</span><span class="sxs-lookup"><span data-stu-id="1686a-555">Guidance and conclusions</span></span>

* <span data-ttu-id="1686a-556">Производительность приложения снижается, если порядковые номера создаются динамически.</span><span class="sxs-lookup"><span data-stu-id="1686a-556">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="1686a-557">Платформа не может автоматически создавать собственные порядковые номера во время выполнения, поскольку необходимая информация не существует, если она не захвачена во время компиляции.</span><span class="sxs-lookup"><span data-stu-id="1686a-557">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="1686a-558">Не записывайте длинные блоки логики, реализованной `RenderTreeBuilder` вручную.</span><span class="sxs-lookup"><span data-stu-id="1686a-558">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="1686a-559">Предпочитать `.razor` файлы и разрешите компилятору работать с порядковыми номерами.</span><span class="sxs-lookup"><span data-stu-id="1686a-559">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span>
* <span data-ttu-id="1686a-560">Если порядковые номера задаются жестко, то для алгоритма diff требуется, чтобы только порядковые номера увеличиваются в значении.</span><span class="sxs-lookup"><span data-stu-id="1686a-560">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="1686a-561">Начальное значение и зазоры несущественны.</span><span class="sxs-lookup"><span data-stu-id="1686a-561">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="1686a-562">Один из этих вариантов — использовать номер строки кода в качестве порядкового номера или начать с нуля и увеличить на единицу или сотни (или любой другой интервал).</span><span class="sxs-lookup"><span data-stu-id="1686a-562">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* <span data-ttu-id="1686a-563">Блазор использует порядковые номера, а другие платформы пользовательского интерфейса для различения структуры не используют их.</span><span class="sxs-lookup"><span data-stu-id="1686a-563">Blazor uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="1686a-564">Сравнение выполняется гораздо быстрее, если используются порядковые номера, и блазор имеет преимущество этапа компиляции, который автоматически обрабатывает порядковые номера для разработчиков `.razor` файлов.</span><span class="sxs-lookup"><span data-stu-id="1686a-564">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring `.razor` files.</span></span>

## <a name="localization"></a><span data-ttu-id="1686a-565">Локализация</span><span class="sxs-lookup"><span data-stu-id="1686a-565">Localization</span></span>

<span data-ttu-id="1686a-566">Блазор приложения на стороне сервера локализованы с использованием по [промежуточного слоя локализации](xref:fundamentals/localization#localization-middleware).</span><span class="sxs-lookup"><span data-stu-id="1686a-566">Blazor server-side apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="1686a-567">По промежуточного слоя выбирает соответствующие языки и региональные параметры для пользователей, запрашивающих ресурсы из приложения.</span><span class="sxs-lookup"><span data-stu-id="1686a-567">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="1686a-568">Язык и региональные параметры можно задать с помощью одного из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="1686a-568">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="1686a-569">Файлы "cookie"</span><span class="sxs-lookup"><span data-stu-id="1686a-569">Cookies</span></span>](#cookies)
* [<span data-ttu-id="1686a-570">Предоставление пользовательского интерфейса для выбора языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="1686a-570">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="1686a-571">Дополнительные сведения и примеры см. в разделе <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="1686a-571">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="1686a-572">Файлы cookie</span><span class="sxs-lookup"><span data-stu-id="1686a-572">Cookies</span></span>

<span data-ttu-id="1686a-573">Файл cookie культуры локализации может сохранять язык и региональные параметры пользователя.</span><span class="sxs-lookup"><span data-stu-id="1686a-573">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="1686a-574">Файл cookie создается `OnGet` методом страницы узла приложения (*pages/Host. cshtml. CS*).</span><span class="sxs-lookup"><span data-stu-id="1686a-574">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="1686a-575">По промежуточного слоя локализации считывает файл cookie при последующих запросах, чтобы задать язык и региональные параметры пользователя.</span><span class="sxs-lookup"><span data-stu-id="1686a-575">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="1686a-576">Использование файла cookie гарантирует, что соединение WebSocket сможет правильно распространить культуру.</span><span class="sxs-lookup"><span data-stu-id="1686a-576">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="1686a-577">Если схемы локализации основаны на пути URL-адреса или строке запроса, схема может не поддерживать работу с WebSockets, поэтому она не сохраняет культуру.</span><span class="sxs-lookup"><span data-stu-id="1686a-577">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="1686a-578">Поэтому рекомендуемым подходом является использование файла cookie языка и региональных параметров локализации.</span><span class="sxs-lookup"><span data-stu-id="1686a-578">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="1686a-579">Любой метод можно использовать для назначения языка и региональных параметров, если язык и региональные параметры сохраняются в файле cookie локализации.</span><span class="sxs-lookup"><span data-stu-id="1686a-579">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="1686a-580">Если у приложения уже есть установленная схема локализации для ASP.NET Core на стороне сервера, продолжайте использовать существующую инфраструктуру локализации приложения и задавайте файл cookie культуры локализации в схеме приложения.</span><span class="sxs-lookup"><span data-stu-id="1686a-580">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="1686a-581">В следующем примере показано, как задать текущий язык и региональные параметры в файле cookie, который может быть прочитан по промежуточного слоя локализации.</span><span class="sxs-lookup"><span data-stu-id="1686a-581">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="1686a-582">Создайте файл *pages/Host. cshtml. CS* со следующим содержимым в приложении блазор на стороне сервера:</span><span class="sxs-lookup"><span data-stu-id="1686a-582">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor server-side app:</span></span>

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

<span data-ttu-id="1686a-583">Локализация обрабатывается в приложении:</span><span class="sxs-lookup"><span data-stu-id="1686a-583">Localization is handled in the app:</span></span>

1. <span data-ttu-id="1686a-584">Браузер отправляет в приложение исходный HTTP-запрос.</span><span class="sxs-lookup"><span data-stu-id="1686a-584">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="1686a-585">Язык и региональные параметры назначаются по промежуточного слоя локализации.</span><span class="sxs-lookup"><span data-stu-id="1686a-585">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="1686a-586">Метод в *_Host. cshtml. CS* сохраняет язык и региональные параметры в файле cookie как часть ответа. `OnGet`</span><span class="sxs-lookup"><span data-stu-id="1686a-586">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="1686a-587">Браузер открывает подключение WebSocket для создания интерактивного сеанса Блазор на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="1686a-587">The browser opens a WebSocket connection to create an interactive Blazor server-side session.</span></span>
1. <span data-ttu-id="1686a-588">По промежуточного слоя для локализации считывает файл cookie и назначает язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="1686a-588">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="1686a-589">Сеанс на стороне сервера Блазор начинается с правильного языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="1686a-589">The Blazor server-side session begins with the correct culture.</span></span>

## <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="1686a-590">Предоставление пользовательского интерфейса для выбора языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="1686a-590">Provide UI to choose the culture</span></span>

<span data-ttu-id="1686a-591">Для предоставления пользовательского интерфейса, позволяющего пользователю выбирать язык и региональные параметры, рекомендуется *подход на основе перенаправления* .</span><span class="sxs-lookup"><span data-stu-id="1686a-591">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="1686a-592">Процесс аналогичен тому, что происходит в веб-приложении, когда пользователь пытается получить доступ к защищенному ресурсу&mdash;, пользователь перенаправляется на страницу входа, а затем перенаправляется обратно к исходному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="1686a-592">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="1686a-593">Приложение сохраняет выбранный язык и региональные параметры пользователя с помощью перенаправления к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="1686a-593">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="1686a-594">Контроллер устанавливает выбранный язык и региональные параметры пользователя в файл cookie и перенаправляет пользователя обратно к исходному коду URI.</span><span class="sxs-lookup"><span data-stu-id="1686a-594">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="1686a-595">Установите конечную точку HTTP на сервере, чтобы задать выбранный язык и региональные параметры пользователя в файле cookie, и выполните перенаправление обратно в исходный URI:</span><span class="sxs-lookup"><span data-stu-id="1686a-595">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

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
> <span data-ttu-id="1686a-596">Используйте результат `LocalRedirect` действия, чтобы предотвратить атаки с открытым перенаправлением.</span><span class="sxs-lookup"><span data-stu-id="1686a-596">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="1686a-597">Дополнительные сведения см. в разделе <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="1686a-597">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="1686a-598">В следующем компоненте показан пример выполнения начального перенаправления, когда пользователь выбирает язык и региональные параметры:</span><span class="sxs-lookup"><span data-stu-id="1686a-598">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```cshtml
@inject IUriHelper UriHelper

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private double textNumber;

    private void OnSelected(UIChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(UriHelper.GetAbsoluteUri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        UriHelper.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

### <a name="use-net-localization-scenarios-in-blazor-apps"></a><span data-ttu-id="1686a-599">Использование сценариев локализации .NET в приложениях Блазор</span><span class="sxs-lookup"><span data-stu-id="1686a-599">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="1686a-600">В приложениях Блазор доступны следующие сценарии локализации и глобализации .NET:</span><span class="sxs-lookup"><span data-stu-id="1686a-600">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="1686a-601">. Система ресурсов NET</span><span class="sxs-lookup"><span data-stu-id="1686a-601">.NET's resources system</span></span>
* <span data-ttu-id="1686a-602">Форматирование чисел и дат, зависящих от языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="1686a-602">Culture-specific number and date formatting</span></span>

<span data-ttu-id="1686a-603">`@bind` Функция блазор выполняет глобализацию на основе текущего языка и региональных параметров пользователя.</span><span class="sxs-lookup"><span data-stu-id="1686a-603">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="1686a-604">Дополнительные сведения см. в разделе [Привязка данных](#data-binding) .</span><span class="sxs-lookup"><span data-stu-id="1686a-604">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="1686a-605">В настоящее время поддерживается ограниченный набор сценариев локализации ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="1686a-605">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="1686a-606">`IStringLocalizer<>`*поддерживается* в приложениях блазор.</span><span class="sxs-lookup"><span data-stu-id="1686a-606">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="1686a-607">`IHtmlLocalizer<>`, `IViewLocalizer<>`и локализация заметок к данным ASP.NET Core сценариев MVC и **не поддерживается** в приложениях блазор.</span><span class="sxs-lookup"><span data-stu-id="1686a-607">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="1686a-608">Дополнительные сведения см. в разделе <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="1686a-608">For more information, see <xref:fundamentals/localization>.</span></span>
