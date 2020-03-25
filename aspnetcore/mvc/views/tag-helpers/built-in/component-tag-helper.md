---
title: Вспомогательная функция тега компонента в ASP.NET Core
author: guardrex
ms.author: riande
description: Узнайте, как использовать вспомогательную функцию тега компонента ASP.NET Core для отрисовки компонентов Razor в страницах и представлениях.
ms.custom: mvc
ms.date: 03/18/2020
no-loc:
- Blazor
- SignalR
uid: mvc/views/tag-helpers/builtin-th/component-tag-helper
ms.openlocfilehash: 801ceb73de5bb4ef7500624e1fbddbf96d1ab89c
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226384"
---
# <a name="component-tag-helper-in-aspnet-core"></a><span data-ttu-id="6c83b-103">Вспомогательная функция тега компонента в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6c83b-103">Component Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="6c83b-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6c83b-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6c83b-105">Чтобы отобразить компонент из страницы или представления, используйте [вспомогательную функцию тега компонента](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper).</span><span class="sxs-lookup"><span data-stu-id="6c83b-105">To render a component from a page or view, use the [Component Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper).</span></span>

<span data-ttu-id="6c83b-106">Следующая вспомогательная функция тега Component визуализирует компонент `Counter` на странице или в представлении:</span><span class="sxs-lookup"><span data-stu-id="6c83b-106">The following Component Tag Helper renders the `Counter` component in a page or view:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

<span data-ttu-id="6c83b-107">Вспомогательная функция тега компонента также может передавать параметры в компоненты.</span><span class="sxs-lookup"><span data-stu-id="6c83b-107">The Component Tag Helper can also pass parameters to components.</span></span> <span data-ttu-id="6c83b-108">Рассмотрим следующий `ColorfulCheckbox` компонент, устанавливающий цвет и размер метки флажка:</span><span class="sxs-lookup"><span data-stu-id="6c83b-108">Consider the following `ColorfulCheckbox` component that sets the check box label's color and size:</span></span>

```razor
<label style="font-size:@(Size)px;color:@Color">
    <input @bind="Value"
           id="survey" 
           name="blazor" 
           type="checkbox" />
    Enjoying Blazor?
</label>

@code {
    [Parameter]
    public bool Value { get; set; }

    [Parameter]
    public int Size { get; set; } = 8;

    [Parameter]
    public string Color { get; set; }

    protected override void OnInitialized()
    {
        Size += 10;
    }
}
```

