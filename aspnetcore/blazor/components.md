---
title: Создание и использование компонентов ASP.NET Core Razor
author: guardrex
description: Узнайте, как создавать и использовать компоненты Razor, включая способы привязки к данным, обработки событий и управления жизненным циклом компонентов.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2019
no-loc:
- Blazor
uid: blazor/components
ms.openlocfilehash: 19636b0f10e71133eddece918b1bb9e2bc25a226
ms.sourcegitcommit: 169ea5116de729c803685725d96450a270bc55b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2019
ms.locfileid: "74733847"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="86dfd-103">Создание и использование компонентов ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="86dfd-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="86dfd-104">[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="86dfd-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="86dfd-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="86dfd-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

Blazor<span data-ttu-id="86dfd-106"> приложения создаются с помощью *компонентов*.</span><span class="sxs-lookup"><span data-stu-id="86dfd-106"> apps are built using *components*.</span></span> <span data-ttu-id="86dfd-107">Компонент — это автономный фрагмент пользовательского интерфейса, например страница, диалоговое окно или форма.</span><span class="sxs-lookup"><span data-stu-id="86dfd-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="86dfd-108">Компонент включает разметку HTML и логику обработки, необходимую для вставки данных или реагирования на события пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="86dfd-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="86dfd-109">Компоненты являются гибкими и легковесными.</span><span class="sxs-lookup"><span data-stu-id="86dfd-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="86dfd-110">Они могут быть вложенными, повторно использованы и совместно использоваться проектами.</span><span class="sxs-lookup"><span data-stu-id="86dfd-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="86dfd-111">Классы компонентов</span><span class="sxs-lookup"><span data-stu-id="86dfd-111">Component classes</span></span>

<span data-ttu-id="86dfd-112">Компоненты реализуются в файлах компонентов [Razor](xref:mvc/views/razor) ( *. Razor*) с помощью комбинации C# и HTML-разметки.</span><span class="sxs-lookup"><span data-stu-id="86dfd-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="86dfd-113">Компонент в Blazor формально называется *компонентом Razor*.</span><span class="sxs-lookup"><span data-stu-id="86dfd-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="86dfd-114">Имя компонента должно начинаться с символа верхнего регистра.</span><span class="sxs-lookup"><span data-stu-id="86dfd-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="86dfd-115">Например, *микулкомпонент. Razor* является допустимым, а *микулкомпонент. Razor* является недопустимым.</span><span class="sxs-lookup"><span data-stu-id="86dfd-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="86dfd-116">Пользовательский интерфейс для компонента определяется с помощью HTML.</span><span class="sxs-lookup"><span data-stu-id="86dfd-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="86dfd-117">Логика динамического отображения (например выражения, циклы и условные выражения) добавляется с помощью встроенного синтаксиса C# под названием [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="86dfd-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="86dfd-118">При компиляции приложения логика разметки и C# отрисовки HTML преобразуется в класс компонента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="86dfd-119">Имя созданного класса соответствует имени файла.</span><span class="sxs-lookup"><span data-stu-id="86dfd-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="86dfd-120">Элементы класса компонента определяются в блоке `@code`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="86dfd-121">В блоке `@code` состояние компонента (свойства, поля) указывается с помощью методов обработки событий или для определения другой логики компонента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="86dfd-122">Допускается более одного блока `@code`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-122">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="86dfd-123">В предыдущих предварительных версиях ASP.NET Core 3,0 блоки `@functions` использовались для той же цели, что и блоки `@code` в компонентах Razor.</span><span class="sxs-lookup"><span data-stu-id="86dfd-123">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="86dfd-124">блоки `@functions` продолжают работать в компонентах Razor, но рекомендуется использовать блок `@code` в ASP.NET Core 3,0 Preview 6 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="86dfd-124">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="86dfd-125">Члены компонента можно использовать как часть логики визуализации компонента с помощью C# выражений, начинающихся с `@`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-125">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="86dfd-126">Например, C# поле подготавливается к просмотру путем добавления `@` к имени поля.</span><span class="sxs-lookup"><span data-stu-id="86dfd-126">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="86dfd-127">В следующем примере вычисляется и выводится следующее:</span><span class="sxs-lookup"><span data-stu-id="86dfd-127">The following example evaluates and renders:</span></span>

* <span data-ttu-id="86dfd-128">`_headingFontStyle` значение свойства CSS для `font-style`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-128">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="86dfd-129">`_headingText` содержимое элемента `<h1>`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-129">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="86dfd-130">После первоначального отображения компонента Компонент повторно создает дерево рендеринга в ответ на события.</span><span class="sxs-lookup"><span data-stu-id="86dfd-130">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> Blazor<span data-ttu-id="86dfd-131"> затем сравнивает новое дерево отрисовки с предыдущим и применяет любые изменения в модель DOM (DOM) браузера.</span><span class="sxs-lookup"><span data-stu-id="86dfd-131"> then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="86dfd-132">Компоненты являются обычными C# классами и могут быть помещены в любое место внутри проекта.</span><span class="sxs-lookup"><span data-stu-id="86dfd-132">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="86dfd-133">Компоненты, создающие веб-страницы, обычно находятся в папке *страниц* .</span><span class="sxs-lookup"><span data-stu-id="86dfd-133">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="86dfd-134">Компоненты, не являющиеся страницами, часто помещаются в *общую* папку или пользовательскую папку, добавленную в проект.</span><span class="sxs-lookup"><span data-stu-id="86dfd-134">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="86dfd-135">Чтобы использовать пользовательскую папку, добавьте пространство имен пользовательской папки либо в родительский компонент, либо в файл *_Imports. Razor* приложения.</span><span class="sxs-lookup"><span data-stu-id="86dfd-135">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="86dfd-136">Например, следующее пространство имен делает компоненты в папке *Components* доступной при `WebApplication`корневого пространства имен приложения:</span><span class="sxs-lookup"><span data-stu-id="86dfd-136">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="86dfd-137">Интеграция компонентов в Razor Pages и приложения MVC</span><span class="sxs-lookup"><span data-stu-id="86dfd-137">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="86dfd-138">Используйте компоненты с существующими Razor Pages и приложениями MVC.</span><span class="sxs-lookup"><span data-stu-id="86dfd-138">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="86dfd-139">Нет необходимости переписывать существующие страницы или представления для использования компонентов Razor.</span><span class="sxs-lookup"><span data-stu-id="86dfd-139">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="86dfd-140">При отображении страницы или представления компоненты предварительно отображаются.</span><span class="sxs-lookup"><span data-stu-id="86dfd-140">When the page or view is rendered, components are prerendered at the same time.</span></span>

::: moniker range=">= aspnetcore-3.1"

<span data-ttu-id="86dfd-141">Чтобы отобразить компонент из страницы или представления, используйте вспомогательную функцию тега `Component`:</span><span class="sxs-lookup"><span data-stu-id="86dfd-141">To render a component from a page or view, use the `Component` Tag Helper:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="86dfd-142">Поддерживается передача параметров (например, `IncrementAmount` в предыдущем примере).</span><span class="sxs-lookup"><span data-stu-id="86dfd-142">Passing parameters (for example, `IncrementAmount` in the preceding example) is supported.</span></span>

<span data-ttu-id="86dfd-143">`RenderMode` настраивает, является ли компонент:</span><span class="sxs-lookup"><span data-stu-id="86dfd-143">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="86dfd-144">Предварительно отображается на странице.</span><span class="sxs-lookup"><span data-stu-id="86dfd-144">Is prerendered into the page.</span></span>
* <span data-ttu-id="86dfd-145">Отображается как статический HTML на странице или включает необходимые сведения для начальной загрузки Blazor приложения из агента пользователя.</span><span class="sxs-lookup"><span data-stu-id="86dfd-145">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="86dfd-146">Описание</span><span class="sxs-lookup"><span data-stu-id="86dfd-146">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="86dfd-147">Преобразует компонент в статический HTML и включает маркер для Blazor серверного приложения.</span><span class="sxs-lookup"><span data-stu-id="86dfd-147">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="86dfd-148">При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения.</span><span class="sxs-lookup"><span data-stu-id="86dfd-148">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="86dfd-149">Отображает маркер для серверного приложения Blazor.</span><span class="sxs-lookup"><span data-stu-id="86dfd-149">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="86dfd-150">Выходные данные компонента не включаются.</span><span class="sxs-lookup"><span data-stu-id="86dfd-150">Output from the component isn't included.</span></span> <span data-ttu-id="86dfd-151">При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения.</span><span class="sxs-lookup"><span data-stu-id="86dfd-151">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="86dfd-152">Преобразует компонент в статический HTML.</span><span class="sxs-lookup"><span data-stu-id="86dfd-152">Renders the component into static HTML.</span></span> |

<span data-ttu-id="86dfd-153">Хотя страницы и представления могут использовать компоненты, наоборот это не так.</span><span class="sxs-lookup"><span data-stu-id="86dfd-153">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="86dfd-154">Компоненты не могут использовать сценарии представления и страницы, такие как частичные представления и разделы.</span><span class="sxs-lookup"><span data-stu-id="86dfd-154">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="86dfd-155">Чтобы использовать логику из частичного представления в компоненте, разнесите логику частичного представления в компонент.</span><span class="sxs-lookup"><span data-stu-id="86dfd-155">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="86dfd-156">Отрисовка компонентов сервера из статической HTML-страницы не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="86dfd-156">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="86dfd-157">Дополнительные сведения о подготовке компонентов к просмотру, состоянии компонентов и вспомогательной функции тега `Component` см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="86dfd-157">For more information on how components are rendered, component state, and the `Component` Tag Helper, see <xref:blazor/hosting-models>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.1"

<span data-ttu-id="86dfd-158">Чтобы отобразить компонент из страницы или представления, используйте вспомогательный метод HTML `RenderComponentAsync<TComponent>`:</span><span class="sxs-lookup"><span data-stu-id="86dfd-158">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
@(await Html.RenderComponentAsync<MyComponent>(RenderMode.ServerPrerendered))
```

<span data-ttu-id="86dfd-159">`RenderMode` настраивает, является ли компонент:</span><span class="sxs-lookup"><span data-stu-id="86dfd-159">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="86dfd-160">Предварительно отображается на странице.</span><span class="sxs-lookup"><span data-stu-id="86dfd-160">Is prerendered into the page.</span></span>
* <span data-ttu-id="86dfd-161">Отображается как статический HTML на странице или включает необходимые сведения для начальной загрузки Blazor приложения из агента пользователя.</span><span class="sxs-lookup"><span data-stu-id="86dfd-161">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="86dfd-162">Описание</span><span class="sxs-lookup"><span data-stu-id="86dfd-162">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="86dfd-163">Преобразует компонент в статический HTML и включает маркер для Blazor серверного приложения.</span><span class="sxs-lookup"><span data-stu-id="86dfd-163">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="86dfd-164">При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения.</span><span class="sxs-lookup"><span data-stu-id="86dfd-164">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="86dfd-165">Параметры не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="86dfd-165">Parameters aren't supported.</span></span> |
| `Server`            | <span data-ttu-id="86dfd-166">Отображает маркер для серверного приложения Blazor.</span><span class="sxs-lookup"><span data-stu-id="86dfd-166">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="86dfd-167">Выходные данные компонента не включаются.</span><span class="sxs-lookup"><span data-stu-id="86dfd-167">Output from the component isn't included.</span></span> <span data-ttu-id="86dfd-168">При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения.</span><span class="sxs-lookup"><span data-stu-id="86dfd-168">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="86dfd-169">Параметры не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="86dfd-169">Parameters aren't supported.</span></span> |
| `Static`            | <span data-ttu-id="86dfd-170">Преобразует компонент в статический HTML.</span><span class="sxs-lookup"><span data-stu-id="86dfd-170">Renders the component into static HTML.</span></span> <span data-ttu-id="86dfd-171">Поддерживаются параметры.</span><span class="sxs-lookup"><span data-stu-id="86dfd-171">Parameters are supported.</span></span> |

<span data-ttu-id="86dfd-172">Хотя страницы и представления могут использовать компоненты, наоборот это не так.</span><span class="sxs-lookup"><span data-stu-id="86dfd-172">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="86dfd-173">Компоненты не могут использовать сценарии представления и страницы, такие как частичные представления и разделы.</span><span class="sxs-lookup"><span data-stu-id="86dfd-173">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="86dfd-174">Чтобы использовать логику из частичного представления в компоненте, разнесите логику частичного представления в компонент.</span><span class="sxs-lookup"><span data-stu-id="86dfd-174">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="86dfd-175">Отрисовка компонентов сервера из статической HTML-страницы не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="86dfd-175">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="86dfd-176">Дополнительные сведения о том, как визуализируются компоненты, состояние компонента и вспомогательный модуль HTML `RenderComponentAsync`, см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="86dfd-176">For more information on how components are rendered, component state, and the `RenderComponentAsync` HTML Helper, see <xref:blazor/hosting-models>.</span></span>

::: moniker-end

## <a name="use-components"></a><span data-ttu-id="86dfd-177">Использование компонентов</span><span class="sxs-lookup"><span data-stu-id="86dfd-177">Use components</span></span>

<span data-ttu-id="86dfd-178">Компоненты могут включать другие компоненты, объявляя их с помощью синтаксиса HTML-элементов.</span><span class="sxs-lookup"><span data-stu-id="86dfd-178">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="86dfd-179">Разметка для использования компонента выглядит как тег HTML с именем, соответствующем типу компонента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-179">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="86dfd-180">Привязка атрибута чувствительна к регистру.</span><span class="sxs-lookup"><span data-stu-id="86dfd-180">Attribute binding is case sensitive.</span></span> <span data-ttu-id="86dfd-181">Например, `@bind` является допустимым, а `@Bind` является недопустимым.</span><span class="sxs-lookup"><span data-stu-id="86dfd-181">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="86dfd-182">Следующая разметка в *index. Razor* визуализирует `HeadingComponent` экземпляр:</span><span class="sxs-lookup"><span data-stu-id="86dfd-182">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="86dfd-183">*Components/хеадингкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="86dfd-183">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

<span data-ttu-id="86dfd-184">Если компонент содержит HTML-элемент с первой буквой в верхнем регистре, который не соответствует имени компонента, выдается предупреждение, указывающее, что элемент имеет непредвиденное имя.</span><span class="sxs-lookup"><span data-stu-id="86dfd-184">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="86dfd-185">Добавление оператора `@using` для пространства имен компонента делает компонент доступным, что приводит к удалению предупреждения.</span><span class="sxs-lookup"><span data-stu-id="86dfd-185">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="86dfd-186">Параметры компонентов</span><span class="sxs-lookup"><span data-stu-id="86dfd-186">Component parameters</span></span>

<span data-ttu-id="86dfd-187">Компоненты могут иметь *Параметры компонента*, которые определяются с помощью общедоступных свойств класса Component с атрибутом `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-187">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="86dfd-188">Используйте атрибуты, чтобы указать аргументы для компонента в разметке.</span><span class="sxs-lookup"><span data-stu-id="86dfd-188">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="86dfd-189">*Components/чилдкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="86dfd-189">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="86dfd-190">В следующем примере `ParentComponent` задает значение свойства `Title` `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-190">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="86dfd-191">*Pages/паренткомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="86dfd-191">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="86dfd-192">Дочернее содержимое</span><span class="sxs-lookup"><span data-stu-id="86dfd-192">Child content</span></span>

