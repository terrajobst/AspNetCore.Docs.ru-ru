---
title: Создание и использование компонентов ASP.NET Core Razor
author: guardrex
description: Узнайте, как создавать и использовать компоненты Razor, включая способы привязки к данным, обработки событий и управления жизненным циклом компонентов.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/28/2019
no-loc:
- Blazor
uid: blazor/components
ms.openlocfilehash: 87f21d84c17e5bbd1247bb955acee81384b890e7
ms.sourcegitcommit: 47d453f34b6fd0179119c572cb8be64c5365cbb6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/02/2020
ms.locfileid: "75597906"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="a9a7a-103">Создание и использование компонентов ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="a9a7a-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="a9a7a-104">[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="a9a7a-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="a9a7a-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a9a7a-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

Blazor<span data-ttu-id="a9a7a-106"> приложения создаются с помощью *компонентов*.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-106"> apps are built using *components*.</span></span> <span data-ttu-id="a9a7a-107">Компонент — это автономный фрагмент пользовательского интерфейса, например страница, диалоговое окно или форма.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="a9a7a-108">Компонент включает разметку HTML и логику обработки, необходимую для вставки данных или реагирования на события пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="a9a7a-109">Компоненты являются гибкими и легковесными.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="a9a7a-110">Они могут быть вложенными, повторно использованы и совместно использоваться проектами.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="a9a7a-111">Классы компонентов</span><span class="sxs-lookup"><span data-stu-id="a9a7a-111">Component classes</span></span>

<span data-ttu-id="a9a7a-112">Компоненты реализуются в файлах компонентов [Razor](xref:mvc/views/razor) ( *. Razor*) с помощью комбинации C# и HTML-разметки.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="a9a7a-113">Компонент в Blazor формально называется *компонентом Razor*.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="a9a7a-114">Имя компонента должно начинаться с символа верхнего регистра.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="a9a7a-115">Например, *микулкомпонент. Razor* является допустимым, а *микулкомпонент. Razor* является недопустимым.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="a9a7a-116">Пользовательский интерфейс для компонента определяется с помощью HTML.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="a9a7a-117">Логика динамического отображения (например выражения, циклы и условные выражения) добавляется с помощью встроенного синтаксиса C# под названием [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="a9a7a-118">При компиляции приложения логика разметки и C# отрисовки HTML преобразуется в класс компонента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="a9a7a-119">Имя созданного класса соответствует имени файла.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="a9a7a-120">Элементы класса компонента определяются в блоке `@code`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="a9a7a-121">В блоке `@code` состояние компонента (свойства, поля) указывается с помощью методов обработки событий или для определения другой логики компонента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="a9a7a-122">Допускается более одного блока `@code`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-122">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="a9a7a-123">В предыдущих предварительных версиях ASP.NET Core 3,0 блоки `@functions` использовались для той же цели, что и блоки `@code` в компонентах Razor.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-123">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="a9a7a-124">блоки `@functions` продолжают работать в компонентах Razor, но рекомендуется использовать блок `@code` в ASP.NET Core 3,0 Preview 6 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-124">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="a9a7a-125">Члены компонента можно использовать как часть логики визуализации компонента с помощью C# выражений, начинающихся с `@`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-125">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="a9a7a-126">Например, C# поле подготавливается к просмотру путем добавления `@` к имени поля.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-126">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="a9a7a-127">В следующем примере вычисляется и выводится следующее:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-127">The following example evaluates and renders:</span></span>

* <span data-ttu-id="a9a7a-128">`_headingFontStyle` значение свойства CSS для `font-style`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-128">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="a9a7a-129">`_headingText` содержимое элемента `<h1>`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-129">`_headingText` to the content of the `<h1>` element.</span></span>

```razor
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="a9a7a-130">После первоначального отображения компонента Компонент повторно создает дерево рендеринга в ответ на события.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-130">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> Blazor<span data-ttu-id="a9a7a-131"> затем сравнивает новое дерево отрисовки с предыдущим и применяет любые изменения в модель DOM (DOM) браузера.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-131"> then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="a9a7a-132">Компоненты являются обычными C# классами и могут быть помещены в любое место внутри проекта.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-132">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="a9a7a-133">Компоненты, создающие веб-страницы, обычно находятся в папке *страниц* .</span><span class="sxs-lookup"><span data-stu-id="a9a7a-133">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="a9a7a-134">Компоненты, не являющиеся страницами, часто помещаются в *общую* папку или пользовательскую папку, добавленную в проект.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-134">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="a9a7a-135">Чтобы использовать пользовательскую папку, добавьте пространство имен пользовательской папки либо в родительский компонент, либо в файл *_Imports. Razor* приложения.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-135">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="a9a7a-136">Например, следующее пространство имен делает компоненты в папке *Components* доступной при `WebApplication`корневого пространства имен приложения:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-136">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```razor
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="a9a7a-137">Интеграция компонентов в Razor Pages и приложения MVC</span><span class="sxs-lookup"><span data-stu-id="a9a7a-137">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="a9a7a-138">Используйте компоненты с существующими Razor Pages и приложениями MVC.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-138">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="a9a7a-139">Нет необходимости переписывать существующие страницы или представления для использования компонентов Razor.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-139">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="a9a7a-140">При отображении страницы или представления компоненты предварительно отображаются.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-140">When the page or view is rendered, components are prerendered at the same time.</span></span>

::: moniker range=">= aspnetcore-3.1"

<span data-ttu-id="a9a7a-141">Чтобы отобразить компонент из страницы или представления, используйте вспомогательную функцию тега `Component`:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-141">To render a component from a page or view, use the `Component` Tag Helper:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="a9a7a-142">Поддерживается передача параметров (например, `IncrementAmount` в предыдущем примере).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-142">Passing parameters (for example, `IncrementAmount` in the preceding example) is supported.</span></span>

<span data-ttu-id="a9a7a-143">`RenderMode` настраивает, является ли компонент:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-143">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="a9a7a-144">Предварительно отображается на странице.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-144">Is prerendered into the page.</span></span>
* <span data-ttu-id="a9a7a-145">Отображается как статический HTML на странице или включает необходимые сведения для начальной загрузки Blazor приложения из агента пользователя.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-145">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="a9a7a-146">Описание</span><span class="sxs-lookup"><span data-stu-id="a9a7a-146">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="a9a7a-147">Преобразует компонент в статический HTML и включает маркер для Blazor серверного приложения.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-147">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="a9a7a-148">При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-148">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="a9a7a-149">Отображает маркер для серверного приложения Blazor.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-149">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="a9a7a-150">Выходные данные компонента не включаются.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-150">Output from the component isn't included.</span></span> <span data-ttu-id="a9a7a-151">При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-151">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="a9a7a-152">Преобразует компонент в статический HTML.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-152">Renders the component into static HTML.</span></span> |

<span data-ttu-id="a9a7a-153">Хотя страницы и представления могут использовать компоненты, наоборот это не так.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-153">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="a9a7a-154">Компоненты не могут использовать сценарии представления и страницы, такие как частичные представления и разделы.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-154">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="a9a7a-155">Чтобы использовать логику из частичного представления в компоненте, разнесите логику частичного представления в компонент.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-155">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="a9a7a-156">Отрисовка компонентов сервера из статической HTML-страницы не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-156">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="a9a7a-157">Дополнительные сведения о подготовке компонентов к просмотру, состоянии компонентов и вспомогательной функции тега `Component` см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-157">For more information on how components are rendered, component state, and the `Component` Tag Helper, see <xref:blazor/hosting-models>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.1"

<span data-ttu-id="a9a7a-158">Чтобы отобразить компонент из страницы или представления, используйте вспомогательный метод HTML `RenderComponentAsync<TComponent>`:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-158">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
@(await Html.RenderComponentAsync<MyComponent>(RenderMode.ServerPrerendered))
```

<span data-ttu-id="a9a7a-159">`RenderMode` настраивает, является ли компонент:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-159">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="a9a7a-160">Предварительно отображается на странице.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-160">Is prerendered into the page.</span></span>
* <span data-ttu-id="a9a7a-161">Отображается как статический HTML на странице или включает необходимые сведения для начальной загрузки Blazor приложения из агента пользователя.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-161">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="a9a7a-162">Описание</span><span class="sxs-lookup"><span data-stu-id="a9a7a-162">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="a9a7a-163">Преобразует компонент в статический HTML и включает маркер для Blazor серверного приложения.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-163">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="a9a7a-164">При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-164">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="a9a7a-165">Параметры не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-165">Parameters aren't supported.</span></span> |
| `Server`            | <span data-ttu-id="a9a7a-166">Отображает маркер для серверного приложения Blazor.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-166">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="a9a7a-167">Выходные данные компонента не включаются.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-167">Output from the component isn't included.</span></span> <span data-ttu-id="a9a7a-168">При запуске агента пользователя этот маркер используется для начальной загрузки Blazor приложения.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-168">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="a9a7a-169">Параметры не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-169">Parameters aren't supported.</span></span> |
| `Static`            | <span data-ttu-id="a9a7a-170">Преобразует компонент в статический HTML.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-170">Renders the component into static HTML.</span></span> <span data-ttu-id="a9a7a-171">Поддерживаются параметры.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-171">Parameters are supported.</span></span> |

<span data-ttu-id="a9a7a-172">Хотя страницы и представления могут использовать компоненты, наоборот это не так.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-172">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="a9a7a-173">Компоненты не могут использовать сценарии представления и страницы, такие как частичные представления и разделы.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-173">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="a9a7a-174">Чтобы использовать логику из частичного представления в компоненте, разнесите логику частичного представления в компонент.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-174">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="a9a7a-175">Отрисовка компонентов сервера из статической HTML-страницы не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-175">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="a9a7a-176">Дополнительные сведения о том, как визуализируются компоненты, состояние компонента и вспомогательный модуль HTML `RenderComponentAsync`, см. в разделе <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-176">For more information on how components are rendered, component state, and the `RenderComponentAsync` HTML Helper, see <xref:blazor/hosting-models>.</span></span>

::: moniker-end

## <a name="use-components"></a><span data-ttu-id="a9a7a-177">Использование компонентов</span><span class="sxs-lookup"><span data-stu-id="a9a7a-177">Use components</span></span>

<span data-ttu-id="a9a7a-178">Компоненты могут включать другие компоненты, объявляя их с помощью синтаксиса HTML-элементов.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-178">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="a9a7a-179">Разметка для использования компонента выглядит как тег HTML с именем, соответствующем типу компонента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-179">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="a9a7a-180">Привязка атрибута чувствительна к регистру.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-180">Attribute binding is case sensitive.</span></span> <span data-ttu-id="a9a7a-181">Например, `@bind` является допустимым, а `@Bind` является недопустимым.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-181">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="a9a7a-182">Следующая разметка в *index. Razor* визуализирует `HeadingComponent` экземпляр:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-182">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

```razor
<HeadingComponent />
```

<span data-ttu-id="a9a7a-183">*Components/хеадингкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-183">*Components/HeadingComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

<span data-ttu-id="a9a7a-184">Если компонент содержит HTML-элемент с первой буквой в верхнем регистре, который не соответствует имени компонента, выдается предупреждение, указывающее, что элемент имеет непредвиденное имя.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-184">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="a9a7a-185">Добавление оператора `@using` для пространства имен компонента делает компонент доступным, что приводит к удалению предупреждения.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-185">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="a9a7a-186">Параметры компонентов</span><span class="sxs-lookup"><span data-stu-id="a9a7a-186">Component parameters</span></span>

<span data-ttu-id="a9a7a-187">Компоненты могут иметь *Параметры компонента*, которые определяются с помощью общедоступных свойств класса Component с атрибутом `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-187">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="a9a7a-188">Используйте атрибуты, чтобы указать аргументы для компонента в разметке.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-188">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="a9a7a-189">*Components/чилдкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-189">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="a9a7a-190">В следующем примере из примера приложения `ParentComponent` задает значение свойства `Title` `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-190">In the following example from the sample app, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="a9a7a-191">*Pages/паренткомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-191">*Pages/ParentComponent.razor*:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClick="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

