---
title: Создание и использование компонентов ASP.NET Core Razor
author: guardrex
description: Узнайте, как создавать и использовать компоненты Razor, включая способы привязки к данным, обработки событий и управления жизненным циклом компонентов.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: blazor/components
ms.openlocfilehash: e51f6745f6e0c748e51d7f8a49193f3d81fd2a06
ms.sourcegitcommit: 07cd66e367d080acb201c7296809541599c947d1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/17/2019
ms.locfileid: "71039184"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="4a297-103">Создание и использование компонентов ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="4a297-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="4a297-104">[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="4a297-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="4a297-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4a297-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4a297-106">Приложения блазор создаются с помощью *компонентов*.</span><span class="sxs-lookup"><span data-stu-id="4a297-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="4a297-107">Компонент — это автономный фрагмент пользовательского интерфейса, например страница, диалоговое окно или форма.</span><span class="sxs-lookup"><span data-stu-id="4a297-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="4a297-108">Компонент включает разметку HTML и логику обработки, необходимую для вставки данных или реагирования на события пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="4a297-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="4a297-109">Компоненты являются гибкими и легковесными.</span><span class="sxs-lookup"><span data-stu-id="4a297-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="4a297-110">Они могут быть вложенными, повторно использованы и совместно использоваться проектами.</span><span class="sxs-lookup"><span data-stu-id="4a297-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="4a297-111">Классы компонентов</span><span class="sxs-lookup"><span data-stu-id="4a297-111">Component classes</span></span>

<span data-ttu-id="4a297-112">Компоненты реализуются в файлах компонентов [Razor](xref:mvc/views/razor) ( *. Razor*) с помощью комбинации C# и HTML-разметки.</span><span class="sxs-lookup"><span data-stu-id="4a297-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="4a297-113">Компонент в Блазор формально называется *компонентом Razor*.</span><span class="sxs-lookup"><span data-stu-id="4a297-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="4a297-114">Имя компонента должно начинаться с символа верхнего регистра.</span><span class="sxs-lookup"><span data-stu-id="4a297-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="4a297-115">Например, *микулкомпонент. Razor* является допустимым, а *микулкомпонент. Razor* является недопустимым.</span><span class="sxs-lookup"><span data-stu-id="4a297-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="4a297-116">Компоненты можно создавать с помощью расширения файла *. cshtml* , если файлы определены как файлы компонентов Razor с помощью `_RazorComponentInclude` свойства MSBuild.</span><span class="sxs-lookup"><span data-stu-id="4a297-116">Components can be authored using the *.cshtml* file extension as long as the files are identified as Razor component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="4a297-117">Например, приложение, которое указывает, что все файлы *. cshtml* в папке *pages* должны рассматриваться как файлы компонентов Razor:</span><span class="sxs-lookup"><span data-stu-id="4a297-117">For example, an app that specifies that all *.cshtml* files under the *Pages* folder should be treated as Razor components files:</span></span>

```xml
<PropertyGroup>
  <_RazorComponentInclude>Pages\**\*.cshtml</_RazorComponentInclude>
</PropertyGroup>
```

<span data-ttu-id="4a297-118">Пользовательский интерфейс для компонента определяется с помощью HTML.</span><span class="sxs-lookup"><span data-stu-id="4a297-118">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="4a297-119">Логика динамического отображения (например выражения, циклы и условные выражения) добавляется с помощью встроенного синтаксиса C# под названием [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="4a297-119">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="4a297-120">При компиляции приложения логика разметки и C# отрисовки HTML преобразуется в класс компонента.</span><span class="sxs-lookup"><span data-stu-id="4a297-120">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="4a297-121">Имя созданного класса соответствует имени файла.</span><span class="sxs-lookup"><span data-stu-id="4a297-121">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="4a297-122">Элементы класса компонента определяются в блоке `@code`.</span><span class="sxs-lookup"><span data-stu-id="4a297-122">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="4a297-123">`@code` В блоке состояние компонента (свойства, поля) указывается с помощью методов обработки событий или для определения другой логики компонента.</span><span class="sxs-lookup"><span data-stu-id="4a297-123">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="4a297-124">Допускается более одного блока `@code`.</span><span class="sxs-lookup"><span data-stu-id="4a297-124">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="4a297-125">В предыдущих предварительных версиях ASP.NET Core 3,0 `@functions` блоки использовались для тех же целей, что `@code` и блоки в компонентах Razor.</span><span class="sxs-lookup"><span data-stu-id="4a297-125">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="4a297-126">`@functions`блоки продолжают функционировать в компонентах Razor, но мы рекомендуем использовать `@code` блок в ASP.NET Core 3,0 Preview 6 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="4a297-126">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="4a297-127">Члены компонента можно использовать как часть логики отрисовки компонента с помощью C# выражений, начинающихся с `@`.</span><span class="sxs-lookup"><span data-stu-id="4a297-127">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="4a297-128">Например, C# поле подготавливается к просмотру путем `@` добавления префикса к имени поля.</span><span class="sxs-lookup"><span data-stu-id="4a297-128">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="4a297-129">В следующем примере вычисляется и выводится следующее:</span><span class="sxs-lookup"><span data-stu-id="4a297-129">The following example evaluates and renders:</span></span>

* <span data-ttu-id="4a297-130">`_headingFontStyle`к значению свойства CSS для `font-style`.</span><span class="sxs-lookup"><span data-stu-id="4a297-130">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="4a297-131">`_headingText`к содержимому `<h1>` элемента.</span><span class="sxs-lookup"><span data-stu-id="4a297-131">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="4a297-132">После первоначального отображения компонента Компонент повторно создает дерево рендеринга в ответ на события.</span><span class="sxs-lookup"><span data-stu-id="4a297-132">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="4a297-133">Затем блазор сравнивает новое дерево отрисовки с предыдущим и применяет любые изменения к модель DOMу (DOM) браузера.</span><span class="sxs-lookup"><span data-stu-id="4a297-133">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="4a297-134">Компоненты являются обычными C# классами и могут быть помещены в любое место внутри проекта.</span><span class="sxs-lookup"><span data-stu-id="4a297-134">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="4a297-135">Компоненты, создающие веб-страницы, обычно находятся в папке *страниц* .</span><span class="sxs-lookup"><span data-stu-id="4a297-135">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="4a297-136">Компоненты, не являющиеся страницами, часто помещаются в *общую* папку или пользовательскую папку, добавленную в проект.</span><span class="sxs-lookup"><span data-stu-id="4a297-136">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="4a297-137">Чтобы использовать пользовательскую папку, добавьте пространство имен пользовательской папки либо в родительский компонент, либо в файл *_Imports. Razor* приложения.</span><span class="sxs-lookup"><span data-stu-id="4a297-137">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="4a297-138">Например, следующее пространство имен делает компоненты в папке *Components* доступной, когда корневое пространство имен приложения имеет `WebApplication`следующий уровень:</span><span class="sxs-lookup"><span data-stu-id="4a297-138">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="4a297-139">Интеграция компонентов в Razor Pages и приложения MVC</span><span class="sxs-lookup"><span data-stu-id="4a297-139">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="4a297-140">Используйте компоненты с существующими Razor Pages и приложениями MVC.</span><span class="sxs-lookup"><span data-stu-id="4a297-140">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="4a297-141">Нет необходимости переписывать существующие страницы или представления для использования компонентов Razor.</span><span class="sxs-lookup"><span data-stu-id="4a297-141">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="4a297-142">При отображении страницы или представления компоненты предварительно отображаются.</span><span class="sxs-lookup"><span data-stu-id="4a297-142">When the page or view is rendered, components are prerendered at the same time.</span></span>

<span data-ttu-id="4a297-143">Чтобы отобразить компонент из страницы или представления, используйте `RenderComponentAsync<TComponent>` вспомогательный метод HTML:</span><span class="sxs-lookup"><span data-stu-id="4a297-143">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="MyComponent">
    @(await Html.RenderComponentAsync<MyComponent>(RenderMode.ServerPrerendered))
</div>
```

<span data-ttu-id="4a297-144">Хотя страницы и представления могут использовать компоненты, наоборот это не так.</span><span class="sxs-lookup"><span data-stu-id="4a297-144">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="4a297-145">Компоненты не могут использовать сценарии представления и страницы, такие как частичные представления и разделы.</span><span class="sxs-lookup"><span data-stu-id="4a297-145">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="4a297-146">Чтобы использовать логику из частичного представления в компоненте, разнесите логику частичного представления в компонент.</span><span class="sxs-lookup"><span data-stu-id="4a297-146">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="4a297-147">Дополнительные сведения о подготовке компонентов к просмотру и управлении состоянием компонентов в приложениях блазор Server см. в <xref:blazor/hosting-models> статье.</span><span class="sxs-lookup"><span data-stu-id="4a297-147">For more information on how components are rendered and component state is managed in Blazor Server apps, see the <xref:blazor/hosting-models> article.</span></span>

## <a name="use-components"></a><span data-ttu-id="4a297-148">Использование компонентов</span><span class="sxs-lookup"><span data-stu-id="4a297-148">Use components</span></span>

<span data-ttu-id="4a297-149">Компоненты могут включать другие компоненты, объявляя их с помощью синтаксиса HTML-элементов.</span><span class="sxs-lookup"><span data-stu-id="4a297-149">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="4a297-150">Разметка для использования компонента выглядит как тег HTML с именем, соответствующем типу компонента.</span><span class="sxs-lookup"><span data-stu-id="4a297-150">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="4a297-151">Привязка атрибута чувствительна к регистру.</span><span class="sxs-lookup"><span data-stu-id="4a297-151">Attribute binding is case sensitive.</span></span> <span data-ttu-id="4a297-152">Например, `@bind` является допустимым и `@Bind` является недопустимым.</span><span class="sxs-lookup"><span data-stu-id="4a297-152">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="4a297-153">Следующая разметка в *index. Razor* визуализирует `HeadingComponent` экземпляр:</span><span class="sxs-lookup"><span data-stu-id="4a297-153">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="4a297-154">*Components/хеадингкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4a297-154">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

<span data-ttu-id="4a297-155">Если компонент содержит HTML-элемент с первой буквой в верхнем регистре, который не соответствует имени компонента, выдается предупреждение, указывающее, что элемент имеет непредвиденное имя.</span><span class="sxs-lookup"><span data-stu-id="4a297-155">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="4a297-156">`@using` Добавление инструкции для пространства имен компонента делает компонент доступным, что приводит к удалению предупреждения.</span><span class="sxs-lookup"><span data-stu-id="4a297-156">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="4a297-157">Параметры компонентов</span><span class="sxs-lookup"><span data-stu-id="4a297-157">Component parameters</span></span>

<span data-ttu-id="4a297-158">Компоненты могут иметь *Параметры компонента*, которые определяются с помощью общедоступных свойств класса Component с `[Parameter]` атрибутом.</span><span class="sxs-lookup"><span data-stu-id="4a297-158">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="4a297-159">Используйте атрибуты, чтобы указать аргументы для компонента в разметке.</span><span class="sxs-lookup"><span data-stu-id="4a297-159">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="4a297-160">*Components/чилдкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4a297-160">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="4a297-161">В следующем примере `ParentComponent` `Title` свойство устанавливает значение свойства объекта `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="4a297-161">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="4a297-162">*Pages/паренткомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4a297-162">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="4a297-163">Дочернее содержимое</span><span class="sxs-lookup"><span data-stu-id="4a297-163">Child content</span></span>

