---
title: ASP.NET Core Блазор Forms и проверка
author: guardrex
description: Узнайте, как использовать формы и сценарии проверки полей в Блазор.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/forms-validation
ms.openlocfilehash: 0b2e38cdbd974a28960b917fb6b5ce370f8c4659
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030328"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a>ASP.NET Core Блазор Forms и проверка

Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)

Формы и проверка поддерживаются в Блазор с помощью заметок к [данным](xref:mvc/models/validation).

> [!NOTE]
> С каждым предварительным выпуском, как правило, изменяются формы и сценарии проверки.

Следующий `ExampleModel` тип определяет логику проверки с помощью заметок к данным:

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

Форма определяется с помощью `EditForm` компонента. В следующей форме показаны типичные элементы, компоненты и код Razor:

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

* Форма проверяет вводимые пользователем данные в `name` поле, используя проверку, определенную `ExampleModel` в типе. Модель создается в `@code` блоке компонента и удерживается в закрытом поле (`exampleModel`). Поле присваивается `Model` атрибуту `<EditForm>` элемента.
* `DataAnnotationsValidator` Компонент прикрепляет поддержку проверки с помощью заметок к данным.
* `ValidationSummary` Компонент обобщает сообщения проверки.
* `HandleValidSubmit`активируется, когда форма успешно отправляется (проходит проверку).

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

Входные компоненты предоставляют поведение по умолчанию для проверки изменения и изменения класса CSS в соответствии с состоянием поля. Некоторые компоненты включают в себя полезную логику синтаксического анализа. Например, `InputDate` и `InputNumber` правильно обрабатывать Неанализируемые значения, регистрируя их как ошибки проверки. Типы, которые могут принимать значения NULL, также поддерживают допустимость значений NULL для целевого поля ( `int?`например,).

Следующий `Starship` тип определяет логику проверки с использованием большего набора свойств и заметок к данным, чем `ExampleModel`раньше.

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

В предыдущем примере является необязательным, `Description` так как заметки к данным отсутствуют.

Следующая форма проверяет ввод пользователя с помощью проверки, определенной в `Starship` модели:

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

`EditForm` создает `EditContext` как [каскадное значение](xref:blazor/components#cascading-values-and-parameters), отслеживающее метаданные о процессе редактирования, включая поля, которые были изменены, и текущие сообщения проверки. Также предоставляет удобные события для допустимых и недопустимых отправок (`OnValidSubmit`, `OnInvalidSubmit`). `EditForm` Кроме того, `OnSubmit` можно использовать для активации значений полей проверки и проверки с помощью пользовательского кода проверки.

Компонент прикрепляет поддержку проверки, используя заметки к данным в `EditContext`каскадных. `DataAnnotationsValidator` Включение поддержки проверки с помощью аннотаций данных в настоящее время требует этого явного жеста, но мы рассматриваем, что это поведение по умолчанию можно переопределить. Чтобы использовать систему проверки, отличную от заметок к данным `DataAnnotationsValidator` , замените на пользовательскую реализацию. Реализация ASP.NET Core доступна для проверки в эталонном источнике: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs). *Реализация ASP.NET Core подлежит быстрому обновлению в течение периода предварительной версии.*

Компонент суммирует все сообщения проверки, которые похожи на вспомогательную функцию [тега сводки проверки.](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper) `ValidationSummary`

Компонент отображает сообщения проверки для определенного поля, похожее на вспомогательную функцию [тега сообщения о проверке](xref:mvc/views/working-with-forms#the-validation-message-tag-helper). `ValidationMessage` Укажите поле для проверки с помощью `For` атрибута и лямбда-выражения с именем свойства модели:

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

Компоненты `ValidationMessage` и`ValidationSummary` поддерживают произвольные атрибуты. Любой атрибут, не совпадающий с параметром компонента, добавляется к `<div>` созданному элементу или `<ul>` .