...
```

## <a name="child-content"></a><span data-ttu-id="a9a7a-192">Дочернее содержимое</span><span class="sxs-lookup"><span data-stu-id="a9a7a-192">Child content</span></span>

<span data-ttu-id="a9a7a-193">Компоненты могут устанавливать содержимое другого компонента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-193">Components can set the content of another component.</span></span> <span data-ttu-id="a9a7a-194">Назначение компонента предоставляет содержимое между тегами, задающих принимающий компонент.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-194">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="a9a7a-195">В следующем примере `ChildComponent` имеет свойство `ChildContent`, представляющее `RenderFragment`, которое представляет сегмент пользовательского интерфейса для отрисовки.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-195">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="a9a7a-196">Значение `ChildContent` размещается в разметке компонента, где должно быть визуализировано содержимое.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-196">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="a9a7a-197">Значение `ChildContent` получено от родительского компонента и отображается внутри `panel-body`панели начальной загрузки.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-197">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="a9a7a-198">*Components/чилдкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-198">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="a9a7a-199">Свойству, получающему `RenderFragment`ное содержимое, необходимо присвоить имя `ChildContent` по соглашению.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-199">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="a9a7a-200">`ParentComponent` в примере приложения может предоставить содержимое для подготовки к просмотру `ChildComponent`, поместив содержимое в теги `<ChildComponent>`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-200">The `ParentComponent` in the sample app can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="a9a7a-201">*Pages/паренткомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-201">*Pages/ParentComponent.razor*:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClick="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

...
```

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="a9a7a-202">Атрибуты Сплаттинг и произвольные параметры</span><span class="sxs-lookup"><span data-stu-id="a9a7a-202">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="a9a7a-203">Компоненты могут записывать и отображать дополнительные атрибуты в дополнение к объявленным параметрам компонента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-203">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="a9a7a-204">Дополнительные атрибуты можно записать в словарь, а затем *сплаттед* на элемент при подготовке компонента к просмотру с помощью директивы [`@attributes`](xref:mvc/views/razor#attributes) Razor.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-204">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [`@attributes`](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="a9a7a-205">Этот сценарий полезен при определении компонента, который создает элемент разметки, поддерживающий разнообразные настройки.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-205">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="a9a7a-206">Например, может быть утомительно определять атрибуты отдельно для `<input>`, который поддерживает много параметров.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-206">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="a9a7a-207">В следующем примере первый элемент `<input>` (`id="useIndividualParams"`) использует параметры отдельных компонентов, а второй элемент `<input>` (`id="useAttributesDict"`) использует атрибут Сплаттинг:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-207">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

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

<span data-ttu-id="a9a7a-208">Тип параметра должен реализовывать `IEnumerable<KeyValuePair<string, object>>` со строковыми ключами.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-208">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="a9a7a-209">Использование `IReadOnlyDictionary<string, object>` также является параметром в этом сценарии.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-209">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="a9a7a-210">Отображаемые `<input>` элементы, использующие оба подхода, идентичны:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-210">The rendered `<input>` elements using both approaches is identical:</span></span>

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

<span data-ttu-id="a9a7a-211">Чтобы принять произвольные атрибуты, определите параметр компонента с помощью атрибута `[Parameter]` со свойством `CaptureUnmatchedValues`, для которого задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-211">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="a9a7a-212">Свойство `CaptureUnmatchedValues` в `[Parameter]` позволяет параметру сопоставлять все атрибуты, не соответствующие ни одному другому параметру.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-212">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="a9a7a-213">Компонент может определять только один параметр с `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-213">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="a9a7a-214">Тип свойства, используемый с `CaptureUnmatchedValues`, должен быть назначен из `Dictionary<string, object>` с строковыми ключами.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-214">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="a9a7a-215">в этом сценарии также используются параметры `IEnumerable<KeyValuePair<string, object>>` и `IReadOnlyDictionary<string, object>`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-215">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="a9a7a-216">Расположение `@attributes` относительно положения атрибутов элемента важно.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-216">The position of `@attributes` relative to the position of element attributes is important.</span></span> <span data-ttu-id="a9a7a-217">Когда `@attributes` сплаттед для элемента, атрибуты обрабатываются справа налево (последний — первый).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-217">When `@attributes` are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="a9a7a-218">Рассмотрим следующий пример компонента, использующего компонент `Child`:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-218">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="a9a7a-219">*Паренткомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-219">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="a9a7a-220">*Чилдкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-220">*ChildComponent.razor*:</span></span>

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="a9a7a-221">Атрибут `extra` `Child` компонента установлен справа от `@attributes`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-221">The `Child` component's `extra` attribute is set to the right of `@attributes`.</span></span> <span data-ttu-id="a9a7a-222">`<div>`, отображаемый компонентом `Parent`, содержит `extra="5"` при передаче через дополнительный атрибут, так как атрибуты обрабатываются справа налево (последний — первый):</span><span class="sxs-lookup"><span data-stu-id="a9a7a-222">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="a9a7a-223">В следующем примере порядок `extra` и `@attributes` отменяется в `<div>`е `Child` компонента:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-223">In the following example, the order of `extra` and `@attributes` is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="a9a7a-224">*Паренткомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-224">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="a9a7a-225">*Чилдкомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-225">*ChildComponent.razor*:</span></span>

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="a9a7a-226">Отображаемые `<div>` в компоненте `Parent` содержат `extra="10"` при передаче через дополнительный атрибут:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-226">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="data-binding"></a><span data-ttu-id="a9a7a-227">привязка данных,</span><span class="sxs-lookup"><span data-stu-id="a9a7a-227">Data binding</span></span>

<span data-ttu-id="a9a7a-228">Привязка данных как к компонентам, так и к элементам DOM осуществляется с помощью атрибута [`@bind`](xref:mvc/views/razor#bind) .</span><span class="sxs-lookup"><span data-stu-id="a9a7a-228">Data binding to both components and DOM elements is accomplished with the [`@bind`](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="a9a7a-229">В следующем примере свойство `CurrentValue` привязывается к значению текстового поля:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-229">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="a9a7a-230">Когда текстовое поле теряет фокус, значение свойства обновляется.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-230">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="a9a7a-231">Текстовое поле обновляется в пользовательском интерфейсе только при подготовке компонента к просмотру, а не в ответ на изменение значения свойства.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-231">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="a9a7a-232">Поскольку компоненты отображаются после выполнения кода обработчика событий, обновления свойств *обычно* отражаются в пользовательском интерфейсе сразу после запуска обработчика событий.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-232">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="a9a7a-233">Использование `@bind` со свойством `CurrentValue` (`<input @bind="CurrentValue" />`) в основном эквивалентно следующему:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-233">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="a9a7a-234">При подготовке к просмотру компонента `value` входного элемента берется из свойства `CurrentValue`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-234">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="a9a7a-235">Когда пользователь вводит данные в текстовое поле и изменяет фокус элемента, запускается событие `onchange`, а свойству `CurrentValue` присваивается измененное значение.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-235">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="a9a7a-236">На практике создание кода более сложно, так как `@bind` обрабатывает случаи, когда выполняются преобразования типов.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-236">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="a9a7a-237">В принципе `@bind` связывает текущее значение выражения с атрибутом `value` и обрабатывает изменения с помощью зарегистрированного обработчика.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-237">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="a9a7a-238">Помимо обработки `onchange`ных событий с помощью синтаксиса `@bind`, свойство или поле можно привязать с помощью других событий, указав атрибут [`@bind-value`](xref:mvc/views/razor#bind) с параметром `event` ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-238">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [`@bind-value`](xref:mvc/views/razor#bind) attribute with an `event` parameter ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="a9a7a-239">В следующем примере выполняется привязка свойства `CurrentValue` для события `oninput`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-239">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```razor
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="a9a7a-240">В отличие от `onchange`, которая срабатывает при потере фокуса элементом, `oninput` срабатывает при изменении значения текстового поля.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-240">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="a9a7a-241">**Неанализируемые значения**</span><span class="sxs-lookup"><span data-stu-id="a9a7a-241">**Unparsable values**</span></span>

<span data-ttu-id="a9a7a-242">Когда пользователь предоставляет неинтерпретируемое значение для элемента с привязкой к данным, неанализируемое значение автоматически возвращается к предыдущему значению при срабатывании события привязки.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-242">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="a9a7a-243">Рассмотрим следующий сценарий:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-243">Consider the following scenario:</span></span>

* <span data-ttu-id="a9a7a-244">Элемент `<input>` привязан к типу `int` с начальным значением `123`:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-244">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="a9a7a-245">Пользователь обновляет значение элемента для `123.45` на странице и изменяет фокус элемента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-245">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="a9a7a-246">В приведенном выше сценарии значение элемента возвращается к `123`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-246">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="a9a7a-247">Если значение `123.45` отклоняется в пользу исходного значения `123`, пользователь понимает, что их значение не принято.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-247">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="a9a7a-248">По умолчанию привязка применяется к событию `onchange` элемента (`@bind="{PROPERTY OR FIELD}"`).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-248">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="a9a7a-249">Для задания другого события используйте `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-249">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="a9a7a-250">Для события `oninput` (`@bind-value:event="oninput"`) переверсия происходит после любого нажатия клавиши, которая вводит неинтерпретируемое значение.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-250">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="a9a7a-251">При нацеливании на событие `oninput` с типом, привязанным к `int`, пользователю запрещено вводить `.` символ.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-251">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="a9a7a-252">`.`ный символ немедленно удаляется, поэтому пользователь получает немедленный отзыв о том, что разрешены только целые числа.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-252">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="a9a7a-253">Существуют сценарии, в которых возвращение значения в событие `oninput` не является идеальным, например, когда пользователю разрешено очищать неинтерпретируемое `<input>` значение.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-253">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="a9a7a-254">К альтернативам относятся:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-254">Alternatives include:</span></span>

* <span data-ttu-id="a9a7a-255">Не используйте событие `oninput`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-255">Don't use the `oninput` event.</span></span> <span data-ttu-id="a9a7a-256">Используйте событие `onchange` по умолчанию (`@bind="{PROPERTY OR FIELD}"`), где недопустимое значение не возвращается, пока элемент не теряет фокус.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-256">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="a9a7a-257">Выполните привязку к типу, допускающему значение null, например `int?` или `string`, и предоставьте настраиваемую логику для обработки недопустимых записей.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-257">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="a9a7a-258">Используйте [компонент проверки формы](xref:blazor/forms-validation), например `InputNumber` или `InputDate`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-258">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="a9a7a-259">Компоненты проверки форм имеют встроенную поддержку для управления недопустимыми входными данными.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-259">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="a9a7a-260">Компоненты проверки формы:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-260">Form validation components:</span></span>
  * <span data-ttu-id="a9a7a-261">Разрешите пользователю указать недопустимые входные данные и получить ошибки проверки для связанного `EditContext`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-261">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="a9a7a-262">Отображает ошибки проверки в пользовательском интерфейсе, не мешая пользователю вводить дополнительные данные из формы.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-262">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

<span data-ttu-id="a9a7a-263">**Глобализация**</span><span class="sxs-lookup"><span data-stu-id="a9a7a-263">**Globalization**</span></span>

<span data-ttu-id="a9a7a-264">`@bind` значения форматируются для вывода и анализируются с использованием правил текущего языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-264">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="a9a7a-265">Доступ к текущему языку и региональным параметрам можно получить из свойства <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-265">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="a9a7a-266">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) используется для следующих типов полей (`<input type="{TYPE}" />`):</span><span class="sxs-lookup"><span data-stu-id="a9a7a-266">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="a9a7a-267">Предыдущие типы полей:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-267">The preceding field types:</span></span>

* <span data-ttu-id="a9a7a-268">Отображаются с использованием соответствующих правил форматирования на основе браузера.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-268">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="a9a7a-269">Не может содержать текст в свободной форме.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-269">Can't contain free-form text.</span></span>
* <span data-ttu-id="a9a7a-270">Предоставление характеристик взаимодействия с пользователем в зависимости от реализации браузера.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-270">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="a9a7a-271">Следующие типы полей имеют особые требования к форматированию, которые в настоящее время не поддерживаются Blazor, так как они не поддерживаются всеми основными браузерами.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-271">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="a9a7a-272">`@bind` поддерживает параметр `@bind:culture`, чтобы предоставить <xref:System.Globalization.CultureInfo?displayProperty=fullName> для синтаксического анализа и форматирования значения.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-272">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="a9a7a-273">Указание языка и региональных параметров не рекомендуется при использовании типов полей `date` и `number`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-273">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="a9a7a-274">`date` и `number` имеют встроенную поддержку Blazor, которая предоставляет требуемый язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-274">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="a9a7a-275">Сведения о том, как задать язык и региональные параметры пользователя, см. в разделе [локализация](#localization).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-275">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="a9a7a-276">**Строки формата**</span><span class="sxs-lookup"><span data-stu-id="a9a7a-276">**Format strings**</span></span>

