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
# <a name="component-tag-helper-in-aspnet-core"></a>Вспомогательная функция тега компонента в ASP.NET Core

Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)

Чтобы отобразить компонент из страницы или представления, используйте [вспомогательную функцию тега компонента](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper).

Следующая вспомогательная функция тега Component визуализирует компонент `Counter` на странице или в представлении:

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

Вспомогательная функция тега компонента также может передавать параметры в компоненты. Рассмотрим следующий `ColorfulCheckbox` компонент, устанавливающий цвет и размер метки флажка:

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

[Параметры компонента](xref:blazor/components#component-parameters) `Size` (`int`) и `Color` (`string`) могут быть заданы вспомогательной функцией тега компонента:

```cshtml
<component type="typeof(ColorfulCheckbox)" render-mode="ServerPrerendered" 
    param-Size="14" param-Color="@("blue")" />
```

Следующий код HTML отображается на странице или в представлении:

```html
<label style="font-size:24px;color:blue">
    <input id="survey" name="blazor" type="checkbox">
    Enjoying Blazor?
</label>
```

Для передачи строки в кавычках требуется [явное выражение Razor](xref:mvc/views/razor#explicit-razor-expressions), как показано для `param-Color` в предыдущем примере. Поведение синтаксического анализа Razor для значения типа `string` не применяется к атрибуту `param-*`, так как атрибут является типом `object`.

Тип параметра должен быть JSON-сериализуемым, что обычно означает, что тип должен иметь конструктор по умолчанию и устанавливаемые свойства. Например, можно указать значение для `Size` и `Color` в предыдущем примере, так как типы `Size` и `Color` являются примитивными типами (`int` и `string`), которые поддерживаются сериализатором JSON.

Параметр <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> настраивает одно из следующих поведений компонента:

* компонент предварительно преобразуется в страницу;
* компонент отображается как статический HTML на странице или включает необходимые сведения для начальной загрузки приложения Blazor из агента пользователя.

| Режим рендеринга | Description |
| ----------- | ----------- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | Преобразует компонент в статический HTML и включает метку приложения Blazor Server. При запуске пользовательского агента эта метка используется для начальной загрузки приложения Blazor. |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | Отображает метку приложения Blazor Server. Выходные данные компонента не включаются. При запуске пользовательского агента эта метка используется для начальной загрузки приложения Blazor. |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | Преобразует компонент в статический HTML. |

Хотя страницы и представления могут использовать компоненты, обратное неверно. Компоненты не могут использовать функции представления и страницы, такие как частичные представления и разделы. Чтобы использовать логику из частичного представления в компоненте, разнесите логику частичного представления в компонент.

Отрисовка компонентов сервера из статической HTML-страницы не поддерживается.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper>
* <xref:mvc/views/tag-helpers/intro>
* <xref:blazor/components>
