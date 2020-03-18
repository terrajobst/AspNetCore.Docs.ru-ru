---
title: Шаблонные компоненты Blazor в ASP.NET Core
author: guardrex
description: Сведения о шаблонных компонентах, которые могут принимать один или несколько шаблонов пользовательского интерфейса в качестве параметров, используемых затем как часть логики отрисовки компонента.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/templated-components
ms.openlocfilehash: b64d6a731e540b13c50b2c6108f75efd0ac9290c
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78646534"
---
# <a name="aspnet-core-opno-locblazor-templated-components"></a><span data-ttu-id="6629b-103">Шаблонные компоненты Blazor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6629b-103">ASP.NET Core Blazor templated components</span></span>

<span data-ttu-id="6629b-104">Авторы: [Люк Латэм](https://github.com/guardrex) (Luke Latham) и [Дэниэл Рот](https://github.com/danroth27) (Daniel Roth)</span><span class="sxs-lookup"><span data-stu-id="6629b-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="6629b-105">Шаблонные компоненты — это компоненты, которые могут принимать один или несколько шаблонов пользовательского интерфейса в качестве параметров, используемых затем как часть логики отрисовки компонента.</span><span class="sxs-lookup"><span data-stu-id="6629b-105">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="6629b-106">Шаблонные компоненты позволяют разрабатывать более общие компоненты, которые удобнее использовать повторно, чем обычные.</span><span class="sxs-lookup"><span data-stu-id="6629b-106">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="6629b-107">Вот несколько примеров:</span><span class="sxs-lookup"><span data-stu-id="6629b-107">A couple of examples include:</span></span>

* <span data-ttu-id="6629b-108">компонент таблицы, позволяющий пользователю задавать шаблоны для заголовка, строк и нижнего колонтитула таблицы;</span><span class="sxs-lookup"><span data-stu-id="6629b-108">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="6629b-109">компонент списка, позволяющий пользователю задавать шаблон для отрисовываемых элементов списка.</span><span class="sxs-lookup"><span data-stu-id="6629b-109">A list component that allows a user to specify a template for rendering items in a list.</span></span>

## <a name="template-parameters"></a><span data-ttu-id="6629b-110">Параметры шаблона</span><span class="sxs-lookup"><span data-stu-id="6629b-110">Template parameters</span></span>

<span data-ttu-id="6629b-111">Шаблонный компонент определяется путем указания одного или нескольких параметров компонента типа `RenderFragment` или `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="6629b-111">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="6629b-112">Фрагмент отрисовки представляет часть пользовательского интерфейса, подлежащую отрисовке.</span><span class="sxs-lookup"><span data-stu-id="6629b-112">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="6629b-113">`RenderFragment<T>` принимает параметр типа, который можно задать при вызове фрагмента отрисовки.</span><span class="sxs-lookup"><span data-stu-id="6629b-113">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="6629b-114">Компонент `TableTemplate`:</span><span class="sxs-lookup"><span data-stu-id="6629b-114">`TableTemplate` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

<span data-ttu-id="6629b-115">При использовании шаблонного компонента параметры шаблона можно задать с помощью дочерних элементов, имена которых совпадают с именами параметров (`TableHeader` и `RowTemplate` в следующем примере):</span><span class="sxs-lookup"><span data-stu-id="6629b-115">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

## <a name="template-context-parameters"></a><span data-ttu-id="6629b-116">Параметры контекста шаблона</span><span class="sxs-lookup"><span data-stu-id="6629b-116">Template context parameters</span></span>

<span data-ttu-id="6629b-117">Аргументы компонента типа `RenderFragment<T>`, передаваемые как элементы, имеют неявный параметр `context` (например, `@context.PetId` в предыдущем примере кода), но его имя можно изменить с помощью атрибута `Context` дочернего элемента.</span><span class="sxs-lookup"><span data-stu-id="6629b-117">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="6629b-118">В следующем примере атрибут `Context` элемента `RowTemplate` задает имя параметра `pet`:</span><span class="sxs-lookup"><span data-stu-id="6629b-118">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="6629b-119">Кроме того, атрибут `Context` можно задать для элемента компонента.</span><span class="sxs-lookup"><span data-stu-id="6629b-119">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="6629b-120">Заданный атрибут `Context` применяется ко всем указанным параметрам шаблона.</span><span class="sxs-lookup"><span data-stu-id="6629b-120">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="6629b-121">Это может быть полезно, если необходимо указать имя параметра содержимого для неявного дочернего содержимого (без содержащего дочернего элемента).</span><span class="sxs-lookup"><span data-stu-id="6629b-121">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="6629b-122">В следующем примере элемент `TableTemplate` имеет атрибут `Context`, который применяется ко всем параметрам шаблона:</span><span class="sxs-lookup"><span data-stu-id="6629b-122">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

## <a name="generic-typed-components"></a><span data-ttu-id="6629b-123">Компоненты универсального типа</span><span class="sxs-lookup"><span data-stu-id="6629b-123">Generic-typed components</span></span>

<span data-ttu-id="6629b-124">Шаблонные компоненты часто имеют универсальный тип.</span><span class="sxs-lookup"><span data-stu-id="6629b-124">Templated components are often generically typed.</span></span> <span data-ttu-id="6629b-125">Например, универсальный компонент `ListViewTemplate` можно использовать для отрисовки значений `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="6629b-125">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="6629b-126">Чтобы определить универсальный компонент, используйте директиву [`@typeparam`](xref:mvc/views/razor#typeparam) и укажите параметры типа:</span><span class="sxs-lookup"><span data-stu-id="6629b-126">To define a generic component, use the [`@typeparam`](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

<span data-ttu-id="6629b-127">При использовании компонентов универсального типа параметр типа выводится, если это возможно:</span><span class="sxs-lookup"><span data-stu-id="6629b-127">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```razor
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="6629b-128">В противном случае параметр типа необходимо указать явно с помощью атрибута, совпадающего с именем параметра типа.</span><span class="sxs-lookup"><span data-stu-id="6629b-128">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="6629b-129">В следующем примере `TItem="Pet"` задает тип:</span><span class="sxs-lookup"><span data-stu-id="6629b-129">In the following example, `TItem="Pet"` specifies the type:</span></span>

```razor
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```