<span data-ttu-id="6c83b-109">[Параметры компонента](xref:blazor/components#component-parameters) `Size` (`int`) и `Color` (`string`) могут быть заданы вспомогательной функцией тега компонента:</span><span class="sxs-lookup"><span data-stu-id="6c83b-109">The `Size` (`int`) and `Color` (`string`) [component parameters](xref:blazor/components#component-parameters) can be set by the Component Tag Helper:</span></span>

```cshtml
<component type="typeof(ColorfulCheckbox)" render-mode="ServerPrerendered" 
    param-Size="14" param-Color="@("blue")" />
```

<span data-ttu-id="6c83b-110">Следующий код HTML отображается на странице или в представлении:</span><span class="sxs-lookup"><span data-stu-id="6c83b-110">The following HTML is rendered in the page or view:</span></span>

```html
<label style="font-size:24px;color:blue">
    <input id="survey" name="blazor" type="checkbox">
    Enjoying Blazor?
</label>
```

<span data-ttu-id="6c83b-111">Для передачи строки в кавычках требуется [явное выражение Razor](xref:mvc/views/razor#explicit-razor-expressions), как показано для `param-Color` в предыдущем примере.</span><span class="sxs-lookup"><span data-stu-id="6c83b-111">Passing a quoted string requires an [explicit Razor expression](xref:mvc/views/razor#explicit-razor-expressions), as shown for `param-Color` in the preceding example.</span></span> <span data-ttu-id="6c83b-112">Поведение синтаксического анализа Razor для значения типа `string` не применяется к атрибуту `param-*`, так как атрибут является типом `object`.</span><span class="sxs-lookup"><span data-stu-id="6c83b-112">The Razor parsing behavior for a `string` type value doesn't apply to a `param-*` attribute because the attribute is an `object` type.</span></span>

<span data-ttu-id="6c83b-113">Тип параметра должен быть JSON-сериализуемым, что обычно означает, что тип должен иметь конструктор по умолчанию и устанавливаемые свойства.</span><span class="sxs-lookup"><span data-stu-id="6c83b-113">The parameter type must be JSON serializable, which typically means that the type must have a default constructor and settable properties.</span></span> <span data-ttu-id="6c83b-114">Например, можно указать значение для `Size` и `Color` в предыдущем примере, так как типы `Size` и `Color` являются примитивными типами (`int` и `string`), которые поддерживаются сериализатором JSON.</span><span class="sxs-lookup"><span data-stu-id="6c83b-114">For example, you can specify a value for `Size` and `Color` in the preceding example because the types of `Size` and `Color` are primitive types (`int` and `string`), which are supported by the JSON serializer.</span></span>

<span data-ttu-id="6c83b-115">Параметр <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> настраивает одно из следующих поведений компонента:</span><span class="sxs-lookup"><span data-stu-id="6c83b-115"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configures whether the component:</span></span>

* <span data-ttu-id="6c83b-116">компонент предварительно преобразуется в страницу;</span><span class="sxs-lookup"><span data-stu-id="6c83b-116">Is prerendered into the page.</span></span>
* <span data-ttu-id="6c83b-117">компонент отображается как статический HTML на странице или включает необходимые сведения для начальной загрузки приложения Blazor из агента пользователя.</span><span class="sxs-lookup"><span data-stu-id="6c83b-117">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| <span data-ttu-id="6c83b-118">Режим рендеринга</span><span class="sxs-lookup"><span data-stu-id="6c83b-118">Render Mode</span></span> | <span data-ttu-id="6c83b-119">Description</span><span class="sxs-lookup"><span data-stu-id="6c83b-119">Description</span></span> |
| ----------- | ----------- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | <span data-ttu-id="6c83b-120">Преобразует компонент в статический HTML и включает метку приложения Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="6c83b-120">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="6c83b-121">При запуске пользовательского агента эта метка используется для начальной загрузки приложения Blazor.</span><span class="sxs-lookup"><span data-stu-id="6c83b-121">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | <span data-ttu-id="6c83b-122">Отображает метку приложения Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="6c83b-122">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="6c83b-123">Выходные данные компонента не включаются.</span><span class="sxs-lookup"><span data-stu-id="6c83b-123">Output from the component isn't included.</span></span> <span data-ttu-id="6c83b-124">При запуске пользовательского агента эта метка используется для начальной загрузки приложения Blazor.</span><span class="sxs-lookup"><span data-stu-id="6c83b-124">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | <span data-ttu-id="6c83b-125">Преобразует компонент в статический HTML.</span><span class="sxs-lookup"><span data-stu-id="6c83b-125">Renders the component into static HTML.</span></span> |

<span data-ttu-id="6c83b-126">Хотя страницы и представления могут использовать компоненты, обратное неверно.</span><span class="sxs-lookup"><span data-stu-id="6c83b-126">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="6c83b-127">Компоненты не могут использовать функции представления и страницы, такие как частичные представления и разделы.</span><span class="sxs-lookup"><span data-stu-id="6c83b-127">Components can't use view- and page-specific features, such as partial views and sections.</span></span> <span data-ttu-id="6c83b-128">Чтобы использовать логику из частичного представления в компоненте, разнесите логику частичного представления в компонент.</span><span class="sxs-lookup"><span data-stu-id="6c83b-128">To use logic from a partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="6c83b-129">Отрисовка компонентов сервера из статической HTML-страницы не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="6c83b-129">Rendering server components from a static HTML page isn't supported.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6c83b-130">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6c83b-130">Additional resources</span></span>

* <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper>
* <xref:mvc/views/tag-helpers/intro>
* <xref:blazor/components>
