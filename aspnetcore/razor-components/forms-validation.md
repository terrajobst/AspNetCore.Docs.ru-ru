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
# <a name="razor-components-forms-and-validation"></a><span data-ttu-id="9da33-103">Razor компоненты формы и проверка</span><span class="sxs-lookup"><span data-stu-id="9da33-103">Razor Components forms and validation</span></span>

<span data-ttu-id="9da33-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9da33-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9da33-105">Формы и проверка поддерживаются в Razor компонентов с помощью [заметок к данным](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="9da33-105">Forms and validation are supported in Razor Components using [data annotations](xref:mvc/models/validation).</span></span>

> [!NOTE]
> <span data-ttu-id="9da33-106">Формы и сценарии проверки, скорее всего, можно изменить с каждым выпуском предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="9da33-106">Forms and validation scenarios are likely to change with each preview release.</span></span>

<span data-ttu-id="9da33-107">Следующие `ExampleModel` тип определяет логику проверки, с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="9da33-107">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="9da33-108">Формы определяется с помощью `<EditForm>` компонента.</span><span class="sxs-lookup"><span data-stu-id="9da33-108">A form is defined using the `<EditForm>` component.</span></span> <span data-ttu-id="9da33-109">Следующий вид демонстрирует типичных элементов, компоненты и кода Razor:</span><span class="sxs-lookup"><span data-stu-id="9da33-109">The following form demonstrates typical elements, components, and Razor code:</span></span>

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

* <span data-ttu-id="9da33-110">Формы проверяет ввод данных пользователем в `name` поле, с помощью проверки, определенные в `ExampleModel` типа.</span><span class="sxs-lookup"><span data-stu-id="9da33-110">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="9da33-111">Создание модели компонента `@functions` блокировать и хранящихся в скрытом поле (`exampleModel`).</span><span class="sxs-lookup"><span data-stu-id="9da33-111">The model is created in the component's `@functions` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="9da33-112">Поле назначается `Model` атрибут `<EditForm>`.</span><span class="sxs-lookup"><span data-stu-id="9da33-112">The field is assigned to the `Model` attribute of the `<EditForm>`.</span></span>
* <span data-ttu-id="9da33-113">Компонент проверки данных заметок (`<DataAnnotationsValidator>`) присоединяет поддержку проверки с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="9da33-113">The Data Annotations Validator component (`<DataAnnotationsValidator>`) attaches validation support using data annotations.</span></span>
* <span data-ttu-id="9da33-114">Сводка проверки компонента (`<ValidationSummary>`) перечислены сообщения проверки.</span><span class="sxs-lookup"><span data-stu-id="9da33-114">The Validation Summary component (`<ValidationSummary>`) summarizes validation messages.</span></span>
* `HandleValidSubmit` <span data-ttu-id="9da33-115">активируется при успешной отправки формы (этапы проверки).</span><span class="sxs-lookup"><span data-stu-id="9da33-115">is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="9da33-116">Набор встроенных компонентов ввода доступны для получения и проверки пользовательского ввода.</span><span class="sxs-lookup"><span data-stu-id="9da33-116">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="9da33-117">Входные данные проверяются при их при изменении и при отправке формы.</span><span class="sxs-lookup"><span data-stu-id="9da33-117">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="9da33-118">В следующей таблице показаны доступные компоненты ввода.</span><span class="sxs-lookup"><span data-stu-id="9da33-118">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="9da33-119">Входного компонента</span><span class="sxs-lookup"><span data-stu-id="9da33-119">Input component</span></span>   | <span data-ttu-id="9da33-120">К просмотру как&hellip;</span><span class="sxs-lookup"><span data-stu-id="9da33-120">Rendered as&hellip;</span></span>       |
| ----------------- | ------------------------- |
| `<InputText>`     | `<input>`                 |
| `<InputTextArea>` | `<textarea>`              |
| `<InputSelect>`   | `<select>`                |
| `<InputNumber>`   | `<input type="number">`   |
| `<InputCheckbox>` | `<input type="checkbox">` |
| `<InputDate>`     | `<input type="date">`     |

<span data-ttu-id="9da33-121">Входными компонентами предоставляет поведение по умолчанию для проверки на изменения и изменения их класса CSS, для отражения состояния поля.</span><span class="sxs-lookup"><span data-stu-id="9da33-121">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="9da33-122">Некоторые компоненты включают полезные логике синтаксического анализа.</span><span class="sxs-lookup"><span data-stu-id="9da33-122">Some components include useful parsing logic.</span></span> <span data-ttu-id="9da33-123">Например `<InputDate>` и `<InputNumber>` корректно обрабатывать аннотацию значения путем регистрации в качестве ошибки проверки.</span><span class="sxs-lookup"><span data-stu-id="9da33-123">For example, `<InputDate>` and `<InputNumber>` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="9da33-124">Типы, которые могут принимать значения null также поддерживать допустимость значений NULL для целевого поля (например, `int?`).</span><span class="sxs-lookup"><span data-stu-id="9da33-124">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="9da33-125">Следующие `Starship` тип определяет логику проверки, с использованием большего набора свойств и [заметок к данным](xref:mvc/models/validation) чем более ранним `ExampleModel`:</span><span class="sxs-lookup"><span data-stu-id="9da33-125">The following `Starship` type defines validation logic using a larger set of properties and [data annotations](xref:mvc/models/validation) than the earlier `ExampleModel`:</span></span>

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

<span data-ttu-id="9da33-126">Следующую форму проверки пользовательского ввода с помощью проверки, определенные в `Starship` модели:</span><span class="sxs-lookup"><span data-stu-id="9da33-126">The following form validates user input using the validation defined in the `Starship` model:</span></span>

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

<span data-ttu-id="9da33-127">`<EditForm>` Создает `EditContext` как [каскадных значение](xref:razor-components/components#cascading-values-and-parameters) метаданные в процессе редактирования, в том числе какие поля были изменены и текущих сообщений проверки, который отслеживает.</span><span class="sxs-lookup"><span data-stu-id="9da33-127">The `<EditForm>` creates an `EditContext` as a [cascading value](xref:razor-components/components#cascading-values-and-parameters) that tracks metadata about the edit process, including what fields have been modified and the current validation messages.</span></span> <span data-ttu-id="9da33-128">`<EditForm>` Также предоставляет удобный для допустимых и недопустимых отправляет (`OnValidSubmit`, `OnInvalidSubmit`).</span><span class="sxs-lookup"><span data-stu-id="9da33-128">The `<EditForm>` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="9da33-129">Кроме того, использовать `OnSubmit` для запуска процедуры проверки и проверки значения поля по пользовательский код проверки.</span><span class="sxs-lookup"><span data-stu-id="9da33-129">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

<span data-ttu-id="9da33-130">Компонент проверки данных заметок (`<DataAnnotationsValidator>`) присоединяет поддержку проверки с помощью заметок к данным для каскадного `EditContext`.</span><span class="sxs-lookup"><span data-stu-id="9da33-130">The Data Annotations Validator component (`<DataAnnotationsValidator>`) attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="9da33-131">Включение поддержки для проверки, в настоящее время с помощью заметок к данным требует это явный жест, но мы рассматриваем, как сделать это поведение по умолчанию, который затем можно переопределить.</span><span class="sxs-lookup"><span data-stu-id="9da33-131">Enabling support for validation using data annotations currently requires this explicit gesture, but we're considering making this the default behavior that you can then override.</span></span> <span data-ttu-id="9da33-132">Для использования системой проверки, чем заметки к данным, замените пользовательскую реализацию заметки данных проверяющего элемента управления.</span><span class="sxs-lookup"><span data-stu-id="9da33-132">To use a different validation system than data annotations, replace the Data Annotations Validator with a custom implementation.</span></span> <span data-ttu-id="9da33-133">Реализация ASP.NET Core доступен для проверки в справочник по исходному коду: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="9da33-133">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs).</span></span> *<span data-ttu-id="9da33-134">Реализация ASP.NET Core распространяется быстрые обновления во время действия предварительной версии выпуска.</span><span class="sxs-lookup"><span data-stu-id="9da33-134">The ASP.NET Core implementation is subject to rapid updates during the preview release period.</span></span>*

<span data-ttu-id="9da33-135">Сводка проверки компонента (`<ValidationSummary>`) перечислены все сообщения проверки, которая напоминает [вспомогательная функция тега Validation Сводка](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="9da33-135">The Validation Summary component (`<ValidationSummary>`) summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span></span>

<span data-ttu-id="9da33-136">Сообщение проверки компонента (`<ValidationMessage>`) отображает сообщения проверки для конкретного поля, которая напоминает [вспомогательная функция тега сообщения проверки](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="9da33-136">The Validation Message component (`<ValidationMessage>`) displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="9da33-137">Указать поля для проверки с `For` атрибут и лямбда-выражение, предоставляющее имя для свойства модели:</span><span class="sxs-lookup"><span data-stu-id="9da33-137">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

> [!NOTE]
> <span data-ttu-id="9da33-138">Встроенные компоненты ввода имеют ограничения, которые мы предполагаем устранить в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="9da33-138">Built-in input components have limitations that we expect to resolve in future releases.</span></span> <span data-ttu-id="9da33-139">Например, нельзя указать произвольные атрибуты в созданном `<input>` теги.</span><span class="sxs-lookup"><span data-stu-id="9da33-139">For example, you can't specify arbitrary attributes on the generated `<input>` tags.</span></span> <span data-ttu-id="9da33-140">Создавайте собственные подклассы компонента для обработки сценариев, недоступен.</span><span class="sxs-lookup"><span data-stu-id="9da33-140">Build your own component subclasses to handle unavailable scenarios.</span></span>
