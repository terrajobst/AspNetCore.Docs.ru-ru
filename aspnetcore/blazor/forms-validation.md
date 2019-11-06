---
title: ASP.NET Core Блазор Forms и проверка
author: guardrex
description: Узнайте, как использовать формы и сценарии проверки полей в Блазор.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/04/2019
uid: blazor/forms-validation
ms.openlocfilehash: 09281779e7f0b31e525e0e79c2d6d9ce9ca5b8ce
ms.sourcegitcommit: 6628cd23793b66e4ce88788db641a5bbf470c3c1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73659786"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a>ASP.NET Core Блазор Forms и проверка

Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)

Формы и проверка поддерживаются в Блазор с помощью [заметок к данным](xref:mvc/models/validation).

Следующий тип `ExampleModel` определяет логику проверки с помощью заметок к данным:

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

Форма определяется с помощью компонента `EditForm`. В следующей форме показаны типичные элементы, компоненты и код Razor:

```csharp
<EditForm Model="@exampleModel" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" @bind-Value="@exampleModel.Name" />

    <button type="submit">Submit</button>
</EditForm>

@code {
    private ExampleModel exampleModel = new ExampleModel();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

* Форма проверяет вводимые пользователем данные в поле `name`, используя проверку, определенную в типе `ExampleModel`. Модель создается в блоке `@code` компонента и удерживается в частном поле (`exampleModel`). Поле присваивается атрибуту `Model` элемента `<EditForm>`.
* Компонент `DataAnnotationsValidator` прикрепляет поддержку проверки с помощью заметок к данным.
* Компонент `ValidationSummary` обобщает сообщения проверки.
* `HandleValidSubmit` активируется, когда форма успешно отправляется (проходит проверку).

Для получения и проверки вводимых пользователем данных доступны наборы встроенных компонентов ввода. Входные данные проверяются при их изменении и отправке формы. Доступные входные компоненты показаны в следующей таблице.

| Входной компонент | Отображается как&hellip;       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

Все входные компоненты, включая `EditForm`, поддерживают произвольные атрибуты. В отображаемый HTML-элемент добавляется любой атрибут, который не соответствует параметру компонента.

Входные компоненты предоставляют поведение по умолчанию для проверки изменения и изменения класса CSS в соответствии с состоянием поля. Некоторые компоненты включают в себя полезную логику синтаксического анализа. Например, `InputDate` и `InputNumber` обрабатывать необрабатываемые значения надлежащим образом, регистрируя их как ошибки проверки. Типы, которые могут принимать значения NULL, также поддерживают допустимость значений NULL для целевого поля (например, `int?`).

Следующий тип `Starship` определяет логику проверки, используя более широкий набор свойств и заметок к данным, чем предыдущие `ExampleModel`:

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    [Required]
    [StringLength(16, ErrorMessage = "Identifier too long (16 character limit).")]
    public string Identifier { get; set; }

    public string Description { get; set; }

    [Required]
    public string Classification { get; set; }

    [Range(1, 100000, ErrorMessage = "Accommodation invalid (1-100000).")]
    public int MaximumAccommodation { get; set; }

    [Required]
    [Range(typeof(bool), "true", "true", 
        ErrorMessage = "This form disallows unapproved ships.")]
    public bool IsValidatedDesign { get; set; }

    [Required]
    public DateTime ProductionDate { get; set; }
}
```

В предыдущем примере `Description` является необязательным, так как заметки к данным отсутствуют.

Следующая форма проверяет ввод пользователя с помощью проверки, определенной в модели `Starship`:

```cshtml
@page "/FormsValidation"

<h1>Starfleet Starship Database</h1>

<h2>New Ship Entry Form</h2>

<EditForm Model="@starship" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label for="identifier">Identifier: </label>
        <InputText id="identifier" @bind-Value="starship.Identifier" />
    </p>
    <p>
        <label for="description">Description (optional): </label>
        <InputTextArea id="description" @bind-Value="starship.Description" />
    </p>
    <p>
        <label for="classification">Primary Classification: </label>
        <InputSelect id="classification" @bind-Value="starship.Classification">
            <option value="">Select classification ...</option>
            <option value="Exploration">Exploration</option>
            <option value="Diplomacy">Diplomacy</option>
            <option value="Defense">Defense</option>
        </InputSelect>
    </p>
    <p>
        <label for="accommodation">Maximum Accommodation: </label>
        <InputNumber id="accommodation" 
            @bind-Value="starship.MaximumAccommodation" />
    </p>
    <p>
        <label for="valid">Engineering Approval: </label>
        <InputCheckbox id="valid" @bind-Value="starship.IsValidatedDesign" />
    </p>
    <p>
        <label for="productionDate">Production Date: </label>
        <InputDate id="productionDate" @bind-Value="starship.ProductionDate" />
    </p>

    <button type="submit">Submit</button>

    <p>
        <a href="http://www.startrek.com/">Star Trek</a>, 
        &copy;1966-2019 CBS Studios, Inc. and 
        <a href="https://www.paramount.com">Paramount Pictures</a>
    </p>
</EditForm>

@code {
    private Starship starship = new Starship();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

`EditForm` создает `EditContext` в виде [каскадного значения](xref:blazor/components#cascading-values-and-parameters) , отслеживающего метаданные о процессе редактирования, включая поля, которые были изменены, и текущие сообщения проверки. `EditForm` также предоставляет удобные события для допустимых и недопустимых отправок (`OnValidSubmit`, `OnInvalidSubmit`). Кроме того, можно использовать `OnSubmit`, чтобы активировать значения полей проверки и проверки с помощью пользовательского кода проверки.

## <a name="inputtext-based-on-the-input-event"></a>Инпуттекст на основе события ввода

Используйте компонент `InputText`, чтобы создать пользовательский компонент, использующий событие `input`, а не событие `change`.

Создайте компонент со следующей разметкой и используйте компонент так же, как `InputText` используется:

```cshtml
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="validation-support"></a>Поддержка проверки

Компонент `DataAnnotationsValidator` прикрепляет поддержку проверки, используя заметки к данным для каскадных `EditContext`. Для включения поддержки проверки с помощью заметок к данным требуется этот явный жест. Чтобы использовать другую систему проверки по сравнению с заметками данных, замените `DataAnnotationsValidator` пользовательской реализацией. Реализация ASP.NET Core доступна для проверки в источнике ссылки: [датааннотатионсвалидатор](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[адддатааннотатионсвалидатион](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).

Компонент `ValidationSummary` суммирует все сообщения проверки, которые похожи на [вспомогательную функцию тега сводки проверки](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).

Компонент `ValidationMessage` отображает сообщения проверки для определенного поля, что аналогично [вспомогательной функции тега сообщения о проверке](xref:mvc/views/working-with-forms#the-validation-message-tag-helper). Укажите поле для проверки с помощью атрибута `For` и лямбда-выражения с именем свойства модели:

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

Компоненты `ValidationMessage` и `ValidationSummary` поддерживают произвольные атрибуты. Любой атрибут, который не соответствует параметру компонента, добавляется в созданный `<div>` или `<ul>` элемент.

::: moniker range=">= aspnetcore-3.1"

**Microsoft. AspNetCore. Блазор. Аннотацияs. пакет проверки**

[Примечания Microsoft. AspNetCore. блазор. Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) — это пакет, который выполняет проверку пропусков проверки с помощью компонента `DataAnnotationsValidator`. В настоящее время пакет *эксперименталь*, и мы планируем добавить эти сценарии в ASP.NET Coreную платформу в будущем выпуске.

Компонент `DataAnnotationsValidator` не проверяет вложенные свойства сложных свойств в проверяющей модели. Элементы свойств типа коллекции не проверяются. Для проверки этих типов пакет `Microsoft.AspNetCore.Blazor.DataAnnotations.Validation` вводит атрибут проверки `ValidateComplexType`, который работает в сочетании с компонентом `ObjectGraphDataAnnotationsValidator`. Пример использования этих типов см. в [примере проверки блазор в репозитории ASPNET/Samples GitHub ](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).

<xref:System.ComponentModel.DataAnnotations.CompareAttribute> плохо работает с компонентом `DataAnnotationsValidator`. Пакет `Microsoft.AspNetCore.Blazor.DataAnnotations.Validation` предоставляет дополнительный атрибут проверки `ComparePropertyAttribute`, который обходит эти ограничения. В приложении Блазор `ComparePropertyAttribute` является непосредственной заменой `CompareAttribute`. Дополнительные сведения см. [в разделе компареаттрибуте Ignore with Онвалидсубмит EditForm (ASPNET/AspNetCore \#10643)](https://github.com/aspnet/AspNetCore/issues/10643#issuecomment-543909748).

::: moniker-end

::: moniker range="< aspnetcore-3.1"

### <a name="validation-of-complex-or-collection-type-properties"></a>Проверка свойств Complex или типа коллекции

Атрибуты проверки, применяемые к свойствам модели, проверяются при отправке формы. Однако свойства коллекций или сложные типы данных модели не проверяются при отправке формы компонентом `DataAnnotationsValidator`. Чтобы учитывать вложенные атрибуты проверки в этом сценарии, используйте пользовательский компонент проверки. Пример см. в примере [проверки блазор в репозитории ASPNET/Samples GitHub](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).

::: moniker-end
