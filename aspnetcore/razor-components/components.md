---
title: Создание и использование компонентов Razor
author: guardrex
description: Узнайте, как создать и использовать компоненты Razor, включая как привязка к данным, обработка событий и управление компонент жизненные циклы.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: razor-components/components
ms.openlocfilehash: f8ac7f3844b94a162e8d1c45f80ae153d89536ee
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515696"
---
# <a name="create-and-use-razor-components"></a><span data-ttu-id="e4ff4-103">Создание и использование компонентов Razor</span><span class="sxs-lookup"><span data-stu-id="e4ff4-103">Create and use Razor Components</span></span>

<span data-ttu-id="e4ff4-104">По [Люк Лэтем](https://github.com/guardrex), [Дэниэл рот](https://github.com/danroth27), и [Morné Zaayman](https://github.com/MorneZaayman)</span><span class="sxs-lookup"><span data-stu-id="e4ff4-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Morné Zaayman](https://github.com/MorneZaayman)</span></span>

<span data-ttu-id="e4ff4-105">[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="e4ff4-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="e4ff4-106">Сведения о необходимых компонентах см. в разделе [Начало работы](xref:razor-components/get-started).</span><span class="sxs-lookup"><span data-stu-id="e4ff4-106">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

<span data-ttu-id="e4ff4-107">Razor компоненты приложения создаются с помощью *компоненты*.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-107">Razor Components apps are built using *components*.</span></span> <span data-ttu-id="e4ff4-108">Компонент — это автономное фрагмент пользовательский интерфейс (UI), например страницы, диалоговое окно или форму.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-108">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="e4ff4-109">Компонент включает разметки HTML и логика обработки, необходимые для вставки данных и реагировать на события пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-109">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="e4ff4-110">Компоненты, гибкий и простой.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-110">Components are flexible and lightweight.</span></span> <span data-ttu-id="e4ff4-111">Они могут быть вложенными повторно и совместно использоваться проектами.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-111">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="e4ff4-112">Классы компонентов</span><span class="sxs-lookup"><span data-stu-id="e4ff4-112">Component classes</span></span>

<span data-ttu-id="e4ff4-113">Компоненты обычно выполнены в файлах Razor компонента (*.razor*) с помощью сочетания C# и HTML-разметку (*.cshtml* файлы используются в приложениях Blazor).</span><span class="sxs-lookup"><span data-stu-id="e4ff4-113">Components are typically implemented in Razor Component files (*.razor*) using a combination of C# and HTML markup (*.cshtml* files are used in Blazor apps).</span></span>

<span data-ttu-id="e4ff4-114">Компоненты могут разрабатываться в приложениях Razor компонентов, с помощью *.cshtml* расширение файла, до тех пор, пока файлы определяются как файлы Razor компонента, с помощью `_RazorComponentInclude` свойства MSBuild.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-114">Components can be authored in Razor Components apps using the *.cshtml* file extension as long as the files are identified as Razor Component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="e4ff4-115">Например, приложение, созданное с помощью шаблона Razor компонент указывает, что все *.cshtml* файлы в разделе *компоненты* папки, которые должны рассматриваться как файлы Razor компоненты:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-115">For example, an app created using the Razor Component template specifies that all *.cshtml* files under the *Components* folder should be treated as Razor Components files:</span></span>

```xml
<_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
```

<span data-ttu-id="e4ff4-116">Пользовательский Интерфейс для компонента определяется с помощью HTML.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="e4ff4-117">Логика динамического отображения (например выражения, циклы и условные выражения) добавляется с помощью встроенного синтаксиса C# под названием [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="e4ff4-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="e4ff4-118">При компиляции приложения компонентов Razor, HTML-разметка и C# логикой отрисовки, преобразуются в класс компонента.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-118">When a Razor Components app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="e4ff4-119">Имя создаваемого класса соответствует имени файла.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="e4ff4-120">Члены класса компонента определены `@functions` блока (более одного `@functions` блок является допустимым).</span><span class="sxs-lookup"><span data-stu-id="e4ff4-120">Members of the component class are defined in a `@functions` block (more than one `@functions` block is permissible).</span></span> <span data-ttu-id="e4ff4-121">В `@functions` блока, состояние компонентов (свойства, поля) указывается с помощью методов для обработки событий или для определения других логикой компонента.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-121">In the `@functions` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span>

<span data-ttu-id="e4ff4-122">Затем можно использовать члены компонента, как отрисовки части компонента приложения логики с помощью C# выражения, которые начинаются с `@`.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-122">Component members can then be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="e4ff4-123">Например C# поле визуализируется с помощью префикса `@` на имя поля.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-123">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="e4ff4-124">Следующий пример вычисляет и отображает:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-124">The following example evaluates and renders:</span></span>

* `_headingFontStyle` <span data-ttu-id="e4ff4-125">значение свойства CSS для `font-style`.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-125">to the CSS property value for `font-style`.</span></span>
* `_headingText` <span data-ttu-id="e4ff4-126">к содержимому `<h1>` элемент.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-126">to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@functions {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="e4ff4-127">После изначально визуализируется компонента, компонент повторно создает его дерева визуализации в ответ на события.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-127">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="e4ff4-128">Компоненты Razor затем сравнивает новое дерево визуализации от предыдущей и применяет все изменения для модели объектов обозревателя документов (DOM).</span><span class="sxs-lookup"><span data-stu-id="e4ff4-128">Razor Components then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="e4ff4-129">Интегрировать компоненты в Razor Pages и MVC-приложения</span><span class="sxs-lookup"><span data-stu-id="e4ff4-129">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="e4ff4-130">Используйте компоненты с существующих приложений Razor Pages и MVC.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-130">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="e4ff4-131">Нет необходимости переписывать существующие страницы или представления, чтобы использовать компоненты Razor.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-131">There's no need to rewrite existing pages or views to use Razor Components.</span></span> <span data-ttu-id="e4ff4-132">При отображении страницы или представления, компоненты являются предварительно визуализированных&dagger; в то же время.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-132">When the page or view is rendered, components are prerendered&dagger; at the same time.</span></span> 

> [!NOTE]
> <span data-ttu-id="e4ff4-133">&dagger;Предварительной визуализации на сервере включен для Razor компонентов приложений по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-133">&dagger;Server-side prerendering is enabled for Razor Components apps by default.</span></span> <span data-ttu-id="e4ff4-134">Blazor клиентских приложений будут поддерживать предварительной визуализации в предстоящем выпуске предварительной версии 4.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-134">Client-side Blazor apps will support prerendering in the upcoming Preview 4 release.</span></span> <span data-ttu-id="e4ff4-135">Дополнительные сведения см. в разделе [обновить шаблоны/по промежуточного слоя для использования MapFallbackToPage/файла](https://github.com/aspnet/AspNetCore/issues/8852).</span><span class="sxs-lookup"><span data-stu-id="e4ff4-135">For more information, see [Update templates/middleware to use MapFallbackToPage/File](https://github.com/aspnet/AspNetCore/issues/8852).</span></span>

<span data-ttu-id="e4ff4-136">Для подготовки к просмотру компонента из страницы или представления, используйте `RenderComponentAsync<TComponent>` вспомогательный метод HTML:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-136">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

<span data-ttu-id="e4ff4-137">Компоненты к просмотру из страницы и представления еще не интерактивных в выпуске предварительной версии 3.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-137">Components rendered from pages and views aren't yet interactive in the Preview 3 release.</span></span> <span data-ttu-id="e4ff4-138">Например выбора кнопки не приводит к возникновению вызов метода.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-138">For example, selecting a button doesn't trigger a method call.</span></span> <span data-ttu-id="e4ff4-139">Следующей предварительной версии будет устранить это ограничение и добавить поддержку для подготовки к просмотру компоненты, используя обычный синтаксис элементов и атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-139">A future preview will address this limitation and add support for rendering components using the normal element and attribute syntax.</span></span>

<span data-ttu-id="e4ff4-140">Хотя страницы и представления можно использовать компоненты, обратное не так.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-140">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="e4ff4-141">Компоненты нельзя использовать сценарии конкретного представления и страницы, такие как частичные представления и разделах.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-141">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="e4ff4-142">Для использования логики из частичного представления в компоненте, показателем логику частичного представления в компонент.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-142">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

## <a name="using-components"></a><span data-ttu-id="e4ff4-143">Использование компонентов</span><span class="sxs-lookup"><span data-stu-id="e4ff4-143">Using components</span></span>

<span data-ttu-id="e4ff4-144">Компоненты можно добавить и другие компоненты, объявляя их с использованием синтаксиса элемента HTML.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-144">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="e4ff4-145">Разметка для использования компонента выглядит как тег HTML с именем, соответствующем типу компонента.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-145">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="e4ff4-146">Следующая разметка отображает `HeadingComponent` (*HeadingComponent.cshtml*) экземпляр:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-146">The following markup renders a `HeadingComponent` (*HeadingComponent.cshtml*) instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.cshtml?name=snippet_HeadingComponent)]

## <a name="component-parameters"></a><span data-ttu-id="e4ff4-147">Параметры компонентов</span><span class="sxs-lookup"><span data-stu-id="e4ff4-147">Component parameters</span></span>

<span data-ttu-id="e4ff4-148">Компоненты могут иметь *параметры компонента*, которые определены с помощью *закрытые* оформлен свойства класса компонента `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-148">Components can have *component parameters*, which are defined using *non-public* properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="e4ff4-149">Используйте атрибуты, чтобы указать аргументы для компонента в разметке.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-149">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="e4ff4-150">В следующем примере `ParentComponent` задает значение `Title` свойство `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-150">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`:</span></span>

<span data-ttu-id="e4ff4-151">*ParentComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-151">*ParentComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=5)]

<span data-ttu-id="e4ff4-152">*ChildComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-152">*ChildComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=7-8)]

## <a name="child-content"></a><span data-ttu-id="e4ff4-153">Дочерний тип содержимого</span><span class="sxs-lookup"><span data-stu-id="e4ff4-153">Child content</span></span>

<span data-ttu-id="e4ff4-154">Компоненты можно установить содержимое из другого компонента.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-154">Components can set the content of another component.</span></span> <span data-ttu-id="e4ff4-155">Назначение компонента предоставляет содержимое между тегами, указывающие получатель.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-155">The assigning component provides the content between the tags that specify the receiving component.</span></span> <span data-ttu-id="e4ff4-156">Например `ParentComponent` можно предоставить содержимое для подготовки к просмотру, дочернего компонента, поместив содержимое внутри `<ChildComponent>` теги.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-156">For example, a `ParentComponent` can provide content for rendering by a Child component by placing the content inside `<ChildComponent>` tags.</span></span>

<span data-ttu-id="e4ff4-157">*ParentComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-157">*ParentComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=6-7)]

<span data-ttu-id="e4ff4-158">У компонента дочерних `ChildContent` свойство, которое представляет `RenderFragment`.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-158">The Child component has a `ChildContent` property that represents a `RenderFragment`.</span></span> <span data-ttu-id="e4ff4-159">Значение `ChildContent` располагается в разметку дочерних компонентов, где должны отображаться содержимое.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-159">The value of `ChildContent` is positioned in the child component's markup where the content should be rendered.</span></span> <span data-ttu-id="e4ff4-160">В следующем примере значение `ChildContent` получен из родительского компонента и отображено внутри панели начальной загрузки `panel-body`.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-160">In the following example, the value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="e4ff4-161">*ChildComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-161">*ChildComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=3,10-11)]

> [!NOTE]
> <span data-ttu-id="e4ff4-162">Получение свойств `RenderFragment` содержимого должен иметь имя `ChildContent` по соглашению.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-162">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

## <a name="data-binding"></a><span data-ttu-id="e4ff4-163">Привязка данных</span><span class="sxs-lookup"><span data-stu-id="e4ff4-163">Data binding</span></span>

<span data-ttu-id="e4ff4-164">Привязка данных для компонентов и элементов DOM осуществляется с помощью `bind` атрибута.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-164">Data binding to both components and DOM elements is accomplished with the `bind` attribute.</span></span> <span data-ttu-id="e4ff4-165">Следующий пример связывает `ItalicsCheck` свойство флажок проверки состояния:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-165">The following example binds the `ItalicsCheck` property to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    bind="@_italicsCheck">
```

<span data-ttu-id="e4ff4-166">Если флажок выбран и очищен, значение свойства присваивается `true` и `false`, соответственно.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-166">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="e4ff4-167">Флажок в пользовательском Интерфейсе обновляется, только в том случае, когда компонент отображается, не в ответ на изменение значения свойства.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-167">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="e4ff4-168">Так как компоненты отображаются после выполнения код обработчика событий, обновления свойств обычно отражаются в пользовательском Интерфейсе сразу же.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-168">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="e4ff4-169">С помощью `bind` с `CurrentValue` свойство (`<input bind="@CurrentValue">`) является по существу эквивалентно следующему:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-169">Using `bind` with a `CurrentValue` property (`<input bind="@CurrentValue">`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue" 
    onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)">
```

<span data-ttu-id="e4ff4-170">При отображении компонента `value` элемента ввода поступают из `CurrentValue` свойство.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-170">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="e4ff4-171">Когда пользователь вводит в текстовом поле `onchange` события и `CurrentValue` свойству присваивается измененное значение.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-171">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="e4ff4-172">На самом деле, формирования кода является чуть более сложный, так как `bind` обрабатывает несколько случаев, когда выполняются преобразования типов.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-172">In reality, the code generation is a little more complex because `bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="e4ff4-173">В принципе `bind` связывает текущее значение выражения с `value` атрибут и обрабатывает изменения, с помощью зарегистрированного обработчика.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-173">In principle, `bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="e4ff4-174">В дополнение к `onchange`, свойство можно привязать с помощью другие события, например `oninput` обеспечивается более подробно о том, что для привязки к:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-174">In addition to `onchange`, the property can be bound using other events like `oninput` by being more explicit about what to bind to:</span></span>

```cshtml
<input type="text" bind-value-oninput="@CurrentValue">
```

<span data-ttu-id="e4ff4-175">В отличие от `onchange`, `oninput` активируется для каждого символа, вводимых в текстовое поле.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-175">Unlike `onchange`, `oninput` fires for every character that is input into the text box.</span></span>

**<span data-ttu-id="e4ff4-176">Строки формата</span><span class="sxs-lookup"><span data-stu-id="e4ff4-176">Format strings</span></span>**

<span data-ttu-id="e4ff4-177">Привязка данных работает с <xref:System.DateTime> форматирования строк.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-177">Data binding works with <xref:System.DateTime> format strings.</span></span> <span data-ttu-id="e4ff4-178">В настоящее время недоступны другие выражения формат, например валюта или числовые форматы.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-178">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input bind="@StartDate" format-value="yyyy-MM-dd">

@functions {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="e4ff4-179">`format-value` Атрибут указывает формат даты для применения к `value` из `input` элемент.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-179">The `format-value` attribute specifies the date format to apply to the `value` of the `input` element.</span></span> <span data-ttu-id="e4ff4-180">Выполнить синтаксический анализ значение также используется формат при `onchange` событием.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-180">The format is also used to parse the value when an `onchange` event occurs.</span></span>

**<span data-ttu-id="e4ff4-181">Параметры компонентов</span><span class="sxs-lookup"><span data-stu-id="e4ff4-181">Component parameters</span></span>**

<span data-ttu-id="e4ff4-182">Привязки также распознает параметры компонента, где `bind-{property}` можно привязать значение свойства по компонентам.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-182">Binding also recognizes component parameters, where `bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="e4ff4-183">В следующем компоненте используются `ChildComponent` и привязывает `ParentYear` параметр из родительского `Year` параметр в компоненте дочерних:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-183">The following component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

<span data-ttu-id="e4ff4-184">Родительский компонент:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-184">Parent component:</span></span>

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent bind-Year="@ParentYear" />

<button class="btn btn-primary" onclick="@ChangeTheYear">
    Change Year to 1986
</button>

@functions {
    [Parameter]
    private int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="e4ff4-185">Дочерний компонент:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-185">Child component:</span></span>

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@functions {
    [Parameter]
    private int Year { get; set; }

    [Parameter]
    private Action<int> YearChanged { get; set; }
}
```

<span data-ttu-id="e4ff4-186">Загрузка `ParentComponent` создает следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-186">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="e4ff4-187">Если значение `ParentYear` изменении свойства, нажав кнопку в `ParentComponent`, `Year` свойство `ChildComponent` обновляется.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-187">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="e4ff4-188">Новое значение `Year` отображается в пользовательском Интерфейсе при `ParentComponent` когда:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-188">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="e4ff4-189">`Year` Параметра возможности привязки, поскольку в нем вспомогательное `YearChanged` событие, которое совпадает с типом `Year` параметра.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-189">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="e4ff4-190">По соглашению `<ChildComponent bind-Year="@ParentYear" />` фактически эквивалентна записи,</span><span class="sxs-lookup"><span data-stu-id="e4ff4-190">By convention, `<ChildComponent bind-Year="@ParentYear" />` is essentially equivalent to writing,</span></span>

```cshtml
    <ChildComponent bind-Year-YearChanged="@ParentYear" />
```

<span data-ttu-id="e4ff4-191">В общем случае можно привязать свойство к соответствующий обработчик событий с помощью `bind-property-event` атрибута.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-191">In general, a property can be bound to a corresponding event handler using `bind-property-event` attribute.</span></span>

## <a name="event-handling"></a><span data-ttu-id="e4ff4-192">Обработка событий</span><span class="sxs-lookup"><span data-stu-id="e4ff4-192">Event handling</span></span>

<span data-ttu-id="e4ff4-193">Компоненты Razor обеспечивают функции обработки событий.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-193">Razor Components provide event handling features.</span></span> <span data-ttu-id="e4ff4-194">Для атрибута элемент HTML с именем `on<event>` (например, `onclick`, `onsubmit`) со значением, типом делегата, компоненты Razor обрабатывает значение атрибута как обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-194">For an HTML element attribute named `on<event>` (for example, `onclick`, `onsubmit`) with a delegate-typed value, Razor Components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="e4ff4-195">Имя атрибута всегда начинается с `on`.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-195">The attribute's name always starts with `on`.</span></span>

<span data-ttu-id="e4ff4-196">Следующий код вызывает `UpdateHeading` метод при нажатии кнопки в пользовательском Интерфейсе:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-196">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    private void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="e4ff4-197">Следующий код вызывает `CheckboxChanged` метода при его изменении в пользовательском Интерфейсе:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-197">The following code calls the `CheckboxChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" onchange="@CheckboxChanged">

@functions {
    private void CheckboxChanged()
    {
        ...
    }
}
```

<span data-ttu-id="e4ff4-198">Обработчики событий также может быть асинхронной и возврата <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-198">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="e4ff4-199">Нет необходимости вручную вызывать функции `StateHasChanged()`.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-199">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="e4ff4-200">Исключения записываются при их возникновении.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-200">Exceptions are logged when they occur.</span></span>

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    private async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="e4ff4-201">Для некоторых событий допускаются типы аргументов событий, связанных с событием.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-201">For some events, event-specific event argument types are permitted.</span></span> <span data-ttu-id="e4ff4-202">Если доступ к одному из таких типов событий, не является обязательным, это не обязательно в вызове метода.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-202">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="e4ff4-203">Приведен список событий, поддерживаемые аргументы.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-203">The list of supported event arguments is:</span></span>

* <span data-ttu-id="e4ff4-204">UIEventArgs</span><span class="sxs-lookup"><span data-stu-id="e4ff4-204">UIEventArgs</span></span>
* <span data-ttu-id="e4ff4-205">UIChangeEventArgs</span><span class="sxs-lookup"><span data-stu-id="e4ff4-205">UIChangeEventArgs</span></span>
* <span data-ttu-id="e4ff4-206">UIKeyboardEventArgs</span><span class="sxs-lookup"><span data-stu-id="e4ff4-206">UIKeyboardEventArgs</span></span>
* <span data-ttu-id="e4ff4-207">UIMouseEventArgs</span><span class="sxs-lookup"><span data-stu-id="e4ff4-207">UIMouseEventArgs</span></span>

<span data-ttu-id="e4ff4-208">Можно также использовать лямбда-выражения:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-208">Lambda expressions can also be used:</span></span>

```cshtml
<button onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="e4ff4-209">Часто бывает удобно закрыть через дополнительные значения, такие как при итерации по набору элементов.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-209">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="e4ff4-210">В следующем примере создается три кнопки, каждый из которых вызывает `UpdateHeading` передача аргумента события (`UIMouseEventArgs`) и ее номер кнопки (`buttonNumber`) при выборе в пользовательском Интерфейсе:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-210">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@functions {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            "mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="e4ff4-211">Сделать **не** использование переменной цикла (`i`) в `for` цикл непосредственно в лямбда-выражение.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-211">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="e4ff4-212">В противном случае используется та же переменная, все лямбда-выражения, в результате чего `i`от значения, чтобы быть одинаковым во всех лямбда-выражения.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-212">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="e4ff4-213">Всегда записать его значение в локальную переменную (`buttonNumber` в предыдущем примере) и затем использовать его.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-213">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

## <a name="capture-references-to-components"></a><span data-ttu-id="e4ff4-214">Захват ссылки на компоненты</span><span class="sxs-lookup"><span data-stu-id="e4ff4-214">Capture references to components</span></span>

<span data-ttu-id="e4ff4-215">Ссылки на компонент предоставить образом получить ссылку на экземпляр компонента таким образом, вы сможете выполнять команды для этого экземпляра, такие как `Show` или `Reset`.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-215">Component references provide a way get a reference to a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="e4ff4-216">Чтобы записать ссылку на компонент, добавьте `ref` атрибут дочерний компонент, а затем определите поле с тем же именем и типом как дочерний компонент.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-216">To capture a component reference, add a `ref` attribute to the child component and then define a field with the same name and the same type as the child component.</span></span>

```cshtml
<MyLoginDialog ref="loginDialog" ... />

@functions {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="e4ff4-217">При отображении компонента `loginDialog` поле заполняется `MyLoginDialog` дочерний экземпляр компонента.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-217">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="e4ff4-218">Затем можно вызывать методы .NET экземпляра компонента.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-218">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e4ff4-219">`loginDialog` Переменной заполняется только после того как компонент отображается, и его выходные данные содержат `MyLoginDialog` элемент.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-219">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="e4ff4-220">До этого момента нет ничего для ссылки.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-220">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="e4ff4-221">Используются для операций ссылки на компонентов после завершения подготовки к просмотру компонента `OnAfterRenderAsync` или `OnAfterRender` методы.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-221">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<span data-ttu-id="e4ff4-222">Тогда как захват ссылки на компонент использует аналогичный синтаксис для [захват ссылок на элементы](xref:razor-components/javascript-interop#capture-references-to-elements), это не так [взаимодействия JavaScript](xref:razor-components/javascript-interop) функции.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-222">While capturing component references uses a similar syntax to [capturing element references](xref:razor-components/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:razor-components/javascript-interop) feature.</span></span> <span data-ttu-id="e4ff4-223">Ссылки на компонент не передаются в код JavaScript; они используются только в коде .NET.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-223">Component references aren't passed to JavaScript code; they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="e4ff4-224">Сделать **не** использовать ссылки на компонент изменяемая состояние дочерних компонентов.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-224">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="e4ff4-225">Вместо этого используйте обычный декларативные параметры для передачи данных дочерними компонентами.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-225">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="e4ff4-226">В результате дочерними компонентами для rerender правильность времени автоматически.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-226">This causes child components to rerender at the correct times automatically.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="e4ff4-227">Методы жизненного цикла</span><span class="sxs-lookup"><span data-stu-id="e4ff4-227">Lifecycle methods</span></span>

`OnInitAsync` <span data-ttu-id="e4ff4-228">и `OnInit` выполнять код для инициализации компонента.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-228">and `OnInit` execute code to initialize the component.</span></span> <span data-ttu-id="e4ff4-229">Чтобы выполнить асинхронную операцию, используйте `OnInitAsync` и `await` ключевое слово на операцию:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-229">To perform an asynchronous operation, use `OnInitAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

<span data-ttu-id="e4ff4-230">Синхронную операцию, используйте `OnInit`:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-230">For a synchronous operation, use `OnInit`:</span></span>

```csharp
protected override void OnInit()
{
    ...
}
```

`OnParametersSetAsync` <span data-ttu-id="e4ff4-231">и `OnParametersSet` вызываются, когда компонент получил параметров из его родительского элемента и значения присваиваются свойствам.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-231">and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="e4ff4-232">Эти методы применяются после инициализации компонента и затем каждый раз компонент отображается:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-232">These methods are executed after component initialization and then each time the component is rendered:</span></span>

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

`OnAfterRenderAsync` <span data-ttu-id="e4ff4-233">и `OnAfterRender` вызываются после компонент завершил отрисовку.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-233">and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="e4ff4-234">На этом этапе заполняются ссылки на элемент и компонента.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-234">Element and component references are populated at this point.</span></span> <span data-ttu-id="e4ff4-235">Позволяет выполнять дополнительные действия по инициализации с использованием готового для просмотра содержимого, например активация сторонних библиотек JavaScript, которые оперируют визуализированных элементов DOM этого этапа.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-235">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

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

`SetParameters` <span data-ttu-id="e4ff4-236">можно переопределить, чтобы выполнять код до параметры заданы:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-236">can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="e4ff4-237">Если `base.SetParameters` не вызван, пользовательский код может интерпретировать входящее значение параметров в любом способом требуется.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-237">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="e4ff4-238">Например параметры входящего не обязательно должны назначаться свойства класса.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-238">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

`ShouldRender` <span data-ttu-id="e4ff4-239">можно переопределить для подавления обновление пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-239">can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="e4ff4-240">Если реализация возвращает `true`, обновляется пользовательский Интерфейс.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-240">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="e4ff4-241">Даже если `ShouldRender` будет переопределено, компонент всегда изначально визуализируется.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-241">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="e4ff4-242">Реализация компонента с IDisposable</span><span class="sxs-lookup"><span data-stu-id="e4ff4-242">Component disposal with IDisposable</span></span>

<span data-ttu-id="e4ff4-243">Если компонент реализует <xref:System.IDisposable>, [метод Dispose](/dotnet/standard/garbage-collection/implementing-dispose) вызывается, когда компонент будет удален из пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-243">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="e4ff4-244">В следующем компоненте используются `@implements IDisposable` и `Dispose` метод:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-244">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```csharp
@using System
@implements IDisposable

...

@functions {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a><span data-ttu-id="e4ff4-245">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="e4ff4-245">Routing</span></span>

<span data-ttu-id="e4ff4-246">Маршрутизация в компонентах Razor достигается с помощью шаблона маршрута для каждого компонента, доступного в приложении.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-246">Routing in Razor Components is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="e4ff4-247">Когда *.cshtml* файл с `@page` компилируется директивы, созданном классе ему присваивается <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> указания шаблона маршрута.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-247">When a *.cshtml* file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="e4ff4-248">Во время выполнения, маршрутизатор ищет классы компонентов с `RouteAttribute` и отображает, какой компонент имеет шаблон маршрута, соответствующий запрошенного URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-248">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="e4ff4-249">Несколько шаблонов маршрута могут применяться к компоненту.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-249">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="e4ff4-250">Следующий компонент отвечает на запросы для `/BlazorRoute` и `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-250">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="e4ff4-251">Параметры маршрута</span><span class="sxs-lookup"><span data-stu-id="e4ff4-251">Route parameters</span></span>

<span data-ttu-id="e4ff4-252">Компоненты могут получать параметры маршрута с шаблоном маршрута в `@page` директива.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-252">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="e4ff4-253">Маршрутизатор использует параметры маршрута для заполнения соответствующих параметров компонента.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-253">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="e4ff4-254">*RouteParameter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-254">*RouteParameter.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter)]

<span data-ttu-id="e4ff4-255">Необязательные параметры не поддерживаются, так что два `@page` директивы применяются в приведенном выше примере.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-255">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="e4ff4-256">Первый позволяет навигации к компоненту без параметров.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-256">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="e4ff4-257">Второй `@page` принимает директивы `{text}` параметра маршрута и присваивает это значение `Text` свойства.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-257">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="e4ff4-258">Наследование базовый класс для взаимодействия кода «программной»</span><span class="sxs-lookup"><span data-stu-id="e4ff4-258">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="e4ff4-259">Файлы компонентов (*.cshtml*) смешивать разметки HTML и C# обработке кода в одном файле.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-259">Component files (*.cshtml*) mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="e4ff4-260">`@inherits` Директива может использоваться для предоставления Razor компонентов приложений с интерфейсом «кода», разделяющий разметки компонента из кода обработки.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-260">The `@inherits` directive can be used to provide Razor Components apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="e4ff4-261">[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) показано, как компонент может наследовать базовому классу, `BlazorRocksBase`, чтобы предоставлять методы и свойства компонента.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-261">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="e4ff4-262">*BlazorRocks.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-262">*BlazorRocks.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.cshtml?name=snippet_BlazorRocks)]

<span data-ttu-id="e4ff4-263">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-263">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="e4ff4-264">Базовый класс должен быть производным от `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-264">The base class should derive from `ComponentBase`.</span></span>

## <a name="razor-support"></a><span data-ttu-id="e4ff4-265">Поддержка Razor</span><span class="sxs-lookup"><span data-stu-id="e4ff4-265">Razor support</span></span>

**<span data-ttu-id="e4ff4-266">Директивы Razor</span><span class="sxs-lookup"><span data-stu-id="e4ff4-266">Razor directives</span></span>**

<span data-ttu-id="e4ff4-267">В следующей таблице показаны директивы Razor.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-267">Razor directives are shown in the following table.</span></span>

| <span data-ttu-id="e4ff4-268">Директива</span><span class="sxs-lookup"><span data-stu-id="e4ff4-268">Directive</span></span> | <span data-ttu-id="e4ff4-269">Описание</span><span class="sxs-lookup"><span data-stu-id="e4ff4-269">Description</span></span> |
| --------- | ----------- |
| [<span data-ttu-id="e4ff4-270">\@Функции</span><span class="sxs-lookup"><span data-stu-id="e4ff4-270">\@functions</span></span>](xref:mvc/views/razor#section-5) | <span data-ttu-id="e4ff4-271">Добавляет C# блок кода в компонент.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-271">Adds a C# code block to a component.</span></span> |
| `@implements` | <span data-ttu-id="e4ff4-272">Реализует интерфейс для класса созданный компонент.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-272">Implements an interface for the generated component class.</span></span> |
| [<span data-ttu-id="e4ff4-273">\@Наследует</span><span class="sxs-lookup"><span data-stu-id="e4ff4-273">\@inherits</span></span>](xref:mvc/views/razor#section-3) | <span data-ttu-id="e4ff4-274">Предоставляет полный контроль над класс, который наследует компонента.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-274">Provides full control of the class that the component inherits.</span></span> |
| [<span data-ttu-id="e4ff4-275">\@внедрить</span><span class="sxs-lookup"><span data-stu-id="e4ff4-275">\@inject</span></span>](xref:mvc/views/razor#section-4) | <span data-ttu-id="e4ff4-276">Позволяет услуг внедрения из [контейнер службы](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e4ff4-276">Enables service injection from the [service container](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e4ff4-277">Дополнительные сведения: [Внедрение зависимостей в представления](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e4ff4-277">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span> |
| `@layout` | <span data-ttu-id="e4ff4-278">Указывает компонента макета.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-278">Specifies a layout component.</span></span> <span data-ttu-id="e4ff4-279">Компоненты макета используются во избежание дублирования и несогласованности.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-279">Layout components are used to avoid code duplication and inconsistency.</span></span> |
| [<span data-ttu-id="e4ff4-280">\@Страница</span><span class="sxs-lookup"><span data-stu-id="e4ff4-280">\@page</span></span>](xref:razor-pages/index#razor-pages) | <span data-ttu-id="e4ff4-281">Указывает, что компонент должен обрабатывать запросы напрямую.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-281">Specifies that the component should handle requests directly.</span></span> <span data-ttu-id="e4ff4-282">`@page` Директивы можно указать параметром маршрута и необязательные параметры.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-282">The `@page` directive can be specified with a route and optional parameters.</span></span> <span data-ttu-id="e4ff4-283">В отличие от Razor Pages `@page` директива не обязательно должен быть первой директивой в верхней части файла.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-283">Unlike Razor Pages, the `@page` directive doesn't need to be the first directive at the top of the file.</span></span> <span data-ttu-id="e4ff4-284">Дополнительные сведения см. в разделе [Маршрутизация](xref:razor-components/routing).</span><span class="sxs-lookup"><span data-stu-id="e4ff4-284">For more information, see [Routing](xref:razor-components/routing).</span></span> |
| [<span data-ttu-id="e4ff4-285">\@С помощью</span><span class="sxs-lookup"><span data-stu-id="e4ff4-285">\@using</span></span>](xref:mvc/views/razor#using) | <span data-ttu-id="e4ff4-286">Добавляет C# `using` директивы в класс созданный компонент.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-286">Adds the C# `using` directive to the generated component class.</span></span> |
| [<span data-ttu-id="e4ff4-287">\@addTagHelper</span><span class="sxs-lookup"><span data-stu-id="e4ff4-287">\@addTagHelper</span></span>](xref:mvc/views/razor#tag-helpers) | <span data-ttu-id="e4ff4-288">Использовать `@addTagHelper` для использования компонента в другой сборке, чем сборка приложения.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-288">Use `@addTagHelper` to use a component in a different assembly than the app's assembly.</span></span> |

**<span data-ttu-id="e4ff4-289">Условные атрибуты</span><span class="sxs-lookup"><span data-stu-id="e4ff4-289">Conditional attributes</span></span>**

<span data-ttu-id="e4ff4-290">Атрибуты отображаются по условию на основе значения .NET.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-290">Attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="e4ff4-291">Если значение равно `false` или `null`, атрибут не отображается.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-291">If the value is `false` or `null`,  the attribute isn't rendered.</span></span> <span data-ttu-id="e4ff4-292">Если значение равно `true`, атрибут визуализируется в свернутом состоянии.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-292">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="e4ff4-293">В следующем примере `IsCompleted` определяет `checked` визуализируется в разметке элемента управления:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-293">In the following example, `IsCompleted` determines if `checked` is rendered in the control's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted">

@functions {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

<span data-ttu-id="e4ff4-294">Если `IsCompleted` является `true`, флажок отображается как:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-294">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked>
```

<span data-ttu-id="e4ff4-295">Если `IsCompleted` является `false`, флажок отображается как:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-295">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox">
```

**<span data-ttu-id="e4ff4-296">Дополнительные сведения о Razor</span><span class="sxs-lookup"><span data-stu-id="e4ff4-296">Additional information on Razor</span></span>**

<span data-ttu-id="e4ff4-297">Дополнительные сведения о Razor, см. в разделе [Справочник по синтаксису Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="e4ff4-297">For more information on Razor, see the [Razor syntax reference](xref:mvc/views/razor).</span></span>

## <a name="raw-html"></a><span data-ttu-id="e4ff4-298">Необработанный HTML</span><span class="sxs-lookup"><span data-stu-id="e4ff4-298">Raw HTML</span></span>

<span data-ttu-id="e4ff4-299">Строки обычно отображаются с использованием DOM текстовые узлы, что означает, что любой разметку, с которой они могут содержать игнорируется и обрабатывается как обычный текст.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-299">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="e4ff4-300">Чтобы отобразить необработанный HTML, поместить содержимое HTML в `MarkupString` значение.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-300">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="e4ff4-301">Значение анализируется как HTML или SVG и вставить в модель DOM.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-301">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="e4ff4-302">Визуализации необработанный HTML, созданный из любой ненадежных источником является **угрозу безопасности** следует избегать использования!</span><span class="sxs-lookup"><span data-stu-id="e4ff4-302">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="e4ff4-303">В следующем примере показано использование `MarkupString` тип добавляемого блока статического содержимого HTML выводимые данные компонента:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-303">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@functions {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="e4ff4-304">Компоненты на основе шаблона</span><span class="sxs-lookup"><span data-stu-id="e4ff4-304">Templated components</span></span>

<span data-ttu-id="e4ff4-305">Шаблонный компонентами являются компоненты, которые принимают один или несколько шаблонов пользовательского интерфейса в качестве параметров, которые затем могут использоваться как часть логики отображения компонента.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-305">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="e4ff4-306">Шаблонный компоненты позволяют создавать компоненты более высокого уровня, более многократно используемых, чем обычных компонентов.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-306">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="e4ff4-307">Включают несколько примеров:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-307">A couple of examples include:</span></span>

* <span data-ttu-id="e4ff4-308">Таблица компонент, который позволяет пользователю указать шаблоны для заголовка, строки и нижнего колонтитула таблицы.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-308">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="e4ff4-309">Компонент списка, который позволяет пользователю указать шаблон для подготовки к просмотру элементов в списке.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-309">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="e4ff4-310">Параметры шаблона</span><span class="sxs-lookup"><span data-stu-id="e4ff4-310">Template parameters</span></span>

<span data-ttu-id="e4ff4-311">Компонент шаблона определяется путем указания одного или нескольких параметров компонента типа `RenderFragment` или `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-311">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="e4ff4-312">Отрисовки фрагмент представляет часть пользовательского интерфейса, который отображается в компоненте.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-312">A render fragment represents a segment of UI that is rendered by the component.</span></span> <span data-ttu-id="e4ff4-313">Фрагмент визуализации при необходимости принимает параметр, который можно указать при вызове фрагмента отрисовки.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-313">A render fragment optionally takes a parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="e4ff4-314">*Components/TableTemplate.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-314">*Components/TableTemplate.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.cshtml)]

<span data-ttu-id="e4ff4-315">При использовании компонента, шаблон, параметры шаблона можно задать с помощью дочерних элементов, совпадающих с именами параметров (`TableHeader` и `RowTemplate` в следующем примере):</span><span class="sxs-lookup"><span data-stu-id="e4ff4-315">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

### <a name="template-context-parameters"></a><span data-ttu-id="e4ff4-316">Контекстные параметры шаблона</span><span class="sxs-lookup"><span data-stu-id="e4ff4-316">Template context parameters</span></span>

<span data-ttu-id="e4ff4-317">Компонент аргументы типа `RenderFragment<T>` передаваемое в качестве элементов имеют неявный параметр с именем `context` (например из в предыдущем образце кода `@context.PetId`), но можно изменить имя параметра с помощью `Context` атрибут дочерних элементов элемент.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-317">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="e4ff4-318">В следующем примере `RowTemplate` элемента `Context` атрибут задает `pet` параметр:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-318">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="e4ff4-319">Кроме того, можно указать `Context` атрибута элемента компонента.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-319">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="e4ff4-320">Указанный `Context` атрибут применяется для всех остальных параметров указанного шаблона.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-320">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="e4ff4-321">Это может быть полезно, когда требуется указать имя параметра содержимого для неявного дочерний тип содержимого (без переносов любой дочерний элемент).</span><span class="sxs-lookup"><span data-stu-id="e4ff4-321">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="e4ff4-322">В следующем примере `Context` атрибут находится в `TableTemplate` элемент и применяется для всех параметров шаблона:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-322">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

### <a name="generic-typed-components"></a><span data-ttu-id="e4ff4-323">Компоненты типизированными универсальными</span><span class="sxs-lookup"><span data-stu-id="e4ff4-323">Generic-typed components</span></span>

<span data-ttu-id="e4ff4-324">Шаблонный компоненты часто универсально типизированы.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-324">Templated components are often generically typed.</span></span> <span data-ttu-id="e4ff4-325">Например, можно использовать общий компонент шаблон представления списка для подготовки к просмотру `IEnumerable<T>` значения.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-325">For example, a generic List View Template component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="e4ff4-326">Чтобы определить общий компонент, используйте `@typeparam` директиву, чтобы задать параметры типа.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-326">To define a generic component, use the `@typeparam` directive to specify type parameters.</span></span>

<span data-ttu-id="e4ff4-327">*Components/ListViewTemplate.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-327">*Components/ListViewTemplate.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.cshtml)]

<span data-ttu-id="e4ff4-328">При использовании компонентов типизированными универсальными, параметр типа выводится, если это возможно:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-328">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="e4ff4-329">В противном случае параметр типа необходимо явно указать с помощью атрибута, который совпадает с именем параметра типа.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-329">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="e4ff4-330">В следующем примере `TItem="Pet"` указывает тип:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-330">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="e4ff4-331">Каскадные значения и параметры</span><span class="sxs-lookup"><span data-stu-id="e4ff4-331">Cascading values and parameters</span></span>

<span data-ttu-id="e4ff4-332">В некоторых сценариях, нет смысла для вывода данных из компонента предка в дочерних компонентов с помощью [параметры компонента](#component-parameters), особенно в том случае, если существуют несколько уровней компонента.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-332">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="e4ff4-333">Каскадные значения и параметры решить эту проблему, предоставляя удобный способ для предка компонента для предоставления значения для всех его дочерних компонентов.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-333">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="e4ff4-334">Каскадные значения и параметры также предоставляют способ компоненты для координации.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-334">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="e4ff4-335">Пример темы</span><span class="sxs-lookup"><span data-stu-id="e4ff4-335">Theme example</span></span>

<span data-ttu-id="e4ff4-336">В следующем *темы* пример из примера приложения, `ThemeInfo` класс задает информацию темы к нижележащим сайтам иерархии компонент таким образом, все кнопки в определенной части приложения тот же стиль.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-336">In the following *Theme* example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="e4ff4-337">*UIThemeClasses/ThemeInfo.cs*:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-337">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="e4ff4-338">Компонент предка можно указать каскадные значение с помощью каскадных значение компонента.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-338">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="e4ff4-339">Компонент каскадных значения ветви иерархии компонент заключает в оболочку и предоставляет одно значение для всех компонентов в этом поддереве.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-339">The Cascading Value component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="e4ff4-340">Например, в примере приложения задает сведения о теме (`ThemeInfo`) в одном из макетов приложений как каскадного параметра для всех компонентов, составляющих тело макета `@Body` свойство.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-340">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> `ButtonClass` <span data-ttu-id="e4ff4-341">присваивается значение `btn-success` в компоненте макета.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-341">is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="e4ff4-342">Любой потомок компонент может использовать это свойство через `ThemeInfo` каскадных объекта.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-342">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="e4ff4-343">*Shared/CascadingValuesParametersLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-343">*Shared/CascadingValuesParametersLayout.cshtml*:</span></span>

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

@functions {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="e4ff4-344">Чтобы использовать каскадных значений, компоненты объявить каскадных параметров с помощью `[CascadingParameter]` атрибута или в зависимости от строковое значение имени:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-344">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute or based on a string name value:</span></span>

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

<span data-ttu-id="e4ff4-345">Привязки со строковым значением имя относится, если у вас несколько каскадных значений одного типа и необходимо разделить их в данном поддереве.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-345">Binding with a string name value is relevant if you have multiple cascading values of the same type and need to differentiate them within the same subtree.</span></span>

<span data-ttu-id="e4ff4-346">Каскадные значения привязаны к каскадные параметры по типу.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-346">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="e4ff4-347">В примере приложения каскадные параметры темы значения компонента привязывается к `ThemeInfo` каскадных значение для каскадного параметра.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-347">In the sample app, the Cascading Values Parameters Theme component binds to the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="e4ff4-348">Параметр используется для задания класс CSS для одной из кнопок, отображаемому в компоненте.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-348">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="e4ff4-349">*Pages/CascadingValuesParametersTheme.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-349">*Pages/CascadingValuesParametersTheme.cshtml*:</span></span>

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" onclick="@IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" onclick="@IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@functions {
    private int currentCount = 0;

    [CascadingParameter] protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

### <a name="tabset-example"></a><span data-ttu-id="e4ff4-350">Пример TabSet</span><span class="sxs-lookup"><span data-stu-id="e4ff4-350">TabSet example</span></span>

<span data-ttu-id="e4ff4-351">Каскадные параметры также включить компоненты для совместной работы по всей иерархии компонента.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-351">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="e4ff4-352">Например, рассмотрим следующие *TabSet* пример, в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-352">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="e4ff4-353">В примере приложения есть `ITab` интерфейса, реализуйте вкладки:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-353">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="e4ff4-354">Каскадные параметры TabSet значения компонент использует компонент набор вкладок, который содержит несколько компонентов вкладки:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-354">The Cascading Values Parameters TabSet component uses the Tab Set component, which contains several Tab components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.cshtml?name=snippet_TabSet)]

<span data-ttu-id="e4ff4-355">Вкладка дочерние компоненты не явно передаются как параметры в набор вкладок.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-355">The child Tab components aren't explicitly passed as parameters to the Tab Set.</span></span> <span data-ttu-id="e4ff4-356">Вместо этого дочерние вкладку компоненты являются частью дочернего содержимого набор вкладок.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-356">Instead, the child Tab components are part of the child content of the Tab Set.</span></span> <span data-ttu-id="e4ff4-357">Тем не менее набор вкладок по-прежнему необходимо знать о каждом компоненте вкладку, чтобы он мог преобразовать заголовки и активной вкладки. Чтобы включить эту координацию без необходимости написания дополнительного кода, компонент набор вкладок *можно указать сам как значение каскадных* , затем выбирается дочерних компонентов вкладки.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-357">However, the Tab Set still needs to know about each Tab component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the Tab Set component *can provide itself as a cascading value* that is then picked up by the descendent Tab components.</span></span>

<span data-ttu-id="e4ff4-358">*Components/TabSet.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-358">*Components/TabSet.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.cshtml)]

<span data-ttu-id="e4ff4-359">Дочерние записи компонентов вкладку содержащего набор вкладок как каскадного параметра, поэтому компоненты вкладку добавлять себя в набор вкладок и координат на вкладках активна.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-359">The descendent Tab components capture the containing Tab Set as a cascading parameter, so the Tab components add themselves to the Tab Set and coordinate on which tab is active.</span></span>

<span data-ttu-id="e4ff4-360">*Components/Tab.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-360">*Components/Tab.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.cshtml)]

## <a name="razor-templates"></a><span data-ttu-id="e4ff4-361">Шаблоны Razor</span><span class="sxs-lookup"><span data-stu-id="e4ff4-361">Razor templates</span></span>

<span data-ttu-id="e4ff4-362">Визуализации фрагментов можно определить с помощью синтаксиса Razor шаблона.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-362">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="e4ff4-363">Шаблоны Razor — это способ определения пользовательского интерфейса фрагмента и предполагают следующий формат:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-363">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="e4ff4-364">В следующем примере показан способ указания `RenderFragment` и `RenderFragment<T>` значения.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-364">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values.</span></span>

<span data-ttu-id="e4ff4-365">*RazorTemplates.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-365">*RazorTemplates.cshtml*:</span></span>

```cshtml
@{
    RenderFragment template = @<p>The time is @DateTime.Now.</p>;
    RenderFragment<Pet> petTemplate = (pet) => @<p>Your pet's name is @pet.Name.</p>;
}
```

<span data-ttu-id="e4ff4-366">Отображать фрагментов, определенных с помощью Razor, шаблоны можно передаются как аргументы шаблона компонентов или отображена непосредственно.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-366">Render fragments defined using Razor templates can be passed as arguments to templated components or rendered directly.</span></span> <span data-ttu-id="e4ff4-367">Например предыдущий шаблоны непосредственно выводятся в виде следующая разметка Razor:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-367">For example, the previous templates are directly rendered with the following Razor markup:</span></span>

```cshtml
@template

@petTemplate(new Pet { Name = "Rex" })
```

<span data-ttu-id="e4ff4-368">Отображенные выходные данные:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-368">Rendered output:</span></span>

```
The time is 10/04/2018 01:26:52.

Your pet's name is Rex.
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="e4ff4-369">Вручную RenderTreeBuilder логики</span><span class="sxs-lookup"><span data-stu-id="e4ff4-369">Manual RenderTreeBuilder logic</span></span>

`Microsoft.AspNetCore.Components.RenderTree` <span data-ttu-id="e4ff4-370">содержит методы для работы компонентов и элементов, в том числе по созданию компонентов вручную в C# кода.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-370">provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="e4ff4-371">Использование `RenderTreeBuilder` для создания компонентов — это расширенный сценарий.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-371">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="e4ff4-372">Компонент имеет неправильный формат (например, тег незакрытые разметки) может привести к неопределенному поведению.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-372">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="e4ff4-373">Рассмотрим следующий компонент Pet сведения (*PetDetails.razor* в Razor компонентов; *PetDetails.cshtml* в Blazor), который может быть вручную встроен в другой компонент:</span><span class="sxs-lookup"><span data-stu-id="e4ff4-373">Consider the following Pet Details component (*PetDetails.razor* in Razor Components; *PetDetails.cshtml* in Blazor), which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote<p>

@functions
{
    [Parameter]
    string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="e4ff4-374">В следующем примере цикла в `CreateComponent` метод создает три компонента Pet сведения.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-374">In the following example, the loop in the `CreateComponent` method generates three Pet Details components.</span></span> <span data-ttu-id="e4ff4-375">При вызове `RenderTreeBuilder` методы для создания компонентов (`OpenComponent` и `AddAttribute`), порядковые номера, номера строк исходного кода.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-375">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="e4ff4-376">Razor компоненты, которые алгоритм различие зависит от различных строкам кода, а не отдельными порядковые номера вызовите вызовов.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-376">The Razor Components difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="e4ff4-377">При создании компонента с `RenderTreeBuilder` методы, жестко задавать аргументы для последовательности чисел.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-377">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> **<span data-ttu-id="e4ff4-378">Использование вычисления или счетчиков для создания порядковый номер может привести к снижению производительности.</span><span class="sxs-lookup"><span data-stu-id="e4ff4-378">Using a calculation or counter to generate the sequence number can lead to poor performance.</span></span>**

<span data-ttu-id="e4ff4-379">Построение компонента (*BuiltContent.razor* в Razor компонентов; *BuiltContent.cshtml* в Blazor):</span><span class="sxs-lookup"><span data-stu-id="e4ff4-379">Built component (*BuiltContent.razor* in Razor Components; *BuiltContent.cshtml* in Blazor):</span></span>

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" onclick="@RenderComponent">
    Create three Pet Details components
</button>

@functions {
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
