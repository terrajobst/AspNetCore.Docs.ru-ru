---
title: ASP.NET Core Блазор Forms и проверка
author: guardrex
description: Узнайте, как использовать формы и сценарии проверки полей в Блазор.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/15/2019
uid: blazor/forms-validation
ms.openlocfilehash: 2fd76db90a53e328cd2ac8f452fba58365db0384
ms.sourcegitcommit: dc5b293e08336dc236de66ed1834f7ef78359531
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/16/2019
ms.locfileid: "71011063"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a><span data-ttu-id="b3e25-103">ASP.NET Core Блазор Forms и проверка</span><span class="sxs-lookup"><span data-stu-id="b3e25-103">ASP.NET Core Blazor forms and validation</span></span>

<span data-ttu-id="b3e25-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b3e25-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b3e25-105">Формы и проверка поддерживаются в Блазор с помощью [заметок к данным](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="b3e25-105">Forms and validation are supported in Blazor using [data annotations](xref:mvc/models/validation).</span></span>

> [!NOTE]
> <span data-ttu-id="b3e25-106">С каждым предварительным выпуском, как правило, изменяются формы и сценарии проверки.</span><span class="sxs-lookup"><span data-stu-id="b3e25-106">Forms and validation scenarios are likely to change with each preview release.</span></span>

<span data-ttu-id="b3e25-107">Следующий `ExampleModel` тип определяет логику проверки с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="b3e25-107">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="b3e25-108">Форма определяется с помощью `EditForm` компонента.</span><span class="sxs-lookup"><span data-stu-id="b3e25-108">A form is defined using the `EditForm` component.</span></span> <span data-ttu-id="b3e25-109">В следующей форме показаны типичные элементы, компоненты и код Razor:</span><span class="sxs-lookup"><span data-stu-id="b3e25-109">The following form demonstrates typical elements, components, and Razor code:</span></span>

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

* <span data-ttu-id="b3e25-110">Форма проверяет вводимые пользователем данные в `name` поле, используя проверку, определенную `ExampleModel` в типе.</span><span class="sxs-lookup"><span data-stu-id="b3e25-110">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="b3e25-111">Модель создается в `@code` блоке компонента и удерживается в закрытом поле (`exampleModel`).</span><span class="sxs-lookup"><span data-stu-id="b3e25-111">The model is created in the component's `@code` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="b3e25-112">Поле присваивается `Model` атрибуту `<EditForm>` элемента.</span><span class="sxs-lookup"><span data-stu-id="b3e25-112">The field is assigned to the `Model` attribute of the `<EditForm>` element.</span></span>
* <span data-ttu-id="b3e25-113">`DataAnnotationsValidator` Компонент прикрепляет поддержку проверки с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="b3e25-113">The `DataAnnotationsValidator` component attaches validation support using data annotations.</span></span>
* <span data-ttu-id="b3e25-114">`ValidationSummary` Компонент обобщает сообщения проверки.</span><span class="sxs-lookup"><span data-stu-id="b3e25-114">The `ValidationSummary` component summarizes validation messages.</span></span>
* <span data-ttu-id="b3e25-115">`HandleValidSubmit`активируется, когда форма успешно отправляется (проходит проверку).</span><span class="sxs-lookup"><span data-stu-id="b3e25-115">`HandleValidSubmit` is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="b3e25-116">Для получения и проверки вводимых пользователем данных доступны наборы встроенных компонентов ввода.</span><span class="sxs-lookup"><span data-stu-id="b3e25-116">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="b3e25-117">Входные данные проверяются при их изменении и отправке формы.</span><span class="sxs-lookup"><span data-stu-id="b3e25-117">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="b3e25-118">Доступные входные компоненты показаны в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="b3e25-118">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="b3e25-119">Входной компонент</span><span class="sxs-lookup"><span data-stu-id="b3e25-119">Input component</span></span> | <span data-ttu-id="b3e25-120">Отображается как&hellip;</span><span class="sxs-lookup"><span data-stu-id="b3e25-120">Rendered as&hellip;</span></span>       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

<span data-ttu-id="b3e25-121">Все входные компоненты, включая `EditForm`, поддерживают произвольные атрибуты.</span><span class="sxs-lookup"><span data-stu-id="b3e25-121">All of the input components, including `EditForm`, support arbitrary attributes.</span></span> <span data-ttu-id="b3e25-122">В отображаемый HTML-элемент добавляется любой атрибут, который не соответствует параметру компонента.</span><span class="sxs-lookup"><span data-stu-id="b3e25-122">Any attribute that doesn't match a component parameter is added to the rendered HTML element.</span></span>

<span data-ttu-id="b3e25-123">Входные компоненты предоставляют поведение по умолчанию для проверки изменения и изменения класса CSS в соответствии с состоянием поля.</span><span class="sxs-lookup"><span data-stu-id="b3e25-123">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="b3e25-124">Некоторые компоненты включают в себя полезную логику синтаксического анализа.</span><span class="sxs-lookup"><span data-stu-id="b3e25-124">Some components include useful parsing logic.</span></span> <span data-ttu-id="b3e25-125">Например, `InputDate` и `InputNumber` правильно обрабатывать Неанализируемые значения, регистрируя их как ошибки проверки.</span><span class="sxs-lookup"><span data-stu-id="b3e25-125">For example, `InputDate` and `InputNumber` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="b3e25-126">Типы, которые могут принимать значения NULL, также поддерживают допустимость значений NULL для целевого поля ( `int?`например,).</span><span class="sxs-lookup"><span data-stu-id="b3e25-126">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="b3e25-127">Следующий `Starship` тип определяет логику проверки с использованием большего набора свойств и заметок к данным, чем `ExampleModel`раньше.</span><span class="sxs-lookup"><span data-stu-id="b3e25-127">The following `Starship` type defines validation logic using a larger set of properties and data annotations than the earlier `ExampleModel`:</span></span>

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

<span data-ttu-id="b3e25-128">В предыдущем примере является необязательным, `Description` так как заметки к данным отсутствуют.</span><span class="sxs-lookup"><span data-stu-id="b3e25-128">In the preceding example, `Description` is optional because no data annotations are present.</span></span>

<span data-ttu-id="b3e25-129">Следующая форма проверяет ввод пользователя с помощью проверки, определенной в `Starship` модели:</span><span class="sxs-lookup"><span data-stu-id="b3e25-129">The following form validates user input using the validation defined in the `Starship` model:</span></span>

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

<span data-ttu-id="b3e25-130">`EditForm` создает `EditContext` как [каскадное значение](xref:blazor/components#cascading-values-and-parameters), отслеживающее метаданные о процессе редактирования, включая поля, которые были изменены, и текущие сообщения проверки.</span><span class="sxs-lookup"><span data-stu-id="b3e25-130">The `EditForm` creates an `EditContext` as a [cascading value](xref:blazor/components#cascading-values-and-parameters) that tracks metadata about the edit process, including which fields have been modified and the current validation messages.</span></span> <span data-ttu-id="b3e25-131">Также предоставляет удобные события для допустимых и недопустимых отправок (`OnValidSubmit`, `OnInvalidSubmit`). `EditForm`</span><span class="sxs-lookup"><span data-stu-id="b3e25-131">The `EditForm` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="b3e25-132">Кроме того, `OnSubmit` можно использовать для активации значений полей проверки и проверки с помощью пользовательского кода проверки.</span><span class="sxs-lookup"><span data-stu-id="b3e25-132">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

## <a name="inputtext-based-on-the-input-event"></a><span data-ttu-id="b3e25-133">Инпуттекст на основе события ввода</span><span class="sxs-lookup"><span data-stu-id="b3e25-133">InputText based on the input event</span></span>

<span data-ttu-id="b3e25-134">Используйте компонент, чтобы создать пользовательский компонент, `input` использующий событие, а не `change` событие. `InputText`</span><span class="sxs-lookup"><span data-stu-id="b3e25-134">Use the `InputText` component to create a custom component that uses the `input` event instead of the `change` event.</span></span>

<span data-ttu-id="b3e25-135">Создайте компонент со следующей разметкой и используйте компонент так же, как `InputText` используется:</span><span class="sxs-lookup"><span data-stu-id="b3e25-135">Create a component with the following markup, and use the component just as `InputText` is used:</span></span>

```cshtml
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="validation-support"></a><span data-ttu-id="b3e25-136">Поддержка проверки</span><span class="sxs-lookup"><span data-stu-id="b3e25-136">Validation support</span></span>

<span data-ttu-id="b3e25-137">Компонент прикрепляет поддержку проверки, используя заметки к данным в `EditContext`каскадных. `DataAnnotationsValidator`</span><span class="sxs-lookup"><span data-stu-id="b3e25-137">The `DataAnnotationsValidator` component attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="b3e25-138">Для включения поддержки проверки с помощью заметок к данным требуется этот явный жест.</span><span class="sxs-lookup"><span data-stu-id="b3e25-138">Enabling support for validation using data annotations requires this explicit gesture.</span></span> <span data-ttu-id="b3e25-139">Чтобы использовать систему проверки, отличную от заметок к данным `DataAnnotationsValidator` , замените на пользовательскую реализацию.</span><span class="sxs-lookup"><span data-stu-id="b3e25-139">To use a different validation system than data annotations, replace the `DataAnnotationsValidator` with a custom implementation.</span></span> <span data-ttu-id="b3e25-140">Реализация ASP.NET Core доступна для проверки в эталонном источнике: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="b3e25-140">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span></span>

<span data-ttu-id="b3e25-141">Компонент суммирует все сообщения проверки, которые похожи на [вспомогательную функцию тега сводки проверки.](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper) `ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="b3e25-141">The `ValidationSummary` component summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span></span>

<span data-ttu-id="b3e25-142">Компонент отображает сообщения проверки для определенного поля, похожее на [вспомогательную функцию тега сообщения о проверке.](xref:mvc/views/working-with-forms#the-validation-message-tag-helper) `ValidationMessage`</span><span class="sxs-lookup"><span data-stu-id="b3e25-142">The `ValidationMessage` component displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="b3e25-143">Укажите поле для проверки с помощью `For` атрибута и лямбда-выражения с именем свойства модели:</span><span class="sxs-lookup"><span data-stu-id="b3e25-143">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

<span data-ttu-id="b3e25-144">Компоненты `ValidationMessage` и`ValidationSummary` поддерживают произвольные атрибуты.</span><span class="sxs-lookup"><span data-stu-id="b3e25-144">The `ValidationMessage` and `ValidationSummary` components support arbitrary attributes.</span></span> <span data-ttu-id="b3e25-145">Любой атрибут, не совпадающий с параметром компонента, добавляется к `<div>` созданному элементу или `<ul>` .</span><span class="sxs-lookup"><span data-stu-id="b3e25-145">Any attribute that doesn't match a component parameter is added to the generated `<div>` or `<ul>` element.</span></span>

### <a name="validation-of-complex-or-collection-type-properties"></a><span data-ttu-id="b3e25-146">Проверка свойств Complex или типа коллекции</span><span class="sxs-lookup"><span data-stu-id="b3e25-146">Validation of complex or collection type properties</span></span>

<span data-ttu-id="b3e25-147">Атрибуты проверки, применяемые к свойствам модели, проверяются при отправке формы.</span><span class="sxs-lookup"><span data-stu-id="b3e25-147">Validation attributes applied to the properties of a model validate when the form is submitted.</span></span> <span data-ttu-id="b3e25-148">Однако свойства коллекций или сложные типы данных модели не проверяются при отправке формы.</span><span class="sxs-lookup"><span data-stu-id="b3e25-148">However, the properties of collections or complex data types of a model aren't validated on form submission.</span></span> <span data-ttu-id="b3e25-149">Чтобы учитывать вложенные атрибуты проверки в этом сценарии, используйте пользовательский компонент проверки.</span><span class="sxs-lookup"><span data-stu-id="b3e25-149">To honor the nested validation attributes in this scenario, use a custom validation component.</span></span> <span data-ttu-id="b3e25-150">Пример см. в примере [проверки блазор в репозитории ASPNET/Samples GitHub](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).</span><span class="sxs-lookup"><span data-stu-id="b3e25-150">For an example, see the [Blazor Validation sample in the aspnet/samples GitHub repository](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).</span></span>