<span data-ttu-id="86dfd-193">Компоненты могут устанавливать содержимое другого компонента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-193">Components can set the content of another component.</span></span> <span data-ttu-id="86dfd-194">Назначение компонента предоставляет содержимое между тегами, задающих принимающий компонент.</span><span class="sxs-lookup"><span data-stu-id="86dfd-194">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="86dfd-195">В следующем примере `ChildComponent` имеет свойство `ChildContent`, представляющее `RenderFragment`, которое представляет сегмент пользовательского интерфейса для отрисовки.</span><span class="sxs-lookup"><span data-stu-id="86dfd-195">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="86dfd-196">Значение `ChildContent` размещается в разметке компонента, где должно быть визуализировано содержимое.</span><span class="sxs-lookup"><span data-stu-id="86dfd-196">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="86dfd-197">Значение `ChildContent` получено от родительского компонента и отображается внутри `panel-body`панели начальной загрузки.</span><span class="sxs-lookup"><span data-stu-id="86dfd-197">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="86dfd-198">*Components/чилдкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="86dfd-198">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="86dfd-199">Свойству, получающему `RenderFragment`ное содержимое, необходимо присвоить имя `ChildContent` по соглашению.</span><span class="sxs-lookup"><span data-stu-id="86dfd-199">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="86dfd-200">Следующий `ParentComponent` может предоставить содержимое для подготовки к просмотру `ChildComponent`, поместив содержимое в теги `<ChildComponent>`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-200">The following `ParentComponent` can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="86dfd-201">*Pages/паренткомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="86dfd-201">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="86dfd-202">Атрибуты Сплаттинг и произвольные параметры</span><span class="sxs-lookup"><span data-stu-id="86dfd-202">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="86dfd-203">Компоненты могут записывать и отображать дополнительные атрибуты в дополнение к объявленным параметрам компонента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-203">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="86dfd-204">Дополнительные атрибуты можно записать в словарь, а затем *сплаттед* на элемент при подготовке компонента к просмотру с помощью директивы [@attributes](xref:mvc/views/razor#attributes) Razor.</span><span class="sxs-lookup"><span data-stu-id="86dfd-204">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [@attributes](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="86dfd-205">Этот сценарий полезен при определении компонента, который создает элемент разметки, поддерживающий разнообразные настройки.</span><span class="sxs-lookup"><span data-stu-id="86dfd-205">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="86dfd-206">Например, может быть утомительно определять атрибуты отдельно для `<input>`, который поддерживает много параметров.</span><span class="sxs-lookup"><span data-stu-id="86dfd-206">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="86dfd-207">В следующем примере первый элемент `<input>` (`id="useIndividualParams"`) использует параметры отдельных компонентов, а второй элемент `<input>` (`id="useAttributesDict"`) использует атрибут Сплаттинг:</span><span class="sxs-lookup"><span data-stu-id="86dfd-207">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

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

<span data-ttu-id="86dfd-208">Тип параметра должен реализовывать `IEnumerable<KeyValuePair<string, object>>` со строковыми ключами.</span><span class="sxs-lookup"><span data-stu-id="86dfd-208">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="86dfd-209">Использование `IReadOnlyDictionary<string, object>` также является параметром в этом сценарии.</span><span class="sxs-lookup"><span data-stu-id="86dfd-209">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="86dfd-210">Отображаемые `<input>` элементы, использующие оба подхода, идентичны:</span><span class="sxs-lookup"><span data-stu-id="86dfd-210">The rendered `<input>` elements using both approaches is identical:</span></span>

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

<span data-ttu-id="86dfd-211">Чтобы принять произвольные атрибуты, определите параметр компонента с помощью атрибута `[Parameter]` со свойством `CaptureUnmatchedValues`, для которого задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-211">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```cshtml
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="86dfd-212">Свойство `CaptureUnmatchedValues` в `[Parameter]` позволяет параметру сопоставлять все атрибуты, не соответствующие ни одному другому параметру.</span><span class="sxs-lookup"><span data-stu-id="86dfd-212">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="86dfd-213">Компонент может определять только один параметр с `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-213">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="86dfd-214">Тип свойства, используемый с `CaptureUnmatchedValues`, должен быть назначен из `Dictionary<string, object>` с строковыми ключами.</span><span class="sxs-lookup"><span data-stu-id="86dfd-214">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="86dfd-215">в этом сценарии также используются параметры `IEnumerable<KeyValuePair<string, object>>` и `IReadOnlyDictionary<string, object>`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-215">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="86dfd-216">Расположение `@attributes` относительно положения атрибутов элемента важно.</span><span class="sxs-lookup"><span data-stu-id="86dfd-216">The position of `@attributes` relative to the position of element attributes is important.</span></span> <span data-ttu-id="86dfd-217">Когда `@attributes` сплаттед для элемента, атрибуты обрабатываются справа налево (последний — первый).</span><span class="sxs-lookup"><span data-stu-id="86dfd-217">When `@attributes` are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="86dfd-218">Рассмотрим следующий пример компонента, использующего компонент `Child`:</span><span class="sxs-lookup"><span data-stu-id="86dfd-218">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="86dfd-219">*Паренткомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="86dfd-219">*ParentComponent.razor*:</span></span>

```cshtml
<ChildComponent extra="10" />
```

<span data-ttu-id="86dfd-220">*Чилдкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="86dfd-220">*ChildComponent.razor*:</span></span>

```cshtml
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="86dfd-221">Атрибут `extra` `Child` компонента установлен справа от `@attributes`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-221">The `Child` component's `extra` attribute is set to the right of `@attributes`.</span></span> <span data-ttu-id="86dfd-222">`<div>`, отображаемый компонентом `Parent`, содержит `extra="5"` при передаче через дополнительный атрибут, так как атрибуты обрабатываются справа налево (последний — первый):</span><span class="sxs-lookup"><span data-stu-id="86dfd-222">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="86dfd-223">В следующем примере порядок `extra` и `@attributes` отменяется в `<div>`е `Child` компонента:</span><span class="sxs-lookup"><span data-stu-id="86dfd-223">In the following example, the order of `extra` and `@attributes` is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="86dfd-224">*Паренткомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="86dfd-224">*ParentComponent.razor*:</span></span>

```cshtml
<ChildComponent extra="10" />
```

<span data-ttu-id="86dfd-225">*Чилдкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="86dfd-225">*ChildComponent.razor*:</span></span>

```cshtml
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="86dfd-226">Отображаемые `<div>` в компоненте `Parent` содержат `extra="10"` при передаче через дополнительный атрибут:</span><span class="sxs-lookup"><span data-stu-id="86dfd-226">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="data-binding"></a><span data-ttu-id="86dfd-227">привязка данных,</span><span class="sxs-lookup"><span data-stu-id="86dfd-227">Data binding</span></span>

<span data-ttu-id="86dfd-228">Привязка данных как к компонентам, так и к элементам DOM осуществляется с помощью атрибута [@bind](xref:mvc/views/razor#bind) .</span><span class="sxs-lookup"><span data-stu-id="86dfd-228">Data binding to both components and DOM elements is accomplished with the [@bind](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="86dfd-229">В следующем примере свойство `CurrentValue` привязывается к значению текстового поля:</span><span class="sxs-lookup"><span data-stu-id="86dfd-229">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```cshtml
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="86dfd-230">Когда текстовое поле теряет фокус, значение свойства обновляется.</span><span class="sxs-lookup"><span data-stu-id="86dfd-230">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="86dfd-231">Текстовое поле обновляется в пользовательском интерфейсе только при подготовке компонента к просмотру, а не в ответ на изменение значения свойства.</span><span class="sxs-lookup"><span data-stu-id="86dfd-231">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="86dfd-232">Поскольку компоненты отображаются после выполнения кода обработчика событий, обновления свойств *обычно* отражаются в пользовательском интерфейсе сразу после запуска обработчика событий.</span><span class="sxs-lookup"><span data-stu-id="86dfd-232">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="86dfd-233">Использование `@bind` со свойством `CurrentValue` (`<input @bind="CurrentValue" />`) в основном эквивалентно следующему:</span><span class="sxs-lookup"><span data-stu-id="86dfd-233">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="86dfd-234">При подготовке к просмотру компонента `value` входного элемента берется из свойства `CurrentValue`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-234">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="86dfd-235">Когда пользователь вводит данные в текстовое поле и изменяет фокус элемента, запускается событие `onchange`, а свойству `CurrentValue` присваивается измененное значение.</span><span class="sxs-lookup"><span data-stu-id="86dfd-235">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="86dfd-236">На практике создание кода более сложно, так как `@bind` обрабатывает случаи, когда выполняются преобразования типов.</span><span class="sxs-lookup"><span data-stu-id="86dfd-236">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="86dfd-237">В принципе `@bind` связывает текущее значение выражения с атрибутом `value` и обрабатывает изменения с помощью зарегистрированного обработчика.</span><span class="sxs-lookup"><span data-stu-id="86dfd-237">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="86dfd-238">Помимо обработки `onchange`ных событий с помощью синтаксиса `@bind`, свойство или поле можно привязать с помощью других событий, указав атрибут [@bind-value](xref:mvc/views/razor#bind) с параметром `event` ([@bind-value:event](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="86dfd-238">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [@bind-value](xref:mvc/views/razor#bind) attribute with an `event` parameter ([@bind-value:event](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="86dfd-239">В следующем примере выполняется привязка свойства `CurrentValue` для события `oninput`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-239">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="86dfd-240">В отличие от `onchange`, которая срабатывает при потере фокуса элементом, `oninput` срабатывает при изменении значения текстового поля.</span><span class="sxs-lookup"><span data-stu-id="86dfd-240">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="86dfd-241">**Неанализируемые значения**</span><span class="sxs-lookup"><span data-stu-id="86dfd-241">**Unparsable values**</span></span>

<span data-ttu-id="86dfd-242">Когда пользователь предоставляет неинтерпретируемое значение для элемента с привязкой к данным, неанализируемое значение автоматически возвращается к предыдущему значению при срабатывании события привязки.</span><span class="sxs-lookup"><span data-stu-id="86dfd-242">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="86dfd-243">Рассмотрим следующий сценарий:</span><span class="sxs-lookup"><span data-stu-id="86dfd-243">Consider the following scenario:</span></span>

* <span data-ttu-id="86dfd-244">Элемент `<input>` привязан к типу `int` с начальным значением `123`:</span><span class="sxs-lookup"><span data-stu-id="86dfd-244">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```cshtml
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="86dfd-245">Пользователь обновляет значение элемента для `123.45` на странице и изменяет фокус элемента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-245">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="86dfd-246">В приведенном выше сценарии значение элемента возвращается к `123`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-246">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="86dfd-247">Если значение `123.45` отклоняется в пользу исходного значения `123`, пользователь понимает, что их значение не принято.</span><span class="sxs-lookup"><span data-stu-id="86dfd-247">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="86dfd-248">По умолчанию привязка применяется к событию `onchange` элемента (`@bind="{PROPERTY OR FIELD}"`).</span><span class="sxs-lookup"><span data-stu-id="86dfd-248">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="86dfd-249">Для задания другого события используйте `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-249">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="86dfd-250">Для события `oninput` (`@bind-value:event="oninput"`) переверсия происходит после любого нажатия клавиши, которая вводит неинтерпретируемое значение.</span><span class="sxs-lookup"><span data-stu-id="86dfd-250">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="86dfd-251">При нацеливании на событие `oninput` с типом, привязанным к `int`, пользователю запрещено вводить `.` символ.</span><span class="sxs-lookup"><span data-stu-id="86dfd-251">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="86dfd-252">`.`ный символ немедленно удаляется, поэтому пользователь получает немедленный отзыв о том, что разрешены только целые числа.</span><span class="sxs-lookup"><span data-stu-id="86dfd-252">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="86dfd-253">Существуют сценарии, в которых возвращение значения в событие `oninput` не является идеальным, например, когда пользователю разрешено очищать неинтерпретируемое `<input>` значение.</span><span class="sxs-lookup"><span data-stu-id="86dfd-253">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="86dfd-254">К альтернативам относятся:</span><span class="sxs-lookup"><span data-stu-id="86dfd-254">Alternatives include:</span></span>

* <span data-ttu-id="86dfd-255">Не используйте событие `oninput`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-255">Don't use the `oninput` event.</span></span> <span data-ttu-id="86dfd-256">Используйте событие `onchange` по умолчанию (`@bind="{PROPERTY OR FIELD}"`), где недопустимое значение не возвращается, пока элемент не теряет фокус.</span><span class="sxs-lookup"><span data-stu-id="86dfd-256">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="86dfd-257">Выполните привязку к типу, допускающему значение null, например `int?` или `string`, и предоставьте настраиваемую логику для обработки недопустимых записей.</span><span class="sxs-lookup"><span data-stu-id="86dfd-257">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="86dfd-258">Используйте [компонент проверки формы](xref:blazor/forms-validation), например `InputNumber` или `InputDate`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-258">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="86dfd-259">Компоненты проверки форм имеют встроенную поддержку для управления недопустимыми входными данными.</span><span class="sxs-lookup"><span data-stu-id="86dfd-259">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="86dfd-260">Компоненты проверки формы:</span><span class="sxs-lookup"><span data-stu-id="86dfd-260">Form validation components:</span></span>
  * <span data-ttu-id="86dfd-261">Разрешите пользователю указать недопустимые входные данные и получить ошибки проверки для связанного `EditContext`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-261">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="86dfd-262">Отображает ошибки проверки в пользовательском интерфейсе, не мешая пользователю вводить дополнительные данные из формы.</span><span class="sxs-lookup"><span data-stu-id="86dfd-262">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

<span data-ttu-id="86dfd-263">**Глобализация**</span><span class="sxs-lookup"><span data-stu-id="86dfd-263">**Globalization**</span></span>

