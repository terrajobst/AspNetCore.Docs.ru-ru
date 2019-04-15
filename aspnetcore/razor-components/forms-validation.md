---
title: Razor компоненты формы и проверка
author: guardrex
description: Сведения об использовании форм и сценарии проверки поля в компонентах Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: razor-components/forms-validation
ms.openlocfilehash: a4ed75efc6b5a733ce4ff4e82f354a8e2fd48500
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515669"
---
# <a name="razor-components-forms-and-validation"></a>Razor компоненты формы и проверка

Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)

Формы и проверка поддерживаются в Razor компонентов с помощью [заметок к данным](xref:mvc/models/validation).

> [!NOTE]
> Формы и сценарии проверки, скорее всего, можно изменить с каждым выпуском предварительной версии.

Следующие `ExampleModel` тип определяет логику проверки, с помощью заметок к данным:

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

Формы определяется с помощью `<EditForm>` компонента. Следующий вид демонстрирует типичных элементов, компоненты и кода Razor:

```csharp
<EditForm Model="@exampleModel" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" bind-Value="@exampleModel.Name" />

    <button type="submit">Submit</button>
</EditForm>

@functions {
    private ExampleModel exampleModel = new ExampleModel();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

* Формы проверяет ввод данных пользователем в `name` поле, с помощью проверки, определенные в `ExampleModel` типа. Создание модели компонента `@functions` блокировать и хранящихся в скрытом поле (`exampleModel`). Поле назначается `Model` атрибут `<EditForm>`.
* Компонент проверки данных заметок (`<DataAnnotationsValidator>`) присоединяет поддержку проверки с помощью заметок к данным.
* Сводка проверки компонента (`<ValidationSummary>`) перечислены сообщения проверки.
* `HandleValidSubmit` активируется при успешной отправки формы (этапы проверки).

Набор встроенных компонентов ввода доступны для получения и проверки пользовательского ввода. Входные данные проверяются при их при изменении и при отправке формы. В следующей таблице показаны доступные компоненты ввода.

| Входного компонента   | К просмотру как&hellip;       |
| ----------------- | ------------------------- |
| `<InputText>`     | `<input>`                 |
| `<InputTextArea>` | `<textarea>`              |
| `<InputSelect>`   | `<select>`                |
| `<InputNumber>`   | `<input type="number">`   |
| `<InputCheckbox>` | `<input type="checkbox">` |
| `<InputDate>`     | `<input type="date">`     |

Входными компонентами предоставляет поведение по умолчанию для проверки на изменения и изменения их класса CSS, для отражения состояния поля. Некоторые компоненты включают полезные логике синтаксического анализа. Например `<InputDate>` и `<InputNumber>` корректно обрабатывать аннотацию значения путем регистрации в качестве ошибки проверки. Типы, которые могут принимать значения null также поддерживать допустимость значений NULL для целевого поля (например, `int?`).

Следующие `Starship` тип определяет логику проверки, с использованием большего набора свойств и [заметок к данным](xref:mvc/models/validation) чем более ранним `ExampleModel`:

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    [Required]
    [StringLength(16, 
        ErrorMessage = "Identifier too long (16 character limit).")]
    public string Identifier { get; set; }

    // Optional (no data annotations)
    public string Description { get; set; }

    [Required]
    public string Classification { get; set; }

    [Range(1, 100000, ErrorMessage = "Accommodation invalid (1-100000).")]
    public int MaximumAccommodation { get; set; }

    [Required]
    [Range(typeof(bool), "true", "true", 
        ErrorMessage = "Form disallowed for unapproved ships.")]
    public bool IsValidatedDesign { get; set; }

    [Required]
    public DateTime ProductionDate { get; set; }
}
```

Следующую форму проверки пользовательского ввода с помощью проверки, определенные в `Starship` модели:

```cshtml
@page "/FormsValidation"

<h1>Starfleet Starship Database</h1>

<h2>New Ship Entry Form</h2>

<EditForm Model="@starship" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label for="identifier">Identifier: </label>
        <InputText id="identifier" bind-Value="@starship.Identifier" />
    </p>
    <p>
        <label for="description">Description (optional): </label>
        <InputTextArea Id="description" bind-Value="@starship.Description" />
    </p>
    <p>
        <label for="classification">Primary Classification: </label>
        <InputSelect id="classification" bind-Value="@starship.Classification">
            <option value"">Select classification ...</option>
            <option value="Defense">Defense</option>
            <option value="Exploration">Exploration</option>
            <option value="Diplomacy">Diplomacy</option>
        </InputSelect>
    </p>
    <p>
        <label for="accommodation">Maximum Accommodation: </label>
        <InputNumber id="accommodation" 
            bind-Value="@starship.MaximumAccommodation" />
    </p>
    <p>
        <label for="valid">Engineering Approval: </label>
        <InputCheckbox id="valid" bind-Value="@starship.IsValidatedDesign" />
    </p>
    <p>
        <label for="productionDate">Production Date: </label>
        <InputDate Id="productionDate" bind-Value="@starship.ProductionDate" />
    </p>

    <button type="submit">Submit</button>

    <p>
        <a href="http://www.startrek.com/">Star Trek</a>, 
        &copy;1966-2019 CBS Studios, Inc. and 
        <a href="http://www.paramount.com">Paramount Pictures</a>
    </p>
</EditForm>

@functions {
    private Starship starship = new Starship();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

`<EditForm>` Создает `EditContext` как [каскадных значение](xref:razor-components/components#cascading-values-and-parameters) метаданные в процессе редактирования, в том числе какие поля были изменены и текущих сообщений проверки, который отслеживает. `<EditForm>` Также предоставляет удобный для допустимых и недопустимых отправляет (`OnValidSubmit`, `OnInvalidSubmit`). Кроме того, использовать `OnSubmit` для запуска процедуры проверки и проверки значения поля по пользовательский код проверки.

Компонент проверки данных заметок (`<DataAnnotationsValidator>`) присоединяет поддержку проверки с помощью заметок к данным для каскадного `EditContext`. Включение поддержки для проверки, в настоящее время с помощью заметок к данным требует это явный жест, но мы рассматриваем, как сделать это поведение по умолчанию, который затем можно переопределить. Для использования системой проверки, чем заметки к данным, замените пользовательскую реализацию заметки данных проверяющего элемента управления. Реализация ASP.NET Core доступен для проверки в справочник по исходному коду: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs). *Реализация ASP.NET Core распространяется быстрые обновления во время действия предварительной версии выпуска.*

Сводка проверки компонента (`<ValidationSummary>`) перечислены все сообщения проверки, которая напоминает [вспомогательная функция тега Validation Сводка](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).

Сообщение проверки компонента (`<ValidationMessage>`) отображает сообщения проверки для конкретного поля, которая напоминает [вспомогательная функция тега сообщения проверки](xref:mvc/views/working-with-forms#the-validation-message-tag-helper). Указать поля для проверки с `For` атрибут и лямбда-выражение, предоставляющее имя для свойства модели:

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

> [!NOTE]
> Встроенные компоненты ввода имеют ограничения, которые мы предполагаем устранить в будущих выпусках. Например, нельзя указать произвольные атрибуты в созданном `<input>` теги. Создавайте собственные подклассы компонента для обработки сценариев, недоступен.