<span data-ttu-id="4a297-164">Компоненты могут устанавливать содержимое другого компонента.</span><span class="sxs-lookup"><span data-stu-id="4a297-164">Components can set the content of another component.</span></span> <span data-ttu-id="4a297-165">Назначение компонента предоставляет содержимое между тегами, задающих принимающий компонент.</span><span class="sxs-lookup"><span data-stu-id="4a297-165">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="4a297-166">В следующем примере объект `ChildComponent` `ChildContent` имеет свойство, представляющее объект `RenderFragment`, который представляет сегмент пользовательского интерфейса для отрисовки.</span><span class="sxs-lookup"><span data-stu-id="4a297-166">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="4a297-167">Значение `ChildContent` размещается в разметке компонента, где должно быть визуализировано содержимое.</span><span class="sxs-lookup"><span data-stu-id="4a297-167">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="4a297-168">Значение `ChildContent` получается от родительского компонента и отображается внутри `panel-body`панели начальной загрузки.</span><span class="sxs-lookup"><span data-stu-id="4a297-168">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="4a297-169">*Components/чилдкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4a297-169">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="4a297-170">Свойству, получающему `RenderFragment` содержимое, необходимо `ChildContent` присвоить имя по соглашению.</span><span class="sxs-lookup"><span data-stu-id="4a297-170">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="4a297-171">Следующие `ParentComponent` данные могут предоставлять содержимое для подготовки к `ChildComponent` просмотру путем `<ChildComponent>` размещения содержимого внутри тегов.</span><span class="sxs-lookup"><span data-stu-id="4a297-171">The following `ParentComponent` can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="4a297-172">*Pages/паренткомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4a297-172">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="4a297-173">Атрибуты Сплаттинг и произвольные параметры</span><span class="sxs-lookup"><span data-stu-id="4a297-173">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="4a297-174">Компоненты могут записывать и отображать дополнительные атрибуты в дополнение к объявленным параметрам компонента.</span><span class="sxs-lookup"><span data-stu-id="4a297-174">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="4a297-175">Дополнительные атрибуты можно записать в словарь, а затем *сплаттед* на элемент при подготовке компонента к просмотру с помощью [@attributes](xref:mvc/views/razor#attributes) директивы Razor.</span><span class="sxs-lookup"><span data-stu-id="4a297-175">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [@attributes](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="4a297-176">Этот сценарий полезен при определении компонента, который создает элемент разметки, поддерживающий разнообразные настройки.</span><span class="sxs-lookup"><span data-stu-id="4a297-176">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="4a297-177">Например, может быть утомительно определять атрибуты отдельно для `<input>` , который поддерживает множество параметров.</span><span class="sxs-lookup"><span data-stu-id="4a297-177">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="4a297-178">В `<input>` следующем примере первый элемент (`id="useIndividualParams"`) использует индивидуальные параметры компонента, а второй `<input>` элемент (`id="useAttributesDict"`) использует атрибут Сплаттинг:</span><span class="sxs-lookup"><span data-stu-id="4a297-178">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

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

<span data-ttu-id="4a297-179">Тип параметра должен реализовываться `IEnumerable<KeyValuePair<string, object>>` со строковыми ключами.</span><span class="sxs-lookup"><span data-stu-id="4a297-179">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="4a297-180">В `IReadOnlyDictionary<string, object>` этом сценарии также используется параметр using.</span><span class="sxs-lookup"><span data-stu-id="4a297-180">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="4a297-181">Отображаемые `<input>` элементы, использующие оба подхода, идентичны:</span><span class="sxs-lookup"><span data-stu-id="4a297-181">The rendered `<input>` elements using both approaches is identical:</span></span>

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

<span data-ttu-id="4a297-182">Чтобы принять произвольные атрибуты, определите параметр компонента с помощью `[Parameter]` атрибута `CaptureUnmatchedValues` со свойством, имеющим `true`значение:</span><span class="sxs-lookup"><span data-stu-id="4a297-182">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```cshtml
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="4a297-183">`CaptureUnmatchedValues` Свойство в`[Parameter]` позволяет параметру сопоставлять все атрибуты, не соответствующие ни одному другому параметру.</span><span class="sxs-lookup"><span data-stu-id="4a297-183">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="4a297-184">Компонент может определять только один параметр с помощью `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="4a297-184">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="4a297-185">Тип свойства, используемый с `CaptureUnmatchedValues` параметром, должен быть `Dictionary<string, object>` назначен с помощью строковых ключей.</span><span class="sxs-lookup"><span data-stu-id="4a297-185">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="4a297-186">`IEnumerable<KeyValuePair<string, object>>`Кроме `IReadOnlyDictionary<string, object>` того, в этом сценарии также есть параметры.</span><span class="sxs-lookup"><span data-stu-id="4a297-186">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

## <a name="data-binding"></a><span data-ttu-id="4a297-187">Привязка данных</span><span class="sxs-lookup"><span data-stu-id="4a297-187">Data binding</span></span>

<span data-ttu-id="4a297-188">Привязка данных как к компонентам, так и к элементам DOM [@bind](xref:mvc/views/razor#bind) осуществляется с помощью атрибута.</span><span class="sxs-lookup"><span data-stu-id="4a297-188">Data binding to both components and DOM elements is accomplished with the [@bind](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="4a297-189">В следующем примере `_italicsCheck` поле привязывается к установленному флажку.</span><span class="sxs-lookup"><span data-stu-id="4a297-189">The following example binds the `_italicsCheck` field to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    @bind="_italicsCheck" />
```

<span data-ttu-id="4a297-190">Если флажок установлен и снят, значение свойства обновляется до `true` и `false`соответственно.</span><span class="sxs-lookup"><span data-stu-id="4a297-190">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="4a297-191">Флажок обновляется в пользовательском интерфейсе только при подготовке компонента к просмотру, а не в ответ на изменение значения свойства.</span><span class="sxs-lookup"><span data-stu-id="4a297-191">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="4a297-192">Поскольку компоненты отображаются после выполнения кода обработчика событий, обновления свойств, как правило, немедленно отражаются в пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="4a297-192">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="4a297-193">Использование `@bind` `<input @bind="CurrentValue" />`со свойством (), по сути, эквивалентно следующему: `CurrentValue`</span><span class="sxs-lookup"><span data-stu-id="4a297-193">Using `@bind` with a `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

<span data-ttu-id="4a297-194">При подготовке компонента к `value` просмотру элемент входного элемента берется `CurrentValue` из свойства.</span><span class="sxs-lookup"><span data-stu-id="4a297-194">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="4a297-195">Когда пользователь вводит текст в текстовое поле, `onchange` создается событие, `CurrentValue` а свойству присваивается измененное значение.</span><span class="sxs-lookup"><span data-stu-id="4a297-195">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="4a297-196">На практике создание кода немного сложнее, так как `@bind` обрабатывает несколько случаев, когда выполняются преобразования типов.</span><span class="sxs-lookup"><span data-stu-id="4a297-196">In reality, the code generation is a little more complex because `@bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="4a297-197">В принципе, `@bind` связывает текущее значение выражения `value` с атрибутом и обрабатывает изменения с помощью зарегистрированного обработчика.</span><span class="sxs-lookup"><span data-stu-id="4a297-197">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="4a297-198">`onchange` Помимо обработки[@bind-value:event](xref:mvc/views/razor#bind)событий с `@bind` синтаксисом, свойство или поле можно привязать с помощью [@bind-value](xref:mvc/views/razor#bind) других событий, `event` указав атрибут с параметром ().</span><span class="sxs-lookup"><span data-stu-id="4a297-198">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [@bind-value](xref:mvc/views/razor#bind) attribute with an `event` parameter ([@bind-value:event](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="4a297-199">В следующем примере показано, `CurrentValue` как привязать свойство `oninput` к событию:</span><span class="sxs-lookup"><span data-stu-id="4a297-199">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />
```

<span data-ttu-id="4a297-200">В отличие `onchange`от, которое срабатывает при потере фокуса элементом `oninput` , срабатывает при изменении значения текстового поля.</span><span class="sxs-lookup"><span data-stu-id="4a297-200">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="4a297-201">**Неанализируемые значения**</span><span class="sxs-lookup"><span data-stu-id="4a297-201">**Unparsable values**</span></span>

<span data-ttu-id="4a297-202">Когда пользователь предоставляет неинтерпретируемое значение для элемента с привязкой к данным, неанализируемое значение автоматически возвращается к предыдущему значению при срабатывании события привязки.</span><span class="sxs-lookup"><span data-stu-id="4a297-202">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="4a297-203">Рассмотрим следующий сценарий.</span><span class="sxs-lookup"><span data-stu-id="4a297-203">Consider the following scenario:</span></span>

* <span data-ttu-id="4a297-204">Элемент привязан к типу с начальным значением `123` `int`: `<input>`</span><span class="sxs-lookup"><span data-stu-id="4a297-204">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```cshtml
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="4a297-205">Пользователь обновляет значение элемента на `123.45` странице и изменяет фокус элемента.</span><span class="sxs-lookup"><span data-stu-id="4a297-205">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="4a297-206">В приведенном выше сценарии значение элемента возвращается к `123`значению.</span><span class="sxs-lookup"><span data-stu-id="4a297-206">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="4a297-207">Если значение `123.45` отклоняется в пользу исходного `123`значения, пользователь понимает, что их значение не принято.</span><span class="sxs-lookup"><span data-stu-id="4a297-207">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="4a297-208">По умолчанию привязка применяется к `onchange` событию элемента (`@bind="{PROPERTY OR FIELD}"`).</span><span class="sxs-lookup"><span data-stu-id="4a297-208">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="4a297-209">Используйте `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` для задания другого события.</span><span class="sxs-lookup"><span data-stu-id="4a297-209">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="4a297-210">Для события (`@bind-value:event="oninput"`) переверсия происходит после любого нажатия клавиши, которая вводит неинтерпретируемое значение. `oninput`</span><span class="sxs-lookup"><span data-stu-id="4a297-210">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="4a297-211">При нацеливании `oninput` на событие `int`с привязкой к нему пользователю запрещено вводить `.` символ.</span><span class="sxs-lookup"><span data-stu-id="4a297-211">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="4a297-212">`.` Символ немедленно удаляется, поэтому пользователь получает немедленный отзыв о том, что разрешены только целые числа.</span><span class="sxs-lookup"><span data-stu-id="4a297-212">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="4a297-213">Существуют ситуации, когда возврат значения `oninput` события не является идеальным, например, когда пользователю следует разрешить очистить `<input>` неинтерпретируемое значение.</span><span class="sxs-lookup"><span data-stu-id="4a297-213">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="4a297-214">К альтернативам относятся:</span><span class="sxs-lookup"><span data-stu-id="4a297-214">Alternatives include:</span></span>

* <span data-ttu-id="4a297-215">Не используйте `oninput` событие.</span><span class="sxs-lookup"><span data-stu-id="4a297-215">Don't use the `oninput` event.</span></span> <span data-ttu-id="4a297-216">Используйте событие по `onchange` умолчанию`@bind="{PROPERTY OR FIELD}"`(), где недопустимое значение не возвращается, пока элемент не теряет фокус.</span><span class="sxs-lookup"><span data-stu-id="4a297-216">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="4a297-217">Выполните привязку к типу, допускающему `int?` значение `string`null, например или, и предоставьте настраиваемую логику для обработки недопустимых записей.</span><span class="sxs-lookup"><span data-stu-id="4a297-217">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="4a297-218">Используйте [компонент проверки формы](xref:blazor/forms-validation), например `InputNumber` или `InputDate`.</span><span class="sxs-lookup"><span data-stu-id="4a297-218">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="4a297-219">Компоненты проверки форм имеют встроенную поддержку для управления недопустимыми входными данными.</span><span class="sxs-lookup"><span data-stu-id="4a297-219">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="4a297-220">Компоненты проверки формы:</span><span class="sxs-lookup"><span data-stu-id="4a297-220">Form validation components:</span></span>
  * <span data-ttu-id="4a297-221">Позволяет пользователю указать недопустимые входные данные и получить ошибки проверки для `EditContext`связанного.</span><span class="sxs-lookup"><span data-stu-id="4a297-221">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="4a297-222">Отображает ошибки проверки в пользовательском интерфейсе, не мешая пользователю вводить дополнительные данные из формы.</span><span class="sxs-lookup"><span data-stu-id="4a297-222">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

<span data-ttu-id="4a297-223">**Глобализация**</span><span class="sxs-lookup"><span data-stu-id="4a297-223">**Globalization**</span></span>

<span data-ttu-id="4a297-224">`@bind`значения форматируются для вывода и анализируются с использованием правил текущего языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="4a297-224">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="4a297-225">Доступ к текущему языку и региональным параметрам можно получить из <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> свойства.</span><span class="sxs-lookup"><span data-stu-id="4a297-225">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="4a297-226">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) используется для следующих типов полей (`<input type="{TYPE}" />`):</span><span class="sxs-lookup"><span data-stu-id="4a297-226">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="4a297-227">Предыдущие типы полей:</span><span class="sxs-lookup"><span data-stu-id="4a297-227">The preceding field types:</span></span>

* <span data-ttu-id="4a297-228">Отображаются с использованием соответствующих правил форматирования на основе браузера.</span><span class="sxs-lookup"><span data-stu-id="4a297-228">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="4a297-229">Не может содержать текст в свободной форме.</span><span class="sxs-lookup"><span data-stu-id="4a297-229">Can't contain free-form text.</span></span>
* <span data-ttu-id="4a297-230">Предоставление характеристик взаимодействия с пользователем в зависимости от реализации браузера.</span><span class="sxs-lookup"><span data-stu-id="4a297-230">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="4a297-231">Следующие типы полей имеют особые требования к форматированию, которые в настоящее время не поддерживаются Блазор, так как они не поддерживаются всеми основными браузерами.</span><span class="sxs-lookup"><span data-stu-id="4a297-231">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="4a297-232">`@bind`поддерживает параметр для <xref:System.Globalization.CultureInfo?displayProperty=fullName> обеспечения синтаксического анализа и форматирования значения. `@bind:culture`</span><span class="sxs-lookup"><span data-stu-id="4a297-232">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="4a297-233">Указание языка и региональных параметров не рекомендуется при `date` использовании `number` типов полей и.</span><span class="sxs-lookup"><span data-stu-id="4a297-233">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="4a297-234">`date`и `number` имеют встроенную поддержку блазор, которая предоставляет требуемый язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="4a297-234">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="4a297-235">Сведения о том, как задать язык и региональные параметры пользователя, см. в разделе [локализация](#localization).</span><span class="sxs-lookup"><span data-stu-id="4a297-235">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="4a297-236">**Строки формата**</span><span class="sxs-lookup"><span data-stu-id="4a297-236">**Format strings**</span></span>

<span data-ttu-id="4a297-237">Привязка данных работает со <xref:System.DateTime> строками формата [@bind:format](xref:mvc/views/razor#bind)с помощью.</span><span class="sxs-lookup"><span data-stu-id="4a297-237">Data binding works with <xref:System.DateTime> format strings using [@bind:format](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="4a297-238">Другие выражения форматирования, такие как денежные или числовые форматы, в настоящее время недоступны.</span><span class="sxs-lookup"><span data-stu-id="4a297-238">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="4a297-239">В приведенном выше коде `<input>` тип поля элемента (`type`) по умолчанию `text`имеет значение.</span><span class="sxs-lookup"><span data-stu-id="4a297-239">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="4a297-240">`@bind:format`поддерживается для привязки следующих типов .NET:</span><span class="sxs-lookup"><span data-stu-id="4a297-240">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="4a297-241"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="4a297-241"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="4a297-242"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="4a297-242"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="4a297-243">Атрибут задает формат даты, `value` `<input>` применяемый к элементу. `@bind:format`</span><span class="sxs-lookup"><span data-stu-id="4a297-243">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="4a297-244">Формат также используется для анализа значения при `onchange` возникновении события.</span><span class="sxs-lookup"><span data-stu-id="4a297-244">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="4a297-245">Указание формата для `date` типа поля не рекомендуется, так как блазор имеет встроенную поддержку для форматирования дат.</span><span class="sxs-lookup"><span data-stu-id="4a297-245">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span>

<span data-ttu-id="4a297-246">**Параметры компонента**</span><span class="sxs-lookup"><span data-stu-id="4a297-246">**Component parameters**</span></span>

<span data-ttu-id="4a297-247">Привязка распознает параметры компонента, `@bind-{property}` где можно привязать значение свойства к разным компонентам.</span><span class="sxs-lookup"><span data-stu-id="4a297-247">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="4a297-248">Следующий дочерний компонент`ChildComponent`() `Year` имеет параметр компонента и `YearChanged` обратный вызов:</span><span class="sxs-lookup"><span data-stu-id="4a297-248">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

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

<span data-ttu-id="4a297-249">`EventCallback<T>`описывается в разделе [вложенный EventCallback](#eventcallback) .</span><span class="sxs-lookup"><span data-stu-id="4a297-249">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="4a297-250">Следующий родительский компонент использует `ChildComponent` и связывает `ParentYear` параметр `Year` из родительского компонента с параметром в дочернем компоненте:</span><span class="sxs-lookup"><span data-stu-id="4a297-250">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

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

<span data-ttu-id="4a297-251">При загрузке `ParentComponent` создается следующая разметка:</span><span class="sxs-lookup"><span data-stu-id="4a297-251">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="4a297-252">Если значение `ParentYear` свойства изменяется путем нажатия кнопки `ParentComponent`в `ChildComponent` , `Year` свойство элемента обновляется.</span><span class="sxs-lookup"><span data-stu-id="4a297-252">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="4a297-253">Новое значение параметра `Year` отображается в пользовательском интерфейсе при его `ParentComponent` визуализации.</span><span class="sxs-lookup"><span data-stu-id="4a297-253">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="4a297-254">Параметр является связываемым, так как содержит сопутствующее `YearChanged` событие, `Year` соответствующее типу параметра. `Year`</span><span class="sxs-lookup"><span data-stu-id="4a297-254">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="4a297-255">По соглашению `<ChildComponent @bind-Year="ParentYear" />` , по сути, эквивалентно написанию:</span><span class="sxs-lookup"><span data-stu-id="4a297-255">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="4a297-256">Как правило, свойство может быть привязано к соответствующему обработчику событий `@bind-property:event` с помощью атрибута.</span><span class="sxs-lookup"><span data-stu-id="4a297-256">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="4a297-257">Например, свойство `MyProp` можно привязать к `MyEventHandler` с помощью следующих двух атрибутов:</span><span class="sxs-lookup"><span data-stu-id="4a297-257">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="4a297-258">Обработка событий</span><span class="sxs-lookup"><span data-stu-id="4a297-258">Event handling</span></span>

<span data-ttu-id="4a297-259">Компоненты Razor предоставляют функции обработки событий.</span><span class="sxs-lookup"><span data-stu-id="4a297-259">Razor components provide event handling features.</span></span> <span data-ttu-id="4a297-260">Для атрибута HTML-элемента с `on{event}` именем (например, `onclick` и `onsubmit`) со значением, вводимым с помощью делегата, компоненты Razor рассматривают значение атрибута как обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="4a297-260">For an HTML element attribute named `on{event}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="4a297-261">Имя атрибута всегда имеет формат [ @on{Event}](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="4a297-261">The attribute's name is always formatted [@on{event}](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="4a297-262">Следующий код вызывает метод, `UpdateHeading` когда кнопка выбрана в пользовательском интерфейсе:</span><span class="sxs-lookup"><span data-stu-id="4a297-262">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

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

<span data-ttu-id="4a297-263">Следующий код вызывает `CheckChanged` метод при изменении флажка в пользовательском интерфейсе:</span><span class="sxs-lookup"><span data-stu-id="4a297-263">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="4a297-264">Обработчики событий также могут быть асинхронными и <xref:System.Threading.Tasks.Task>возвращать.</span><span class="sxs-lookup"><span data-stu-id="4a297-264">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="4a297-265">Нет необходимости вручную вызывать `StateHasChanged()`.</span><span class="sxs-lookup"><span data-stu-id="4a297-265">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="4a297-266">Исключения регистрируются при их возникновении.</span><span class="sxs-lookup"><span data-stu-id="4a297-266">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="4a297-267">В следующем примере `UpdateHeading` метод вызывается асинхронно при выборе кнопки:</span><span class="sxs-lookup"><span data-stu-id="4a297-267">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

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

### <a name="event-argument-types"></a><span data-ttu-id="4a297-268">Типы аргументов событий</span><span class="sxs-lookup"><span data-stu-id="4a297-268">Event argument types</span></span>

<span data-ttu-id="4a297-269">Для некоторых событий разрешены типы аргументов событий.</span><span class="sxs-lookup"><span data-stu-id="4a297-269">For some events, event argument types are permitted.</span></span> <span data-ttu-id="4a297-270">Если доступ к одному из этих типов событий не нужен, в вызове метода это не требуется.</span><span class="sxs-lookup"><span data-stu-id="4a297-270">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="4a297-271">Поддерживаемые [EventArgs](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview9/src/Components/Web/src/Web) показаны в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="4a297-271">Supported [EventArgs](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview9/src/Components/Web/src/Web) are shown in the following table.</span></span>

| <span data-ttu-id="4a297-272">событие</span><span class="sxs-lookup"><span data-stu-id="4a297-272">Event</span></span> | <span data-ttu-id="4a297-273">Класс</span><span class="sxs-lookup"><span data-stu-id="4a297-273">Class</span></span> |
| ----- | ----- |
| <span data-ttu-id="4a297-274">буфер обмена</span><span class="sxs-lookup"><span data-stu-id="4a297-274">Clipboard</span></span>        | `ClipboardEventArgs` |
| <span data-ttu-id="4a297-275">Переместить</span><span class="sxs-lookup"><span data-stu-id="4a297-275">Drag</span></span>             | <span data-ttu-id="4a297-276">`DragEventArgs`&ndash; и`DataTransferItem`содержат перетаскиваемые данные элемента. `DataTransfer`</span><span class="sxs-lookup"><span data-stu-id="4a297-276">`DragEventArgs` &ndash; `DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="4a297-277">Ошибка</span><span class="sxs-lookup"><span data-stu-id="4a297-277">Error</span></span>            | `ErrorEventArgs` |
| <span data-ttu-id="4a297-278">Фокус</span><span class="sxs-lookup"><span data-stu-id="4a297-278">Focus</span></span>            | <span data-ttu-id="4a297-279">`FocusEventArgs`Не включает поддержку для `relatedTarget`. &ndash;</span><span class="sxs-lookup"><span data-stu-id="4a297-279">`FocusEventArgs` &ndash; Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="4a297-280">Изменение`<input>`</span><span class="sxs-lookup"><span data-stu-id="4a297-280">`<input>` change</span></span> | `ChangeEventArgs` |
| <span data-ttu-id="4a297-281">Клавиатура</span><span class="sxs-lookup"><span data-stu-id="4a297-281">Keyboard</span></span>         | `KeyboardEventArgs` |
| <span data-ttu-id="4a297-282">Мышь</span><span class="sxs-lookup"><span data-stu-id="4a297-282">Mouse</span></span>            | `MouseEventArgs` |
| <span data-ttu-id="4a297-283">Указатель мыши</span><span class="sxs-lookup"><span data-stu-id="4a297-283">Mouse pointer</span></span>    | `PointerEventArgs` |
| <span data-ttu-id="4a297-284">Колесо мыши</span><span class="sxs-lookup"><span data-stu-id="4a297-284">Mouse wheel</span></span>      | `WheelEventArgs` |
| <span data-ttu-id="4a297-285">Ход выполнения</span><span class="sxs-lookup"><span data-stu-id="4a297-285">Progress</span></span>         | `ProgressEventArgs` |
| <span data-ttu-id="4a297-286">Сенсорные технологии</span><span class="sxs-lookup"><span data-stu-id="4a297-286">Touch</span></span>            | <span data-ttu-id="4a297-287">`TouchEventArgs`&ndash; представляетоднуточкуконтактанаустройствес`TouchPoint` сенсорным вводом.</span><span class="sxs-lookup"><span data-stu-id="4a297-287">`TouchEventArgs` &ndash; `TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="4a297-288">Сведения о свойствах и поведении событий, посвященных событиям в предыдущей таблице, см. в разделе [классы EventArgs в эталонном источнике (ASPNET/AspNetCore Release/3.0-preview9 branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview9/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="4a297-288">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source (aspnet/AspNetCore release/3.0-preview9 branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview9/src/Components/Web/src/Web).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="4a297-289">Лямбда-выражения</span><span class="sxs-lookup"><span data-stu-id="4a297-289">Lambda expressions</span></span>

<span data-ttu-id="4a297-290">Лямбда-выражения также могут использоваться:</span><span class="sxs-lookup"><span data-stu-id="4a297-290">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="4a297-291">Часто бывает удобно закрывать дополнительные значения, например при переборе набора элементов.</span><span class="sxs-lookup"><span data-stu-id="4a297-291">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="4a297-292">В следующем примере создаются три кнопки, каждый из которых вызывает `UpdateHeading` передачу аргумента события (`MouseEventArgs`) и номер кнопки (`buttonNumber`) при выборе в пользовательском интерфейсе:</span><span class="sxs-lookup"><span data-stu-id="4a297-292">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

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
> <span data-ttu-id="4a297-293">**Не** используйте переменную цикла (`i`) в `for` цикле непосредственно в лямбда-выражении.</span><span class="sxs-lookup"><span data-stu-id="4a297-293">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="4a297-294">В противном случае одна и та же переменная используется всеми `i`лямбда-выражениями, которые вызывают одинаковое значение во всех лямбдаах.</span><span class="sxs-lookup"><span data-stu-id="4a297-294">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="4a297-295">Всегда запишите свое значение в локальную переменную (`buttonNumber` в предыдущем примере), а затем используйте ее.</span><span class="sxs-lookup"><span data-stu-id="4a297-295">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="4a297-296">Вложенный EventCallback</span><span class="sxs-lookup"><span data-stu-id="4a297-296">EventCallback</span></span>

<span data-ttu-id="4a297-297">Распространенным сценарием с вложенными компонентами является желание запускать метод родительского компонента при возникновении&mdash;события дочернего компонента, например `onclick` при возникновении события в дочернем элементе.</span><span class="sxs-lookup"><span data-stu-id="4a297-297">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="4a297-298">Чтобы предоставить события для компонентов, используйте `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="4a297-298">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="4a297-299">Родительский компонент может назначить метод обратного вызова для дочернего `EventCallback`компонента.</span><span class="sxs-lookup"><span data-stu-id="4a297-299">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="4a297-300">В примере приложения `EventCallback` демонстрируется настройка `onclick` обработчика кнопки на получение делегата из примера `ParentComponent`. `ChildComponent`</span><span class="sxs-lookup"><span data-stu-id="4a297-300">The `ChildComponent` in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="4a297-301">Вводится с помощью `MouseEventArgs` ,`onclick` что подходит для события с периферийного устройства. `EventCallback`</span><span class="sxs-lookup"><span data-stu-id="4a297-301">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="4a297-302">Задает для дочернего `EventCallback<T>` элемента метод:`ShowMessage` `ParentComponent`</span><span class="sxs-lookup"><span data-stu-id="4a297-302">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="4a297-303">При выборе кнопки в `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="4a297-303">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="4a297-304">`ParentComponent` Вызывается`ShowMessage` метод.</span><span class="sxs-lookup"><span data-stu-id="4a297-304">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="4a297-305">`messageText`обновляется и отображается в `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="4a297-305">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="4a297-306">Вызов `StateHasChanged` метода не требуется в методе обратного вызова (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="4a297-306">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="4a297-307">`StateHasChanged`вызывается автоматически для перевизуализации `ParentComponent`компонента, как и в случае, когда дочерние события активируют компонент в обработчиках событий, которые выполняются внутри дочернего элемента.</span><span class="sxs-lookup"><span data-stu-id="4a297-307">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="4a297-308">`EventCallback`и `EventCallback<T>` разрешите асинхронные делегаты.</span><span class="sxs-lookup"><span data-stu-id="4a297-308">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="4a297-309">`EventCallback<T>`является строго типизированным и требует определенного типа аргумента.</span><span class="sxs-lookup"><span data-stu-id="4a297-309">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="4a297-310">`EventCallback`слабо типизирован и допускает любой тип аргумента.</span><span class="sxs-lookup"><span data-stu-id="4a297-310">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="4a297-311">Вызовите `EventCallback<T>` `InvokeAsync` или с параметром и <xref:System.Threading.Tasks.Task>дождаться: `EventCallback`</span><span class="sxs-lookup"><span data-stu-id="4a297-311">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="4a297-312">Используйте `EventCallback` и`EventCallback<T>` для обработки событий и параметров компонента привязки.</span><span class="sxs-lookup"><span data-stu-id="4a297-312">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="4a297-313">Предпочитать строго типизированный `EventCallback<T>`. `EventCallback`</span><span class="sxs-lookup"><span data-stu-id="4a297-313">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="4a297-314">`EventCallback<T>`обеспечивает лучшую реакцию на ошибки для пользователей компонента.</span><span class="sxs-lookup"><span data-stu-id="4a297-314">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="4a297-315">Как и в случае с другими обработчиками событий пользовательского интерфейса, указание параметра события является необязательным.</span><span class="sxs-lookup"><span data-stu-id="4a297-315">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="4a297-316">Используется `EventCallback` при отсутствии значения, переданного обратному вызову.</span><span class="sxs-lookup"><span data-stu-id="4a297-316">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="chained-bind"></a><span data-ttu-id="4a297-317">Привязка к цепочке</span><span class="sxs-lookup"><span data-stu-id="4a297-317">Chained bind</span></span>

<span data-ttu-id="4a297-318">Распространенным сценарием является привязка привязанного к данным параметра к элементу страницы в выходных данных компонента.</span><span class="sxs-lookup"><span data-stu-id="4a297-318">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="4a297-319">Этот сценарий называется *связанной* привязкой, так как несколько уровней привязки происходят одновременно.</span><span class="sxs-lookup"><span data-stu-id="4a297-319">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="4a297-320">Невозможно реализовать цепочку привязки с `@bind` синтаксисом в элементе страницы.</span><span class="sxs-lookup"><span data-stu-id="4a297-320">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="4a297-321">Обработчик событий и значение должны быть указаны отдельно.</span><span class="sxs-lookup"><span data-stu-id="4a297-321">The event handler and value must be specified separately.</span></span> <span data-ttu-id="4a297-322">Однако родительский компонент может использовать `@bind` синтаксис с параметром компонента.</span><span class="sxs-lookup"><span data-stu-id="4a297-322">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="4a297-323">Следующий `PasswordField` компонент (*пассвордфиелд. Razor*):</span><span class="sxs-lookup"><span data-stu-id="4a297-323">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="4a297-324">Задает значение `Password` элемента для свойства. `<input>`</span><span class="sxs-lookup"><span data-stu-id="4a297-324">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="4a297-325">Предоставляет изменения `Password` свойства в родительский компонент с помощью [вложенный EventCallback](#eventcallback).</span><span class="sxs-lookup"><span data-stu-id="4a297-325">Exposes changes of the `Password` property to a parent component with an [EventCallback](#eventcallback).</span></span>

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

<span data-ttu-id="4a297-326">`PasswordField` Компонент используется в другом компоненте:</span><span class="sxs-lookup"><span data-stu-id="4a297-326">The `PasswordField` component is used in another component:</span></span>

```cshtml
<PasswordField @bind-Password="password" />

@code {
    private string password;
}
```

<span data-ttu-id="4a297-327">Для выполнения проверок или перехвата ошибок в пароле в предыдущем примере:</span><span class="sxs-lookup"><span data-stu-id="4a297-327">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="4a297-328">Создайте резервное поле для `Password` (`password` в следующем примере кода).</span><span class="sxs-lookup"><span data-stu-id="4a297-328">Create a backing field for `Password` (`password` in the following example code).</span></span>
* <span data-ttu-id="4a297-329">Выполнение проверок или перехвата ошибок в `Password` методе задания.</span><span class="sxs-lookup"><span data-stu-id="4a297-329">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="4a297-330">В следующем примере представлена немедленная реакция пользователя, если в значении пароля используется пробел:</span><span class="sxs-lookup"><span data-stu-id="4a297-330">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

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

## <a name="capture-references-to-components"></a><span data-ttu-id="4a297-331">Запись ссылок на компоненты</span><span class="sxs-lookup"><span data-stu-id="4a297-331">Capture references to components</span></span>

<span data-ttu-id="4a297-332">Ссылки на компоненты предоставляют способ ссылки на экземпляр компонента, чтобы можно было выполнять команды для этого экземпляра, например `Show` или. `Reset`</span><span class="sxs-lookup"><span data-stu-id="4a297-332">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="4a297-333">Чтобы записать ссылку на компонент, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="4a297-333">To capture a component reference:</span></span>

* <span data-ttu-id="4a297-334">[@ref](xref:mvc/views/razor#ref) Добавьте атрибут к дочернему компоненту.</span><span class="sxs-lookup"><span data-stu-id="4a297-334">Add an [@ref](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="4a297-335">Определите поле с тем же типом, что и у дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="4a297-335">Define a field with the same type as the child component.</span></span>

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

<span data-ttu-id="4a297-336">При подготовке компонента к просмотру `loginDialog` поле заполняется `MyLoginDialog` экземпляром дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="4a297-336">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="4a297-337">Затем можно вызывать методы .NET в экземпляре компонента.</span><span class="sxs-lookup"><span data-stu-id="4a297-337">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4a297-338">Переменная заполняется только после подготовки компонента, и ее выходные данные `MyLoginDialog` включают элемент. `loginDialog`</span><span class="sxs-lookup"><span data-stu-id="4a297-338">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="4a297-339">До этого момента нет никаких ссылок на.</span><span class="sxs-lookup"><span data-stu-id="4a297-339">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="4a297-340">Чтобы управлять ссылками на компоненты после завершения подготовки компонента к просмотру `OnAfterRenderAsync` , `OnAfterRender` используйте методы или.</span><span class="sxs-lookup"><span data-stu-id="4a297-340">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<span data-ttu-id="4a297-341">При захвате ссылок на компоненты используется аналогичный синтаксис для [записи ссылок на элементы](xref:blazor/javascript-interop#capture-references-to-elements), но это не функция [взаимодействия JavaScript](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="4a297-341">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="4a297-342">Ссылки на компоненты не передаются&mdash;в код JavaScript, они используются только в коде .NET.</span><span class="sxs-lookup"><span data-stu-id="4a297-342">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="4a297-343">**Не** используйте ссылки на компоненты для изменения состояния дочерних компонентов.</span><span class="sxs-lookup"><span data-stu-id="4a297-343">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="4a297-344">Вместо этого используйте обычные декларативные параметры для передачи данных дочерним компонентам.</span><span class="sxs-lookup"><span data-stu-id="4a297-344">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="4a297-345">Использование обычных декларативных параметров приводит к тому, что дочерние компоненты автоматически визуализируются в нужное время.</span><span class="sxs-lookup"><span data-stu-id="4a297-345">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="4a297-346">Вызывать методы компонента извне для обновления состояния</span><span class="sxs-lookup"><span data-stu-id="4a297-346">Invoke component methods externally to update state</span></span>

<span data-ttu-id="4a297-347">Блазор использует `SynchronizationContext` для принудительного применения одного логического потока выполнения.</span><span class="sxs-lookup"><span data-stu-id="4a297-347">Blazor uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="4a297-348">В этом `SynchronizationContext`случае выполняются методы жизненного цикла компонента и любые обратные вызовы событий, вызванные блазор.</span><span class="sxs-lookup"><span data-stu-id="4a297-348">A component's lifecycle methods and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="4a297-349">В случае, если компонент должен быть обновлен на основе внешнего события, такого как таймер или другие уведомления, используйте `InvokeAsync` метод, который будет отправляться в `SynchronizationContext`блазор.</span><span class="sxs-lookup"><span data-stu-id="4a297-349">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="4a297-350">Например, рассмотрим *службу уведомления* , которая может уведомлять любой компонент, выполняющий прослушивание, в обновленном состоянии:</span><span class="sxs-lookup"><span data-stu-id="4a297-350">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

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

<span data-ttu-id="4a297-351">`NotifierService` Использование для обновления компонента:</span><span class="sxs-lookup"><span data-stu-id="4a297-351">Usage of the `NotifierService` to update a component:</span></span>

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

<span data-ttu-id="4a297-352">В предыдущем примере `NotifierService` вызывает `OnNotify` метод компонента за пределами блазор. `SynchronizationContext`</span><span class="sxs-lookup"><span data-stu-id="4a297-352">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="4a297-353">`InvokeAsync`используется для переключения на правильный контекст и постановка в очередь визуализации.</span><span class="sxs-lookup"><span data-stu-id="4a297-353">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="4a297-354">Использование \@ключа для управления сохранением элементов и компонентов</span><span class="sxs-lookup"><span data-stu-id="4a297-354">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="4a297-355">При последующем изменении списка элементов или компонентов, а также элементов или компонентов алгоритм сравнения Блазор должен решить, какие из предыдущих элементов или компонентов могут быть сохранены и как объекты модели должны сопоставляться с ними.</span><span class="sxs-lookup"><span data-stu-id="4a297-355">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="4a297-356">Обычно этот процесс выполняется автоматически и его можно игнорировать, но в некоторых случаях может потребоваться управление процессом.</span><span class="sxs-lookup"><span data-stu-id="4a297-356">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="4a297-357">Рассмотрим следующий пример.</span><span class="sxs-lookup"><span data-stu-id="4a297-357">Consider the following example:</span></span>

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

<span data-ttu-id="4a297-358">Содержимое `People` коллекции может изменяться при вставке, удалении или повторном упорядочении записей.</span><span class="sxs-lookup"><span data-stu-id="4a297-358">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="4a297-359">Когда компонент перерисовывается, `<DetailsEditor>` компонент может измениться для получения различных `Details` значений параметров.</span><span class="sxs-lookup"><span data-stu-id="4a297-359">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="4a297-360">Это может привести к более сложной визуализации, чем ожидалось.</span><span class="sxs-lookup"><span data-stu-id="4a297-360">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="4a297-361">В некоторых случаях перерисовка может привести к появлению видимых различий в поведении, таких как потеря фокуса элементов.</span><span class="sxs-lookup"><span data-stu-id="4a297-361">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="4a297-362">Процесс сопоставления можно контролировать с помощью `@key` атрибута директивы.</span><span class="sxs-lookup"><span data-stu-id="4a297-362">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="4a297-363">`@key`заставляет алгоритм сравнения гарантировать сохранение элементов или компонентов на основе значения ключа:</span><span class="sxs-lookup"><span data-stu-id="4a297-363">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

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

<span data-ttu-id="4a297-364">При изменении `<DetailsEditor>` `person` коллекции алгоритм сравнения удерживает связи между экземплярами и экземплярами. `People`</span><span class="sxs-lookup"><span data-stu-id="4a297-364">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="4a297-365">Если объект `Person` удаляется `People` из списка, то из пользовательского интерфейса `<DetailsEditor>` удаляется только соответствующий экземпляр.</span><span class="sxs-lookup"><span data-stu-id="4a297-365">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="4a297-366">Другие экземпляры остаются без изменений.</span><span class="sxs-lookup"><span data-stu-id="4a297-366">Other instances are left unchanged.</span></span>
* <span data-ttu-id="4a297-367">Если в `Person` какой-либо позиции в списке вставляется элемент, то `<DetailsEditor>` в соответствующую позиции вставляется один новый экземпляр.</span><span class="sxs-lookup"><span data-stu-id="4a297-367">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="4a297-368">Другие экземпляры остаются без изменений.</span><span class="sxs-lookup"><span data-stu-id="4a297-368">Other instances are left unchanged.</span></span>
* <span data-ttu-id="4a297-369">При `Person` повторном упорядочении записей соответствующие `<DetailsEditor>` экземпляры сохраняются и повторно упорядочиваются в пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="4a297-369">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="4a297-370">В некоторых сценариях использование `@key` сводится к уменьшению сложности повторного отображения и позволяет избежать потенциальных проблем с элементами, изменяющимися с отслеживанием состояния DOM, например положением фокуса.</span><span class="sxs-lookup"><span data-stu-id="4a297-370">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4a297-371">Ключи являются локальными для каждого элемента или компонента контейнера.</span><span class="sxs-lookup"><span data-stu-id="4a297-371">Keys are local to each container element or component.</span></span> <span data-ttu-id="4a297-372">Ключи не сравниваются глобально по всему документу.</span><span class="sxs-lookup"><span data-stu-id="4a297-372">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="4a297-373">Когда следует использовать \@ключ</span><span class="sxs-lookup"><span data-stu-id="4a297-373">When to use \@key</span></span>

<span data-ttu-id="4a297-374">Обычно имеет смысл использовать `@key` каждый раз при подготовке списка (например, `@foreach` в блоке), а подходящее `@key`значение существует для определения.</span><span class="sxs-lookup"><span data-stu-id="4a297-374">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="4a297-375">Кроме того, можно `@key` использовать, чтобы предотвратить блазор с сохранением поддерева элемента или компонента при изменении объекта:</span><span class="sxs-lookup"><span data-stu-id="4a297-375">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="4a297-376">Если `@currentPerson` `<div>` изменяется`@key` , директива атрибута заставляет блазор отбросить все и его потомки и перестроить поддерево в пользовательском интерфейсе с помощью новых элементов и компонентов.</span><span class="sxs-lookup"><span data-stu-id="4a297-376">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="4a297-377">Это может быть полезно, если необходимо гарантировать, что при `@currentPerson` изменении состояние пользовательского интерфейса не сохраняется.</span><span class="sxs-lookup"><span data-stu-id="4a297-377">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="4a297-378">Когда не следует использовать \@ключ</span><span class="sxs-lookup"><span data-stu-id="4a297-378">When not to use \@key</span></span>

<span data-ttu-id="4a297-379">При использовании различий в `@key`производительности возникают затраты на производительность.</span><span class="sxs-lookup"><span data-stu-id="4a297-379">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="4a297-380">Затраты на производительность не слишком велики, но указывают `@key` только в том случае, если управление правилами сохранения элементов или компонентов выгодно для приложения.</span><span class="sxs-lookup"><span data-stu-id="4a297-380">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="4a297-381">Даже если `@key` не используется, блазор сохраняет дочерние элементы и экземпляры компонентов насколько это возможно.</span><span class="sxs-lookup"><span data-stu-id="4a297-381">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="4a297-382">Единственным преимуществом использования `@key` является управление *тем, как* экземпляры модели сопоставляются с сохраненными экземплярами компонента, а не алгоритмом сравнения, который выбирает сопоставление.</span><span class="sxs-lookup"><span data-stu-id="4a297-382">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="4a297-383">Какие значения следует использовать для \@ключа</span><span class="sxs-lookup"><span data-stu-id="4a297-383">What values to use for \@key</span></span>

<span data-ttu-id="4a297-384">Как правило, имеет смысл указать одно из следующих видов значений для `@key`:</span><span class="sxs-lookup"><span data-stu-id="4a297-384">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="4a297-385">Экземпляры объектов Model (например, `Person` экземпляр, как в предыдущем примере).</span><span class="sxs-lookup"><span data-stu-id="4a297-385">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="4a297-386">Это гарантирует сохранение на основе равенства ссылок на объекты.</span><span class="sxs-lookup"><span data-stu-id="4a297-386">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="4a297-387">Уникальные идентификаторы (например, значения первичного ключа типа `int`, `string`или `Guid`).</span><span class="sxs-lookup"><span data-stu-id="4a297-387">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="4a297-388">Убедитесь, что значения, `@key` используемые для, не конфликтуют.</span><span class="sxs-lookup"><span data-stu-id="4a297-388">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="4a297-389">Если конфликтные значения обнаруживаются в одном родительском элементе, Блазор создает исключение, поскольку оно не может детерминированно сопоставлять старые элементы или компоненты с новыми элементами или компонентами.</span><span class="sxs-lookup"><span data-stu-id="4a297-389">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="4a297-390">Используйте только уникальные значения, такие как экземпляры объекта или значения первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="4a297-390">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="4a297-391">Методы жизненного цикла</span><span class="sxs-lookup"><span data-stu-id="4a297-391">Lifecycle methods</span></span>

<span data-ttu-id="4a297-392">`OnInitializedAsync`и `OnInitialized` выполняют код для инициализации компонента.</span><span class="sxs-lookup"><span data-stu-id="4a297-392">`OnInitializedAsync` and `OnInitialized` execute code to initialize the component.</span></span> <span data-ttu-id="4a297-393">Для выполнения асинхронной операции используйте `OnInitializedAsync` `await` и ключевое слово для операции:</span><span class="sxs-lookup"><span data-stu-id="4a297-393">To perform an asynchronous operation, use `OnInitializedAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

<span data-ttu-id="4a297-394">Для синхронной операции используйте `OnInitialized`:</span><span class="sxs-lookup"><span data-stu-id="4a297-394">For a synchronous operation, use `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="4a297-395">`OnParametersSetAsync`и `OnParametersSet` вызываются, когда компонент получил параметры от родительского элемента, и значения присваиваются свойствам.</span><span class="sxs-lookup"><span data-stu-id="4a297-395">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="4a297-396">Эти методы выполняются после инициализации компонента и при каждом отображении компонента:</span><span class="sxs-lookup"><span data-stu-id="4a297-396">These methods are executed after component initialization and each time the component is rendered:</span></span>

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

<span data-ttu-id="4a297-397">`OnAfterRenderAsync`и `OnAfterRender` вызываются после завершения подготовки компонента к просмотру.</span><span class="sxs-lookup"><span data-stu-id="4a297-397">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="4a297-398">В этот момент заполнены ссылки на элементы и компоненты.</span><span class="sxs-lookup"><span data-stu-id="4a297-398">Element and component references are populated at this point.</span></span> <span data-ttu-id="4a297-399">Используйте этот этап для выполнения дополнительных шагов инициализации с помощью готового к просмотру содержимого, например для активации сторонних библиотек JavaScript, которые работают с визуализированными элементами DOM.</span><span class="sxs-lookup"><span data-stu-id="4a297-399">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="4a297-400">`OnAfterRender`*не вызывается при предварительной отрисовке на сервере.*</span><span class="sxs-lookup"><span data-stu-id="4a297-400">`OnAfterRender` *is not called when prerendering on the server.*</span></span>

<span data-ttu-id="4a297-401">Параметр для `OnAfterRenderAsync` и`OnAfterRender`имеетзначение. `firstRender`</span><span class="sxs-lookup"><span data-stu-id="4a297-401">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender` is:</span></span>

* <span data-ttu-id="4a297-402">Устанавливается в `true` первый раз, когда вызывается экземпляр компонента.</span><span class="sxs-lookup"><span data-stu-id="4a297-402">Set to `true` the first time that the component instance is invoked.</span></span>
* <span data-ttu-id="4a297-403">Гарантирует, что работа по инициализации выполняется только один раз.</span><span class="sxs-lookup"><span data-stu-id="4a297-403">Ensures that initialization work is only performed once.</span></span>

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

### <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="4a297-404">Обработка незавершенных асинхронных действий при отрисовке</span><span class="sxs-lookup"><span data-stu-id="4a297-404">Handle incomplete async actions at render</span></span>

<span data-ttu-id="4a297-405">Асинхронные действия, выполняемые в событиях жизненного цикла, могут не завершиться до подготовки компонента к просмотру.</span><span class="sxs-lookup"><span data-stu-id="4a297-405">Asynchronous actions performed in lifecycle events may not have completed before the component is rendered.</span></span> <span data-ttu-id="4a297-406">Объекты могут быть `null` заполнены или незаполнены данными во время выполнения метода жизненного цикла.</span><span class="sxs-lookup"><span data-stu-id="4a297-406">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="4a297-407">Предоставьте логику отрисовки для подтверждения инициализации объектов.</span><span class="sxs-lookup"><span data-stu-id="4a297-407">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="4a297-408">Отрисовывает элементы пользовательского интерфейса заполнителя (например, сообщение загрузки), пока объекты `null`являются объектами.</span><span class="sxs-lookup"><span data-stu-id="4a297-408">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="4a297-409">В компоненте `OnInitializedAsync` шаблонов блазор переопределяется в асичронаусли получения данных прогноза (`forecasts`). `FetchData`</span><span class="sxs-lookup"><span data-stu-id="4a297-409">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="4a297-410">Если `forecasts` параметр `null`имеет значение, пользователю выводится сообщение о загрузке.</span><span class="sxs-lookup"><span data-stu-id="4a297-410">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="4a297-411">После завершения `OnInitializedAsync` возврата по завершении компонент перерисовывается с обновленным состоянием. `Task`</span><span class="sxs-lookup"><span data-stu-id="4a297-411">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="4a297-412">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="4a297-412">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a><span data-ttu-id="4a297-413">Выполнить код до установки параметров</span><span class="sxs-lookup"><span data-stu-id="4a297-413">Execute code before parameters are set</span></span>

<span data-ttu-id="4a297-414">`SetParameters`можно переопределить для выполнения кода перед установкой параметров:</span><span class="sxs-lookup"><span data-stu-id="4a297-414">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterView parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="4a297-415">Если `base.SetParameters` не вызывается, Пользовательский код может интерпретировать значение входящих параметров любым необходимым образом.</span><span class="sxs-lookup"><span data-stu-id="4a297-415">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="4a297-416">Например, входящие параметры не обязательно должны быть назначены свойствам класса.</span><span class="sxs-lookup"><span data-stu-id="4a297-416">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

### <a name="suppress-refreshing-of-the-ui"></a><span data-ttu-id="4a297-417">Отключить обновление пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="4a297-417">Suppress refreshing of the UI</span></span>

<span data-ttu-id="4a297-418">`ShouldRender`можно переопределить, чтобы отключить обновление пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="4a297-418">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="4a297-419">Если реализация возвращает `true`, Пользовательский интерфейс обновляется.</span><span class="sxs-lookup"><span data-stu-id="4a297-419">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="4a297-420">Даже если `ShouldRender` переопределяется, компонент всегда первоначально готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="4a297-420">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="4a297-421">Освобождение компонентов с помощью IDisposable</span><span class="sxs-lookup"><span data-stu-id="4a297-421">Component disposal with IDisposable</span></span>

<span data-ttu-id="4a297-422">Если компонент реализует <xref:System.IDisposable>, [метод Dispose](/dotnet/standard/garbage-collection/implementing-dispose) вызывается при удалении компонента из пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="4a297-422">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="4a297-423">Следующий компонент использует `@implements IDisposable` `Dispose` и метод:</span><span class="sxs-lookup"><span data-stu-id="4a297-423">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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

## <a name="routing"></a><span data-ttu-id="4a297-424">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="4a297-424">Routing</span></span>

<span data-ttu-id="4a297-425">Маршрутизация в Блазор достигается путем предоставления шаблона маршрута для каждого доступного компонента в приложении.</span><span class="sxs-lookup"><span data-stu-id="4a297-425">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="4a297-426">При компиляции файла Razor с `@page` директивой созданному классу <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> присваивается Указание шаблона маршрута.</span><span class="sxs-lookup"><span data-stu-id="4a297-426">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="4a297-427">Во время выполнения маршрутизатор ищет классы компонентов с помощью `RouteAttribute` и отображает, какой из компонентов имеет шаблон маршрута, соответствующий запрошенному URL-адресу.</span><span class="sxs-lookup"><span data-stu-id="4a297-427">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="4a297-428">К компоненту можно применить несколько шаблонов маршрутов.</span><span class="sxs-lookup"><span data-stu-id="4a297-428">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="4a297-429">Следующий компонент отвечает на запросы `/BlazorRoute` и: `/DifferentBlazorRoute`</span><span class="sxs-lookup"><span data-stu-id="4a297-429">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="4a297-430">Параметры маршрута</span><span class="sxs-lookup"><span data-stu-id="4a297-430">Route parameters</span></span>

<span data-ttu-id="4a297-431">Компоненты могут принимать параметры маршрута из шаблона маршрута, указанного в `@page` директиве.</span><span class="sxs-lookup"><span data-stu-id="4a297-431">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="4a297-432">Маршрутизатор использует параметры маршрута для заполнения соответствующих параметров компонента.</span><span class="sxs-lookup"><span data-stu-id="4a297-432">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="4a297-433">*Компонент параметра маршрута*:</span><span class="sxs-lookup"><span data-stu-id="4a297-433">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="4a297-434">Необязательные параметры не поддерживаются `@page` , поэтому в приведенном выше примере применяются две директивы.</span><span class="sxs-lookup"><span data-stu-id="4a297-434">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="4a297-435">Первый позволяет переходить к компоненту без параметра.</span><span class="sxs-lookup"><span data-stu-id="4a297-435">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="4a297-436">Вторая `@page` директива `{text}` принимает параметр Route и присваивает значение `Text` свойству.</span><span class="sxs-lookup"><span data-stu-id="4a297-436">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="4a297-437">Наследование базового класса для взаимодействия с кодом программной части</span><span class="sxs-lookup"><span data-stu-id="4a297-437">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="4a297-438">Файлы компонентов сочетают разметку HTML C# и обрабатывают код в одном файле.</span><span class="sxs-lookup"><span data-stu-id="4a297-438">Component files mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="4a297-439">`@inherits` Директиву можно использовать для предоставления блазор приложений с «кодом программной части», который отделяет разметку компонента от кода обработки.</span><span class="sxs-lookup"><span data-stu-id="4a297-439">The `@inherits` directive can be used to provide Blazor apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="4a297-440">В [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) показано, как компонент может наследовать базовый класс, `BlazorRocksBase`чтобы предоставить свойства и методы компонента.</span><span class="sxs-lookup"><span data-stu-id="4a297-440">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="4a297-441">*Pages/блазорроккс. Razor*:</span><span class="sxs-lookup"><span data-stu-id="4a297-441">*Pages/BlazorRocks.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

<span data-ttu-id="4a297-442">*BlazorRocksBase.CS*:</span><span class="sxs-lookup"><span data-stu-id="4a297-442">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="4a297-443">Базовый класс должен быть производным от `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="4a297-443">The base class should derive from `ComponentBase`.</span></span>

## <a name="import-components"></a><span data-ttu-id="4a297-444">Импорт компонентов</span><span class="sxs-lookup"><span data-stu-id="4a297-444">Import components</span></span>

<span data-ttu-id="4a297-445">Пространство имен компонента, созданного с помощью Razor, основано на следующих:</span><span class="sxs-lookup"><span data-stu-id="4a297-445">The namespace of a component authored with Razor is based on:</span></span>

* <span data-ttu-id="4a297-446">Проект `RootNamespace`.</span><span class="sxs-lookup"><span data-stu-id="4a297-446">The project's `RootNamespace`.</span></span>
* <span data-ttu-id="4a297-447">Путь от корня проекта к компоненту.</span><span class="sxs-lookup"><span data-stu-id="4a297-447">The path from the project root to the component.</span></span> <span data-ttu-id="4a297-448">Например, `ComponentsSample/Pages/Index.razor` находится в пространстве имен `ComponentsSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="4a297-448">For example, `ComponentsSample/Pages/Index.razor` is in the namespace `ComponentsSample.Pages`.</span></span> <span data-ttu-id="4a297-449">Компоненты следуют C# правилам привязки имен.</span><span class="sxs-lookup"><span data-stu-id="4a297-449">Components follow C# name binding rules.</span></span> <span data-ttu-id="4a297-450">В случае *index. Razor*все компоненты в той же папке, *страницах*и родительской папке *компонентссампле*находятся в области действия.</span><span class="sxs-lookup"><span data-stu-id="4a297-450">In the case of *Index.razor*, all components in the same folder, *Pages*, and the parent folder, *ComponentsSample*, are in scope.</span></span>

<span data-ttu-id="4a297-451">Компоненты, определенные в другом пространстве имен, могут быть добавлены в область с помощью директивы [ \@using](xref:mvc/views/razor#using) Razor.</span><span class="sxs-lookup"><span data-stu-id="4a297-451">Components defined in a different namespace can be brought into scope using Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="4a297-452">Если в папке `ComponentsSample/Shared/`существует `NavMenu.razor`другой компонент, то этот компонент можно использовать в `Index.razor` со следующей `@using` инструкцией:</span><span class="sxs-lookup"><span data-stu-id="4a297-452">If another component, `NavMenu.razor`, exists in the folder `ComponentsSample/Shared/`, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using ComponentsSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="4a297-453">На компоненты также можно ссылаться с помощью полных имен, что устраняет необходимость в [ \@](xref:mvc/views/razor#using) директиве using:</span><span class="sxs-lookup"><span data-stu-id="4a297-453">Components can also be referenced using their fully qualified names, which removes the need for the [\@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<ComponentsSample.Shared.NavMenu></ComponentsSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="4a297-454">`global::` Квалификация не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="4a297-454">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="4a297-455">Импорт компонентов с псевдонимами `using` операторов (например, `@using Foo = Bar`) не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="4a297-455">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="4a297-456">Частично определенные имена не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="4a297-456">Partially qualified names aren't supported.</span></span> <span data-ttu-id="4a297-457">Например, добавление `@using ComponentsSample` и создание ссылок `NavMenu.razor` с `<Shared.NavMenu></Shared.NavMenu>` помощью не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="4a297-457">For example, adding `@using ComponentsSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="4a297-458">Атрибуты условного HTML-элемента</span><span class="sxs-lookup"><span data-stu-id="4a297-458">Conditional HTML element attributes</span></span>

<span data-ttu-id="4a297-459">Атрибуты HTML-элемента условно подготавливаются в зависимости от значения .NET.</span><span class="sxs-lookup"><span data-stu-id="4a297-459">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="4a297-460">Если значение равно `false` или `null`, то атрибут не подготавливается к просмотру.</span><span class="sxs-lookup"><span data-stu-id="4a297-460">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="4a297-461">Если значение равно `true`, атрибут отображается в виде сворачивания.</span><span class="sxs-lookup"><span data-stu-id="4a297-461">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="4a297-462">В следующем примере определяет, `IsCompleted` отображается ли `checked` в разметке элемента:</span><span class="sxs-lookup"><span data-stu-id="4a297-462">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="4a297-463">Если `IsCompleted` имеет `true`значение, флажок отображается следующим образом:</span><span class="sxs-lookup"><span data-stu-id="4a297-463">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="4a297-464">Если `IsCompleted` имеет `false`значение, флажок отображается следующим образом:</span><span class="sxs-lookup"><span data-stu-id="4a297-464">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="4a297-465">Дополнительные сведения см. в разделе <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="4a297-465">For more information, see <xref:mvc/views/razor>.</span></span>

## <a name="raw-html"></a><span data-ttu-id="4a297-466">Необработанный HTML</span><span class="sxs-lookup"><span data-stu-id="4a297-466">Raw HTML</span></span>

<span data-ttu-id="4a297-467">Строки обычно подготавливаются с помощью текстовых узлов DOM, что означает, что любая разметка, которую они могут содержать, игнорируется и обрабатывается как литеральный текст.</span><span class="sxs-lookup"><span data-stu-id="4a297-467">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="4a297-468">Для отрисовки необработанного HTML-кода заключите `MarkupString` содержимое HTML в значение.</span><span class="sxs-lookup"><span data-stu-id="4a297-468">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="4a297-469">Значение анализируется как HTML или SVG и вставляется в модель DOM.</span><span class="sxs-lookup"><span data-stu-id="4a297-469">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="4a297-470">Визуализация необработанного HTML-кода, созданного из любого ненадежного источника, является **угрозой безопасности** , и ее следует избегать.</span><span class="sxs-lookup"><span data-stu-id="4a297-470">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="4a297-471">В следующем примере показано использование `MarkupString` типа для добавления блока статического HTML-содержимого в отображаемые выходные данные компонента:</span><span class="sxs-lookup"><span data-stu-id="4a297-471">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="4a297-472">Шаблонные компоненты</span><span class="sxs-lookup"><span data-stu-id="4a297-472">Templated components</span></span>

<span data-ttu-id="4a297-473">Шаблонные компоненты — это компоненты, принимающие один или несколько шаблонов пользовательского интерфейса в качестве параметров, которые затем можно использовать как часть логики отрисовки компонента.</span><span class="sxs-lookup"><span data-stu-id="4a297-473">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="4a297-474">Шаблонные компоненты позволяют создавать более высокие компоненты более высокого уровня, чем обычные компоненты.</span><span class="sxs-lookup"><span data-stu-id="4a297-474">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="4a297-475">Ниже приведены несколько примеров.</span><span class="sxs-lookup"><span data-stu-id="4a297-475">A couple of examples include:</span></span>

* <span data-ttu-id="4a297-476">Компонент таблицы, позволяющий пользователю указывать шаблоны для заголовков, строк и нижних колонтитулов таблицы.</span><span class="sxs-lookup"><span data-stu-id="4a297-476">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="4a297-477">Компонент списка, позволяющий пользователю указать шаблон для отрисовки элементов в списке.</span><span class="sxs-lookup"><span data-stu-id="4a297-477">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="4a297-478">Параметры шаблона</span><span class="sxs-lookup"><span data-stu-id="4a297-478">Template parameters</span></span>

<span data-ttu-id="4a297-479">Шаблонный компонент определяется путем указания одного или нескольких параметров компонента типа `RenderFragment` или. `RenderFragment<T>`</span><span class="sxs-lookup"><span data-stu-id="4a297-479">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="4a297-480">Фрагмент отрисовки представляет сегмент пользовательского интерфейса для отрисовки.</span><span class="sxs-lookup"><span data-stu-id="4a297-480">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="4a297-481">`RenderFragment<T>`принимает параметр типа, который может быть указан при вызове фрагмента прорисовки.</span><span class="sxs-lookup"><span data-stu-id="4a297-481">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="4a297-482">`TableTemplate`см</span><span class="sxs-lookup"><span data-stu-id="4a297-482">`TableTemplate` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

<span data-ttu-id="4a297-483">При использовании компонента шаблона можно указать параметры шаблона с помощью дочерних элементов, совпадающих с именами параметров (`TableHeader` и `RowTemplate` в следующем примере):</span><span class="sxs-lookup"><span data-stu-id="4a297-483">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

### <a name="template-context-parameters"></a><span data-ttu-id="4a297-484">Параметры контекста шаблона</span><span class="sxs-lookup"><span data-stu-id="4a297-484">Template context parameters</span></span>

<span data-ttu-id="4a297-485">Аргументы компонента типа `RenderFragment<T>` , переданные как элементы, имеют неявный параметр с именем `context` (например, из предыдущего `@context.PetId`примера кода,), но можно изменить имя параметра с `Context` помощью атрибута дочернего элемента дерев.</span><span class="sxs-lookup"><span data-stu-id="4a297-485">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="4a297-486">В следующем примере `RowTemplate` `Context` атрибут элемента задает `pet` параметр:</span><span class="sxs-lookup"><span data-stu-id="4a297-486">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="4a297-487">Кроме того, можно указать `Context` атрибут для элемента Component.</span><span class="sxs-lookup"><span data-stu-id="4a297-487">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="4a297-488">Указанный `Context` атрибут применяется ко всем указанным параметрам шаблона.</span><span class="sxs-lookup"><span data-stu-id="4a297-488">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="4a297-489">Это может быть полезно, если необходимо указать имя параметра содержимого для неявного дочернего содержимого (без обертывания дочернего элемента).</span><span class="sxs-lookup"><span data-stu-id="4a297-489">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="4a297-490">В следующем примере `Context` атрибут отображается `TableTemplate` в элементе и применяется ко всем параметрам шаблона:</span><span class="sxs-lookup"><span data-stu-id="4a297-490">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

### <a name="generic-typed-components"></a><span data-ttu-id="4a297-491">Универсальные типы компонентов</span><span class="sxs-lookup"><span data-stu-id="4a297-491">Generic-typed components</span></span>

<span data-ttu-id="4a297-492">Шаблонные компоненты часто вводятся в универсальном виде.</span><span class="sxs-lookup"><span data-stu-id="4a297-492">Templated components are often generically typed.</span></span> <span data-ttu-id="4a297-493">Например, универсальный `ListViewTemplate` компонент можно использовать для отображения `IEnumerable<T>` значений.</span><span class="sxs-lookup"><span data-stu-id="4a297-493">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="4a297-494">Чтобы определить универсальный компонент, используйте `@typeparam` директиву для указания параметров типа:</span><span class="sxs-lookup"><span data-stu-id="4a297-494">To define a generic component, use the `@typeparam` directive to specify type parameters:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

<span data-ttu-id="4a297-495">При использовании компонентов универсальной типизации параметр типа выводится, если это возможно:</span><span class="sxs-lookup"><span data-stu-id="4a297-495">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="4a297-496">В противном случае параметр типа необходимо явно указать с помощью атрибута, совпадающего с именем параметра типа.</span><span class="sxs-lookup"><span data-stu-id="4a297-496">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="4a297-497">В следующем примере `TItem="Pet"` указывает тип:</span><span class="sxs-lookup"><span data-stu-id="4a297-497">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="4a297-498">Каскадные значения и параметры</span><span class="sxs-lookup"><span data-stu-id="4a297-498">Cascading values and parameters</span></span>

<span data-ttu-id="4a297-499">В некоторых сценариях неудобно передавать данные из компонента-предка в дочерний компонент с помощью [параметров компонента](#component-parameters), особенно при наличии нескольких слоев компонентов.</span><span class="sxs-lookup"><span data-stu-id="4a297-499">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="4a297-500">Каскадные значения и параметры позволяют решить эту проблему, предоставляя удобный способ, позволяющий компоненту-предку предоставить значение всем его дочерним компонентам.</span><span class="sxs-lookup"><span data-stu-id="4a297-500">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="4a297-501">Каскадные значения и параметры также предоставляют подход для координации компонентов.</span><span class="sxs-lookup"><span data-stu-id="4a297-501">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="4a297-502">Пример темы</span><span class="sxs-lookup"><span data-stu-id="4a297-502">Theme example</span></span>

<span data-ttu-id="4a297-503">В следующем примере из примера приложения `ThemeInfo` класс указывает сведения о теме для перетекания иерархии компонентов, чтобы все кнопки в пределах данной части приложения совпадали с тем же стилем.</span><span class="sxs-lookup"><span data-stu-id="4a297-503">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="4a297-504">*Уисемеклассес/themeinfo указывает расположение. CS*:</span><span class="sxs-lookup"><span data-stu-id="4a297-504">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="4a297-505">Компонент-предок может предоставлять каскадное значение с помощью компонента каскадного значения.</span><span class="sxs-lookup"><span data-stu-id="4a297-505">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="4a297-506">`CascadingValue` Компонент заключает поддерево иерархии компонентов и предоставляет одно значение всем компонентам в этом поддереве.</span><span class="sxs-lookup"><span data-stu-id="4a297-506">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="4a297-507">Например, в примере приложения указываются сведения о теме`ThemeInfo`() в одном из макетов приложения в виде каскадного параметра для всех компонентов, составляющих текст `@Body` макета свойства.</span><span class="sxs-lookup"><span data-stu-id="4a297-507">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="4a297-508">`ButtonClass`присваивается значение `btn-success` в компоненте макета.</span><span class="sxs-lookup"><span data-stu-id="4a297-508">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="4a297-509">Любой компонент-потомок может использовать это свойство через `ThemeInfo` каскадный объект.</span><span class="sxs-lookup"><span data-stu-id="4a297-509">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="4a297-510">`CascadingValuesParametersLayout`см</span><span class="sxs-lookup"><span data-stu-id="4a297-510">`CascadingValuesParametersLayout` component:</span></span>

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

<span data-ttu-id="4a297-511">Чтобы использовать каскадные значения, компоненты объявляют каскадные параметры с помощью `[CascadingParameter]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="4a297-511">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="4a297-512">Каскадные значения привязываются к каскадным параметрам по типу.</span><span class="sxs-lookup"><span data-stu-id="4a297-512">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="4a297-513">В примере приложения `CascadingValuesParametersTheme` компонент привязывает `ThemeInfo` каскадное значение к каскадному параметру.</span><span class="sxs-lookup"><span data-stu-id="4a297-513">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="4a297-514">Параметр используется для задания класса CSS для одной из кнопок, отображаемых компонентом.</span><span class="sxs-lookup"><span data-stu-id="4a297-514">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="4a297-515">`CascadingValuesParametersTheme`см</span><span class="sxs-lookup"><span data-stu-id="4a297-515">`CascadingValuesParametersTheme` component:</span></span>

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

<span data-ttu-id="4a297-516">Чтобы вычислить несколько значений одного типа в одном поддереве, укажите уникальную `Name` строку для каждого `CascadingValue` компонента и соответствующий `CascadingParameter`ему объект.</span><span class="sxs-lookup"><span data-stu-id="4a297-516">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="4a297-517">В следующем примере два `CascadingValue` компонента разворачивают разные `MyCascadingType` экземпляры по имени:</span><span class="sxs-lookup"><span data-stu-id="4a297-517">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

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

<span data-ttu-id="4a297-518">В компоненте-потомках каскадные параметры получают значения из соответствующих каскадных значений в компоненте-предке по имени:</span><span class="sxs-lookup"><span data-stu-id="4a297-518">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```cshtml
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="4a297-519">Пример Табсет</span><span class="sxs-lookup"><span data-stu-id="4a297-519">TabSet example</span></span>

<span data-ttu-id="4a297-520">Каскадные параметры также позволяют компонентам работать в иерархии компонентов.</span><span class="sxs-lookup"><span data-stu-id="4a297-520">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="4a297-521">Например, рассмотрим следующий пример *табсет* в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="4a297-521">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="4a297-522">Пример приложения имеет интерфейс, `ITab` который реализуется с помощью вкладок:</span><span class="sxs-lookup"><span data-stu-id="4a297-522">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="4a297-523">Компонент использует компонент, который содержит несколько `Tab` компонентов: `TabSet` `CascadingValuesParametersTabSet`</span><span class="sxs-lookup"><span data-stu-id="4a297-523">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="4a297-524">Дочерние `Tab` компоненты явно не передаются в `TabSet`качестве параметров в.</span><span class="sxs-lookup"><span data-stu-id="4a297-524">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="4a297-525">Вместо этого дочерние `Tab` компоненты являются частью дочернего содержимого `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="4a297-525">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="4a297-526">Тем не менее `TabSet` , по-прежнему необходимо узнать `Tab` о каждом компоненте, чтобы он мог отобразить заголовки и активную вкладку. Чтобы обеспечить эту координацию, не требуя дополнительного `TabSet` кода, компонент *может предоставить себя как каскадное значение* , которое затем будет использоваться компонентами- `Tab` потомками.</span><span class="sxs-lookup"><span data-stu-id="4a297-526">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="4a297-527">`TabSet`см</span><span class="sxs-lookup"><span data-stu-id="4a297-527">`TabSet` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

<span data-ttu-id="4a297-528">Компоненты- `Tab` потомки захватывают `TabSet` объект, содержащийся в виде каскадного `Tab` параметра, поэтому компоненты добавляются `TabSet` в и координаты, на которой активна вкладка.</span><span class="sxs-lookup"><span data-stu-id="4a297-528">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="4a297-529">`Tab`см</span><span class="sxs-lookup"><span data-stu-id="4a297-529">`Tab` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="4a297-530">Шаблоны Razor</span><span class="sxs-lookup"><span data-stu-id="4a297-530">Razor templates</span></span>

<span data-ttu-id="4a297-531">Фрагменты визуализации можно определить с помощью синтаксиса шаблона Razor.</span><span class="sxs-lookup"><span data-stu-id="4a297-531">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="4a297-532">Шаблоны Razor позволяют определить фрагмент пользовательского интерфейса и принять следующий формат:</span><span class="sxs-lookup"><span data-stu-id="4a297-532">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="4a297-533">В следующем примере показано, как задавать `RenderFragment` значения `RenderFragment<T>` и, а также подготавливать шаблоны непосредственно в компоненте.</span><span class="sxs-lookup"><span data-stu-id="4a297-533">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="4a297-534">Фрагменты рендеринга также могут передаваться в качестве аргументов в [шаблонные компоненты](#templated-components).</span><span class="sxs-lookup"><span data-stu-id="4a297-534">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

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

<span data-ttu-id="4a297-535">Отображаемые выходные данные предыдущего кода:</span><span class="sxs-lookup"><span data-stu-id="4a297-535">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="4a297-536">Логика Рендертрибуилдер вручную</span><span class="sxs-lookup"><span data-stu-id="4a297-536">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="4a297-537">`Microsoft.AspNetCore.Components.RenderTree`предоставляет методы для управления компонентами и элементами, включая создание компонентов вручную в C# коде.</span><span class="sxs-lookup"><span data-stu-id="4a297-537">`Microsoft.AspNetCore.Components.RenderTree` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="4a297-538">`RenderTreeBuilder` Использование для создания компонентов является расширенным сценарием.</span><span class="sxs-lookup"><span data-stu-id="4a297-538">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="4a297-539">Неправильно сформированный компонент (например, незакрытый тег разметки) может привести к неопределенному поведению.</span><span class="sxs-lookup"><span data-stu-id="4a297-539">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="4a297-540">Рассмотрим следующий `PetDetails` компонент, который можно вручную встроить в другой компонент:</span><span class="sxs-lookup"><span data-stu-id="4a297-540">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="4a297-541">В следующем примере цикл в `CreateComponent` методе создает три `PetDetails` компонента.</span><span class="sxs-lookup"><span data-stu-id="4a297-541">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="4a297-542">При вызове `RenderTreeBuilder` методов для создания компонентов (`OpenComponent` и `AddAttribute`) порядковые номера представляют собой номера строк исходного кода.</span><span class="sxs-lookup"><span data-stu-id="4a297-542">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="4a297-543">Алгоритм разницы Блазор зависит от порядковых номеров, соответствующих разным строкам кода, а не отдельных вызовов вызова.</span><span class="sxs-lookup"><span data-stu-id="4a297-543">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="4a297-544">При создании компонента с `RenderTreeBuilder` методами жестко указывайте аргументы для порядковых номеров.</span><span class="sxs-lookup"><span data-stu-id="4a297-544">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="4a297-545">**Использование вычисления или счетчика для формирования порядкового номера может привести к снижению производительности.**</span><span class="sxs-lookup"><span data-stu-id="4a297-545">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="4a297-546">Дополнительные сведения см. в разделе [порядковые номера относятся к разделу номера строк кода и порядок выполнения](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="4a297-546">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="4a297-547">`BuiltContent`см</span><span class="sxs-lookup"><span data-stu-id="4a297-547">`BuiltContent` component:</span></span>

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

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="4a297-548">Порядковые номера связаны с номерами строк кода, а не с порядком выполнения</span><span class="sxs-lookup"><span data-stu-id="4a297-548">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="4a297-549">Файлы `.razor` блазор всегда компилируются.</span><span class="sxs-lookup"><span data-stu-id="4a297-549">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="4a297-550">Это может быть отличным преимуществом `.razor` , поскольку этап компиляции можно использовать для вставки сведений, повышающих производительность приложения во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="4a297-550">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="4a297-551">Ключевым примером этих улучшений являются *порядковые номера*.</span><span class="sxs-lookup"><span data-stu-id="4a297-551">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="4a297-552">Порядковые номера указывают среде выполнения, какие выходные данные поступили из разных и упорядоченных строк кода.</span><span class="sxs-lookup"><span data-stu-id="4a297-552">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="4a297-553">Среда выполнения использует эти сведения для создания эффективных различий дерева в линейное время, что является гораздо быстрее, чем обычно возможно для общего алгоритма различения дерева.</span><span class="sxs-lookup"><span data-stu-id="4a297-553">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="4a297-554">Рассмотрим следующий простой `.razor` файл:</span><span class="sxs-lookup"><span data-stu-id="4a297-554">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="4a297-555">Предыдущий код компилируется примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="4a297-555">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="4a297-556">Когда код выполняется в первый раз, если `someFlag` имеет `true`значение, построитель получает:</span><span class="sxs-lookup"><span data-stu-id="4a297-556">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="4a297-557">Sequence</span><span class="sxs-lookup"><span data-stu-id="4a297-557">Sequence</span></span> | <span data-ttu-id="4a297-558">Тип</span><span class="sxs-lookup"><span data-stu-id="4a297-558">Type</span></span>      | <span data-ttu-id="4a297-559">Данные</span><span class="sxs-lookup"><span data-stu-id="4a297-559">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="4a297-560">0</span><span class="sxs-lookup"><span data-stu-id="4a297-560">0</span></span>        | <span data-ttu-id="4a297-561">Текстовый узел</span><span class="sxs-lookup"><span data-stu-id="4a297-561">Text node</span></span> | <span data-ttu-id="4a297-562">Первая</span><span class="sxs-lookup"><span data-stu-id="4a297-562">First</span></span>  |
| <span data-ttu-id="4a297-563">1</span><span class="sxs-lookup"><span data-stu-id="4a297-563">1</span></span>        | <span data-ttu-id="4a297-564">Текстовый узел</span><span class="sxs-lookup"><span data-stu-id="4a297-564">Text node</span></span> | <span data-ttu-id="4a297-565">Вторая</span><span class="sxs-lookup"><span data-stu-id="4a297-565">Second</span></span> |

<span data-ttu-id="4a297-566">Представьте, `someFlag` что `false`преобразуются, и разметка снова готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="4a297-566">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="4a297-567">На этот раз построитель получит:</span><span class="sxs-lookup"><span data-stu-id="4a297-567">This time, the builder receives:</span></span>

| <span data-ttu-id="4a297-568">Sequence</span><span class="sxs-lookup"><span data-stu-id="4a297-568">Sequence</span></span> | <span data-ttu-id="4a297-569">Тип</span><span class="sxs-lookup"><span data-stu-id="4a297-569">Type</span></span>       | <span data-ttu-id="4a297-570">Данные</span><span class="sxs-lookup"><span data-stu-id="4a297-570">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="4a297-571">1</span><span class="sxs-lookup"><span data-stu-id="4a297-571">1</span></span>        | <span data-ttu-id="4a297-572">Текстовый узел</span><span class="sxs-lookup"><span data-stu-id="4a297-572">Text node</span></span>  | <span data-ttu-id="4a297-573">Вторая</span><span class="sxs-lookup"><span data-stu-id="4a297-573">Second</span></span> |

<span data-ttu-id="4a297-574">Когда среда выполнения выполняет поиск различий, она видит, что элемент в `0` последовательности был удален, поэтому он создает следующий тривиальный *сценарий редактирования*:</span><span class="sxs-lookup"><span data-stu-id="4a297-574">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="4a297-575">Удалите первый текстовый узел.</span><span class="sxs-lookup"><span data-stu-id="4a297-575">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="4a297-576">Что становится неправильным, если вы создаете порядковые номера программным способом</span><span class="sxs-lookup"><span data-stu-id="4a297-576">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="4a297-577">Вместо этого Представьте себе, что вы написали следующую логику конструктора деревьев визуализации:</span><span class="sxs-lookup"><span data-stu-id="4a297-577">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="4a297-578">Теперь первые выходные данные:</span><span class="sxs-lookup"><span data-stu-id="4a297-578">Now, the first output is:</span></span>

| <span data-ttu-id="4a297-579">Sequence</span><span class="sxs-lookup"><span data-stu-id="4a297-579">Sequence</span></span> | <span data-ttu-id="4a297-580">Тип</span><span class="sxs-lookup"><span data-stu-id="4a297-580">Type</span></span>      | <span data-ttu-id="4a297-581">Данные</span><span class="sxs-lookup"><span data-stu-id="4a297-581">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="4a297-582">0</span><span class="sxs-lookup"><span data-stu-id="4a297-582">0</span></span>        | <span data-ttu-id="4a297-583">Текстовый узел</span><span class="sxs-lookup"><span data-stu-id="4a297-583">Text node</span></span> | <span data-ttu-id="4a297-584">Первая</span><span class="sxs-lookup"><span data-stu-id="4a297-584">First</span></span>  |
| <span data-ttu-id="4a297-585">1</span><span class="sxs-lookup"><span data-stu-id="4a297-585">1</span></span>        | <span data-ttu-id="4a297-586">Текстовый узел</span><span class="sxs-lookup"><span data-stu-id="4a297-586">Text node</span></span> | <span data-ttu-id="4a297-587">Вторая</span><span class="sxs-lookup"><span data-stu-id="4a297-587">Second</span></span> |

<span data-ttu-id="4a297-588">Этот результат идентичен предыдущему случаю, поэтому отрицательные проблемы не возникают.</span><span class="sxs-lookup"><span data-stu-id="4a297-588">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="4a297-589">`someFlag`находится `false` во второй визуализации, а выходные данные:</span><span class="sxs-lookup"><span data-stu-id="4a297-589">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="4a297-590">Sequence</span><span class="sxs-lookup"><span data-stu-id="4a297-590">Sequence</span></span> | <span data-ttu-id="4a297-591">Тип</span><span class="sxs-lookup"><span data-stu-id="4a297-591">Type</span></span>      | <span data-ttu-id="4a297-592">Данные</span><span class="sxs-lookup"><span data-stu-id="4a297-592">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="4a297-593">0</span><span class="sxs-lookup"><span data-stu-id="4a297-593">0</span></span>        | <span data-ttu-id="4a297-594">Текстовый узел</span><span class="sxs-lookup"><span data-stu-id="4a297-594">Text node</span></span> | <span data-ttu-id="4a297-595">Вторая</span><span class="sxs-lookup"><span data-stu-id="4a297-595">Second</span></span> |

<span data-ttu-id="4a297-596">На этот раз алгоритм diff видит, что были внесены *два* изменения, и алгоритм создает следующий сценарий редактирования:</span><span class="sxs-lookup"><span data-stu-id="4a297-596">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="4a297-597">Измените значение первого текстового узла на `Second`.</span><span class="sxs-lookup"><span data-stu-id="4a297-597">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="4a297-598">Удалите второй текстовый узел.</span><span class="sxs-lookup"><span data-stu-id="4a297-598">Remove the second text node.</span></span>

<span data-ttu-id="4a297-599">При формировании порядковых номеров теряются все полезные сведения о том `if/else` , где находятся ветви и циклы в исходном коде.</span><span class="sxs-lookup"><span data-stu-id="4a297-599">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="4a297-600">Это приводит к **удвоению в два раза больше** , чем раньше.</span><span class="sxs-lookup"><span data-stu-id="4a297-600">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="4a297-601">Это тривиальный пример.</span><span class="sxs-lookup"><span data-stu-id="4a297-601">This is a trivial example.</span></span> <span data-ttu-id="4a297-602">В более реалистичных случаях со сложными и глубокими вложенными структурами, особенно с циклами, затраты на производительность более серьезны.</span><span class="sxs-lookup"><span data-stu-id="4a297-602">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="4a297-603">Вместо того, чтобы сразу определять, какие блоки или ветви цикла были вставлены или удалены, алгоритм diff должен рекурсивно пребираться в деревьях отрисовки и, как правило, создает гораздо более длительные операции редактирования, так как он сообщает о том, как старую и новую структуры связь друг с другом.</span><span class="sxs-lookup"><span data-stu-id="4a297-603">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="4a297-604">Руководство и выводы</span><span class="sxs-lookup"><span data-stu-id="4a297-604">Guidance and conclusions</span></span>

* <span data-ttu-id="4a297-605">Производительность приложения снижается, если порядковые номера создаются динамически.</span><span class="sxs-lookup"><span data-stu-id="4a297-605">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="4a297-606">Платформа не может автоматически создавать собственные порядковые номера во время выполнения, поскольку необходимая информация не существует, если она не захвачена во время компиляции.</span><span class="sxs-lookup"><span data-stu-id="4a297-606">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="4a297-607">Не записывайте длинные блоки логики, реализованной `RenderTreeBuilder` вручную.</span><span class="sxs-lookup"><span data-stu-id="4a297-607">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="4a297-608">Предпочитать `.razor` файлы и разрешите компилятору работать с порядковыми номерами.</span><span class="sxs-lookup"><span data-stu-id="4a297-608">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span>
* <span data-ttu-id="4a297-609">Если порядковые номера задаются жестко, то для алгоритма diff требуется, чтобы только порядковые номера увеличиваются в значении.</span><span class="sxs-lookup"><span data-stu-id="4a297-609">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="4a297-610">Начальное значение и зазоры несущественны.</span><span class="sxs-lookup"><span data-stu-id="4a297-610">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="4a297-611">Один из этих вариантов — использовать номер строки кода в качестве порядкового номера или начать с нуля и увеличить на единицу или сотни (или любой другой интервал).</span><span class="sxs-lookup"><span data-stu-id="4a297-611">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* <span data-ttu-id="4a297-612">Блазор использует порядковые номера, а другие платформы пользовательского интерфейса для различения структуры не используют их.</span><span class="sxs-lookup"><span data-stu-id="4a297-612">Blazor uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="4a297-613">Сравнение выполняется гораздо быстрее, если используются порядковые номера, и блазор имеет преимущество этапа компиляции, который автоматически обрабатывает порядковые номера для разработчиков `.razor` файлов.</span><span class="sxs-lookup"><span data-stu-id="4a297-613">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring `.razor` files.</span></span>

## <a name="localization"></a><span data-ttu-id="4a297-614">Локализация</span><span class="sxs-lookup"><span data-stu-id="4a297-614">Localization</span></span>

<span data-ttu-id="4a297-615">Блазор серверные приложения локализованы по [промежуточного слоя локализации](xref:fundamentals/localization#localization-middleware).</span><span class="sxs-lookup"><span data-stu-id="4a297-615">Blazor Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="4a297-616">По промежуточного слоя выбирает соответствующие языки и региональные параметры для пользователей, запрашивающих ресурсы из приложения.</span><span class="sxs-lookup"><span data-stu-id="4a297-616">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="4a297-617">Язык и региональные параметры можно задать с помощью одного из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="4a297-617">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="4a297-618">Файлы "cookie"</span><span class="sxs-lookup"><span data-stu-id="4a297-618">Cookies</span></span>](#cookies)
* [<span data-ttu-id="4a297-619">Предоставление пользовательского интерфейса для выбора языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="4a297-619">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="4a297-620">Дополнительные сведения и примеры см. в разделе <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="4a297-620">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="4a297-621">Файлы cookie</span><span class="sxs-lookup"><span data-stu-id="4a297-621">Cookies</span></span>

<span data-ttu-id="4a297-622">Файл cookie культуры локализации может сохранять язык и региональные параметры пользователя.</span><span class="sxs-lookup"><span data-stu-id="4a297-622">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="4a297-623">Файл cookie создается `OnGet` методом страницы узла приложения (*pages/Host. cshtml. CS*).</span><span class="sxs-lookup"><span data-stu-id="4a297-623">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="4a297-624">По промежуточного слоя локализации считывает файл cookie при последующих запросах, чтобы задать язык и региональные параметры пользователя.</span><span class="sxs-lookup"><span data-stu-id="4a297-624">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="4a297-625">Использование файла cookie гарантирует, что соединение WebSocket сможет правильно распространить культуру.</span><span class="sxs-lookup"><span data-stu-id="4a297-625">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="4a297-626">Если схемы локализации основаны на пути URL-адреса или строке запроса, схема может не поддерживать работу с WebSockets, поэтому она не сохраняет культуру.</span><span class="sxs-lookup"><span data-stu-id="4a297-626">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="4a297-627">Поэтому рекомендуемым подходом является использование файла cookie языка и региональных параметров локализации.</span><span class="sxs-lookup"><span data-stu-id="4a297-627">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="4a297-628">Любой метод можно использовать для назначения языка и региональных параметров, если язык и региональные параметры сохраняются в файле cookie локализации.</span><span class="sxs-lookup"><span data-stu-id="4a297-628">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="4a297-629">Если у приложения уже есть установленная схема локализации для ASP.NET Core на стороне сервера, продолжайте использовать существующую инфраструктуру локализации приложения и задавайте файл cookie культуры локализации в схеме приложения.</span><span class="sxs-lookup"><span data-stu-id="4a297-629">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="4a297-630">В следующем примере показано, как задать текущий язык и региональные параметры в файле cookie, который может быть прочитан по промежуточного слоя локализации.</span><span class="sxs-lookup"><span data-stu-id="4a297-630">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="4a297-631">Создайте файл *pages/Host. cshtml. CS* со следующим содержимым в приложении блазор Server:</span><span class="sxs-lookup"><span data-stu-id="4a297-631">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

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

<span data-ttu-id="4a297-632">Локализация обрабатывается в приложении:</span><span class="sxs-lookup"><span data-stu-id="4a297-632">Localization is handled in the app:</span></span>

1. <span data-ttu-id="4a297-633">Браузер отправляет в приложение исходный HTTP-запрос.</span><span class="sxs-lookup"><span data-stu-id="4a297-633">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="4a297-634">Язык и региональные параметры назначаются по промежуточного слоя локализации.</span><span class="sxs-lookup"><span data-stu-id="4a297-634">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="4a297-635">Метод в *_Host. cshtml. CS* сохраняет язык и региональные параметры в файле cookie как часть ответа. `OnGet`</span><span class="sxs-lookup"><span data-stu-id="4a297-635">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="4a297-636">Браузер открывает соединение WebSocket для создания интерактивного сеанса Блазор Server.</span><span class="sxs-lookup"><span data-stu-id="4a297-636">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="4a297-637">По промежуточного слоя для локализации считывает файл cookie и назначает язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="4a297-637">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="4a297-638">Сеанс сервера Блазор начинается с правильного языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="4a297-638">The Blazor Server session begins with the correct culture.</span></span>

## <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="4a297-639">Предоставление пользовательского интерфейса для выбора языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="4a297-639">Provide UI to choose the culture</span></span>

<span data-ttu-id="4a297-640">Для предоставления пользовательского интерфейса, позволяющего пользователю выбирать язык и региональные параметры, рекомендуется *подход на основе перенаправления* .</span><span class="sxs-lookup"><span data-stu-id="4a297-640">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="4a297-641">Процесс аналогичен тому, что происходит в веб-приложении, когда пользователь пытается получить доступ к защищенному ресурсу&mdash;, пользователь перенаправляется на страницу входа, а затем перенаправляется обратно к исходному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="4a297-641">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="4a297-642">Приложение сохраняет выбранный язык и региональные параметры пользователя с помощью перенаправления к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="4a297-642">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="4a297-643">Контроллер устанавливает выбранный язык и региональные параметры пользователя в файл cookie и перенаправляет пользователя обратно к исходному коду URI.</span><span class="sxs-lookup"><span data-stu-id="4a297-643">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="4a297-644">Установите конечную точку HTTP на сервере, чтобы задать выбранный язык и региональные параметры пользователя в файле cookie, и выполните перенаправление обратно в исходный URI:</span><span class="sxs-lookup"><span data-stu-id="4a297-644">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

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
> <span data-ttu-id="4a297-645">Используйте результат `LocalRedirect` действия, чтобы предотвратить атаки с открытым перенаправлением.</span><span class="sxs-lookup"><span data-stu-id="4a297-645">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="4a297-646">Дополнительные сведения см. в разделе <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="4a297-646">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="4a297-647">В следующем компоненте показан пример выполнения начального перенаправления, когда пользователь выбирает язык и региональные параметры:</span><span class="sxs-lookup"><span data-stu-id="4a297-647">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

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

### <a name="use-net-localization-scenarios-in-blazor-apps"></a><span data-ttu-id="4a297-648">Использование сценариев локализации .NET в приложениях Блазор</span><span class="sxs-lookup"><span data-stu-id="4a297-648">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="4a297-649">В приложениях Блазор доступны следующие сценарии локализации и глобализации .NET:</span><span class="sxs-lookup"><span data-stu-id="4a297-649">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="4a297-650">. Система ресурсов NET</span><span class="sxs-lookup"><span data-stu-id="4a297-650">.NET's resources system</span></span>
* <span data-ttu-id="4a297-651">Форматирование чисел и дат, зависящих от языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="4a297-651">Culture-specific number and date formatting</span></span>

<span data-ttu-id="4a297-652">`@bind` Функция блазор выполняет глобализацию на основе текущего языка и региональных параметров пользователя.</span><span class="sxs-lookup"><span data-stu-id="4a297-652">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="4a297-653">Дополнительные сведения см. в разделе [Привязка данных](#data-binding) .</span><span class="sxs-lookup"><span data-stu-id="4a297-653">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="4a297-654">В настоящее время поддерживается ограниченный набор сценариев локализации ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="4a297-654">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="4a297-655">`IStringLocalizer<>`*поддерживается* в приложениях блазор.</span><span class="sxs-lookup"><span data-stu-id="4a297-655">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="4a297-656">`IHtmlLocalizer<>`, `IViewLocalizer<>`и локализация заметок к данным ASP.NET Core сценариев MVC и **не поддерживается** в приложениях блазор.</span><span class="sxs-lookup"><span data-stu-id="4a297-656">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="4a297-657">Дополнительные сведения см. в разделе <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="4a297-657">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="4a297-658">Масштабируемые изображения векторной графики (SVG)</span><span class="sxs-lookup"><span data-stu-id="4a297-658">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="4a297-659">Поскольку блазор визуализирует HTML-файлы, поддерживаемые браузером изображения, включая масштабируемые векторные графики (SVG *),* поддерживаются с `<img>` помощью тега:</span><span class="sxs-lookup"><span data-stu-id="4a297-659">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="4a297-660">Аналогичным образом SVG-изображения поддерживаются в правилах CSS файла стилей ( *. CSS*):</span><span class="sxs-lookup"><span data-stu-id="4a297-660">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="4a297-661">Однако встроенная разметка SVG не поддерживается во всех сценариях.</span><span class="sxs-lookup"><span data-stu-id="4a297-661">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="4a297-662">Если `<svg>` тег размещается непосредственно в файле компонента ( *. Razor*), то поддерживается базовая визуализация изображений, но многие расширенные сценарии пока не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="4a297-662">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="4a297-663">Например, `<use>` в настоящее время теги не учитываются и `@bind` не могут использоваться с некоторыми тегами SVG.</span><span class="sxs-lookup"><span data-stu-id="4a297-663">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="4a297-664">В будущем выпуске мы планируем устранить эти ограничения.</span><span class="sxs-lookup"><span data-stu-id="4a297-664">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4a297-665">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="4a297-665">Additional resources</span></span>

* <span data-ttu-id="4a297-666"><xref:security/blazor/server>&ndash; Содержит рекомендации по созданию приложений блазор Server, которые должны конкурировать с нехваткой ресурсов.</span><span class="sxs-lookup"><span data-stu-id="4a297-666"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