<span data-ttu-id="a9a7a-277">Привязка данных работает со строками формата <xref:System.DateTime> с помощью [`@bind:format`](xref:mvc/views/razor#bind).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-277">Data binding works with <xref:System.DateTime> format strings using [`@bind:format`](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="a9a7a-278">Другие выражения форматирования, такие как денежные или числовые форматы, в настоящее время недоступны.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-278">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="a9a7a-279">В приведенном выше коде тип поля `<input>` элемента (`type`) по умолчанию имеет значение `text`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-279">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="a9a7a-280">`@bind:format` поддерживается для привязки следующих типов .NET:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-280">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="a9a7a-281"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="a9a7a-281"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="a9a7a-282"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="a9a7a-282"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="a9a7a-283">Атрибут `@bind:format` задает формат даты, применяемый к `value` элемента `<input>`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-283">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="a9a7a-284">Формат также используется для анализа значения при возникновении `onchange` события.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-284">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="a9a7a-285">Указание формата для типа поля `date` не рекомендуется, так как Blazor имеет встроенную поддержку для форматирования дат.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-285">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span> <span data-ttu-id="a9a7a-286">Несмотря на рекомендацию, используйте формат даты `yyyy-MM-dd` для правильной работы привязки только в том случае, если указан формат с типом поля `date`:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-286">In spite of the recommendation, only use the `yyyy-MM-dd` date format for binding to work correctly if a format is supplied with the `date` field type:</span></span>

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

<span data-ttu-id="a9a7a-287">**Параметры компонента**</span><span class="sxs-lookup"><span data-stu-id="a9a7a-287">**Component parameters**</span></span>

<span data-ttu-id="a9a7a-288">Привязка распознает параметры компонента, где `@bind-{property}` может привязать значение свойства к разным компонентам.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-288">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="a9a7a-289">Следующий дочерний компонент (`ChildComponent`) имеет параметр компонента `Year` и обратный вызов `YearChanged`:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-289">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

```razor
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="a9a7a-290">`EventCallback<T>` описывается в разделе [вложенный EventCallback](#eventcallback) .</span><span class="sxs-lookup"><span data-stu-id="a9a7a-290">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="a9a7a-291">Следующий родительский компонент использует `ChildComponent` и привязывает параметр `ParentYear` из родительского к параметру `Year` дочернего компонента:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-291">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

```razor
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

<span data-ttu-id="a9a7a-292">При загрузке `ParentComponent` создается следующая разметка:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-292">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="a9a7a-293">Если значение свойства `ParentYear` изменяется путем нажатия кнопки в `ParentComponent`, свойство `Year` `ChildComponent` обновляется.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-293">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="a9a7a-294">Новое значение `Year` отображается в пользовательском интерфейсе при перевизуализации `ParentComponent`:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-294">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="a9a7a-295">Параметр `Year` является связываемым, так как имеет сопутствующее `YearChanged` событие, соответствующее типу параметра `Year`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-295">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="a9a7a-296">По соглашению `<ChildComponent @bind-Year="ParentYear" />`, по сути, эквивалентен написанию:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-296">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="a9a7a-297">Как правило, свойство может быть привязано к соответствующему обработчику событий с помощью атрибута `@bind-property:event`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-297">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="a9a7a-298">Например, свойство `MyProp` можно привязать к `MyEventHandler`у с помощью следующих двух атрибутов:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-298">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="a9a7a-299">Обработка событий</span><span class="sxs-lookup"><span data-stu-id="a9a7a-299">Event handling</span></span>

<span data-ttu-id="a9a7a-300">Компоненты Razor предоставляют функции обработки событий.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-300">Razor components provide event handling features.</span></span> <span data-ttu-id="a9a7a-301">Для атрибута HTML-элемента с именем `on{EVENT}` (например, `onclick` и `onsubmit`) со значением, вводимым с помощью делегата, компоненты Razor рассматривают значение атрибута как обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-301">For an HTML element attribute named `on{EVENT}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="a9a7a-302">Имя атрибута всегда имеет формат [`@on{EVENT}`](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-302">The attribute's name is always formatted [`@on{EVENT}`](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="a9a7a-303">Следующий код вызывает метод `UpdateHeading`, когда кнопка выбрана в пользовательском интерфейсе:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-303">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```razor
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

<span data-ttu-id="a9a7a-304">Следующий код вызывает метод `CheckChanged` при изменении флажка в пользовательском интерфейсе:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-304">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="a9a7a-305">Обработчики событий также могут быть асинхронными и возвращать <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-305">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="a9a7a-306">Нет необходимости вручную вызывать [статехасчанжед](xref:blazor/lifecycle#state-changes).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-306">There's no need to manually call [StateHasChanged](xref:blazor/lifecycle#state-changes).</span></span> <span data-ttu-id="a9a7a-307">Исключения регистрируются при их возникновении.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-307">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="a9a7a-308">В следующем примере `UpdateHeading` вызывается асинхронно при выборе кнопки:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-308">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```razor
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

### <a name="event-argument-types"></a><span data-ttu-id="a9a7a-309">Типы аргументов событий</span><span class="sxs-lookup"><span data-stu-id="a9a7a-309">Event argument types</span></span>

<span data-ttu-id="a9a7a-310">Для некоторых событий разрешены типы аргументов событий.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-310">For some events, event argument types are permitted.</span></span> <span data-ttu-id="a9a7a-311">Если доступ к одному из этих типов событий не нужен, в вызове метода это не требуется.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-311">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="a9a7a-312">Поддерживаемые `EventArgs` приведены в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-312">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="a9a7a-313">Event</span><span class="sxs-lookup"><span data-stu-id="a9a7a-313">Event</span></span>            | <span data-ttu-id="a9a7a-314">Класс</span><span class="sxs-lookup"><span data-stu-id="a9a7a-314">Class</span></span>                | <span data-ttu-id="a9a7a-315">События DOM и заметки</span><span class="sxs-lookup"><span data-stu-id="a9a7a-315">DOM events and notes</span></span> |
| ---------------- | -------------------- | -------------------- |
| <span data-ttu-id="a9a7a-316">буфер обмена</span><span class="sxs-lookup"><span data-stu-id="a9a7a-316">Clipboard</span></span>        | `ClipboardEventArgs` | <span data-ttu-id="a9a7a-317">`oncut`значение `oncopy`значение `onpaste`</span><span class="sxs-lookup"><span data-stu-id="a9a7a-317">`oncut`, `oncopy`, `onpaste`</span></span> |
| <span data-ttu-id="a9a7a-318">Перетаскивание</span><span class="sxs-lookup"><span data-stu-id="a9a7a-318">Drag</span></span>             | `DragEventArgs`      | <span data-ttu-id="a9a7a-319">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span><span class="sxs-lookup"><span data-stu-id="a9a7a-319">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span></span><br><br><span data-ttu-id="a9a7a-320">`DataTransfer` и `DataTransferItem` содержать перетаскиваемые данные элемента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-320">`DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="a9a7a-321">Ошибка .</span><span class="sxs-lookup"><span data-stu-id="a9a7a-321">Error</span></span>            | `ErrorEventArgs`     | `onerror` |
| <span data-ttu-id="a9a7a-322">Event</span><span class="sxs-lookup"><span data-stu-id="a9a7a-322">Event</span></span>            | `EventArgs`          | <span data-ttu-id="a9a7a-323">*Общие*</span><span class="sxs-lookup"><span data-stu-id="a9a7a-323">*General*</span></span><br><span data-ttu-id="a9a7a-324">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span><span class="sxs-lookup"><span data-stu-id="a9a7a-324">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span></span><br><br><span data-ttu-id="a9a7a-325">*Буфер обмена*</span><span class="sxs-lookup"><span data-stu-id="a9a7a-325">*Clipboard*</span></span><br><span data-ttu-id="a9a7a-326">`onbeforecut`значение `onbeforecopy`значение `onbeforepaste`</span><span class="sxs-lookup"><span data-stu-id="a9a7a-326">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span></span><br><br><span data-ttu-id="a9a7a-327">*Ввод*</span><span class="sxs-lookup"><span data-stu-id="a9a7a-327">*Input*</span></span><br><span data-ttu-id="a9a7a-328">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span><span class="sxs-lookup"><span data-stu-id="a9a7a-328">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span></span><br><br><span data-ttu-id="a9a7a-329">*Мультимедиа*</span><span class="sxs-lookup"><span data-stu-id="a9a7a-329">*Media*</span></span><br><span data-ttu-id="a9a7a-330">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span><span class="sxs-lookup"><span data-stu-id="a9a7a-330">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span></span> |
| <span data-ttu-id="a9a7a-331">Фокус</span><span class="sxs-lookup"><span data-stu-id="a9a7a-331">Focus</span></span>            | `FocusEventArgs`     | <span data-ttu-id="a9a7a-332">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span><span class="sxs-lookup"><span data-stu-id="a9a7a-332">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span></span><br><br><span data-ttu-id="a9a7a-333">Не включает поддержку для `relatedTarget`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-333">Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="a9a7a-334">Input</span><span class="sxs-lookup"><span data-stu-id="a9a7a-334">Input</span></span>            | `ChangeEventArgs`    | <span data-ttu-id="a9a7a-335">`onchange`, `oninput`</span><span class="sxs-lookup"><span data-stu-id="a9a7a-335">`onchange`, `oninput`</span></span> |
| <span data-ttu-id="a9a7a-336">Клавиатура</span><span class="sxs-lookup"><span data-stu-id="a9a7a-336">Keyboard</span></span>         | `KeyboardEventArgs`  | <span data-ttu-id="a9a7a-337">`onkeydown`значение `onkeypress`значение `onkeyup`</span><span class="sxs-lookup"><span data-stu-id="a9a7a-337">`onkeydown`, `onkeypress`, `onkeyup`</span></span> |
| <span data-ttu-id="a9a7a-338">Мышь</span><span class="sxs-lookup"><span data-stu-id="a9a7a-338">Mouse</span></span>            | `MouseEventArgs`     | <span data-ttu-id="a9a7a-339">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span><span class="sxs-lookup"><span data-stu-id="a9a7a-339">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span></span> |
| <span data-ttu-id="a9a7a-340">Указатель мыши</span><span class="sxs-lookup"><span data-stu-id="a9a7a-340">Mouse pointer</span></span>    | `PointerEventArgs`   | <span data-ttu-id="a9a7a-341">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span><span class="sxs-lookup"><span data-stu-id="a9a7a-341">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span></span> |
| <span data-ttu-id="a9a7a-342">Колесико мыши</span><span class="sxs-lookup"><span data-stu-id="a9a7a-342">Mouse wheel</span></span>      | `WheelEventArgs`     | <span data-ttu-id="a9a7a-343">`onwheel`, `onmousewheel`</span><span class="sxs-lookup"><span data-stu-id="a9a7a-343">`onwheel`, `onmousewheel`</span></span> |
| <span data-ttu-id="a9a7a-344">Ход выполнения</span><span class="sxs-lookup"><span data-stu-id="a9a7a-344">Progress</span></span>         | `ProgressEventArgs`  | <span data-ttu-id="a9a7a-345">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span><span class="sxs-lookup"><span data-stu-id="a9a7a-345">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span></span> |
| <span data-ttu-id="a9a7a-346">Сенсорные технологии</span><span class="sxs-lookup"><span data-stu-id="a9a7a-346">Touch</span></span>            | `TouchEventArgs`     | <span data-ttu-id="a9a7a-347">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span><span class="sxs-lookup"><span data-stu-id="a9a7a-347">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span></span><br><br><span data-ttu-id="a9a7a-348">`TouchPoint` представляет одну точку контакта на устройстве с сенсорным вводом.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-348">`TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="a9a7a-349">Сведения о свойствах и поведении событий, описанных в приведенной выше таблице, см. в разделе [классы EventArgs в источнике ссылки (ветвь ASPNET/AspNetCore Release/3.0)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-349">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source (aspnet/AspNetCore release/3.0 branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="a9a7a-350">Лямбда-выражения</span><span class="sxs-lookup"><span data-stu-id="a9a7a-350">Lambda expressions</span></span>

<span data-ttu-id="a9a7a-351">Лямбда-выражения также могут использоваться:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-351">Lambda expressions can also be used:</span></span>

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="a9a7a-352">Часто бывает удобно закрывать дополнительные значения, например при переборе набора элементов.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-352">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="a9a7a-353">В следующем примере создаются три кнопки, каждый из которых вызывает `UpdateHeading` передачи аргумента события (`MouseEventArgs`) и номера кнопки (`buttonNumber`) при выборе в пользовательском интерфейсе:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-353">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```razor
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
> <span data-ttu-id="a9a7a-354">**Не** используйте переменную цикла (`i`) в цикле `for` непосредственно в лямбда-выражении.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-354">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="a9a7a-355">В противном случае одна и та же переменная используется во всех лямбда-выражениях, в результате чего значение `i`будет одинаковым во всех лямбдаах.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-355">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="a9a7a-356">Всегда запишите свое значение в локальную переменную (`buttonNumber` в предыдущем примере), а затем используйте ее.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-356">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="a9a7a-357">Вложенный EventCallback</span><span class="sxs-lookup"><span data-stu-id="a9a7a-357">EventCallback</span></span>

<span data-ttu-id="a9a7a-358">Распространенным сценарием с вложенными компонентами является желание запускать метод родительского компонента при возникновении события дочернего компонента&mdash;например, когда в дочернем элементе возникает событие `onclick`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-358">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="a9a7a-359">Чтобы обеспечить доступ к событиям по компонентам, используйте `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-359">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="a9a7a-360">Родительский компонент может назначить метод обратного вызова `EventCallback`у дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-360">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="a9a7a-361">`ChildComponent` в примере приложения (*Components/чилдкомпонент. Razor*) демонстрирует настройку обработчика `onclick`а кнопки на получение делегата `EventCallback` из `ParentComponent`в примере.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-361">The `ChildComponent` in the sample app (*Components/ChildComponent.razor*) demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="a9a7a-362">`EventCallback` вводится `MouseEventArgs`, который подходит для события `onclick` на периферийном устройстве.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-362">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="a9a7a-363">`ParentComponent` устанавливает `EventCallback<T>` дочернего элемента в его метод `ShowMessage`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-363">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method.</span></span>

<span data-ttu-id="a9a7a-364">*Pages/паренткомпонент. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-364">*Pages/ParentComponent.razor*:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClick="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

<p><b>@messageText</b></p>

@code {
    private string messageText;

    private void ShowMessage(MouseEventArgs e)
    {
        messageText = $"Blaze a new trail with Blazor! ({e.ScreenX}, {e.ScreenY})";
    }
}
```

<span data-ttu-id="a9a7a-365">При выборе кнопки в `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-365">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="a9a7a-366">Вызывается метод `ShowMessage` `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-366">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="a9a7a-367">`messageText` обновляется и отображается в `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-367">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="a9a7a-368">Вызов [статехасчанжед](xref:blazor/lifecycle#state-changes) не требуется в методе обратного вызова (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-368">A call to [StateHasChanged](xref:blazor/lifecycle#state-changes) isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="a9a7a-369">`StateHasChanged` вызывается автоматически для реотрисовки `ParentComponent`, так же как и дочерние события. компонент активируется в обработчиках событий, которые выполняются внутри дочернего элемента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-369">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="a9a7a-370">`EventCallback` и `EventCallback<T>` разрешается выполнять асинхронные делегаты.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-370">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="a9a7a-371">`EventCallback<T>` является строго типизированным и требует определенного типа аргумента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-371">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="a9a7a-372">`EventCallback` слабо типизирован и допускает любой тип аргумента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-372">`EventCallback` is weakly typed and allows any argument type.</span></span>

```razor
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="a9a7a-373">Вызов `EventCallback` или `EventCallback<T>` с `InvokeAsync` и ожидание <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-373">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="a9a7a-374">Используйте `EventCallback` и `EventCallback<T>` для обработки событий и параметров компонента привязки.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-374">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="a9a7a-375">Предпочитать строго типизированный `EventCallback<T>` поверх `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-375">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="a9a7a-376">`EventCallback<T>` предоставляет более лучшую реакцию на ошибки для пользователей компонента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-376">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="a9a7a-377">Как и в случае с другими обработчиками событий пользовательского интерфейса, указание параметра события является необязательным.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-377">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="a9a7a-378">Используйте `EventCallback`, если не передается значение обратного вызова.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-378">Use `EventCallback` when there's no value passed to the callback.</span></span>

::: moniker range=">= aspnetcore-3.1"

### <a name="prevent-default-actions"></a><span data-ttu-id="a9a7a-379">Запретить действия по умолчанию</span><span class="sxs-lookup"><span data-stu-id="a9a7a-379">Prevent default actions</span></span>

<span data-ttu-id="a9a7a-380">Используйте атрибут директивы [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) , чтобы предотвратить действие по умолчанию для события.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-380">Use the [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) directive attribute to prevent the default action for an event.</span></span>

<span data-ttu-id="a9a7a-381">Если ключ выбран на устройстве ввода, а элемент находится в текстовом поле, то в браузере обычно отображается символ ключа в текстовом поле.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-381">When a key is selected on an input device and the element focus is on a text box, a browser normally displays the key's character in the text box.</span></span> <span data-ttu-id="a9a7a-382">В следующем примере поведение по умолчанию запрещено путем указания атрибута директивы `@onkeypress:preventDefault`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-382">In the following example, the default behavior is prevented by specifying the `@onkeypress:preventDefault` directive attribute.</span></span> <span data-ttu-id="a9a7a-383">Счетчик увеличивается, а ключ **+** не записывается в значение элемента `<input>`:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-383">The counter increments, and the **+** key isn't captured into the `<input>` element's value:</span></span>

```razor
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

<span data-ttu-id="a9a7a-384">Указание `@on{EVENT}:preventDefault` атрибута без значения эквивалентно `@on{EVENT}:preventDefault="true"`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-384">Specifying the `@on{EVENT}:preventDefault` attribute without a value is equivalent to `@on{EVENT}:preventDefault="true"`.</span></span>

<span data-ttu-id="a9a7a-385">Значение атрибута также может быть выражением.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-385">The value of the attribute can also be an expression.</span></span> <span data-ttu-id="a9a7a-386">В следующем примере `_shouldPreventDefault` является полем `bool`, для которого задано значение `true` или `false`:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-386">In the following example, `_shouldPreventDefault` is a `bool` field set to either `true` or `false`:</span></span>

```razor
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

<span data-ttu-id="a9a7a-387">Для предотвращения действия по умолчанию не требуется обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-387">An event handler isn't required to prevent the default action.</span></span> <span data-ttu-id="a9a7a-388">Обработчик событий и предотвращение сценариев действий по умолчанию можно использовать независимо друг от друга.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-388">The event handler and prevent default action scenarios can be used independently.</span></span>

### <a name="stop-event-propagation"></a><span data-ttu-id="a9a7a-389">Отключить распространение событий</span><span class="sxs-lookup"><span data-stu-id="a9a7a-389">Stop event propagation</span></span>

<span data-ttu-id="a9a7a-390">Используйте атрибут директивы [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) для отмены распространения событий.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-390">Use the [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) directive attribute to stop event propagation.</span></span>

<span data-ttu-id="a9a7a-391">В следующем примере при установке флажка события Click из второго дочернего `<div>` не распространяются на родительский `<div>`:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-391">In the following example, selecting the check box prevents click events from the second child `<div>` from propagating to the parent `<div>`:</span></span>

```razor
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

## <a name="chained-bind"></a><span data-ttu-id="a9a7a-392">Привязка к цепочке</span><span class="sxs-lookup"><span data-stu-id="a9a7a-392">Chained bind</span></span>

<span data-ttu-id="a9a7a-393">Распространенным сценарием является привязка привязанного к данным параметра к элементу страницы в выходных данных компонента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-393">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="a9a7a-394">Этот сценарий называется *связанной* привязкой, так как несколько уровней привязки происходят одновременно.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-394">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="a9a7a-395">Связанную с цепочкой привязку нельзя реализовать с помощью синтаксиса `@bind` в элементе страницы.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-395">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="a9a7a-396">Обработчик событий и значение должны быть указаны отдельно.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-396">The event handler and value must be specified separately.</span></span> <span data-ttu-id="a9a7a-397">Однако родительский компонент может использовать `@bind` синтаксис с параметром компонента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-397">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="a9a7a-398">Следующий `PasswordField` компонент (*пассвордфиелд. Razor*):</span><span class="sxs-lookup"><span data-stu-id="a9a7a-398">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="a9a7a-399">Задает значение `<input>` элемента для свойства `Password`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-399">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="a9a7a-400">Предоставляет изменения свойства `Password` в родительский компонент с помощью [вложенный EventCallback](#eventcallback).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-400">Exposes changes of the `Password` property to a parent component with an [EventCallback](#eventcallback).</span></span>

```razor
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

<span data-ttu-id="a9a7a-401">Компонент `PasswordField` используется в другом компоненте:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-401">The `PasswordField` component is used in another component:</span></span>

```razor
<PasswordField @bind-Password="password" />

@code {
    private string password;
}
```

<span data-ttu-id="a9a7a-402">Для выполнения проверок или перехвата ошибок в пароле в предыдущем примере:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-402">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="a9a7a-403">Создайте резервное поле для `Password` (`password` в следующем примере кода).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-403">Create a backing field for `Password` (`password` in the following example code).</span></span>
* <span data-ttu-id="a9a7a-404">Выполните проверки или ошибки ловушек в методе задания `Password`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-404">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="a9a7a-405">В следующем примере представлена немедленная реакция пользователя, если в значении пароля используется пробел:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-405">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

```razor
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

## <a name="capture-references-to-components"></a><span data-ttu-id="a9a7a-406">Запись ссылок на компоненты</span><span class="sxs-lookup"><span data-stu-id="a9a7a-406">Capture references to components</span></span>

<span data-ttu-id="a9a7a-407">Ссылки на компоненты предоставляют способ ссылки на экземпляр компонента, чтобы можно было выполнять команды для этого экземпляра, например `Show` или `Reset`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-407">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="a9a7a-408">Чтобы записать ссылку на компонент, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-408">To capture a component reference:</span></span>

* <span data-ttu-id="a9a7a-409">Добавьте атрибут [`@ref`](xref:mvc/views/razor#ref) в дочерний компонент.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-409">Add an [`@ref`](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="a9a7a-410">Определите поле с тем же типом, что и у дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-410">Define a field with the same type as the child component.</span></span>

```razor
<MyLoginDialog @ref="loginDialog" ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="a9a7a-411">При подготовке компонента к просмотру `loginDialog` поле заполняется экземпляром `MyLoginDialog` дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-411">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="a9a7a-412">Затем можно вызывать методы .NET в экземпляре компонента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-412">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a9a7a-413">`loginDialog` переменная заполняется только после подготовки компонента, а ее выходные данные включают элемент `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-413">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="a9a7a-414">До этого момента нет никаких ссылок на.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-414">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="a9a7a-415">Чтобы управлять ссылками на компоненты после завершения подготовки компонента к просмотру, используйте [методы онафтеррендерасинк или онафтеррендер](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-415">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="a9a7a-416">При захвате ссылок на компоненты используется аналогичный синтаксис для [записи ссылок на элементы](xref:blazor/javascript-interop#capture-references-to-elements), но это не функция [взаимодействия JavaScript](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="a9a7a-416">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="a9a7a-417">Ссылки на компоненты не передаются в код JavaScript&mdash;они используются только в коде .NET.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-417">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="a9a7a-418">**Не** используйте ссылки на компоненты для изменения состояния дочерних компонентов.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-418">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="a9a7a-419">Вместо этого используйте обычные декларативные параметры для передачи данных дочерним компонентам.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-419">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="a9a7a-420">Использование обычных декларативных параметров приводит к тому, что дочерние компоненты автоматически визуализируются в нужное время.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-420">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="a9a7a-421">Вызывать методы компонента извне для обновления состояния</span><span class="sxs-lookup"><span data-stu-id="a9a7a-421">Invoke component methods externally to update state</span></span>

Blazor<span data-ttu-id="a9a7a-422"> использует `SynchronizationContext` для принудительного применения одного логического потока выполнения.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-422"> uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="a9a7a-423">В этом `SynchronizationContext`выполняются [методы жизненного цикла](xref:blazor/lifecycle) компонента и любые обратные вызовы событий, вызванные Blazor.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-423">A component's [lifecycle methods](xref:blazor/lifecycle) and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="a9a7a-424">В случае, если компонент должен быть обновлен на основе внешнего события, такого как таймер или другие уведомления, используйте метод `InvokeAsync`, который будет отправляться в `SynchronizationContext`Blazor.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-424">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="a9a7a-425">Например, рассмотрим *службу уведомления* , которая может уведомлять любой компонент, выполняющий прослушивание, в обновленном состоянии:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-425">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

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

<span data-ttu-id="a9a7a-426">Использование `NotifierService` для обновления компонента:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-426">Usage of the `NotifierService` to update a component:</span></span>

```razor
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

<span data-ttu-id="a9a7a-427">В предыдущем примере `NotifierService` вызывает метод `OnNotify` компонента за пределами Blazor`SynchronizationContext`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-427">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="a9a7a-428">`InvokeAsync` используется для переключения на правильный контекст и постановка в очередь визуализации.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-428">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="a9a7a-429">Использование ключа \@для управления сохранением элементов и компонентов</span><span class="sxs-lookup"><span data-stu-id="a9a7a-429">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="a9a7a-430">При последующем изменении списка элементов или компонентов, а также элементов или компонентов алгоритм сравнения Blazorдолжен решить, какие из предыдущих элементов или компонентов могут быть сохранены и как объекты модели должны сопоставляться с ними.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-430">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="a9a7a-431">Обычно этот процесс выполняется автоматически и его можно игнорировать, но в некоторых случаях может потребоваться управление процессом.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-431">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="a9a7a-432">Рассмотрим следующий пример.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-432">Consider the following example:</span></span>

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

<span data-ttu-id="a9a7a-433">Содержимое коллекции `People` может изменяться при вставке, удалении или повторном упорядочении записей.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-433">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="a9a7a-434">Когда компонент перерисовывается, компонент `<DetailsEditor>` может измениться на получение различных значений параметров `Details`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-434">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="a9a7a-435">Это может привести к более сложной визуализации, чем ожидалось.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-435">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="a9a7a-436">В некоторых случаях перерисовка может привести к появлению видимых различий в поведении, таких как потеря фокуса элементов.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-436">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="a9a7a-437">Процесс сопоставления можно контролировать с помощью атрибута директивы `@key`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-437">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="a9a7a-438">`@key` заставляет алгоритм сравнения гарантировать сохранение элементов или компонентов на основе значения ключа:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-438">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

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

<span data-ttu-id="a9a7a-439">При изменении коллекции `People` алгоритм сравнения удерживает связи между экземплярами `<DetailsEditor>` и экземплярами `person`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-439">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="a9a7a-440">Если `Person` удаляется из списка `People`, то из пользовательского интерфейса удаляется только соответствующий экземпляр `<DetailsEditor>`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-440">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="a9a7a-441">Другие экземпляры остаются без изменений.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-441">Other instances are left unchanged.</span></span>
* <span data-ttu-id="a9a7a-442">Если в какой-либо позиции в списке вставляется `Person`, то в соответствующей позиции вставляется один новый экземпляр `<DetailsEditor>`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-442">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="a9a7a-443">Другие экземпляры остаются без изменений.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-443">Other instances are left unchanged.</span></span>
* <span data-ttu-id="a9a7a-444">При повторном упорядочении записей `Person` соответствующие экземпляры `<DetailsEditor>` сохраняются и переупорядочиваются в пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-444">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="a9a7a-445">В некоторых сценариях использование `@key` уменьшает сложность повторного отображения и позволяет избежать потенциальных проблем с элементами, изменяющимися с отслеживанием состояния DOM, например положением фокуса.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-445">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a9a7a-446">Ключи являются локальными для каждого элемента или компонента контейнера.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-446">Keys are local to each container element or component.</span></span> <span data-ttu-id="a9a7a-447">Ключи не сравниваются глобально по всему документу.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-447">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="a9a7a-448">Когда следует использовать ключ \@</span><span class="sxs-lookup"><span data-stu-id="a9a7a-448">When to use \@key</span></span>

<span data-ttu-id="a9a7a-449">Как правило, имеет смысл использовать `@key` всякий раз при подготовке списка (например, в блоке `@foreach`) и при наличии подходящего значения для определения `@key`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-449">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="a9a7a-450">Можно также использовать `@key`, чтобы запретить Blazor сохранение поддерева элемента или компонента при изменении объекта.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-450">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="a9a7a-451">Если `@currentPerson` меняется, директива атрибута `@key` заставляет Blazor отбросить все `<div>` и его потомков и перестроить поддерево в пользовательском интерфейсе с помощью новых элементов и компонентов.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-451">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="a9a7a-452">Это может быть полезно, если необходимо гарантировать, что при `@currentPerson` изменений состояние пользовательского интерфейса не сохраняется.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-452">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="a9a7a-453">Когда не следует использовать ключ \@</span><span class="sxs-lookup"><span data-stu-id="a9a7a-453">When not to use \@key</span></span>

<span data-ttu-id="a9a7a-454">При использовании сравнения с `@key`ми возникают затраты на производительность.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-454">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="a9a7a-455">Затраты на производительность не слишком велики, но указываются только `@key` если управление правилами сохранения элементов или компонентов выгодно для приложения.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-455">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="a9a7a-456">Даже если `@key` не используется, Blazor сохраняет как можно больше экземпляров дочернего элемента и компонента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-456">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="a9a7a-457">Единственным преимуществом использования `@key` является управление *тем, как* экземпляры модели сопоставляются с сохраненными экземплярами компонента, а не алгоритмом сравнения, который выбирает сопоставление.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-457">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="a9a7a-458">Какие значения следует использовать для ключа \@</span><span class="sxs-lookup"><span data-stu-id="a9a7a-458">What values to use for \@key</span></span>

<span data-ttu-id="a9a7a-459">Как правило, имеет смысл указать одно из следующих значений для `@key`:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-459">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="a9a7a-460">Экземпляры объектов Model (например, экземпляр `Person`, как в предыдущем примере).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-460">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="a9a7a-461">Это гарантирует сохранение на основе равенства ссылок на объекты.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-461">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="a9a7a-462">Уникальные идентификаторы (например, значения первичного ключа типа `int`, `string`или `Guid`).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-462">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="a9a7a-463">Убедитесь, что значения, используемые для `@key`, не конфликтуют.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-463">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="a9a7a-464">Если конфликтные значения обнаруживаются в одном родительском элементе, Blazor создает исключение, поскольку оно не может детерминированно сопоставлять старые элементы или компоненты с новыми элементами или компонентами.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-464">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="a9a7a-465">Используйте только уникальные значения, такие как экземпляры объекта или значения первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-465">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="routing"></a><span data-ttu-id="a9a7a-466">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="a9a7a-466">Routing</span></span>

<span data-ttu-id="a9a7a-467">Маршрутизация в Blazor достигается путем предоставления шаблона маршрута для каждого доступного компонента в приложении.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-467">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="a9a7a-468">При компиляции файла Razor с директивой `@page` созданному классу присваивается <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> с указанием шаблона маршрута.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-468">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="a9a7a-469">Во время выполнения маршрутизатор ищет классы компонентов с `RouteAttribute` и отображает, какой из компонентов имеет шаблон маршрута, соответствующий запрошенному URL-адресу.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-469">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="a9a7a-470">К компоненту можно применить несколько шаблонов маршрутов.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-470">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="a9a7a-471">Следующий компонент отвечает на запросы для `/BlazorRoute` и `/DifferentBlazorRoute`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-471">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`.</span></span>

<span data-ttu-id="a9a7a-472">*Pages/блазоррауте. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-472">*Pages/BlazorRoute.razor*:</span></span>

```razor
@page "/BlazorRoute"
@page "/DifferentBlazorRoute"

<h1>Blazor routing</h1>
```

## <a name="route-parameters"></a><span data-ttu-id="a9a7a-473">Параметры маршрута</span><span class="sxs-lookup"><span data-stu-id="a9a7a-473">Route parameters</span></span>

<span data-ttu-id="a9a7a-474">Компоненты могут принимать параметры маршрута из шаблона маршрута, указанного в директиве `@page`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-474">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="a9a7a-475">Маршрутизатор использует параметры маршрута для заполнения соответствующих параметров компонента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-475">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="a9a7a-476">*Pages/раутепараметер. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-476">*Pages/RouteParameter.razor*:</span></span>

```razor
@page "/RouteParameter"
@page "/RouteParameter/{text}"

<h1>Blazor is @Text!</h1>

@code {
    [Parameter]
    public string Text { get; set; }

    protected override void OnInitialized()
    {
        Text = Text ?? "fantastic";
    }
}
```

<span data-ttu-id="a9a7a-477">Необязательные параметры не поддерживаются, поэтому в приведенном выше примере применяются две директивы `@page`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-477">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="a9a7a-478">Первый позволяет переходить к компоненту без параметра.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-478">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="a9a7a-479">Вторая директива `@page` принимает параметр `{text}` Route и присваивает значение свойству `Text`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-479">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

<span data-ttu-id="a9a7a-480">Синтаксис параметра *Catch-All* (`*`/`**`), который захватывает путь для нескольких папок, **не** поддерживается в компонентах Razor ( *. Razor*).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-480">*Catch-all* parameter syntax (`*`/`**`), which captures the path across multiple folder boundaries, is **not** supported in Razor components (*.razor*).</span></span>

::: moniker range=">= aspnetcore-3.1"

## <a name="partial-class-support"></a><span data-ttu-id="a9a7a-481">Поддержка разделяемых классов</span><span class="sxs-lookup"><span data-stu-id="a9a7a-481">Partial class support</span></span>

<span data-ttu-id="a9a7a-482">Компоненты Razor создаются как разделяемые классы.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-482">Razor components are generated as partial classes.</span></span> <span data-ttu-id="a9a7a-483">Компоненты Razor создаются с помощью любого из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-483">Razor components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="a9a7a-484">C#код определяется в [`@code`](xref:mvc/views/razor#code) блоке с разметкой HTML и кодом Razor в одном файле.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-484">C# code is defined in an [`@code`](xref:mvc/views/razor#code) block with HTML markup and Razor code in a single file.</span></span> <span data-ttu-id="a9a7a-485">с помощью этого подхода шаблоны Blazor определяют свои компоненты Razor.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-485">Blazor templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="a9a7a-486">C#код помещается в файл кода программной части, определенный как разделяемый класс.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-486">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="a9a7a-487">В следующем примере показан компонент `Counter` по умолчанию с блоком `@code` в приложении, созданном из шаблона Blazor.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-487">The following example shows the default `Counter` component with an `@code` block in an app generated from a Blazor template.</span></span> <span data-ttu-id="a9a7a-488">Разметка HTML, код Razor и C# код находятся в одном файле:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-488">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="a9a7a-489">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-489">*Counter.razor*:</span></span>

```razor
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

<span data-ttu-id="a9a7a-490">`Counter` компонент можно также создать с помощью файла кода программной части с разделяемым классом:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-490">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="a9a7a-491">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-491">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="a9a7a-492">*Counter.Razor.CS*:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-492">*Counter.razor.cs*:</span></span>

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

## <a name="specify-a-component-base-class"></a><span data-ttu-id="a9a7a-493">Укажите базовый класс компонента</span><span class="sxs-lookup"><span data-stu-id="a9a7a-493">Specify a component base class</span></span>

<span data-ttu-id="a9a7a-494">Директиву `@inherits` можно использовать, чтобы указать базовый класс для компонента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-494">The `@inherits` directive can be used to specify a base class for a component.</span></span>

<span data-ttu-id="a9a7a-495">В [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) показано, как компонент может наследовать базовый класс `BlazorRocksBase`, чтобы предоставить свойства и методы компонента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-495">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="a9a7a-496">*Pages/блазорроккс. Razor*:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-496">*Pages/BlazorRocks.razor*:</span></span>

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="a9a7a-497">*BlazorRocksBase.CS*:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-497">*BlazorRocksBase.cs*:</span></span>

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

<span data-ttu-id="a9a7a-498">Базовый класс должен быть производным от `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-498">The base class should derive from `ComponentBase`.</span></span>

::: moniker-end

## <a name="import-components"></a><span data-ttu-id="a9a7a-499">Импорт компонентов</span><span class="sxs-lookup"><span data-stu-id="a9a7a-499">Import components</span></span>

<span data-ttu-id="a9a7a-500">Пространство имен компонента, созданного с помощью Razor, основано на (в порядке приоритета):</span><span class="sxs-lookup"><span data-stu-id="a9a7a-500">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="a9a7a-501">[`@namespace`е](xref:mvc/views/razor#namespace) обозначение в разметке файла Razor ( *. Razor*) (`@namespace BlazorSample.MyNamespace`).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-501">[`@namespace`](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="a9a7a-502">`RootNamespace` проекта в файле проекта (`<RootNamespace>BlazorSample</RootNamespace>`).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-502">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="a9a7a-503">Имя проекта, полученное из имени файла проекта (*CSPROJ*), и путь из корневого каталога проекта к компоненту.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-503">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="a9a7a-504">Например, платформа разрешает *{root Project}/Пажес/индекс.Разор* (*блазорсампле. csproj*) к пространству имен `BlazorSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-504">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="a9a7a-505">Компоненты следуют C# правилам привязки имен.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-505">Components follow C# name binding rules.</span></span> <span data-ttu-id="a9a7a-506">Для компонента `Index` в этом примере компоненты в области являются всеми компонентами:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-506">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="a9a7a-507">В той же папке *страницы*.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-507">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="a9a7a-508">Компоненты в корне проекта, которые не задают явно другое пространство имен.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-508">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="a9a7a-509">Компоненты, определенные в другом пространстве имен, помещаются в область с помощью директивы [`@using`](xref:mvc/views/razor#using) Razor.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-509">Components defined in a different namespace are brought into scope using Razor's [`@using`](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="a9a7a-510">Если другой компонент, `NavMenu.razor`, существует в папке *блазорсампле/Shared/* Folder, компонент можно использовать в `Index.razor` со следующей инструкцией `@using`:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-510">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="a9a7a-511">На компоненты также можно ссылаться с помощью полных имен, для которых не требуется директива [`@using`](xref:mvc/views/razor#using) :</span><span class="sxs-lookup"><span data-stu-id="a9a7a-511">Components can also be referenced using their fully qualified names, which doesn't require the [`@using`](xref:mvc/views/razor#using) directive:</span></span>

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="a9a7a-512">Квалификация `global::` не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-512">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="a9a7a-513">Импорт компонентов с псевдонимами `using` операторы (например, `@using Foo = Bar`) не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-513">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="a9a7a-514">Частично определенные имена не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-514">Partially qualified names aren't supported.</span></span> <span data-ttu-id="a9a7a-515">Например, добавление `@using BlazorSample` и ссылки на `NavMenu.razor` с `<Shared.NavMenu></Shared.NavMenu>` не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-515">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="a9a7a-516">Атрибуты условного HTML-элемента</span><span class="sxs-lookup"><span data-stu-id="a9a7a-516">Conditional HTML element attributes</span></span>

<span data-ttu-id="a9a7a-517">Атрибуты HTML-элемента условно подготавливаются в зависимости от значения .NET.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-517">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="a9a7a-518">Если значение равно `false` или `null`, то атрибут не отображается.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-518">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="a9a7a-519">Если значение равно `true`, атрибут отображается в виде сворачивания.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-519">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="a9a7a-520">В следующем примере `IsCompleted` определяет, отображается ли `checked` в разметке элемента:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-520">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="a9a7a-521">Если `IsCompleted` `true`, флажок отображается следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-521">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="a9a7a-522">Если `IsCompleted` `false`, флажок отображается следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-522">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="a9a7a-523">Для получения дополнительной информации см. <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-523">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="a9a7a-524">Некоторые атрибуты HTML, такие как [ARIA-Pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), не работают должным образом, если тип .net является `bool`ом.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-524">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="a9a7a-525">В этих случаях используйте `string` тип вместо `bool`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-525">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="a9a7a-526">Необработанный HTML</span><span class="sxs-lookup"><span data-stu-id="a9a7a-526">Raw HTML</span></span>

<span data-ttu-id="a9a7a-527">Строки обычно подготавливаются с помощью текстовых узлов DOM, что означает, что любая разметка, которую они могут содержать, игнорируется и обрабатывается как литеральный текст.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-527">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="a9a7a-528">Для отрисовки необработанного HTML-кода заключите содержимое HTML в `MarkupString` значение.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-528">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="a9a7a-529">Значение анализируется как HTML или SVG и вставляется в модель DOM.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-529">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="a9a7a-530">Визуализация необработанного HTML-кода, созданного из любого ненадежного источника, является **угрозой безопасности** , и ее следует избегать.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-530">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="a9a7a-531">В следующем примере показано использование типа `MarkupString` для добавления блока статического HTML-содержимого в отображаемые выходные данные компонента:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-531">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="a9a7a-532">Шаблонные компоненты</span><span class="sxs-lookup"><span data-stu-id="a9a7a-532">Templated components</span></span>

<span data-ttu-id="a9a7a-533">Шаблонные компоненты — это компоненты, принимающие один или несколько шаблонов пользовательского интерфейса в качестве параметров, которые затем можно использовать как часть логики отрисовки компонента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-533">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="a9a7a-534">Шаблонные компоненты позволяют создавать более высокие компоненты более высокого уровня, чем обычные компоненты.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-534">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="a9a7a-535">Ниже приведены несколько примеров.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-535">A couple of examples include:</span></span>

* <span data-ttu-id="a9a7a-536">Компонент таблицы, позволяющий пользователю указывать шаблоны для заголовков, строк и нижних колонтитулов таблицы.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-536">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="a9a7a-537">Компонент списка, позволяющий пользователю указать шаблон для отрисовки элементов в списке.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-537">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="a9a7a-538">Параметры шаблона</span><span class="sxs-lookup"><span data-stu-id="a9a7a-538">Template parameters</span></span>

<span data-ttu-id="a9a7a-539">Шаблонный компонент определяется путем указания одного или нескольких параметров компонента типа `RenderFragment` или `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-539">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="a9a7a-540">Фрагмент отрисовки представляет сегмент пользовательского интерфейса для отрисовки.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-540">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="a9a7a-541">`RenderFragment<T>` принимает параметр типа, который может быть указан при вызове фрагмента прорисовки.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-541">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="a9a7a-542">`TableTemplate` компонент:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-542">`TableTemplate` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

<span data-ttu-id="a9a7a-543">При использовании компонент-шаблона можно указать параметры шаблона с помощью дочерних элементов, совпадающих с именами параметров (`TableHeader` и `RowTemplate` в следующем примере):</span><span class="sxs-lookup"><span data-stu-id="a9a7a-543">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```razor
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

### <a name="template-context-parameters"></a><span data-ttu-id="a9a7a-544">Параметры контекста шаблона</span><span class="sxs-lookup"><span data-stu-id="a9a7a-544">Template context parameters</span></span>

<span data-ttu-id="a9a7a-545">Аргументы компонента типа `RenderFragment<T>` передаются как элементы, имеют неявный параметр с именем `context` (например, из предыдущего примера кода, `@context.PetId`), но можно изменить имя параметра с помощью атрибута `Context` дочернего элемента.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-545">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="a9a7a-546">В следующем примере атрибут `Context` элемента `RowTemplate` указывает параметр `pet`:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-546">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```razor
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

<span data-ttu-id="a9a7a-547">Кроме того, можно указать атрибут `Context` в элементе Component.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-547">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="a9a7a-548">Указанный атрибут `Context` применяется ко всем указанным параметрам шаблона.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-548">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="a9a7a-549">Это может быть полезно, если необходимо указать имя параметра содержимого для неявного дочернего содержимого (без обертывания дочернего элемента).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-549">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="a9a7a-550">В следующем примере атрибут `Context` отображается в элементе `TableTemplate` и применяется ко всем параметрам шаблона:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-550">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```razor
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

### <a name="generic-typed-components"></a><span data-ttu-id="a9a7a-551">Универсальные типы компонентов</span><span class="sxs-lookup"><span data-stu-id="a9a7a-551">Generic-typed components</span></span>

<span data-ttu-id="a9a7a-552">Шаблонные компоненты часто вводятся в универсальном виде.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-552">Templated components are often generically typed.</span></span> <span data-ttu-id="a9a7a-553">Например, универсальный компонент `ListViewTemplate` можно использовать для отображения значений `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-553">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="a9a7a-554">Чтобы определить универсальный компонент, используйте директиву [`@typeparam`](xref:mvc/views/razor#typeparam) для указания параметров типа:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-554">To define a generic component, use the [`@typeparam`](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

<span data-ttu-id="a9a7a-555">При использовании компонентов универсальной типизации параметр типа выводится, если это возможно:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-555">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```razor
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="a9a7a-556">В противном случае параметр типа необходимо явно указать с помощью атрибута, совпадающего с именем параметра типа.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-556">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="a9a7a-557">В следующем примере `TItem="Pet"` указывает тип:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-557">In the following example, `TItem="Pet"` specifies the type:</span></span>

```razor
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="a9a7a-558">Каскадные значения и параметры</span><span class="sxs-lookup"><span data-stu-id="a9a7a-558">Cascading values and parameters</span></span>

<span data-ttu-id="a9a7a-559">В некоторых сценариях неудобно передавать данные из компонента-предка в дочерний компонент с помощью [параметров компонента](#component-parameters), особенно при наличии нескольких слоев компонентов.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-559">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="a9a7a-560">Каскадные значения и параметры позволяют решить эту проблему, предоставляя удобный способ, позволяющий компоненту-предку предоставить значение всем его дочерним компонентам.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-560">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="a9a7a-561">Каскадные значения и параметры также предоставляют подход для координации компонентов.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-561">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="a9a7a-562">Пример темы</span><span class="sxs-lookup"><span data-stu-id="a9a7a-562">Theme example</span></span>

<span data-ttu-id="a9a7a-563">В следующем примере из примера приложения класс `ThemeInfo` указывает сведения о теме для перетекания иерархии компонентов, чтобы все кнопки в пределах данной части приложения совпадали с тем же стилем.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-563">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="a9a7a-564">*Уисемеклассес/themeinfo указывает расположение. CS*:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-564">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="a9a7a-565">Компонент-предок может предоставлять каскадное значение с помощью компонента каскадного значения.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-565">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="a9a7a-566">Компонент `CascadingValue` упаковывает поддерево иерархии компонентов и предоставляет одно значение всем компонентам в этом поддереве.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-566">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="a9a7a-567">Например, в примере приложения указываются сведения о теме (`ThemeInfo`) в одном из макетов приложения в виде каскадного параметра для всех компонентов, составляющих текст макета свойства `@Body`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-567">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="a9a7a-568">`ButtonClass` присваивается значение `btn-success` в компоненте макета.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-568">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="a9a7a-569">Любой компонент-потомок может использовать это свойство через `ThemeInfo` каскадный объект.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-569">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="a9a7a-570">`CascadingValuesParametersLayout` компонент:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-570">`CascadingValuesParametersLayout` component:</span></span>

```razor
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

<span data-ttu-id="a9a7a-571">Чтобы использовать каскадные значения, компоненты объявляют каскадные параметры с помощью атрибута `[CascadingParameter]`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-571">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="a9a7a-572">Каскадные значения привязываются к каскадным параметрам по типу.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-572">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="a9a7a-573">В примере приложения компонент `CascadingValuesParametersTheme` привязывает `ThemeInfo` каскадное значение к каскадному параметру.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-573">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="a9a7a-574">Параметр используется для задания класса CSS для одной из кнопок, отображаемых компонентом.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-574">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="a9a7a-575">`CascadingValuesParametersTheme` компонент:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-575">`CascadingValuesParametersTheme` component:</span></span>

```razor
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

<span data-ttu-id="a9a7a-576">Чтобы вычислить несколько значений одного типа в одном поддереве, укажите уникальную строку `Name` для каждого компонента `CascadingValue` и соответствующего `CascadingParameter`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-576">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="a9a7a-577">В следующем примере два `CascadingValue` компонентов каскадом разворачивают разные экземпляры `MyCascadingType` по имени:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-577">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

```razor
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

<span data-ttu-id="a9a7a-578">В компоненте-потомках каскадные параметры получают значения из соответствующих каскадных значений в компоненте-предке по имени:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-578">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="a9a7a-579">Пример Табсет</span><span class="sxs-lookup"><span data-stu-id="a9a7a-579">TabSet example</span></span>

<span data-ttu-id="a9a7a-580">Каскадные параметры также позволяют компонентам работать в иерархии компонентов.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-580">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="a9a7a-581">Например, рассмотрим следующий пример *табсет* в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-581">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="a9a7a-582">В примере приложения имеется интерфейс `ITab`, который реализуется вкладками:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-582">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="a9a7a-583">Компонент `CascadingValuesParametersTabSet` использует компонент `TabSet`, который содержит несколько `Tab` компонентов:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-583">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

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

<span data-ttu-id="a9a7a-584">Дочерние `Tab` компоненты явно не передаются в качестве параметров в `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-584">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="a9a7a-585">Вместо этого дочерние `Tab` компоненты являются частью дочернего содержимого `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-585">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="a9a7a-586">Однако `TabSet` по-прежнему необходимо узнать о каждом компоненте `Tab`, чтобы он мог отобразить заголовки и активную вкладку. Чтобы обеспечить эту координацию без необходимости дополнительного кода, компонент `TabSet` *может предоставить себя как каскадное значение* , которое затем будет выбиралось компонентами-потомками `Tab`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-586">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="a9a7a-587">`TabSet` компонент:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-587">`TabSet` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="a9a7a-588">Дочерние `Tab` компоненты захватывают содержащий `TabSet` в виде каскадного параметра, поэтому `Tab` компоненты добавляются в `TabSet` и координирует, какая вкладка активна.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-588">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="a9a7a-589">`Tab` компонент:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-589">`Tab` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="a9a7a-590">Шаблоны Razor</span><span class="sxs-lookup"><span data-stu-id="a9a7a-590">Razor templates</span></span>

<span data-ttu-id="a9a7a-591">Фрагменты визуализации можно определить с помощью синтаксиса шаблона Razor.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-591">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="a9a7a-592">Шаблоны Razor позволяют определить фрагмент пользовательского интерфейса и принять следующий формат:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-592">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```razor
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="a9a7a-593">В следующем примере показано, как указать значения `RenderFragment` и `RenderFragment<T>` и визуализировать шаблоны непосредственно в компоненте.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-593">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="a9a7a-594">Фрагменты рендеринга также могут передаваться в качестве аргументов в [шаблонные компоненты](#templated-components).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-594">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

```razor
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

<span data-ttu-id="a9a7a-595">Отображаемые выходные данные предыдущего кода:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-595">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="a9a7a-596">Логика Рендертрибуилдер вручную</span><span class="sxs-lookup"><span data-stu-id="a9a7a-596">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="a9a7a-597">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` предоставляет методы для управления компонентами и элементами, включая создание компонентов вручную в C# коде.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-597">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="a9a7a-598">Использование `RenderTreeBuilder` для создания компонентов является расширенным сценарием.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-598">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="a9a7a-599">Неправильно сформированный компонент (например, незакрытый тег разметки) может привести к неопределенному поведению.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-599">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="a9a7a-600">Рассмотрим следующий компонент `PetDetails`, который можно вручную встроить в другой компонент:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-600">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="a9a7a-601">В следующем примере цикл в методе `CreateComponent` создает три `PetDetails` компонентов.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-601">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="a9a7a-602">При вызове методов `RenderTreeBuilder` для создания компонентов (`OpenComponent` и `AddAttribute`) порядковые номера представляют собой номера строк исходного кода.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-602">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="a9a7a-603">Алгоритм разницы Blazor зависит от порядковых номеров, соответствующих разным строкам кода, а не отдельных вызовов вызова.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-603">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="a9a7a-604">При создании компонента с помощью методов `RenderTreeBuilder` жестко указывайте аргументы для порядковых номеров.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-604">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="a9a7a-605">**Использование вычисления или счетчика для формирования порядкового номера может привести к снижению производительности.**</span><span class="sxs-lookup"><span data-stu-id="a9a7a-605">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="a9a7a-606">Дополнительные сведения см. в разделе [порядковые номера относятся к разделу номера строк кода и порядок выполнения](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="a9a7a-606">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="a9a7a-607">`BuiltContent` компонент:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-607">`BuiltContent` component:</span></span>

```razor
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

> [!WARNING]
> <span data-ttu-id="a9a7a-608">Типы в `Microsoft.AspNetCore.Components.RenderTree` позволяют обрабатывать *результаты* операций отрисовки.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-608">The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="a9a7a-609">Это внутренние сведения о реализации Blazor Framework.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-609">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="a9a7a-610">Эти типы следует считать *нестабильными* и могут быть изменены в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-610">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="a9a7a-611">Порядковые номера связаны с номерами строк кода, а не с порядком выполнения</span><span class="sxs-lookup"><span data-stu-id="a9a7a-611">Sequence numbers relate to code line numbers and not execution order</span></span>

Blazor<span data-ttu-id="a9a7a-612"> файлы `.razor` компилируются всегда.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-612"> `.razor` files are always compiled.</span></span> <span data-ttu-id="a9a7a-613">Это, вероятно, является отличным преимуществом для `.razor`, поскольку этап компиляции можно использовать для вставки сведений, повышающих производительность приложения во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-613">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="a9a7a-614">Ключевым примером этих улучшений являются *порядковые номера*.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-614">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="a9a7a-615">Порядковые номера указывают среде выполнения, какие выходные данные поступили из разных и упорядоченных строк кода.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-615">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="a9a7a-616">Среда выполнения использует эти сведения для создания эффективных различий дерева в линейное время, что является гораздо быстрее, чем обычно возможно для общего алгоритма различения дерева.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-616">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="a9a7a-617">Рассмотрим следующий файл компонента Razor ( *. Razor*):</span><span class="sxs-lookup"><span data-stu-id="a9a7a-617">Consider the following Razor component (*.razor*) file:</span></span>

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="a9a7a-618">Предыдущий код компилируется примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-618">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="a9a7a-619">Когда код выполняется в первый раз, если `someFlag` `true`, построитель получит следующее:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-619">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="a9a7a-620">Sequence</span><span class="sxs-lookup"><span data-stu-id="a9a7a-620">Sequence</span></span> | <span data-ttu-id="a9a7a-621">Тип</span><span class="sxs-lookup"><span data-stu-id="a9a7a-621">Type</span></span>      | <span data-ttu-id="a9a7a-622">Данные</span><span class="sxs-lookup"><span data-stu-id="a9a7a-622">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="a9a7a-623">0</span><span class="sxs-lookup"><span data-stu-id="a9a7a-623">0</span></span>        | <span data-ttu-id="a9a7a-624">Узел Text</span><span class="sxs-lookup"><span data-stu-id="a9a7a-624">Text node</span></span> | <span data-ttu-id="a9a7a-625">First</span><span class="sxs-lookup"><span data-stu-id="a9a7a-625">First</span></span>  |
| <span data-ttu-id="a9a7a-626">1</span><span class="sxs-lookup"><span data-stu-id="a9a7a-626">1</span></span>        | <span data-ttu-id="a9a7a-627">Узел Text</span><span class="sxs-lookup"><span data-stu-id="a9a7a-627">Text node</span></span> | <span data-ttu-id="a9a7a-628">Second</span><span class="sxs-lookup"><span data-stu-id="a9a7a-628">Second</span></span> |

<span data-ttu-id="a9a7a-629">Представьте, что `someFlag` становится `false`, и разметка снова готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-629">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="a9a7a-630">На этот раз построитель получит:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-630">This time, the builder receives:</span></span>

| <span data-ttu-id="a9a7a-631">Sequence</span><span class="sxs-lookup"><span data-stu-id="a9a7a-631">Sequence</span></span> | <span data-ttu-id="a9a7a-632">Тип</span><span class="sxs-lookup"><span data-stu-id="a9a7a-632">Type</span></span>       | <span data-ttu-id="a9a7a-633">Данные</span><span class="sxs-lookup"><span data-stu-id="a9a7a-633">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="a9a7a-634">1</span><span class="sxs-lookup"><span data-stu-id="a9a7a-634">1</span></span>        | <span data-ttu-id="a9a7a-635">Узел Text</span><span class="sxs-lookup"><span data-stu-id="a9a7a-635">Text node</span></span>  | <span data-ttu-id="a9a7a-636">Second</span><span class="sxs-lookup"><span data-stu-id="a9a7a-636">Second</span></span> |

<span data-ttu-id="a9a7a-637">Когда среда выполнения выполняет поиск различий, она видит, что элемент в последовательности `0` был удален, поэтому он создает следующий тривиальный *сценарий редактирования*:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-637">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="a9a7a-638">Удалите первый текстовый узел.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-638">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="a9a7a-639">Что становится неправильным, если вы создаете порядковые номера программным способом</span><span class="sxs-lookup"><span data-stu-id="a9a7a-639">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="a9a7a-640">Вместо этого Представьте себе, что вы написали следующую логику конструктора деревьев визуализации:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-640">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="a9a7a-641">Теперь первые выходные данные:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-641">Now, the first output is:</span></span>

| <span data-ttu-id="a9a7a-642">Sequence</span><span class="sxs-lookup"><span data-stu-id="a9a7a-642">Sequence</span></span> | <span data-ttu-id="a9a7a-643">Тип</span><span class="sxs-lookup"><span data-stu-id="a9a7a-643">Type</span></span>      | <span data-ttu-id="a9a7a-644">Данные</span><span class="sxs-lookup"><span data-stu-id="a9a7a-644">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="a9a7a-645">0</span><span class="sxs-lookup"><span data-stu-id="a9a7a-645">0</span></span>        | <span data-ttu-id="a9a7a-646">Узел Text</span><span class="sxs-lookup"><span data-stu-id="a9a7a-646">Text node</span></span> | <span data-ttu-id="a9a7a-647">First</span><span class="sxs-lookup"><span data-stu-id="a9a7a-647">First</span></span>  |
| <span data-ttu-id="a9a7a-648">1</span><span class="sxs-lookup"><span data-stu-id="a9a7a-648">1</span></span>        | <span data-ttu-id="a9a7a-649">Узел Text</span><span class="sxs-lookup"><span data-stu-id="a9a7a-649">Text node</span></span> | <span data-ttu-id="a9a7a-650">Second</span><span class="sxs-lookup"><span data-stu-id="a9a7a-650">Second</span></span> |

<span data-ttu-id="a9a7a-651">Этот результат идентичен предыдущему случаю, поэтому отрицательные проблемы не возникают.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-651">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="a9a7a-652">во второй отрисовке `someFlag` `false`, а выходные данные:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-652">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="a9a7a-653">Sequence</span><span class="sxs-lookup"><span data-stu-id="a9a7a-653">Sequence</span></span> | <span data-ttu-id="a9a7a-654">Тип</span><span class="sxs-lookup"><span data-stu-id="a9a7a-654">Type</span></span>      | <span data-ttu-id="a9a7a-655">Данные</span><span class="sxs-lookup"><span data-stu-id="a9a7a-655">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="a9a7a-656">0</span><span class="sxs-lookup"><span data-stu-id="a9a7a-656">0</span></span>        | <span data-ttu-id="a9a7a-657">Узел Text</span><span class="sxs-lookup"><span data-stu-id="a9a7a-657">Text node</span></span> | <span data-ttu-id="a9a7a-658">Second</span><span class="sxs-lookup"><span data-stu-id="a9a7a-658">Second</span></span> |

<span data-ttu-id="a9a7a-659">На этот раз алгоритм diff видит, что были внесены *два* изменения, и алгоритм создает следующий сценарий редактирования:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-659">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="a9a7a-660">Измените значение первого текстового узла на `Second`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-660">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="a9a7a-661">Удалите второй текстовый узел.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-661">Remove the second text node.</span></span>

<span data-ttu-id="a9a7a-662">При формировании порядковых номеров теряются все полезные сведения о том, где в исходном коде находятся `if/else` ветви и циклы.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-662">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="a9a7a-663">Это приводит к **удвоению в два раза больше** , чем раньше.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-663">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="a9a7a-664">Это тривиальный пример.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-664">This is a trivial example.</span></span> <span data-ttu-id="a9a7a-665">В более реалистичных случаях со сложными и глубокими вложенными структурами, особенно с циклами, затраты на производительность более серьезны.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-665">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="a9a7a-666">Вместо того, чтобы сразу определять, какие блоки или ветви цикла были вставлены или удалены, алгоритм diff должен рекурсивно пребираться в деревьях отрисовки и, как правило, создает гораздо более длительные операции редактирования, так как он сообщает о том, как старую и новую структуры связь друг с другом.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-666">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="a9a7a-667">Руководство и выводы</span><span class="sxs-lookup"><span data-stu-id="a9a7a-667">Guidance and conclusions</span></span>

* <span data-ttu-id="a9a7a-668">Производительность приложения снижается, если порядковые номера создаются динамически.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-668">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="a9a7a-669">Платформа не может автоматически создавать собственные порядковые номера во время выполнения, поскольку необходимая информация не существует, если она не захвачена во время компиляции.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-669">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="a9a7a-670">Не записывайте длинные блоки `RenderTreeBuilder` логики, реализуемой вручную.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-670">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="a9a7a-671">Предпочитать `.razor` файлы и позволяют компилятору обрабатывать порядковые номера.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-671">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="a9a7a-672">Если не удается избежать ручного `RenderTreeBuilder` логики, разделите длинные блоки кода на более мелкие части, заключенные в `OpenRegion`/`CloseRegion` вызовы.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-672">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="a9a7a-673">Каждый регион имеет собственное отдельное пространство порядковых номеров, поэтому вы можете перезапускаться от нуля (или любого другого произвольного числа) внутри каждого региона.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-673">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="a9a7a-674">Если порядковые номера задаются жестко, то для алгоритма diff требуется, чтобы только порядковые номера увеличиваются в значении.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-674">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="a9a7a-675">Начальное значение и зазоры несущественны.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-675">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="a9a7a-676">Один из этих вариантов — использовать номер строки кода в качестве порядкового номера или начать с нуля и увеличить на единицу или сотни (или любой другой интервал).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-676">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* Blazor<span data-ttu-id="a9a7a-677"> использует порядковые номера, в то время как другие платформы, использующие средства различения дерева, не используют их.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-677"> uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="a9a7a-678">Сравнение выполняется гораздо быстрее, если используются порядковые номера, и Blazor имеет преимущество этапа компиляции, который автоматически обрабатывает порядковые номера для разработчиков, занимающихся созданием файлов *Razor* .</span><span class="sxs-lookup"><span data-stu-id="a9a7a-678">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring *.razor* files.</span></span>

## <a name="localization"></a><span data-ttu-id="a9a7a-679">Локализация</span><span class="sxs-lookup"><span data-stu-id="a9a7a-679">Localization</span></span>

Blazor<span data-ttu-id="a9a7a-680"> серверные приложения локализованы по [промежуточного слоя локализации](xref:fundamentals/localization#localization-middleware).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-680"> Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="a9a7a-681">По промежуточного слоя выбирает соответствующие языки и региональные параметры для пользователей, запрашивающих ресурсы из приложения.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-681">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="a9a7a-682">Язык и региональные параметры можно задать с помощью одного из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-682">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="a9a7a-683">Файлы cookie</span><span class="sxs-lookup"><span data-stu-id="a9a7a-683">Cookies</span></span>](#cookies)
* [<span data-ttu-id="a9a7a-684">Предоставление пользовательского интерфейса для выбора языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="a9a7a-684">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="a9a7a-685">Дополнительные сведения и примеры см. в разделе <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-685">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="configure-the-linker-for-internationalization-opno-locblazor-webassembly"></a><span data-ttu-id="a9a7a-686">Настройка компоновщика для интернационализации (Blazorная сборка)</span><span class="sxs-lookup"><span data-stu-id="a9a7a-686">Configure the linker for internationalization (Blazor WebAssembly)</span></span>

<span data-ttu-id="a9a7a-687">По умолчанию конфигурация компоновщика Blazor для приложений Blazor WebAssembly исключает сведения об интернационализации, кроме явно запрошенных языковых стандартов.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-687">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="a9a7a-688">Дополнительные сведения и рекомендации по управлению поведением компоновщика см. в разделе <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-688">For more information and guidance on controlling the linker's behavior, see <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="a9a7a-689">Файлы cookie</span><span class="sxs-lookup"><span data-stu-id="a9a7a-689">Cookies</span></span>

<span data-ttu-id="a9a7a-690">Файл cookie культуры локализации может сохранять язык и региональные параметры пользователя.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-690">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="a9a7a-691">Файл cookie создается методом `OnGet` страницы узла приложения (*pages/Host. cshtml. CS*).</span><span class="sxs-lookup"><span data-stu-id="a9a7a-691">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="a9a7a-692">По промежуточного слоя локализации считывает файл cookie при последующих запросах, чтобы задать язык и региональные параметры пользователя.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-692">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="a9a7a-693">Использование файла cookie гарантирует, что соединение WebSocket сможет правильно распространить культуру.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-693">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="a9a7a-694">Если схемы локализации основаны на пути URL-адреса или строке запроса, схема может не поддерживать работу с WebSockets, поэтому она не сохраняет культуру.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-694">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="a9a7a-695">Поэтому рекомендуемым подходом является использование файла cookie языка и региональных параметров локализации.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-695">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="a9a7a-696">Любой метод можно использовать для назначения языка и региональных параметров, если язык и региональные параметры сохраняются в файле cookie локализации.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-696">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="a9a7a-697">Если у приложения уже есть установленная схема локализации для ASP.NET Core на стороне сервера, продолжайте использовать существующую инфраструктуру локализации приложения и задавайте файл cookie культуры локализации в схеме приложения.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-697">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="a9a7a-698">В следующем примере показано, как задать текущий язык и региональные параметры в файле cookie, который может быть прочитан по промежуточного слоя локализации.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-698">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="a9a7a-699">Создайте файл *pages/Host. cshtml. CS* со следующим содержимым в приложении Blazor Server:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-699">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

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

<span data-ttu-id="a9a7a-700">Локализация обрабатывается в приложении:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-700">Localization is handled in the app:</span></span>

1. <span data-ttu-id="a9a7a-701">Браузер отправляет в приложение исходный HTTP-запрос.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-701">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="a9a7a-702">Язык и региональные параметры назначаются по промежуточного слоя локализации.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-702">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="a9a7a-703">Метод `OnGet` в *_Host. cshtml. CS* сохраняет язык и региональные параметры в файле cookie как часть ответа.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-703">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="a9a7a-704">Браузер открывает соединение WebSocket для создания интерактивного сеанса Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-704">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="a9a7a-705">По промежуточного слоя для локализации считывает файл cookie и назначает язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-705">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="a9a7a-706">Сеанс Blazor Server начинается с правильного языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-706">The Blazor Server session begins with the correct culture.</span></span>

### <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="a9a7a-707">Предоставление пользовательского интерфейса для выбора языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="a9a7a-707">Provide UI to choose the culture</span></span>

<span data-ttu-id="a9a7a-708">Для предоставления пользовательского интерфейса, позволяющего пользователю выбирать язык и региональные параметры, рекомендуется *подход на основе перенаправления* .</span><span class="sxs-lookup"><span data-stu-id="a9a7a-708">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="a9a7a-709">Процесс аналогичен тому, что происходит в веб-приложении, когда пользователь пытается получить доступ к защищенному ресурсу,&mdash;пользователь перенаправляется на страницу входа, а затем перенаправляется обратно к исходному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-709">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="a9a7a-710">Приложение сохраняет выбранный язык и региональные параметры пользователя с помощью перенаправления к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-710">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="a9a7a-711">Контроллер устанавливает выбранный язык и региональные параметры пользователя в файл cookie и перенаправляет пользователя обратно к исходному коду URI.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-711">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="a9a7a-712">Установите конечную точку HTTP на сервере, чтобы задать выбранный язык и региональные параметры пользователя в файле cookie, и выполните перенаправление обратно в исходный URI:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-712">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

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
> <span data-ttu-id="a9a7a-713">Для предотвращения атак с открытым перенаправлением используйте результат действия `LocalRedirect`.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-713">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="a9a7a-714">Для получения дополнительной информации см. <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-714">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="a9a7a-715">В следующем компоненте показан пример выполнения начального перенаправления, когда пользователь выбирает язык и региональные параметры:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-715">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```razor
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

### <a name="use-net-localization-scenarios-in-opno-locblazor-apps"></a><span data-ttu-id="a9a7a-716">Использование сценариев локализации .NET в Blazor приложениях</span><span class="sxs-lookup"><span data-stu-id="a9a7a-716">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="a9a7a-717">В Blazor приложениях доступны следующие сценарии локализации и глобализации .NET:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-717">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="a9a7a-718">. Система ресурсов NET</span><span class="sxs-lookup"><span data-stu-id="a9a7a-718">.NET's resources system</span></span>
* <span data-ttu-id="a9a7a-719">Форматирование чисел и дат, зависящих от языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="a9a7a-719">Culture-specific number and date formatting</span></span>

<span data-ttu-id="a9a7a-720">функции `@bind` Blazorвыполняют глобализацию на основе текущего языка и региональных параметров пользователя.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-720">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="a9a7a-721">Дополнительные сведения см. в разделе [Привязка данных](#data-binding) .</span><span class="sxs-lookup"><span data-stu-id="a9a7a-721">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="a9a7a-722">В настоящее время поддерживается ограниченный набор сценариев локализации ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-722">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="a9a7a-723">`IStringLocalizer<>` *поддерживается* в Blazor приложениях.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-723">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="a9a7a-724">Локализация `IHtmlLocalizer<>`, `IViewLocalizer<>`и аннотаций данных ASP.NET Core сценариев MVC и **не поддерживается** в Blazor приложениях.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-724">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="a9a7a-725">Для получения дополнительной информации см. <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-725">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="a9a7a-726">Масштабируемые изображения векторной графики (SVG)</span><span class="sxs-lookup"><span data-stu-id="a9a7a-726">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="a9a7a-727">Поскольку Blazor отображает HTML-файлы, поддерживаемые браузером изображения, включая масштабируемые векторные графики (SVG *),* поддерживаются с помощью тега `<img>`:</span><span class="sxs-lookup"><span data-stu-id="a9a7a-727">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="a9a7a-728">Аналогичным образом SVG-изображения поддерживаются в правилах CSS файла стилей ( *. CSS*):</span><span class="sxs-lookup"><span data-stu-id="a9a7a-728">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="a9a7a-729">Однако встроенная разметка SVG не поддерживается во всех сценариях.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-729">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="a9a7a-730">Если поместить тег `<svg>` непосредственно в файл компонента (*Razor*), то поддерживается базовая визуализация изображений, но многие расширенные сценарии пока не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-730">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="a9a7a-731">Например, `<use>` теги в настоящее время не учитываются, и `@bind` нельзя использовать с некоторыми тегами SVG.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-731">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="a9a7a-732">В будущем выпуске мы планируем устранить эти ограничения.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-732">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a9a7a-733">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a9a7a-733">Additional resources</span></span>

* <span data-ttu-id="a9a7a-734"><xref:security/blazor/server> &ndash; содержит рекомендации по созданию серверных приложений Blazor, которые должны конкурировать с нехваткой ресурсов.</span><span class="sxs-lookup"><span data-stu-id="a9a7a-734"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