<span data-ttu-id="86dfd-264">`@bind` значения форматируются для вывода и анализируются с использованием правил текущего языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="86dfd-264">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="86dfd-265">Доступ к текущему языку и региональным параметрам можно получить из свойства <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="86dfd-265">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="86dfd-266">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) используется для следующих типов полей (`<input type="{TYPE}" />`):</span><span class="sxs-lookup"><span data-stu-id="86dfd-266">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="86dfd-267">Предыдущие типы полей:</span><span class="sxs-lookup"><span data-stu-id="86dfd-267">The preceding field types:</span></span>

* <span data-ttu-id="86dfd-268">Отображаются с использованием соответствующих правил форматирования на основе браузера.</span><span class="sxs-lookup"><span data-stu-id="86dfd-268">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="86dfd-269">Не может содержать текст в свободной форме.</span><span class="sxs-lookup"><span data-stu-id="86dfd-269">Can't contain free-form text.</span></span>
* <span data-ttu-id="86dfd-270">Предоставление характеристик взаимодействия с пользователем в зависимости от реализации браузера.</span><span class="sxs-lookup"><span data-stu-id="86dfd-270">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="86dfd-271">Следующие типы полей имеют особые требования к форматированию, которые в настоящее время не поддерживаются Blazor, так как они не поддерживаются всеми основными браузерами.</span><span class="sxs-lookup"><span data-stu-id="86dfd-271">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="86dfd-272">`@bind` поддерживает параметр `@bind:culture`, чтобы предоставить <xref:System.Globalization.CultureInfo?displayProperty=fullName> для синтаксического анализа и форматирования значения.</span><span class="sxs-lookup"><span data-stu-id="86dfd-272">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="86dfd-273">Указание языка и региональных параметров не рекомендуется при использовании типов полей `date` и `number`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-273">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="86dfd-274">`date` и `number` имеют встроенную поддержку Blazor, которая предоставляет требуемый язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="86dfd-274">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="86dfd-275">Сведения о том, как задать язык и региональные параметры пользователя, см. в разделе [Локализация](#localization) .</span><span class="sxs-lookup"><span data-stu-id="86dfd-275">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="86dfd-276">**Строки формата**</span><span class="sxs-lookup"><span data-stu-id="86dfd-276">**Format strings**</span></span>

