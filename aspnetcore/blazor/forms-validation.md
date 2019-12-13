---
title: ASP.NET Core Blazor форм и проверка
author: guardrex
description: Узнайте, как использовать сценарии проверки форм и полей в Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/forms-validation
ms.openlocfilehash: f4c1845ee4b6ff9274b7117167367ccdd9f36c12
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2019
ms.locfileid: "74943697"
---
# <a name="aspnet-core-opno-locblazor-forms-and-validation"></a>ASP.NET Core Blazor форм и проверка

Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)

Формы и проверка поддерживаются в Blazor с помощью [заметок к данным](xref:mvc/models/validation).

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

    <InputText id="name" @bind-Value="exampleModel.Name" />

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

```razor
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

`EditForm` создает `EditContext` как [каскадное значение](xref:blazor/components#cascading-values-and-parameters), отслеживающее метаданные о процессе редактирования, включая поля, которые были изменены, и текущие сообщения проверки. `EditForm` также предоставляет удобные события для допустимых и недопустимых отправок (`OnValidSubmit`, `OnInvalidSubmit`). Кроме того, можно использовать `OnSubmit`, чтобы активировать значения полей проверки и проверки с помощью пользовательского кода проверки.

## <a name="inputtext-based-on-the-input-event"></a>Инпуттекст на основе события ввода

Используйте компонент `InputText`, чтобы создать пользовательский компонент, использующий событие `input`, а не событие `change`.

Создайте компонент со следующей разметкой и используйте компонент так же, как `InputText` используется:

```razor
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="validation-support"></a>Поддержка проверки

Компонент `DataAnnotationsValidator` прикрепляет поддержку проверки, используя заметки к данным для каскадных `EditContext`. Для включения поддержки проверки с помощью заметок к данным требуется этот явный жест. Чтобы использовать другую систему проверки по сравнению с заметками данных, замените `DataAnnotationsValidator` пользовательской реализацией. Реализация ASP.NET Core доступна для проверки в эталонном источнике: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).

Blazor выполняет два типа проверки:

* *Проверка полей* выполняется, когда пользователь выходит за пределы поля. Во время проверки поля компонент `DataAnnotationsValidator` связывает все результаты проверки с полем.
* *Проверка модели* выполняется, когда пользователь отправляет форму. Во время проверки модели компонент `DataAnnotationsValidator` пытается определить поле на основе имени члена, полученного отчетом о результатах проверки. Результаты проверки, не связанные с отдельным элементом, связаны с моделью, а не с полем.

### <a name="validation-summary-and-validation-message-components"></a>Компоненты для сообщений о проверке и сообщения проверки

Компонент `ValidationSummary` суммирует все сообщения проверки, которые похожи на [вспомогательную функцию тега сводки проверки](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):

```razor
<ValidationSummary />
```

Выходные сообщения проверки для конкретной модели с параметром `Model`:
  
```razor
<ValidationSummary Model="@starship" />
```

Компонент `ValidationMessage` отображает сообщения проверки для определенного поля, что аналогично [вспомогательной функции тега сообщения о проверке](xref:mvc/views/working-with-forms#the-validation-message-tag-helper). Укажите поле для проверки с помощью атрибута `For` и лямбда-выражения с именем свойства модели:

```razor
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

Компоненты `ValidationMessage` и `ValidationSummary` поддерживают произвольные атрибуты. Любой атрибут, который не соответствует параметру компонента, добавляется в созданный `<div>` или `<ul>` элемент.

### <a name="custom-validation-attributes"></a>Настраиваемые атрибуты проверки

Чтобы убедиться, что результат проверки правильно связан с полем при использовании [настраиваемого атрибута проверки](xref:mvc/models/validation#custom-attributes), передайте <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> контекста проверки при создании <xref:System.ComponentModel.DataAnnotations.ValidationResult>:

```csharp
using System;
using System.ComponentModel.DataAnnotations;

private class MyCustomValidator : ValidationAttribute
{
    protected override ValidationResult IsValid(object value, 
        ValidationContext validationContext)
    {
        ...

        return new ValidationResult("Validation message to user.",
            new[] { validationContext.MemberName });
    }
}
```

::: moniker range=">= aspnetcore-3.1"

### <a name="opno-locblazor-data-annotations-validation-package"></a>пакет проверки заметок к данным Blazor

Объект [Microsoft. AspNetCore.Blazor. Аннотации. Проверка](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) — это пакет, который выполняет проверку пропусков проверки с помощью компонента `DataAnnotationsValidator`. В настоящее время пакет *эксперименталь*.

### <a name="compareproperty-attribute"></a>Атрибут [Компарепроперти]

<xref:System.ComponentModel.DataAnnotations.CompareAttribute> плохо работает с компонентом `DataAnnotationsValidator`. Объект [Microsoft. AspNetCore.Blazor. Аннотации данных.](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) *экспериментальный* пакет проверки вводит дополнительный атрибут проверки `ComparePropertyAttribute`, который обходит эти ограничения. В Blazor приложении `[CompareProperty]` является непосредственной заменой атрибута `[Compare]`. Дополнительные сведения см. [в разделе компареаттрибуте Ignore with Онвалидсубмит EditForm (ASPNET/AspNetCore #10643)](https://github.com/aspnet/AspNetCore/issues/10643#issuecomment-543909748).

### <a name="nested-models-collection-types-and-complex-types"></a>Вложенные модели, типы коллекций и сложные типы

Blazor обеспечивает поддержку проверки входных данных формы с помощью заметок к данным со встроенными `DataAnnotationsValidator`. Однако `DataAnnotationsValidator` проверяет только свойства верхнего уровня модели, привязанные к форме, которая не имеет свойств сбора или сложного типа.

Чтобы проверить все графы объектов привязанной модели, в том числе свойства сбора и сложного типа, используйте `ObjectGraphDataAnnotationsValidator`, предоставляемый *экспериментальным* [Microsoft. AspNetCore.Blazor. Аннотация. пакет проверки](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) :

```razor
<EditForm Model="@model" OnValidSubmit="@HandleValidSubmit">
    <ObjectGraphDataAnnotationsValidator />
    ...
</EditForm>
```

Закомментировать свойства модели с помощью `[ValidateComplexType]`. В следующих классах модели `ShipDescription` класс содержит дополнительные заметки к данным для проверки при привязке модели к форме:

*Starship.CS*:

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    ...

    [ValidateComplexType]
    public ShipDescription ShipDescription { get; set; }

    ...
}
```

*ShipDescription.CS*:

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class ShipDescription
{
    [Required]
    [StringLength(40, ErrorMessage = "Description too long (40 char).")]
    public string ShortDescription { get; set; }
    
    [Required]
    [StringLength(240, ErrorMessage = "Description too long (240 char).")]
    public string LongDescription { get; set; }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

### <a name="validation-of-complex-or-collection-type-properties"></a>Проверка свойств Complex или типа коллекции

Атрибуты проверки, применяемые к свойствам модели, проверяются при отправке формы. Однако свойства коллекций или сложные типы данных модели не проверяются при отправке формы компонентом `DataAnnotationsValidator`. Чтобы учитывать вложенные атрибуты проверки в этом сценарии, используйте пользовательский компонент проверки. Пример см. в разделе [Пример проверкиBlazor (ASPNET/Samples)](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).

::: moniker-end
