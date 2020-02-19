---
title: ASP.NET Core Blazor шаблонных компонентов
author: guardrex
description: Узнайте, как составные компоненты могут принимать один или несколько шаблонов пользовательского интерфейса в качестве параметров, которые затем можно использовать как часть логики отрисовки компонента.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/templated-components
ms.openlocfilehash: b64d6a731e540b13c50b2c6108f75efd0ac9290c
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453154"
---
# <a name="aspnet-core-opno-locblazor-templated-components"></a>ASP.NET Core Blazor шаблонных компонентов

[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)

Шаблонные компоненты — это компоненты, принимающие один или несколько шаблонов пользовательского интерфейса в качестве параметров, которые затем можно использовать как часть логики отрисовки компонента. Шаблонные компоненты позволяют создавать более высокие компоненты более высокого уровня, чем обычные компоненты. Ниже приведены несколько примеров.

* Компонент таблицы, позволяющий пользователю указывать шаблоны для заголовков, строк и нижних колонтитулов таблицы.
* Компонент списка, позволяющий пользователю указать шаблон для отрисовки элементов в списке.

## <a name="template-parameters"></a>Параметры шаблона

Шаблонный компонент определяется путем указания одного или нескольких параметров компонента типа `RenderFragment` или `RenderFragment<T>`. Фрагмент отрисовки представляет сегмент пользовательского интерфейса для отрисовки. `RenderFragment<T>` принимает параметр типа, который может быть указан при вызове фрагмента прорисовки.

`TableTemplate` компонент:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

При использовании компонент-шаблона можно указать параметры шаблона с помощью дочерних элементов, совпадающих с именами параметров (`TableHeader` и `RowTemplate` в следующем примере):

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

## <a name="template-context-parameters"></a>Параметры контекста шаблона

Аргументы компонента типа `RenderFragment<T>` передаются как элементы, имеют неявный параметр с именем `context` (например, из предыдущего примера кода, `@context.PetId`), но можно изменить имя параметра с помощью атрибута `Context` дочернего элемента. В следующем примере атрибут `Context` элемента `RowTemplate` указывает параметр `pet`:

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

Кроме того, можно указать атрибут `Context` в элементе Component. Указанный атрибут `Context` применяется ко всем указанным параметрам шаблона. Это может быть полезно, если необходимо указать имя параметра содержимого для неявного дочернего содержимого (без обертывания дочернего элемента). В следующем примере атрибут `Context` отображается в элементе `TableTemplate` и применяется ко всем параметрам шаблона:

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

## <a name="generic-typed-components"></a>Универсальные типы компонентов

Шаблонные компоненты часто вводятся в универсальном виде. Например, универсальный компонент `ListViewTemplate` можно использовать для отображения значений `IEnumerable<T>`. Чтобы определить универсальный компонент, используйте директиву [`@typeparam`](xref:mvc/views/razor#typeparam) для указания параметров типа:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

При использовании компонентов универсальной типизации параметр типа выводится, если это возможно:

```razor
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

В противном случае параметр типа необходимо явно указать с помощью атрибута, совпадающего с именем параметра типа. В следующем примере `TItem="Pet"` указывает тип:

```razor
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```