<span data-ttu-id="86dfd-277">Привязка данных работает со строками формата <xref:System.DateTime> с помощью [@bind:format](xref:mvc/views/razor#bind).</span><span class="sxs-lookup"><span data-stu-id="86dfd-277">Data binding works with <xref:System.DateTime> format strings using [@bind:format](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="86dfd-278">Другие выражения форматирования, такие как денежные или числовые форматы, в настоящее время недоступны.</span><span class="sxs-lookup"><span data-stu-id="86dfd-278">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="86dfd-279">В приведенном выше коде тип поля `<input>` элемента (`type`) по умолчанию имеет значение `text`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-279">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="86dfd-280">`@bind:format` поддерживается для привязки следующих типов .NET:</span><span class="sxs-lookup"><span data-stu-id="86dfd-280">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="86dfd-281"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="86dfd-281"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="86dfd-282"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="86dfd-282"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="86dfd-283">Атрибут `@bind:format` задает формат даты, применяемый к `value` элемента `<input>`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-283">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="86dfd-284">Формат также используется для анализа значения при возникновении `onchange` события.</span><span class="sxs-lookup"><span data-stu-id="86dfd-284">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="86dfd-285">Указание формата для типа поля `date` не рекомендуется, так как Blazor имеет встроенную поддержку для форматирования дат.</span><span class="sxs-lookup"><span data-stu-id="86dfd-285">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span> <span data-ttu-id="86dfd-286">Несмотря на рекомендацию, используйте формат даты `yyyy-MM-dd` для правильной работы привязки только в том случае, если указан формат с типом поля `date`:</span><span class="sxs-lookup"><span data-stu-id="86dfd-286">In spite of the recommendation, only use the `yyyy-MM-dd` date format for binding to work correctly if a format is supplied with the `date` field type:</span></span>

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

<span data-ttu-id="86dfd-287">**Параметры компонента**</span><span class="sxs-lookup"><span data-stu-id="86dfd-287">**Component parameters**</span></span>

<span data-ttu-id="86dfd-288">Привязка распознает параметры компонента, где `@bind-{property}` может привязать значение свойства к разным компонентам.</span><span class="sxs-lookup"><span data-stu-id="86dfd-288">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="86dfd-289">Следующий дочерний компонент (`ChildComponent`) имеет параметр компонента `Year` и обратный вызов `YearChanged`:</span><span class="sxs-lookup"><span data-stu-id="86dfd-289">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

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

<span data-ttu-id="86dfd-290">`EventCallback<T>` описывается в разделе [вложенный EventCallback](#eventcallback) .</span><span class="sxs-lookup"><span data-stu-id="86dfd-290">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="86dfd-291">Следующий родительский компонент использует `ChildComponent` и привязывает параметр `ParentYear` из родительского к параметру `Year` дочернего компонента:</span><span class="sxs-lookup"><span data-stu-id="86dfd-291">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

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

<span data-ttu-id="86dfd-292">При загрузке `ParentComponent` создается следующая разметка:</span><span class="sxs-lookup"><span data-stu-id="86dfd-292">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="86dfd-293">Если значение свойства `ParentYear` изменяется путем нажатия кнопки в `ParentComponent`, свойство `Year` `ChildComponent` обновляется.</span><span class="sxs-lookup"><span data-stu-id="86dfd-293">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="86dfd-294">Новое значение `Year` отображается в пользовательском интерфейсе при перевизуализации `ParentComponent`:</span><span class="sxs-lookup"><span data-stu-id="86dfd-294">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="86dfd-295">Параметр `Year` является связываемым, так как имеет сопутствующее `YearChanged` событие, соответствующее типу параметра `Year`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-295">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="86dfd-296">По соглашению `<ChildComponent @bind-Year="ParentYear" />`, по сути, эквивалентен написанию:</span><span class="sxs-lookup"><span data-stu-id="86dfd-296">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="86dfd-297">Как правило, свойство может быть привязано к соответствующему обработчику событий с помощью атрибута `@bind-property:event`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-297">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="86dfd-298">Например, свойство `MyProp` можно привязать к `MyEventHandler`у с помощью следующих двух атрибутов:</span><span class="sxs-lookup"><span data-stu-id="86dfd-298">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="86dfd-299">Обработка событий</span><span class="sxs-lookup"><span data-stu-id="86dfd-299">Event handling</span></span>

<span data-ttu-id="86dfd-300">Компоненты Razor предоставляют функции обработки событий.</span><span class="sxs-lookup"><span data-stu-id="86dfd-300">Razor components provide event handling features.</span></span> <span data-ttu-id="86dfd-301">Для атрибута HTML-элемента с именем `on{EVENT}` (например, `onclick` и `onsubmit`) со значением, вводимым с помощью делегата, компоненты Razor рассматривают значение атрибута как обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="86dfd-301">For an HTML element attribute named `on{EVENT}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="86dfd-302">Имя атрибута всегда отформатировано [@on{Event}](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="86dfd-302">The attribute's name is always formatted [@on{EVENT}](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="86dfd-303">Следующий код вызывает метод `UpdateHeading`, когда кнопка выбрана в пользовательском интерфейсе:</span><span class="sxs-lookup"><span data-stu-id="86dfd-303">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

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

<span data-ttu-id="86dfd-304">Следующий код вызывает метод `CheckChanged` при изменении флажка в пользовательском интерфейсе:</span><span class="sxs-lookup"><span data-stu-id="86dfd-304">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="86dfd-305">Обработчики событий также могут быть асинхронными и возвращать <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="86dfd-305">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="86dfd-306">Нет необходимости вручную вызывать [статехасчанжед](xref:blazor/lifecycle#state-changes).</span><span class="sxs-lookup"><span data-stu-id="86dfd-306">There's no need to manually call [StateHasChanged](xref:blazor/lifecycle#state-changes).</span></span> <span data-ttu-id="86dfd-307">Исключения регистрируются при их возникновении.</span><span class="sxs-lookup"><span data-stu-id="86dfd-307">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="86dfd-308">В следующем примере `UpdateHeading` вызывается асинхронно при выборе кнопки:</span><span class="sxs-lookup"><span data-stu-id="86dfd-308">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

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

### <a name="event-argument-types"></a><span data-ttu-id="86dfd-309">Типы аргументов событий</span><span class="sxs-lookup"><span data-stu-id="86dfd-309">Event argument types</span></span>

<span data-ttu-id="86dfd-310">Для некоторых событий разрешены типы аргументов событий.</span><span class="sxs-lookup"><span data-stu-id="86dfd-310">For some events, event argument types are permitted.</span></span> <span data-ttu-id="86dfd-311">Если доступ к одному из этих типов событий не нужен, в вызове метода это не требуется.</span><span class="sxs-lookup"><span data-stu-id="86dfd-311">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="86dfd-312">Поддерживаемые `EventArgs` приведены в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="86dfd-312">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="86dfd-313">Event</span><span class="sxs-lookup"><span data-stu-id="86dfd-313">Event</span></span>            | <span data-ttu-id="86dfd-314">Класс</span><span class="sxs-lookup"><span data-stu-id="86dfd-314">Class</span></span>                | <span data-ttu-id="86dfd-315">События DOM и заметки</span><span class="sxs-lookup"><span data-stu-id="86dfd-315">DOM events and notes</span></span> |
| ---------------- | -------------------- | -------------------- |
| <span data-ttu-id="86dfd-316">буфер обмена</span><span class="sxs-lookup"><span data-stu-id="86dfd-316">Clipboard</span></span>        | `ClipboardEventArgs` | <span data-ttu-id="86dfd-317">`oncut`значение `oncopy`значение `onpaste`</span><span class="sxs-lookup"><span data-stu-id="86dfd-317">`oncut`, `oncopy`, `onpaste`</span></span> |
| <span data-ttu-id="86dfd-318">Переместить</span><span class="sxs-lookup"><span data-stu-id="86dfd-318">Drag</span></span>             | `DragEventArgs`      | <span data-ttu-id="86dfd-319">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span><span class="sxs-lookup"><span data-stu-id="86dfd-319">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span></span><br><br><span data-ttu-id="86dfd-320">`DataTransfer` и `DataTransferItem` содержать перетаскиваемые данные элемента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-320">`DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="86dfd-321">Ошибка .</span><span class="sxs-lookup"><span data-stu-id="86dfd-321">Error</span></span>            | `ErrorEventArgs`     | `onerror` |
| <span data-ttu-id="86dfd-322">Event</span><span class="sxs-lookup"><span data-stu-id="86dfd-322">Event</span></span>            | `EventArgs`          | <span data-ttu-id="86dfd-323">*Общие*</span><span class="sxs-lookup"><span data-stu-id="86dfd-323">*General*</span></span><br><span data-ttu-id="86dfd-324">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span><span class="sxs-lookup"><span data-stu-id="86dfd-324">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span></span><br><br><span data-ttu-id="86dfd-325">*Буфер обмена*</span><span class="sxs-lookup"><span data-stu-id="86dfd-325">*Clipboard*</span></span><br><span data-ttu-id="86dfd-326">`onbeforecut`значение `onbeforecopy`значение `onbeforepaste`</span><span class="sxs-lookup"><span data-stu-id="86dfd-326">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span></span><br><br><span data-ttu-id="86dfd-327">*Ввод*</span><span class="sxs-lookup"><span data-stu-id="86dfd-327">*Input*</span></span><br><span data-ttu-id="86dfd-328">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span><span class="sxs-lookup"><span data-stu-id="86dfd-328">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span></span><br><br><span data-ttu-id="86dfd-329">*Носител*</span><span class="sxs-lookup"><span data-stu-id="86dfd-329">*Media*</span></span><br><span data-ttu-id="86dfd-330">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span><span class="sxs-lookup"><span data-stu-id="86dfd-330">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span></span> |
| <span data-ttu-id="86dfd-331">Фокус</span><span class="sxs-lookup"><span data-stu-id="86dfd-331">Focus</span></span>            | `FocusEventArgs`     | <span data-ttu-id="86dfd-332">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span><span class="sxs-lookup"><span data-stu-id="86dfd-332">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span></span><br><br><span data-ttu-id="86dfd-333">Не включает поддержку для `relatedTarget`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-333">Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="86dfd-334">Input</span><span class="sxs-lookup"><span data-stu-id="86dfd-334">Input</span></span>            | `ChangeEventArgs`    | <span data-ttu-id="86dfd-335">`onchange`, `oninput`</span><span class="sxs-lookup"><span data-stu-id="86dfd-335">`onchange`, `oninput`</span></span> |
| <span data-ttu-id="86dfd-336">Клавиатура</span><span class="sxs-lookup"><span data-stu-id="86dfd-336">Keyboard</span></span>         | `KeyboardEventArgs`  | <span data-ttu-id="86dfd-337">`onkeydown`значение `onkeypress`значение `onkeyup`</span><span class="sxs-lookup"><span data-stu-id="86dfd-337">`onkeydown`, `onkeypress`, `onkeyup`</span></span> |
| <span data-ttu-id="86dfd-338">Мышь</span><span class="sxs-lookup"><span data-stu-id="86dfd-338">Mouse</span></span>            | `MouseEventArgs`     | <span data-ttu-id="86dfd-339">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span><span class="sxs-lookup"><span data-stu-id="86dfd-339">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span></span> |
| <span data-ttu-id="86dfd-340">Указатель мыши</span><span class="sxs-lookup"><span data-stu-id="86dfd-340">Mouse pointer</span></span>    | `PointerEventArgs`   | <span data-ttu-id="86dfd-341">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span><span class="sxs-lookup"><span data-stu-id="86dfd-341">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span></span> |
| <span data-ttu-id="86dfd-342">Колесо мыши</span><span class="sxs-lookup"><span data-stu-id="86dfd-342">Mouse wheel</span></span>      | `WheelEventArgs`     | <span data-ttu-id="86dfd-343">`onwheel`, `onmousewheel`</span><span class="sxs-lookup"><span data-stu-id="86dfd-343">`onwheel`, `onmousewheel`</span></span> |
| <span data-ttu-id="86dfd-344">Ход выполнения</span><span class="sxs-lookup"><span data-stu-id="86dfd-344">Progress</span></span>         | `ProgressEventArgs`  | <span data-ttu-id="86dfd-345">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span><span class="sxs-lookup"><span data-stu-id="86dfd-345">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span></span> |
| <span data-ttu-id="86dfd-346">Сенсорные технологии</span><span class="sxs-lookup"><span data-stu-id="86dfd-346">Touch</span></span>            | `TouchEventArgs`     | <span data-ttu-id="86dfd-347">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span><span class="sxs-lookup"><span data-stu-id="86dfd-347">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span></span><br><br><span data-ttu-id="86dfd-348">`TouchPoint` представляет одну точку контакта на устройстве с сенсорным вводом.</span><span class="sxs-lookup"><span data-stu-id="86dfd-348">`TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="86dfd-349">Сведения о свойствах и поведении событий, описанных в приведенной выше таблице, см. в разделе [классы EventArgs в источнике ссылки (ветвь ASPNET/AspNetCore Release/3.0)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="86dfd-349">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source (aspnet/AspNetCore release/3.0 branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="86dfd-350">Лямбда-выражения</span><span class="sxs-lookup"><span data-stu-id="86dfd-350">Lambda expressions</span></span>

<span data-ttu-id="86dfd-351">Лямбда-выражения также могут использоваться:</span><span class="sxs-lookup"><span data-stu-id="86dfd-351">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="86dfd-352">Часто бывает удобно закрывать дополнительные значения, например при переборе набора элементов.</span><span class="sxs-lookup"><span data-stu-id="86dfd-352">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="86dfd-353">В следующем примере создаются три кнопки, каждый из которых вызывает `UpdateHeading` передачи аргумента события (`MouseEventArgs`) и номера кнопки (`buttonNumber`) при выборе в пользовательском интерфейсе:</span><span class="sxs-lookup"><span data-stu-id="86dfd-353">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

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
> <span data-ttu-id="86dfd-354">**Не** используйте переменную цикла (`i`) в цикле `for` непосредственно в лямбда-выражении.</span><span class="sxs-lookup"><span data-stu-id="86dfd-354">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="86dfd-355">В противном случае одна и та же переменная используется во всех лямбда-выражениях, в результате чего значение `i`будет одинаковым во всех лямбдаах.</span><span class="sxs-lookup"><span data-stu-id="86dfd-355">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="86dfd-356">Всегда запишите свое значение в локальную переменную (`buttonNumber` в предыдущем примере), а затем используйте ее.</span><span class="sxs-lookup"><span data-stu-id="86dfd-356">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="86dfd-357">Вложенный EventCallback</span><span class="sxs-lookup"><span data-stu-id="86dfd-357">EventCallback</span></span>

<span data-ttu-id="86dfd-358">Распространенным сценарием с вложенными компонентами является желание запускать метод родительского компонента при возникновении события дочернего компонента&mdash;например, когда в дочернем элементе возникает событие `onclick`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-358">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="86dfd-359">Чтобы обеспечить доступ к событиям по компонентам, используйте `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-359">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="86dfd-360">Родительский компонент может назначить метод обратного вызова `EventCallback`у дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-360">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="86dfd-361">`ChildComponent` в примере приложения демонстрирует настройку обработчика `onclick`а кнопки на получение делегата `EventCallback` из `ParentComponent`образца.</span><span class="sxs-lookup"><span data-stu-id="86dfd-361">The `ChildComponent` in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="86dfd-362">`EventCallback` вводится `MouseEventArgs`, который подходит для события `onclick` на периферийном устройстве.</span><span class="sxs-lookup"><span data-stu-id="86dfd-362">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="86dfd-363">`ParentComponent` устанавливает `EventCallback<T>` дочернего элемента в метод `ShowMessage`:</span><span class="sxs-lookup"><span data-stu-id="86dfd-363">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="86dfd-364">При выборе кнопки в `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="86dfd-364">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="86dfd-365">Вызывается метод `ShowMessage` `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-365">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="86dfd-366">`messageText` обновляется и отображается в `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-366">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="86dfd-367">Вызов [статехасчанжед](xref:blazor/lifecycle#state-changes) не требуется в методе обратного вызова (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="86dfd-367">A call to [StateHasChanged](xref:blazor/lifecycle#state-changes) isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="86dfd-368">`StateHasChanged` вызывается автоматически для реотрисовки `ParentComponent`, так же как и дочерние события. компонент активируется в обработчиках событий, которые выполняются внутри дочернего элемента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-368">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="86dfd-369">`EventCallback` и `EventCallback<T>` разрешается выполнять асинхронные делегаты.</span><span class="sxs-lookup"><span data-stu-id="86dfd-369">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="86dfd-370">`EventCallback<T>` является строго типизированным и требует определенного типа аргумента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-370">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="86dfd-371">`EventCallback` слабо типизирован и допускает любой тип аргумента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-371">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="86dfd-372">Вызов `EventCallback` или `EventCallback<T>` с `InvokeAsync` и ожидание <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="86dfd-372">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="86dfd-373">Используйте `EventCallback` и `EventCallback<T>` для обработки событий и параметров компонента привязки.</span><span class="sxs-lookup"><span data-stu-id="86dfd-373">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="86dfd-374">Предпочитать строго типизированный `EventCallback<T>` поверх `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-374">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="86dfd-375">`EventCallback<T>` предоставляет более лучшую реакцию на ошибки для пользователей компонента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-375">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="86dfd-376">Как и в случае с другими обработчиками событий пользовательского интерфейса, указание параметра события является необязательным.</span><span class="sxs-lookup"><span data-stu-id="86dfd-376">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="86dfd-377">Используйте `EventCallback`, если не передается значение обратного вызова.</span><span class="sxs-lookup"><span data-stu-id="86dfd-377">Use `EventCallback` when there's no value passed to the callback.</span></span>

::: moniker range=">= aspnetcore-3.1"

### <a name="prevent-default-actions"></a><span data-ttu-id="86dfd-378">Запретить действия по умолчанию</span><span class="sxs-lookup"><span data-stu-id="86dfd-378">Prevent default actions</span></span>

<span data-ttu-id="86dfd-379">Используйте атрибут [@on{Event}:P ревентдефаулт](xref:mvc/views/razor#oneventpreventdefault) , чтобы предотвратить действие по умолчанию для события.</span><span class="sxs-lookup"><span data-stu-id="86dfd-379">Use the [@on{EVENT}:preventDefault](xref:mvc/views/razor#oneventpreventdefault) directive attribute to prevent the default action for an event.</span></span>

<span data-ttu-id="86dfd-380">Если ключ выбран на устройстве ввода, а элемент находится в текстовом поле, то в браузере обычно отображается символ ключа в текстовом поле.</span><span class="sxs-lookup"><span data-stu-id="86dfd-380">When a key is selected on an input device and the element focus is on a text box, a browser normally displays the key's character in the text box.</span></span> <span data-ttu-id="86dfd-381">В следующем примере поведение по умолчанию запрещено путем указания атрибута директивы `@onkeypress:preventDefault`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-381">In the following example, the default behavior is prevented by specifying the `@onkeypress:preventDefault` directive attribute.</span></span> <span data-ttu-id="86dfd-382">Счетчик увеличивается, а ключ **+** не записывается в значение элемента `<input>`:</span><span class="sxs-lookup"><span data-stu-id="86dfd-382">The counter increments, and the **+** key isn't captured into the `<input>` element's value:</span></span>

```cshtml
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />

@code {
    private int _count = 0;

    private void KeyHandler(KeyboardEventArgs e)
    {
        if (e.Key == "+")
        {
            _count++;
        }
    }
}
```

<span data-ttu-id="86dfd-383">Указание `@on{EVENT}:preventDefault` атрибута без значения эквивалентно `@on{EVENT}:preventDefault="true"`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-383">Specifying the `@on{EVENT}:preventDefault` attribute without a value is equivalent to `@on{EVENT}:preventDefault="true"`.</span></span>

<span data-ttu-id="86dfd-384">Значение атрибута также может быть выражением.</span><span class="sxs-lookup"><span data-stu-id="86dfd-384">The value of the attribute can also be an expression.</span></span> <span data-ttu-id="86dfd-385">В следующем примере `_shouldPreventDefault` является полем `bool`, для которого задано значение `true` или `false`:</span><span class="sxs-lookup"><span data-stu-id="86dfd-385">In the following example, `_shouldPreventDefault` is a `bool` field set to either `true` or `false`:</span></span>

```cshtml
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

<span data-ttu-id="86dfd-386">Для предотвращения действия по умолчанию не требуется обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="86dfd-386">An event handler isn't required to prevent the default action.</span></span> <span data-ttu-id="86dfd-387">Обработчик событий и предотвращение сценариев действий по умолчанию можно использовать независимо друг от друга.</span><span class="sxs-lookup"><span data-stu-id="86dfd-387">The event handler and prevent default action scenarios can be used independently.</span></span>

### <a name="stop-event-propagation"></a><span data-ttu-id="86dfd-388">Отключить распространение событий</span><span class="sxs-lookup"><span data-stu-id="86dfd-388">Stop event propagation</span></span>

<span data-ttu-id="86dfd-389">Используйте атрибут директивы [@on{Event}: stopPropagation](xref:mvc/views/razor#oneventstoppropagation) для отмены распространения событий.</span><span class="sxs-lookup"><span data-stu-id="86dfd-389">Use the [@on{EVENT}:stopPropagation](xref:mvc/views/razor#oneventstoppropagation) directive attribute to stop event propagation.</span></span>

<span data-ttu-id="86dfd-390">В следующем примере при установке флажка события Click из второго дочернего `<div>` не распространяются на родительский `<div>`:</span><span class="sxs-lookup"><span data-stu-id="86dfd-390">In the following example, selecting the check box prevents click events from the second child `<div>` from propagating to the parent `<div>`:</span></span>

```cshtml
<label>
    <input @bind="_stopPropagation" type="checkbox" />
    Stop Propagation
</label>

<div @onclick="OnSelectParentDiv">
    <h3>Parent div</h3>

    <div @onclick="OnSelectChildDiv">
        Child div that doesn't stop propagation when selected.
    </div>

    <div @onclick="OnSelectChildDiv" @onclick:stopPropagation="_stopPropagation">
        Child div that stops propagation when selected.
    </div>
</div>

@code {
    private bool _stopPropagation = false;

    private void OnSelectParentDiv() => 
        Console.WriteLine($"The parent div was selected. {DateTime.Now}");
    private void OnSelectChildDiv() => 
        Console.WriteLine($"A child div was selected. {DateTime.Now}");
}
```

::: moniker-end

## <a name="chained-bind"></a><span data-ttu-id="86dfd-391">Привязка к цепочке</span><span class="sxs-lookup"><span data-stu-id="86dfd-391">Chained bind</span></span>

<span data-ttu-id="86dfd-392">Распространенным сценарием является привязка привязанного к данным параметра к элементу страницы в выходных данных компонента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-392">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="86dfd-393">Этот сценарий называется *связанной* привязкой, так как несколько уровней привязки происходят одновременно.</span><span class="sxs-lookup"><span data-stu-id="86dfd-393">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="86dfd-394">Связанную с цепочкой привязку нельзя реализовать с помощью синтаксиса `@bind` в элементе страницы.</span><span class="sxs-lookup"><span data-stu-id="86dfd-394">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="86dfd-395">Обработчик событий и значение должны быть указаны отдельно.</span><span class="sxs-lookup"><span data-stu-id="86dfd-395">The event handler and value must be specified separately.</span></span> <span data-ttu-id="86dfd-396">Однако родительский компонент может использовать `@bind` синтаксис с параметром компонента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-396">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="86dfd-397">Следующий `PasswordField` компонент (*пассвордфиелд. Razor*):</span><span class="sxs-lookup"><span data-stu-id="86dfd-397">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="86dfd-398">Задает значение `<input>` элемента для свойства `Password`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-398">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="86dfd-399">Предоставляет изменения свойства `Password` в родительский компонент с помощью [вложенный EventCallback](#eventcallback).</span><span class="sxs-lookup"><span data-stu-id="86dfd-399">Exposes changes of the `Password` property to a parent component with an [EventCallback](#eventcallback).</span></span>

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

<span data-ttu-id="86dfd-400">Компонент `PasswordField` используется в другом компоненте:</span><span class="sxs-lookup"><span data-stu-id="86dfd-400">The `PasswordField` component is used in another component:</span></span>

```cshtml
<PasswordField @bind-Password="password" />

@code {
    private string password;
}
```

<span data-ttu-id="86dfd-401">Для выполнения проверок или перехвата ошибок в пароле в предыдущем примере:</span><span class="sxs-lookup"><span data-stu-id="86dfd-401">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="86dfd-402">Создайте резервное поле для `Password` (`password` в следующем примере кода).</span><span class="sxs-lookup"><span data-stu-id="86dfd-402">Create a backing field for `Password` (`password` in the following example code).</span></span>
* <span data-ttu-id="86dfd-403">Выполните проверки или ошибки ловушек в методе задания `Password`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-403">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="86dfd-404">В следующем примере представлена немедленная реакция пользователя, если в значении пароля используется пробел:</span><span class="sxs-lookup"><span data-stu-id="86dfd-404">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

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

## <a name="capture-references-to-components"></a><span data-ttu-id="86dfd-405">Запись ссылок на компоненты</span><span class="sxs-lookup"><span data-stu-id="86dfd-405">Capture references to components</span></span>

<span data-ttu-id="86dfd-406">Ссылки на компоненты предоставляют способ ссылки на экземпляр компонента, чтобы можно было выполнять команды для этого экземпляра, например `Show` или `Reset`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-406">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="86dfd-407">Чтобы записать ссылку на компонент, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="86dfd-407">To capture a component reference:</span></span>

* <span data-ttu-id="86dfd-408">Добавьте атрибут [@ref](xref:mvc/views/razor#ref) в дочерний компонент.</span><span class="sxs-lookup"><span data-stu-id="86dfd-408">Add an [@ref](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="86dfd-409">Определите поле с тем же типом, что и у дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-409">Define a field with the same type as the child component.</span></span>

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

<span data-ttu-id="86dfd-410">При подготовке компонента к просмотру `loginDialog` поле заполняется экземпляром `MyLoginDialog` дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-410">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="86dfd-411">Затем можно вызывать методы .NET в экземпляре компонента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-411">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="86dfd-412">`loginDialog` переменная заполняется только после подготовки компонента, а ее выходные данные включают элемент `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-412">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="86dfd-413">До этого момента нет никаких ссылок на.</span><span class="sxs-lookup"><span data-stu-id="86dfd-413">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="86dfd-414">Чтобы управлять ссылками на компоненты после завершения подготовки компонента к просмотру, используйте [методы онафтеррендерасинк или онафтеррендер](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="86dfd-414">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="86dfd-415">При захвате ссылок на компоненты используется аналогичный синтаксис для [записи ссылок на элементы](xref:blazor/javascript-interop#capture-references-to-elements), но это не функция [взаимодействия JavaScript](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="86dfd-415">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="86dfd-416">Ссылки на компоненты не передаются в код JavaScript&mdash;они используются только в коде .NET.</span><span class="sxs-lookup"><span data-stu-id="86dfd-416">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="86dfd-417">**Не** используйте ссылки на компоненты для изменения состояния дочерних компонентов.</span><span class="sxs-lookup"><span data-stu-id="86dfd-417">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="86dfd-418">Вместо этого используйте обычные декларативные параметры для передачи данных дочерним компонентам.</span><span class="sxs-lookup"><span data-stu-id="86dfd-418">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="86dfd-419">Использование обычных декларативных параметров приводит к тому, что дочерние компоненты автоматически визуализируются в нужное время.</span><span class="sxs-lookup"><span data-stu-id="86dfd-419">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="86dfd-420">Вызывать методы компонента извне для обновления состояния</span><span class="sxs-lookup"><span data-stu-id="86dfd-420">Invoke component methods externally to update state</span></span>

Blazor<span data-ttu-id="86dfd-421"> использует `SynchronizationContext` для принудительного применения одного логического потока выполнения.</span><span class="sxs-lookup"><span data-stu-id="86dfd-421"> uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="86dfd-422">В этом `SynchronizationContext`выполняются [методы жизненного цикла](xref:blazor/lifecycle) компонента и любые обратные вызовы событий, вызванные Blazor.</span><span class="sxs-lookup"><span data-stu-id="86dfd-422">A component's [lifecycle methods](xref:blazor/lifecycle) and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="86dfd-423">В случае, если компонент должен быть обновлен на основе внешнего события, такого как таймер или другие уведомления, используйте метод `InvokeAsync`, который будет отправляться в `SynchronizationContext`Blazor.</span><span class="sxs-lookup"><span data-stu-id="86dfd-423">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="86dfd-424">Например, рассмотрим *службу уведомления* , которая может уведомлять любой компонент, выполняющий прослушивание, в обновленном состоянии:</span><span class="sxs-lookup"><span data-stu-id="86dfd-424">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

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

<span data-ttu-id="86dfd-425">Использование `NotifierService` для обновления компонента:</span><span class="sxs-lookup"><span data-stu-id="86dfd-425">Usage of the `NotifierService` to update a component:</span></span>

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

<span data-ttu-id="86dfd-426">В предыдущем примере `NotifierService` вызывает метод `OnNotify` компонента за пределами Blazor`SynchronizationContext`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-426">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="86dfd-427">`InvokeAsync` используется для переключения на правильный контекст и постановка в очередь визуализации.</span><span class="sxs-lookup"><span data-stu-id="86dfd-427">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="86dfd-428">Использование ключа \@для управления сохранением элементов и компонентов</span><span class="sxs-lookup"><span data-stu-id="86dfd-428">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="86dfd-429">При последующем изменении списка элементов или компонентов, а также элементов или компонентов алгоритм сравнения Blazorдолжен решить, какие из предыдущих элементов или компонентов могут быть сохранены и как объекты модели должны сопоставляться с ними.</span><span class="sxs-lookup"><span data-stu-id="86dfd-429">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="86dfd-430">Обычно этот процесс выполняется автоматически и его можно игнорировать, но в некоторых случаях может потребоваться управление процессом.</span><span class="sxs-lookup"><span data-stu-id="86dfd-430">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="86dfd-431">Рассмотрим следующий пример.</span><span class="sxs-lookup"><span data-stu-id="86dfd-431">Consider the following example:</span></span>

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

<span data-ttu-id="86dfd-432">Содержимое коллекции `People` может изменяться при вставке, удалении или повторном упорядочении записей.</span><span class="sxs-lookup"><span data-stu-id="86dfd-432">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="86dfd-433">Когда компонент перерисовывается, компонент `<DetailsEditor>` может измениться на получение различных значений параметров `Details`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-433">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="86dfd-434">Это может привести к более сложной визуализации, чем ожидалось.</span><span class="sxs-lookup"><span data-stu-id="86dfd-434">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="86dfd-435">В некоторых случаях перерисовка может привести к появлению видимых различий в поведении, таких как потеря фокуса элементов.</span><span class="sxs-lookup"><span data-stu-id="86dfd-435">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="86dfd-436">Процесс сопоставления можно контролировать с помощью атрибута директивы `@key`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-436">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="86dfd-437">`@key` заставляет алгоритм сравнения гарантировать сохранение элементов или компонентов на основе значения ключа:</span><span class="sxs-lookup"><span data-stu-id="86dfd-437">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

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

<span data-ttu-id="86dfd-438">При изменении коллекции `People` алгоритм сравнения удерживает связи между экземплярами `<DetailsEditor>` и экземплярами `person`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-438">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="86dfd-439">Если `Person` удаляется из списка `People`, то из пользовательского интерфейса удаляется только соответствующий экземпляр `<DetailsEditor>`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-439">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="86dfd-440">Другие экземпляры остаются без изменений.</span><span class="sxs-lookup"><span data-stu-id="86dfd-440">Other instances are left unchanged.</span></span>
* <span data-ttu-id="86dfd-441">Если в какой-либо позиции в списке вставляется `Person`, то в соответствующей позиции вставляется один новый экземпляр `<DetailsEditor>`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-441">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="86dfd-442">Другие экземпляры остаются без изменений.</span><span class="sxs-lookup"><span data-stu-id="86dfd-442">Other instances are left unchanged.</span></span>
* <span data-ttu-id="86dfd-443">При повторном упорядочении записей `Person` соответствующие экземпляры `<DetailsEditor>` сохраняются и переупорядочиваются в пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="86dfd-443">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="86dfd-444">В некоторых сценариях использование `@key` уменьшает сложность повторного отображения и позволяет избежать потенциальных проблем с элементами, изменяющимися с отслеживанием состояния DOM, например положением фокуса.</span><span class="sxs-lookup"><span data-stu-id="86dfd-444">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="86dfd-445">Ключи являются локальными для каждого элемента или компонента контейнера.</span><span class="sxs-lookup"><span data-stu-id="86dfd-445">Keys are local to each container element or component.</span></span> <span data-ttu-id="86dfd-446">Ключи не сравниваются глобально по всему документу.</span><span class="sxs-lookup"><span data-stu-id="86dfd-446">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="86dfd-447">Когда следует использовать ключ \@</span><span class="sxs-lookup"><span data-stu-id="86dfd-447">When to use \@key</span></span>

<span data-ttu-id="86dfd-448">Как правило, имеет смысл использовать `@key` всякий раз при подготовке списка (например, в блоке `@foreach`) и при наличии подходящего значения для определения `@key`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-448">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="86dfd-449">Можно также использовать `@key`, чтобы запретить Blazor сохранение поддерева элемента или компонента при изменении объекта.</span><span class="sxs-lookup"><span data-stu-id="86dfd-449">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="86dfd-450">Если `@currentPerson` меняется, директива атрибута `@key` заставляет Blazor отбросить все `<div>` и его потомков и перестроить поддерево в пользовательском интерфейсе с помощью новых элементов и компонентов.</span><span class="sxs-lookup"><span data-stu-id="86dfd-450">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="86dfd-451">Это может быть полезно, если необходимо гарантировать, что при `@currentPerson` изменений состояние пользовательского интерфейса не сохраняется.</span><span class="sxs-lookup"><span data-stu-id="86dfd-451">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="86dfd-452">Когда не следует использовать ключ \@</span><span class="sxs-lookup"><span data-stu-id="86dfd-452">When not to use \@key</span></span>

<span data-ttu-id="86dfd-453">При использовании сравнения с `@key`ми возникают затраты на производительность.</span><span class="sxs-lookup"><span data-stu-id="86dfd-453">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="86dfd-454">Затраты на производительность не слишком велики, но указываются только `@key` если управление правилами сохранения элементов или компонентов выгодно для приложения.</span><span class="sxs-lookup"><span data-stu-id="86dfd-454">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="86dfd-455">Даже если `@key` не используется, Blazor сохраняет как можно больше экземпляров дочернего элемента и компонента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-455">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="86dfd-456">Единственным преимуществом использования `@key` является управление *тем, как* экземпляры модели сопоставляются с сохраненными экземплярами компонента, а не алгоритмом сравнения, который выбирает сопоставление.</span><span class="sxs-lookup"><span data-stu-id="86dfd-456">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="86dfd-457">Какие значения следует использовать для ключа \@</span><span class="sxs-lookup"><span data-stu-id="86dfd-457">What values to use for \@key</span></span>

<span data-ttu-id="86dfd-458">Как правило, имеет смысл указать одно из следующих значений для `@key`:</span><span class="sxs-lookup"><span data-stu-id="86dfd-458">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="86dfd-459">Экземпляры объектов Model (например, экземпляр `Person`, как в предыдущем примере).</span><span class="sxs-lookup"><span data-stu-id="86dfd-459">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="86dfd-460">Это гарантирует сохранение на основе равенства ссылок на объекты.</span><span class="sxs-lookup"><span data-stu-id="86dfd-460">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="86dfd-461">Уникальные идентификаторы (например, значения первичного ключа типа `int`, `string`или `Guid`).</span><span class="sxs-lookup"><span data-stu-id="86dfd-461">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="86dfd-462">Убедитесь, что значения, используемые для `@key`, не конфликтуют.</span><span class="sxs-lookup"><span data-stu-id="86dfd-462">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="86dfd-463">Если конфликтные значения обнаруживаются в одном родительском элементе, Blazor создает исключение, поскольку оно не может детерминированно сопоставлять старые элементы или компоненты с новыми элементами или компонентами.</span><span class="sxs-lookup"><span data-stu-id="86dfd-463">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="86dfd-464">Используйте только уникальные значения, такие как экземпляры объекта или значения первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="86dfd-464">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="routing"></a><span data-ttu-id="86dfd-465">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="86dfd-465">Routing</span></span>

<span data-ttu-id="86dfd-466">Маршрутизация в Blazor достигается путем предоставления шаблона маршрута для каждого доступного компонента в приложении.</span><span class="sxs-lookup"><span data-stu-id="86dfd-466">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="86dfd-467">При компиляции файла Razor с директивой `@page` созданному классу присваивается <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> с указанием шаблона маршрута.</span><span class="sxs-lookup"><span data-stu-id="86dfd-467">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="86dfd-468">Во время выполнения маршрутизатор ищет классы компонентов с `RouteAttribute` и отображает, какой из компонентов имеет шаблон маршрута, соответствующий запрошенному URL-адресу.</span><span class="sxs-lookup"><span data-stu-id="86dfd-468">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="86dfd-469">К компоненту можно применить несколько шаблонов маршрутов.</span><span class="sxs-lookup"><span data-stu-id="86dfd-469">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="86dfd-470">Следующий компонент отвечает на запросы `/BlazorRoute` и `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="86dfd-470">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="86dfd-471">Параметры маршрута</span><span class="sxs-lookup"><span data-stu-id="86dfd-471">Route parameters</span></span>

<span data-ttu-id="86dfd-472">Компоненты могут принимать параметры маршрута из шаблона маршрута, указанного в директиве `@page`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-472">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="86dfd-473">Маршрутизатор использует параметры маршрута для заполнения соответствующих параметров компонента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-473">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="86dfd-474">*Компонент параметра маршрута*:</span><span class="sxs-lookup"><span data-stu-id="86dfd-474">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="86dfd-475">Необязательные параметры не поддерживаются, поэтому в приведенном выше примере применяются две директивы `@page`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-475">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="86dfd-476">Первый позволяет переходить к компоненту без параметра.</span><span class="sxs-lookup"><span data-stu-id="86dfd-476">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="86dfd-477">Вторая директива `@page` принимает параметр `{text}` Route и присваивает значение свойству `Text`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-477">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

<span data-ttu-id="86dfd-478">Синтаксис параметра *Catch-All* (`*`/`**`), который захватывает путь для нескольких папок, **не** поддерживается в компонентах Razor ( *. Razor*).</span><span class="sxs-lookup"><span data-stu-id="86dfd-478">*Catch-all* parameter syntax (`*`/`**`), which captures the path across multiple folder boundaries, is **not** supported in Razor components (*.razor*).</span></span>

::: moniker range=">= aspnetcore-3.1"

## <a name="partial-class-support"></a><span data-ttu-id="86dfd-479">Поддержка разделяемых классов</span><span class="sxs-lookup"><span data-stu-id="86dfd-479">Partial class support</span></span>

<span data-ttu-id="86dfd-480">Компоненты Razor создаются как разделяемые классы.</span><span class="sxs-lookup"><span data-stu-id="86dfd-480">Razor components are generated as partial classes.</span></span> <span data-ttu-id="86dfd-481">Компоненты Razor создаются с помощью любого из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="86dfd-481">Razor components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="86dfd-482">C#код определяется в [@code](xref:mvc/views/razor#code) блоке с разметкой HTML и кодом Razor в одном файле.</span><span class="sxs-lookup"><span data-stu-id="86dfd-482">C# code is defined in an [@code](xref:mvc/views/razor#code) block with HTML markup and Razor code in a single file.</span></span> <span data-ttu-id="86dfd-483">с помощью этого подхода шаблоны Blazor определяют свои компоненты Razor.</span><span class="sxs-lookup"><span data-stu-id="86dfd-483">Blazor templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="86dfd-484">C#код помещается в файл кода программной части, определенный как разделяемый класс.</span><span class="sxs-lookup"><span data-stu-id="86dfd-484">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="86dfd-485">В следующем примере показан компонент `Counter` по умолчанию с блоком `@code` в приложении, созданном из шаблона Blazor.</span><span class="sxs-lookup"><span data-stu-id="86dfd-485">The following example shows the default `Counter` component with an `@code` block in an app generated from a Blazor template.</span></span> <span data-ttu-id="86dfd-486">Разметка HTML, код Razor и C# код находятся в одном файле:</span><span class="sxs-lookup"><span data-stu-id="86dfd-486">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="86dfd-487">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="86dfd-487">*Counter.razor*:</span></span>

```cshtml
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    int currentCount = 0;

    void IncrementCount()
    {
        currentCount++;
    }
}
```

<span data-ttu-id="86dfd-488">`Counter` компонент можно также создать с помощью файла кода программной части с разделяемым классом:</span><span class="sxs-lookup"><span data-stu-id="86dfd-488">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="86dfd-489">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="86dfd-489">*Counter.razor*:</span></span>

```cshtml
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="86dfd-490">*Counter.Razor.CS*:</span><span class="sxs-lookup"><span data-stu-id="86dfd-490">*Counter.razor.cs*:</span></span>

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount++;
        }
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

## <a name="specify-a-component-base-class"></a><span data-ttu-id="86dfd-491">Укажите базовый класс компонента</span><span class="sxs-lookup"><span data-stu-id="86dfd-491">Specify a component base class</span></span>

<span data-ttu-id="86dfd-492">Директиву `@inherits` можно использовать, чтобы указать базовый класс для компонента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-492">The `@inherits` directive can be used to specify a base class for a component.</span></span>

<span data-ttu-id="86dfd-493">В [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) показано, как компонент может наследовать базовый класс `BlazorRocksBase`, чтобы предоставить свойства и методы компонента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-493">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="86dfd-494">*Pages/блазорроккс. Razor*:</span><span class="sxs-lookup"><span data-stu-id="86dfd-494">*Pages/BlazorRocks.razor*:</span></span>

```cshtml
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="86dfd-495">*BlazorRocksBase.CS*:</span><span class="sxs-lookup"><span data-stu-id="86dfd-495">*BlazorRocksBase.cs*:</span></span>

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

<span data-ttu-id="86dfd-496">Базовый класс должен быть производным от `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-496">The base class should derive from `ComponentBase`.</span></span>

::: moniker-end

## <a name="import-components"></a><span data-ttu-id="86dfd-497">Импорт компонентов</span><span class="sxs-lookup"><span data-stu-id="86dfd-497">Import components</span></span>

<span data-ttu-id="86dfd-498">Пространство имен компонента, созданного с помощью Razor, основано на (в порядке приоритета):</span><span class="sxs-lookup"><span data-stu-id="86dfd-498">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="86dfd-499">[@namespaceе](xref:mvc/views/razor#namespace) обозначение в разметке файла Razor ( *. Razor*) (`@namespace BlazorSample.MyNamespace`).</span><span class="sxs-lookup"><span data-stu-id="86dfd-499">[@namespace](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="86dfd-500">`RootNamespace` проекта в файле проекта (`<RootNamespace>BlazorSample</RootNamespace>`).</span><span class="sxs-lookup"><span data-stu-id="86dfd-500">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="86dfd-501">Имя проекта, полученное из имени файла проекта (*CSPROJ*), и путь из корневого каталога проекта к компоненту.</span><span class="sxs-lookup"><span data-stu-id="86dfd-501">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="86dfd-502">Например, платформа разрешает *{root Project}/Пажес/индекс.Разор* (*блазорсампле. csproj*) к пространству имен `BlazorSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-502">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="86dfd-503">Компоненты следуют C# правилам привязки имен.</span><span class="sxs-lookup"><span data-stu-id="86dfd-503">Components follow C# name binding rules.</span></span> <span data-ttu-id="86dfd-504">Для компонента `Index` в этом примере компоненты в области являются всеми компонентами:</span><span class="sxs-lookup"><span data-stu-id="86dfd-504">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="86dfd-505">В той же папке *страницы*.</span><span class="sxs-lookup"><span data-stu-id="86dfd-505">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="86dfd-506">Компоненты в корне проекта, которые не задают явно другое пространство имен.</span><span class="sxs-lookup"><span data-stu-id="86dfd-506">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="86dfd-507">Компоненты, определенные в другом пространстве имен, помещаются в область с помощью директивы [@using](xref:mvc/views/razor#using) Razor.</span><span class="sxs-lookup"><span data-stu-id="86dfd-507">Components defined in a different namespace are brought into scope using Razor's [@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="86dfd-508">Если другой компонент, `NavMenu.razor`, существует в папке *блазорсампле/Shared/* Folder, компонент можно использовать в `Index.razor` со следующей инструкцией `@using`:</span><span class="sxs-lookup"><span data-stu-id="86dfd-508">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="86dfd-509">На компоненты также можно ссылаться с помощью полных имен, для которых не требуется директива [@using](xref:mvc/views/razor#using) :</span><span class="sxs-lookup"><span data-stu-id="86dfd-509">Components can also be referenced using their fully qualified names, which doesn't require the [@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="86dfd-510">Квалификация `global::` не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="86dfd-510">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="86dfd-511">Импорт компонентов с псевдонимами `using` операторы (например, `@using Foo = Bar`) не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="86dfd-511">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="86dfd-512">Частично определенные имена не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="86dfd-512">Partially qualified names aren't supported.</span></span> <span data-ttu-id="86dfd-513">Например, добавление `@using BlazorSample` и ссылки на `NavMenu.razor` с `<Shared.NavMenu></Shared.NavMenu>` не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="86dfd-513">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="86dfd-514">Атрибуты условного HTML-элемента</span><span class="sxs-lookup"><span data-stu-id="86dfd-514">Conditional HTML element attributes</span></span>

<span data-ttu-id="86dfd-515">Атрибуты HTML-элемента условно подготавливаются в зависимости от значения .NET.</span><span class="sxs-lookup"><span data-stu-id="86dfd-515">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="86dfd-516">Если значение равно `false` или `null`, то атрибут не отображается.</span><span class="sxs-lookup"><span data-stu-id="86dfd-516">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="86dfd-517">Если значение равно `true`, атрибут отображается в виде сворачивания.</span><span class="sxs-lookup"><span data-stu-id="86dfd-517">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="86dfd-518">В следующем примере `IsCompleted` определяет, отображается ли `checked` в разметке элемента:</span><span class="sxs-lookup"><span data-stu-id="86dfd-518">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="86dfd-519">Если `IsCompleted` `true`, флажок отображается следующим образом:</span><span class="sxs-lookup"><span data-stu-id="86dfd-519">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="86dfd-520">Если `IsCompleted` `false`, флажок отображается следующим образом:</span><span class="sxs-lookup"><span data-stu-id="86dfd-520">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="86dfd-521">Для получения дополнительной информации см. <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="86dfd-521">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="86dfd-522">Некоторые атрибуты HTML, такие как [ARIA-Pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), не работают должным образом, если тип .net является `bool`ом.</span><span class="sxs-lookup"><span data-stu-id="86dfd-522">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="86dfd-523">В этих случаях используйте `string` тип вместо `bool`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-523">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="86dfd-524">Необработанный HTML</span><span class="sxs-lookup"><span data-stu-id="86dfd-524">Raw HTML</span></span>

<span data-ttu-id="86dfd-525">Строки обычно подготавливаются с помощью текстовых узлов DOM, что означает, что любая разметка, которую они могут содержать, игнорируется и обрабатывается как литеральный текст.</span><span class="sxs-lookup"><span data-stu-id="86dfd-525">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="86dfd-526">Для отрисовки необработанного HTML-кода заключите содержимое HTML в `MarkupString` значение.</span><span class="sxs-lookup"><span data-stu-id="86dfd-526">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="86dfd-527">Значение анализируется как HTML или SVG и вставляется в модель DOM.</span><span class="sxs-lookup"><span data-stu-id="86dfd-527">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="86dfd-528">Визуализация необработанного HTML-кода, созданного из любого ненадежного источника, является **угрозой безопасности** , и ее следует избегать.</span><span class="sxs-lookup"><span data-stu-id="86dfd-528">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="86dfd-529">В следующем примере показано использование типа `MarkupString` для добавления блока статического HTML-содержимого в отображаемые выходные данные компонента:</span><span class="sxs-lookup"><span data-stu-id="86dfd-529">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="86dfd-530">Шаблонные компоненты</span><span class="sxs-lookup"><span data-stu-id="86dfd-530">Templated components</span></span>

<span data-ttu-id="86dfd-531">Шаблонные компоненты — это компоненты, принимающие один или несколько шаблонов пользовательского интерфейса в качестве параметров, которые затем можно использовать как часть логики отрисовки компонента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-531">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="86dfd-532">Шаблонные компоненты позволяют создавать более высокие компоненты более высокого уровня, чем обычные компоненты.</span><span class="sxs-lookup"><span data-stu-id="86dfd-532">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="86dfd-533">Ниже приведены несколько примеров.</span><span class="sxs-lookup"><span data-stu-id="86dfd-533">A couple of examples include:</span></span>

* <span data-ttu-id="86dfd-534">Компонент таблицы, позволяющий пользователю указывать шаблоны для заголовков, строк и нижних колонтитулов таблицы.</span><span class="sxs-lookup"><span data-stu-id="86dfd-534">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="86dfd-535">Компонент списка, позволяющий пользователю указать шаблон для отрисовки элементов в списке.</span><span class="sxs-lookup"><span data-stu-id="86dfd-535">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="86dfd-536">Параметры шаблона</span><span class="sxs-lookup"><span data-stu-id="86dfd-536">Template parameters</span></span>

<span data-ttu-id="86dfd-537">Шаблонный компонент определяется путем указания одного или нескольких параметров компонента типа `RenderFragment` или `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-537">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="86dfd-538">Фрагмент отрисовки представляет сегмент пользовательского интерфейса для отрисовки.</span><span class="sxs-lookup"><span data-stu-id="86dfd-538">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="86dfd-539">`RenderFragment<T>` принимает параметр типа, который может быть указан при вызове фрагмента прорисовки.</span><span class="sxs-lookup"><span data-stu-id="86dfd-539">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="86dfd-540">`TableTemplate` компонент:</span><span class="sxs-lookup"><span data-stu-id="86dfd-540">`TableTemplate` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

<span data-ttu-id="86dfd-541">При использовании компонент-шаблона можно указать параметры шаблона с помощью дочерних элементов, совпадающих с именами параметров (`TableHeader` и `RowTemplate` в следующем примере):</span><span class="sxs-lookup"><span data-stu-id="86dfd-541">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

### <a name="template-context-parameters"></a><span data-ttu-id="86dfd-542">Параметры контекста шаблона</span><span class="sxs-lookup"><span data-stu-id="86dfd-542">Template context parameters</span></span>

<span data-ttu-id="86dfd-543">Аргументы компонента типа `RenderFragment<T>` передаются как элементы, имеют неявный параметр с именем `context` (например, из предыдущего примера кода, `@context.PetId`), но можно изменить имя параметра с помощью атрибута `Context` дочернего элемента.</span><span class="sxs-lookup"><span data-stu-id="86dfd-543">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="86dfd-544">В следующем примере атрибут `Context` элемента `RowTemplate` указывает параметр `pet`:</span><span class="sxs-lookup"><span data-stu-id="86dfd-544">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="86dfd-545">Кроме того, можно указать атрибут `Context` в элементе Component.</span><span class="sxs-lookup"><span data-stu-id="86dfd-545">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="86dfd-546">Указанный атрибут `Context` применяется ко всем указанным параметрам шаблона.</span><span class="sxs-lookup"><span data-stu-id="86dfd-546">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="86dfd-547">Это может быть полезно, если необходимо указать имя параметра содержимого для неявного дочернего содержимого (без обертывания дочернего элемента).</span><span class="sxs-lookup"><span data-stu-id="86dfd-547">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="86dfd-548">В следующем примере атрибут `Context` отображается в элементе `TableTemplate` и применяется ко всем параметрам шаблона:</span><span class="sxs-lookup"><span data-stu-id="86dfd-548">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

### <a name="generic-typed-components"></a><span data-ttu-id="86dfd-549">Универсальные типы компонентов</span><span class="sxs-lookup"><span data-stu-id="86dfd-549">Generic-typed components</span></span>

<span data-ttu-id="86dfd-550">Шаблонные компоненты часто вводятся в универсальном виде.</span><span class="sxs-lookup"><span data-stu-id="86dfd-550">Templated components are often generically typed.</span></span> <span data-ttu-id="86dfd-551">Например, универсальный компонент `ListViewTemplate` можно использовать для отображения значений `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-551">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="86dfd-552">Чтобы определить универсальный компонент, используйте директиву [@typeparam](xref:mvc/views/razor#typeparam) для указания параметров типа:</span><span class="sxs-lookup"><span data-stu-id="86dfd-552">To define a generic component, use the [@typeparam](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

<span data-ttu-id="86dfd-553">При использовании компонентов универсальной типизации параметр типа выводится, если это возможно:</span><span class="sxs-lookup"><span data-stu-id="86dfd-553">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="86dfd-554">В противном случае параметр типа необходимо явно указать с помощью атрибута, совпадающего с именем параметра типа.</span><span class="sxs-lookup"><span data-stu-id="86dfd-554">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="86dfd-555">В следующем примере `TItem="Pet"` указывает тип:</span><span class="sxs-lookup"><span data-stu-id="86dfd-555">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="86dfd-556">Каскадные значения и параметры</span><span class="sxs-lookup"><span data-stu-id="86dfd-556">Cascading values and parameters</span></span>

<span data-ttu-id="86dfd-557">В некоторых сценариях неудобно передавать данные из компонента-предка в дочерний компонент с помощью [параметров компонента](#component-parameters), особенно при наличии нескольких слоев компонентов.</span><span class="sxs-lookup"><span data-stu-id="86dfd-557">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="86dfd-558">Каскадные значения и параметры позволяют решить эту проблему, предоставляя удобный способ, позволяющий компоненту-предку предоставить значение всем его дочерним компонентам.</span><span class="sxs-lookup"><span data-stu-id="86dfd-558">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="86dfd-559">Каскадные значения и параметры также предоставляют подход для координации компонентов.</span><span class="sxs-lookup"><span data-stu-id="86dfd-559">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="86dfd-560">Пример темы</span><span class="sxs-lookup"><span data-stu-id="86dfd-560">Theme example</span></span>

<span data-ttu-id="86dfd-561">В следующем примере из примера приложения класс `ThemeInfo` указывает сведения о теме для перетекания иерархии компонентов, чтобы все кнопки в пределах данной части приложения совпадали с тем же стилем.</span><span class="sxs-lookup"><span data-stu-id="86dfd-561">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="86dfd-562">*Уисемеклассес/themeinfo указывает расположение. CS*:</span><span class="sxs-lookup"><span data-stu-id="86dfd-562">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="86dfd-563">Компонент-предок может предоставлять каскадное значение с помощью компонента каскадного значения.</span><span class="sxs-lookup"><span data-stu-id="86dfd-563">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="86dfd-564">Компонент `CascadingValue` упаковывает поддерево иерархии компонентов и предоставляет одно значение всем компонентам в этом поддереве.</span><span class="sxs-lookup"><span data-stu-id="86dfd-564">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="86dfd-565">Например, в примере приложения указываются сведения о теме (`ThemeInfo`) в одном из макетов приложения в виде каскадного параметра для всех компонентов, составляющих текст макета свойства `@Body`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-565">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="86dfd-566">`ButtonClass` присваивается значение `btn-success` в компоненте макета.</span><span class="sxs-lookup"><span data-stu-id="86dfd-566">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="86dfd-567">Любой компонент-потомок может использовать это свойство через `ThemeInfo` каскадный объект.</span><span class="sxs-lookup"><span data-stu-id="86dfd-567">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="86dfd-568">`CascadingValuesParametersLayout` компонент:</span><span class="sxs-lookup"><span data-stu-id="86dfd-568">`CascadingValuesParametersLayout` component:</span></span>

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

<span data-ttu-id="86dfd-569">Чтобы использовать каскадные значения, компоненты объявляют каскадные параметры с помощью атрибута `[CascadingParameter]`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-569">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="86dfd-570">Каскадные значения привязываются к каскадным параметрам по типу.</span><span class="sxs-lookup"><span data-stu-id="86dfd-570">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="86dfd-571">В примере приложения компонент `CascadingValuesParametersTheme` привязывает `ThemeInfo` каскадное значение к каскадному параметру.</span><span class="sxs-lookup"><span data-stu-id="86dfd-571">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="86dfd-572">Параметр используется для задания класса CSS для одной из кнопок, отображаемых компонентом.</span><span class="sxs-lookup"><span data-stu-id="86dfd-572">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="86dfd-573">`CascadingValuesParametersTheme` компонент:</span><span class="sxs-lookup"><span data-stu-id="86dfd-573">`CascadingValuesParametersTheme` component:</span></span>

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

<span data-ttu-id="86dfd-574">Чтобы вычислить несколько значений одного типа в одном поддереве, укажите уникальную строку `Name` для каждого компонента `CascadingValue` и соответствующего `CascadingParameter`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-574">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="86dfd-575">В следующем примере два `CascadingValue` компонентов каскадом разворачивают разные экземпляры `MyCascadingType` по имени:</span><span class="sxs-lookup"><span data-stu-id="86dfd-575">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

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

<span data-ttu-id="86dfd-576">В компоненте-потомках каскадные параметры получают значения из соответствующих каскадных значений в компоненте-предке по имени:</span><span class="sxs-lookup"><span data-stu-id="86dfd-576">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```cshtml
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="86dfd-577">Пример Табсет</span><span class="sxs-lookup"><span data-stu-id="86dfd-577">TabSet example</span></span>

<span data-ttu-id="86dfd-578">Каскадные параметры также позволяют компонентам работать в иерархии компонентов.</span><span class="sxs-lookup"><span data-stu-id="86dfd-578">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="86dfd-579">Например, рассмотрим следующий пример *табсет* в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="86dfd-579">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="86dfd-580">В примере приложения имеется интерфейс `ITab`, который реализуется вкладками:</span><span class="sxs-lookup"><span data-stu-id="86dfd-580">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="86dfd-581">Компонент `CascadingValuesParametersTabSet` использует компонент `TabSet`, который содержит несколько `Tab` компонентов:</span><span class="sxs-lookup"><span data-stu-id="86dfd-581">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="86dfd-582">Дочерние `Tab` компоненты явно не передаются в качестве параметров в `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-582">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="86dfd-583">Вместо этого дочерние `Tab` компоненты являются частью дочернего содержимого `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-583">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="86dfd-584">Однако `TabSet` по-прежнему необходимо узнать о каждом компоненте `Tab`, чтобы он мог отобразить заголовки и активную вкладку. Чтобы обеспечить эту координацию без необходимости дополнительного кода, компонент `TabSet` *может предоставить себя как каскадное значение* , которое затем будет выбиралось компонентами-потомками `Tab`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-584">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="86dfd-585">`TabSet` компонент:</span><span class="sxs-lookup"><span data-stu-id="86dfd-585">`TabSet` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="86dfd-586">Дочерние `Tab` компоненты захватывают содержащий `TabSet` в виде каскадного параметра, поэтому `Tab` компоненты добавляются в `TabSet` и координирует, какая вкладка активна.</span><span class="sxs-lookup"><span data-stu-id="86dfd-586">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="86dfd-587">`Tab` компонент:</span><span class="sxs-lookup"><span data-stu-id="86dfd-587">`Tab` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="86dfd-588">Шаблоны Razor</span><span class="sxs-lookup"><span data-stu-id="86dfd-588">Razor templates</span></span>

<span data-ttu-id="86dfd-589">Фрагменты визуализации можно определить с помощью синтаксиса шаблона Razor.</span><span class="sxs-lookup"><span data-stu-id="86dfd-589">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="86dfd-590">Шаблоны Razor позволяют определить фрагмент пользовательского интерфейса и принять следующий формат:</span><span class="sxs-lookup"><span data-stu-id="86dfd-590">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="86dfd-591">В следующем примере показано, как указать значения `RenderFragment` и `RenderFragment<T>` и визуализировать шаблоны непосредственно в компоненте.</span><span class="sxs-lookup"><span data-stu-id="86dfd-591">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="86dfd-592">Фрагменты рендеринга также могут передаваться в качестве аргументов в [шаблонные компоненты](#templated-components).</span><span class="sxs-lookup"><span data-stu-id="86dfd-592">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

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

<span data-ttu-id="86dfd-593">Отображаемые выходные данные предыдущего кода:</span><span class="sxs-lookup"><span data-stu-id="86dfd-593">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="86dfd-594">Логика Рендертрибуилдер вручную</span><span class="sxs-lookup"><span data-stu-id="86dfd-594">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="86dfd-595">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` предоставляет методы для управления компонентами и элементами, включая создание компонентов вручную в C# коде.</span><span class="sxs-lookup"><span data-stu-id="86dfd-595">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="86dfd-596">Использование `RenderTreeBuilder` для создания компонентов является расширенным сценарием.</span><span class="sxs-lookup"><span data-stu-id="86dfd-596">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="86dfd-597">Неправильно сформированный компонент (например, незакрытый тег разметки) может привести к неопределенному поведению.</span><span class="sxs-lookup"><span data-stu-id="86dfd-597">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="86dfd-598">Рассмотрим следующий компонент `PetDetails`, который можно вручную встроить в другой компонент:</span><span class="sxs-lookup"><span data-stu-id="86dfd-598">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="86dfd-599">В следующем примере цикл в методе `CreateComponent` создает три `PetDetails` компонентов.</span><span class="sxs-lookup"><span data-stu-id="86dfd-599">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="86dfd-600">При вызове методов `RenderTreeBuilder` для создания компонентов (`OpenComponent` и `AddAttribute`) порядковые номера представляют собой номера строк исходного кода.</span><span class="sxs-lookup"><span data-stu-id="86dfd-600">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="86dfd-601">Алгоритм разницы Blazor зависит от порядковых номеров, соответствующих разным строкам кода, а не отдельных вызовов вызова.</span><span class="sxs-lookup"><span data-stu-id="86dfd-601">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="86dfd-602">При создании компонента с помощью методов `RenderTreeBuilder` жестко указывайте аргументы для порядковых номеров.</span><span class="sxs-lookup"><span data-stu-id="86dfd-602">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="86dfd-603">**Использование вычисления или счетчика для формирования порядкового номера может привести к снижению производительности.**</span><span class="sxs-lookup"><span data-stu-id="86dfd-603">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="86dfd-604">Дополнительные сведения см. в разделе [порядковые номера относятся к разделу номера строк кода и порядок выполнения](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="86dfd-604">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="86dfd-605">`BuiltContent` компонент:</span><span class="sxs-lookup"><span data-stu-id="86dfd-605">`BuiltContent` component:</span></span>

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

> <span data-ttu-id="86dfd-606">! ! Типы в `Microsoft.AspNetCore.Components.RenderTree` позволяют обрабатывать *результаты* операций отрисовки.</span><span class="sxs-lookup"><span data-stu-id="86dfd-606">![WARNING] The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="86dfd-607">Это внутренние сведения о реализации Blazor Framework.</span><span class="sxs-lookup"><span data-stu-id="86dfd-607">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="86dfd-608">Эти типы следует считать *нестабильными* и могут быть изменены в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="86dfd-608">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="86dfd-609">Порядковые номера связаны с номерами строк кода, а не с порядком выполнения</span><span class="sxs-lookup"><span data-stu-id="86dfd-609">Sequence numbers relate to code line numbers and not execution order</span></span>

Blazor<span data-ttu-id="86dfd-610"> файлы `.razor` компилируются всегда.</span><span class="sxs-lookup"><span data-stu-id="86dfd-610"> `.razor` files are always compiled.</span></span> <span data-ttu-id="86dfd-611">Это, вероятно, является отличным преимуществом для `.razor`, поскольку этап компиляции можно использовать для вставки сведений, повышающих производительность приложения во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="86dfd-611">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="86dfd-612">Ключевым примером этих улучшений являются *порядковые номера*.</span><span class="sxs-lookup"><span data-stu-id="86dfd-612">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="86dfd-613">Порядковые номера указывают среде выполнения, какие выходные данные поступили из разных и упорядоченных строк кода.</span><span class="sxs-lookup"><span data-stu-id="86dfd-613">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="86dfd-614">Среда выполнения использует эти сведения для создания эффективных различий дерева в линейное время, что является гораздо быстрее, чем обычно возможно для общего алгоритма различения дерева.</span><span class="sxs-lookup"><span data-stu-id="86dfd-614">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="86dfd-615">Рассмотрим следующий простой файл `.razor`:</span><span class="sxs-lookup"><span data-stu-id="86dfd-615">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="86dfd-616">Предыдущий код компилируется примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="86dfd-616">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="86dfd-617">Когда код выполняется в первый раз, если `someFlag` `true`, построитель получит следующее:</span><span class="sxs-lookup"><span data-stu-id="86dfd-617">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="86dfd-618">Sequence</span><span class="sxs-lookup"><span data-stu-id="86dfd-618">Sequence</span></span> | <span data-ttu-id="86dfd-619">Тип</span><span class="sxs-lookup"><span data-stu-id="86dfd-619">Type</span></span>      | <span data-ttu-id="86dfd-620">Data</span><span class="sxs-lookup"><span data-stu-id="86dfd-620">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="86dfd-621">0</span><span class="sxs-lookup"><span data-stu-id="86dfd-621">0</span></span>        | <span data-ttu-id="86dfd-622">Узел Text</span><span class="sxs-lookup"><span data-stu-id="86dfd-622">Text node</span></span> | <span data-ttu-id="86dfd-623">First</span><span class="sxs-lookup"><span data-stu-id="86dfd-623">First</span></span>  |
| <span data-ttu-id="86dfd-624">1</span><span class="sxs-lookup"><span data-stu-id="86dfd-624">1</span></span>        | <span data-ttu-id="86dfd-625">Узел Text</span><span class="sxs-lookup"><span data-stu-id="86dfd-625">Text node</span></span> | <span data-ttu-id="86dfd-626">Second</span><span class="sxs-lookup"><span data-stu-id="86dfd-626">Second</span></span> |

<span data-ttu-id="86dfd-627">Представьте, что `someFlag` становится `false`, и разметка снова готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="86dfd-627">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="86dfd-628">На этот раз построитель получит:</span><span class="sxs-lookup"><span data-stu-id="86dfd-628">This time, the builder receives:</span></span>

| <span data-ttu-id="86dfd-629">Sequence</span><span class="sxs-lookup"><span data-stu-id="86dfd-629">Sequence</span></span> | <span data-ttu-id="86dfd-630">Тип</span><span class="sxs-lookup"><span data-stu-id="86dfd-630">Type</span></span>       | <span data-ttu-id="86dfd-631">Data</span><span class="sxs-lookup"><span data-stu-id="86dfd-631">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="86dfd-632">1</span><span class="sxs-lookup"><span data-stu-id="86dfd-632">1</span></span>        | <span data-ttu-id="86dfd-633">Узел Text</span><span class="sxs-lookup"><span data-stu-id="86dfd-633">Text node</span></span>  | <span data-ttu-id="86dfd-634">Second</span><span class="sxs-lookup"><span data-stu-id="86dfd-634">Second</span></span> |

<span data-ttu-id="86dfd-635">Когда среда выполнения выполняет поиск различий, она видит, что элемент в последовательности `0` был удален, поэтому он создает следующий тривиальный *сценарий редактирования*:</span><span class="sxs-lookup"><span data-stu-id="86dfd-635">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="86dfd-636">Удалите первый текстовый узел.</span><span class="sxs-lookup"><span data-stu-id="86dfd-636">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="86dfd-637">Что становится неправильным, если вы создаете порядковые номера программным способом</span><span class="sxs-lookup"><span data-stu-id="86dfd-637">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="86dfd-638">Вместо этого Представьте себе, что вы написали следующую логику конструктора деревьев визуализации:</span><span class="sxs-lookup"><span data-stu-id="86dfd-638">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="86dfd-639">Теперь первые выходные данные:</span><span class="sxs-lookup"><span data-stu-id="86dfd-639">Now, the first output is:</span></span>

| <span data-ttu-id="86dfd-640">Sequence</span><span class="sxs-lookup"><span data-stu-id="86dfd-640">Sequence</span></span> | <span data-ttu-id="86dfd-641">Тип</span><span class="sxs-lookup"><span data-stu-id="86dfd-641">Type</span></span>      | <span data-ttu-id="86dfd-642">Data</span><span class="sxs-lookup"><span data-stu-id="86dfd-642">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="86dfd-643">0</span><span class="sxs-lookup"><span data-stu-id="86dfd-643">0</span></span>        | <span data-ttu-id="86dfd-644">Узел Text</span><span class="sxs-lookup"><span data-stu-id="86dfd-644">Text node</span></span> | <span data-ttu-id="86dfd-645">First</span><span class="sxs-lookup"><span data-stu-id="86dfd-645">First</span></span>  |
| <span data-ttu-id="86dfd-646">1</span><span class="sxs-lookup"><span data-stu-id="86dfd-646">1</span></span>        | <span data-ttu-id="86dfd-647">Узел Text</span><span class="sxs-lookup"><span data-stu-id="86dfd-647">Text node</span></span> | <span data-ttu-id="86dfd-648">Second</span><span class="sxs-lookup"><span data-stu-id="86dfd-648">Second</span></span> |

<span data-ttu-id="86dfd-649">Этот результат идентичен предыдущему случаю, поэтому отрицательные проблемы не возникают.</span><span class="sxs-lookup"><span data-stu-id="86dfd-649">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="86dfd-650">во второй отрисовке `someFlag` `false`, а выходные данные:</span><span class="sxs-lookup"><span data-stu-id="86dfd-650">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="86dfd-651">Sequence</span><span class="sxs-lookup"><span data-stu-id="86dfd-651">Sequence</span></span> | <span data-ttu-id="86dfd-652">Тип</span><span class="sxs-lookup"><span data-stu-id="86dfd-652">Type</span></span>      | <span data-ttu-id="86dfd-653">Data</span><span class="sxs-lookup"><span data-stu-id="86dfd-653">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="86dfd-654">0</span><span class="sxs-lookup"><span data-stu-id="86dfd-654">0</span></span>        | <span data-ttu-id="86dfd-655">Узел Text</span><span class="sxs-lookup"><span data-stu-id="86dfd-655">Text node</span></span> | <span data-ttu-id="86dfd-656">Second</span><span class="sxs-lookup"><span data-stu-id="86dfd-656">Second</span></span> |

<span data-ttu-id="86dfd-657">На этот раз алгоритм diff видит, что были внесены *два* изменения, и алгоритм создает следующий сценарий редактирования:</span><span class="sxs-lookup"><span data-stu-id="86dfd-657">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="86dfd-658">Измените значение первого текстового узла на `Second`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-658">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="86dfd-659">Удалите второй текстовый узел.</span><span class="sxs-lookup"><span data-stu-id="86dfd-659">Remove the second text node.</span></span>

<span data-ttu-id="86dfd-660">При формировании порядковых номеров теряются все полезные сведения о том, где в исходном коде находятся `if/else` ветви и циклы.</span><span class="sxs-lookup"><span data-stu-id="86dfd-660">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="86dfd-661">Это приводит к **удвоению в два раза больше** , чем раньше.</span><span class="sxs-lookup"><span data-stu-id="86dfd-661">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="86dfd-662">Это тривиальный пример.</span><span class="sxs-lookup"><span data-stu-id="86dfd-662">This is a trivial example.</span></span> <span data-ttu-id="86dfd-663">В более реалистичных случаях со сложными и глубокими вложенными структурами, особенно с циклами, затраты на производительность более серьезны.</span><span class="sxs-lookup"><span data-stu-id="86dfd-663">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="86dfd-664">Вместо того, чтобы сразу определять, какие блоки или ветви цикла были вставлены или удалены, алгоритм diff должен рекурсивно пребираться в деревьях отрисовки и, как правило, создает гораздо более длительные операции редактирования, так как он сообщает о том, как старую и новую структуры связь друг с другом.</span><span class="sxs-lookup"><span data-stu-id="86dfd-664">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="86dfd-665">Руководство и выводы</span><span class="sxs-lookup"><span data-stu-id="86dfd-665">Guidance and conclusions</span></span>

* <span data-ttu-id="86dfd-666">Производительность приложения снижается, если порядковые номера создаются динамически.</span><span class="sxs-lookup"><span data-stu-id="86dfd-666">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="86dfd-667">Платформа не может автоматически создавать собственные порядковые номера во время выполнения, поскольку необходимая информация не существует, если она не захвачена во время компиляции.</span><span class="sxs-lookup"><span data-stu-id="86dfd-667">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="86dfd-668">Не записывайте длинные блоки `RenderTreeBuilder` логики, реализуемой вручную.</span><span class="sxs-lookup"><span data-stu-id="86dfd-668">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="86dfd-669">Предпочитать `.razor` файлы и позволяют компилятору обрабатывать порядковые номера.</span><span class="sxs-lookup"><span data-stu-id="86dfd-669">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="86dfd-670">Если не удается избежать ручного `RenderTreeBuilder` логики, разделите длинные блоки кода на более мелкие части, заключенные в `OpenRegion`/`CloseRegion` вызовы.</span><span class="sxs-lookup"><span data-stu-id="86dfd-670">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="86dfd-671">Каждый регион имеет собственное отдельное пространство порядковых номеров, поэтому вы можете перезапускаться от нуля (или любого другого произвольного числа) внутри каждого региона.</span><span class="sxs-lookup"><span data-stu-id="86dfd-671">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="86dfd-672">Если порядковые номера задаются жестко, то для алгоритма diff требуется, чтобы только порядковые номера увеличиваются в значении.</span><span class="sxs-lookup"><span data-stu-id="86dfd-672">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="86dfd-673">Начальное значение и зазоры несущественны.</span><span class="sxs-lookup"><span data-stu-id="86dfd-673">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="86dfd-674">Один из этих вариантов — использовать номер строки кода в качестве порядкового номера или начать с нуля и увеличить на единицу или сотни (или любой другой интервал).</span><span class="sxs-lookup"><span data-stu-id="86dfd-674">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* Blazor<span data-ttu-id="86dfd-675"> использует порядковые номера, в то время как другие платформы, использующие средства различения дерева, не используют их.</span><span class="sxs-lookup"><span data-stu-id="86dfd-675"> uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="86dfd-676">Сравнение выполняется гораздо быстрее, если используются порядковые номера, и Blazor имеет преимущество этапа компиляции, который автоматически обрабатывает порядковые номера для разработчиков, занимающихся созданием файлов *Razor* .</span><span class="sxs-lookup"><span data-stu-id="86dfd-676">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring *.razor* files.</span></span>

## <a name="localization"></a><span data-ttu-id="86dfd-677">Локализация</span><span class="sxs-lookup"><span data-stu-id="86dfd-677">Localization</span></span>

Blazor<span data-ttu-id="86dfd-678"> серверные приложения локализованы по [промежуточного слоя локализации](xref:fundamentals/localization#localization-middleware).</span><span class="sxs-lookup"><span data-stu-id="86dfd-678"> Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="86dfd-679">По промежуточного слоя выбирает соответствующие языки и региональные параметры для пользователей, запрашивающих ресурсы из приложения.</span><span class="sxs-lookup"><span data-stu-id="86dfd-679">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="86dfd-680">Язык и региональные параметры можно задать с помощью одного из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="86dfd-680">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="86dfd-681">Файлы cookie</span><span class="sxs-lookup"><span data-stu-id="86dfd-681">Cookies</span></span>](#cookies)
* [<span data-ttu-id="86dfd-682">Предоставление пользовательского интерфейса для выбора языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="86dfd-682">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="86dfd-683">Дополнительные сведения и примеры см. в разделе <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="86dfd-683">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="configure-the-linker-for-internationalization-opno-locblazor-webassembly"></a><span data-ttu-id="86dfd-684">Настройка компоновщика для интернационализации (Blazorная сборка)</span><span class="sxs-lookup"><span data-stu-id="86dfd-684">Configure the linker for internationalization (Blazor WebAssembly)</span></span>

<span data-ttu-id="86dfd-685">По умолчанию конфигурация компоновщика Blazorдля Blazor приложений веб – сборки удаляет сведения о международной локализации, за исключением явно запрошенных языковых стандартов.</span><span class="sxs-lookup"><span data-stu-id="86dfd-685">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="86dfd-686">Дополнительные сведения и рекомендации по управлению поведением компоновщика см. в разделе <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span><span class="sxs-lookup"><span data-stu-id="86dfd-686">For more information and guidance on controlling the linker's behavior, see <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="86dfd-687">Файлы cookie</span><span class="sxs-lookup"><span data-stu-id="86dfd-687">Cookies</span></span>

<span data-ttu-id="86dfd-688">Файл cookie культуры локализации может сохранять язык и региональные параметры пользователя.</span><span class="sxs-lookup"><span data-stu-id="86dfd-688">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="86dfd-689">Файл cookie создается методом `OnGet` страницы узла приложения (*pages/Host. cshtml. CS*).</span><span class="sxs-lookup"><span data-stu-id="86dfd-689">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="86dfd-690">По промежуточного слоя локализации считывает файл cookie при последующих запросах, чтобы задать язык и региональные параметры пользователя.</span><span class="sxs-lookup"><span data-stu-id="86dfd-690">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="86dfd-691">Использование файла cookie гарантирует, что соединение WebSocket сможет правильно распространить культуру.</span><span class="sxs-lookup"><span data-stu-id="86dfd-691">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="86dfd-692">Если схемы локализации основаны на пути URL-адреса или строке запроса, схема может не поддерживать работу с WebSockets, поэтому она не сохраняет культуру.</span><span class="sxs-lookup"><span data-stu-id="86dfd-692">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="86dfd-693">Поэтому рекомендуемым подходом является использование файла cookie языка и региональных параметров локализации.</span><span class="sxs-lookup"><span data-stu-id="86dfd-693">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="86dfd-694">Любой метод можно использовать для назначения языка и региональных параметров, если язык и региональные параметры сохраняются в файле cookie локализации.</span><span class="sxs-lookup"><span data-stu-id="86dfd-694">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="86dfd-695">Если у приложения уже есть установленная схема локализации для ASP.NET Core на стороне сервера, продолжайте использовать существующую инфраструктуру локализации приложения и задавайте файл cookie культуры локализации в схеме приложения.</span><span class="sxs-lookup"><span data-stu-id="86dfd-695">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="86dfd-696">В следующем примере показано, как задать текущий язык и региональные параметры в файле cookie, который может быть прочитан по промежуточного слоя локализации.</span><span class="sxs-lookup"><span data-stu-id="86dfd-696">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="86dfd-697">Создайте файл *pages/Host. cshtml. CS* со следующим содержимым в приложении Blazor Server:</span><span class="sxs-lookup"><span data-stu-id="86dfd-697">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

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

<span data-ttu-id="86dfd-698">Локализация обрабатывается в приложении:</span><span class="sxs-lookup"><span data-stu-id="86dfd-698">Localization is handled in the app:</span></span>

1. <span data-ttu-id="86dfd-699">Браузер отправляет в приложение исходный HTTP-запрос.</span><span class="sxs-lookup"><span data-stu-id="86dfd-699">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="86dfd-700">Язык и региональные параметры назначаются по промежуточного слоя локализации.</span><span class="sxs-lookup"><span data-stu-id="86dfd-700">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="86dfd-701">Метод `OnGet` в *_Host. cshtml. CS* сохраняет язык и региональные параметры в файле cookie как часть ответа.</span><span class="sxs-lookup"><span data-stu-id="86dfd-701">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="86dfd-702">Браузер открывает соединение WebSocket для создания интерактивного сеанса Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="86dfd-702">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="86dfd-703">По промежуточного слоя для локализации считывает файл cookie и назначает язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="86dfd-703">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="86dfd-704">Сеанс Blazor Server начинается с правильного языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="86dfd-704">The Blazor Server session begins with the correct culture.</span></span>

### <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="86dfd-705">Предоставление пользовательского интерфейса для выбора языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="86dfd-705">Provide UI to choose the culture</span></span>

<span data-ttu-id="86dfd-706">Для предоставления пользовательского интерфейса, позволяющего пользователю выбирать язык и региональные параметры, рекомендуется *подход на основе перенаправления* .</span><span class="sxs-lookup"><span data-stu-id="86dfd-706">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="86dfd-707">Процесс аналогичен тому, что происходит в веб-приложении, когда пользователь пытается получить доступ к защищенному ресурсу,&mdash;пользователь перенаправляется на страницу входа, а затем перенаправляется обратно к исходному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="86dfd-707">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="86dfd-708">Приложение сохраняет выбранный язык и региональные параметры пользователя с помощью перенаправления к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="86dfd-708">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="86dfd-709">Контроллер устанавливает выбранный язык и региональные параметры пользователя в файл cookie и перенаправляет пользователя обратно к исходному коду URI.</span><span class="sxs-lookup"><span data-stu-id="86dfd-709">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="86dfd-710">Установите конечную точку HTTP на сервере, чтобы задать выбранный язык и региональные параметры пользователя в файле cookie, и выполните перенаправление обратно в исходный URI:</span><span class="sxs-lookup"><span data-stu-id="86dfd-710">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

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
> <span data-ttu-id="86dfd-711">Для предотвращения атак с открытым перенаправлением используйте результат действия `LocalRedirect`.</span><span class="sxs-lookup"><span data-stu-id="86dfd-711">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="86dfd-712">Для получения дополнительной информации см. <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="86dfd-712">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="86dfd-713">В следующем компоненте показан пример выполнения начального перенаправления, когда пользователь выбирает язык и региональные параметры:</span><span class="sxs-lookup"><span data-stu-id="86dfd-713">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

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

### <a name="use-net-localization-scenarios-in-opno-locblazor-apps"></a><span data-ttu-id="86dfd-714">Использование сценариев локализации .NET в Blazor приложениях</span><span class="sxs-lookup"><span data-stu-id="86dfd-714">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="86dfd-715">В Blazor приложениях доступны следующие сценарии локализации и глобализации .NET:</span><span class="sxs-lookup"><span data-stu-id="86dfd-715">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="86dfd-716">. Система ресурсов NET</span><span class="sxs-lookup"><span data-stu-id="86dfd-716">.NET's resources system</span></span>
* <span data-ttu-id="86dfd-717">Форматирование чисел и дат, зависящих от языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="86dfd-717">Culture-specific number and date formatting</span></span>

<span data-ttu-id="86dfd-718">функции `@bind` Blazorвыполняют глобализацию на основе текущего языка и региональных параметров пользователя.</span><span class="sxs-lookup"><span data-stu-id="86dfd-718">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="86dfd-719">Дополнительные сведения см. в разделе [Привязка данных](#data-binding) .</span><span class="sxs-lookup"><span data-stu-id="86dfd-719">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="86dfd-720">В настоящее время поддерживается ограниченный набор сценариев локализации ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="86dfd-720">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="86dfd-721">`IStringLocalizer<>` *поддерживается* в Blazor приложениях.</span><span class="sxs-lookup"><span data-stu-id="86dfd-721">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="86dfd-722">Локализация `IHtmlLocalizer<>`, `IViewLocalizer<>`и аннотаций данных ASP.NET Core сценариев MVC и **не поддерживается** в Blazor приложениях.</span><span class="sxs-lookup"><span data-stu-id="86dfd-722">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="86dfd-723">Для получения дополнительной информации см. <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="86dfd-723">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="86dfd-724">Масштабируемые изображения векторной графики (SVG)</span><span class="sxs-lookup"><span data-stu-id="86dfd-724">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="86dfd-725">Поскольку Blazor отображает HTML-файлы, поддерживаемые браузером изображения, включая масштабируемые векторные графики (SVG *),* поддерживаются с помощью тега `<img>`:</span><span class="sxs-lookup"><span data-stu-id="86dfd-725">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="86dfd-726">Аналогичным образом SVG-изображения поддерживаются в правилах CSS файла стилей ( *. CSS*):</span><span class="sxs-lookup"><span data-stu-id="86dfd-726">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="86dfd-727">Однако встроенная разметка SVG не поддерживается во всех сценариях.</span><span class="sxs-lookup"><span data-stu-id="86dfd-727">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="86dfd-728">Если поместить тег `<svg>` непосредственно в файл компонента (*Razor*), то поддерживается базовая визуализация изображений, но многие расширенные сценарии пока не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="86dfd-728">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="86dfd-729">Например, `<use>` теги в настоящее время не учитываются, и `@bind` нельзя использовать с некоторыми тегами SVG.</span><span class="sxs-lookup"><span data-stu-id="86dfd-729">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="86dfd-730">В будущем выпуске мы планируем устранить эти ограничения.</span><span class="sxs-lookup"><span data-stu-id="86dfd-730">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="86dfd-731">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="86dfd-731">Additional resources</span></span>

* <span data-ttu-id="86dfd-732"><xref:security/blazor/server> &ndash; содержит рекомендации по созданию серверных приложений Blazor, которые должны конкурировать с нехваткой ресурсов.</span><span class="sxs-lookup"><span data-stu-id="86dfd-732"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
