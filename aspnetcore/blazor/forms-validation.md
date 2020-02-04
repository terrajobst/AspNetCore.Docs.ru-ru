---
title: ASP.NET Core Blazor форм и проверка
author: guardrex
description: Узнайте, как использовать сценарии проверки форм и полей в Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/forms-validation
ms.openlocfilehash: 2758bcbbc76c8a59716fe224dd2deb4ca8c06929
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726891"
---
# <a name="aspnet-core-opno-locblazor-forms-and-validation"></a><span data-ttu-id="18c03-103">ASP.NET Core [!OP.NO-LOC(Blazor)] форм и проверка</span><span class="sxs-lookup"><span data-stu-id="18c03-103">ASP.NET Core [!OP.NO-LOC(Blazor)] forms and validation</span></span>

<span data-ttu-id="18c03-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="18c03-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="18c03-105">Формы и проверка поддерживаются в [!OP.NO-LOC(Blazor)] с помощью [заметок к данным](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="18c03-105">Forms and validation are supported in [!OP.NO-LOC(Blazor)] using [data annotations](xref:mvc/models/validation).</span></span>

<span data-ttu-id="18c03-106">Следующий тип `ExampleModel` определяет логику проверки с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="18c03-106">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="18c03-107">Форма определяется с помощью компонента `EditForm`.</span><span class="sxs-lookup"><span data-stu-id="18c03-107">A form is defined using the `EditForm` component.</span></span> <span data-ttu-id="18c03-108">В следующей форме показаны типичные элементы, компоненты и код Razor:</span><span class="sxs-lookup"><span data-stu-id="18c03-108">The following form demonstrates typical elements, components, and Razor code:</span></span>

```razor
<EditForm Model="@_exampleModel" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" @bind-Value="_exampleModel.Name" />

    <button type="submit">Submit</button>
</EditForm>

@code {
    private ExampleModel _exampleModel = new ExampleModel();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

<span data-ttu-id="18c03-109">В предшествующем примере:</span><span class="sxs-lookup"><span data-stu-id="18c03-109">In the preceding example:</span></span>

* <span data-ttu-id="18c03-110">Форма проверяет вводимые пользователем данные в поле `name`, используя проверку, определенную в типе `ExampleModel`.</span><span class="sxs-lookup"><span data-stu-id="18c03-110">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="18c03-111">Модель создается в блоке `@code` компонента и удерживается в частном поле (`_exampleModel`).</span><span class="sxs-lookup"><span data-stu-id="18c03-111">The model is created in the component's `@code` block and held in a private field (`_exampleModel`).</span></span> <span data-ttu-id="18c03-112">Поле присваивается атрибуту `Model` элемента `<EditForm>`.</span><span class="sxs-lookup"><span data-stu-id="18c03-112">The field is assigned to the `Model` attribute of the `<EditForm>` element.</span></span>
* <span data-ttu-id="18c03-113">`@bind-Value` привязки `InputText` компонента:</span><span class="sxs-lookup"><span data-stu-id="18c03-113">The `InputText` component's `@bind-Value` binds:</span></span>
  * <span data-ttu-id="18c03-114">Свойство модели (`_exampleModel.Name`) к свойству `Value` компонента `InputText`.</span><span class="sxs-lookup"><span data-stu-id="18c03-114">The model property (`_exampleModel.Name`) to the `InputText` component's `Value` property.</span></span>
  * <span data-ttu-id="18c03-115">Делегат события Change для свойства `ValueChanged` компонента `InputText`.</span><span class="sxs-lookup"><span data-stu-id="18c03-115">A change event delegate to the `InputText` component's `ValueChanged` property.</span></span>
* <span data-ttu-id="18c03-116">Компонент `DataAnnotationsValidator` прикрепляет поддержку проверки с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="18c03-116">The `DataAnnotationsValidator` component attaches validation support using data annotations.</span></span>
* <span data-ttu-id="18c03-117">Компонент `ValidationSummary` обобщает сообщения проверки.</span><span class="sxs-lookup"><span data-stu-id="18c03-117">The `ValidationSummary` component summarizes validation messages.</span></span>
* <span data-ttu-id="18c03-118">`HandleValidSubmit` активируется, когда форма успешно отправляется (проходит проверку).</span><span class="sxs-lookup"><span data-stu-id="18c03-118">`HandleValidSubmit` is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="18c03-119">Для получения и проверки вводимых пользователем данных доступны наборы встроенных компонентов ввода.</span><span class="sxs-lookup"><span data-stu-id="18c03-119">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="18c03-120">Входные данные проверяются при их изменении и отправке формы.</span><span class="sxs-lookup"><span data-stu-id="18c03-120">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="18c03-121">Доступные входные компоненты показаны в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="18c03-121">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="18c03-122">Входной компонент</span><span class="sxs-lookup"><span data-stu-id="18c03-122">Input component</span></span> | <span data-ttu-id="18c03-123">Отображается как&hellip;</span><span class="sxs-lookup"><span data-stu-id="18c03-123">Rendered as&hellip;</span></span>       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

<span data-ttu-id="18c03-124">Все входные компоненты, включая `EditForm`, поддерживают произвольные атрибуты.</span><span class="sxs-lookup"><span data-stu-id="18c03-124">All of the input components, including `EditForm`, support arbitrary attributes.</span></span> <span data-ttu-id="18c03-125">В отображаемый HTML-элемент добавляется любой атрибут, который не соответствует параметру компонента.</span><span class="sxs-lookup"><span data-stu-id="18c03-125">Any attribute that doesn't match a component parameter is added to the rendered HTML element.</span></span>

<span data-ttu-id="18c03-126">Входные компоненты предоставляют поведение по умолчанию для проверки изменения и изменения класса CSS в соответствии с состоянием поля.</span><span class="sxs-lookup"><span data-stu-id="18c03-126">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="18c03-127">Некоторые компоненты включают в себя полезную логику синтаксического анализа.</span><span class="sxs-lookup"><span data-stu-id="18c03-127">Some components include useful parsing logic.</span></span> <span data-ttu-id="18c03-128">Например, `InputDate` и `InputNumber` обрабатывать необрабатываемые значения надлежащим образом, регистрируя их как ошибки проверки.</span><span class="sxs-lookup"><span data-stu-id="18c03-128">For example, `InputDate` and `InputNumber` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="18c03-129">Типы, которые могут принимать значения NULL, также поддерживают допустимость значений NULL для целевого поля (например, `int?`).</span><span class="sxs-lookup"><span data-stu-id="18c03-129">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="18c03-130">Следующий тип `Starship` определяет логику проверки, используя более широкий набор свойств и заметок к данным, чем предыдущие `ExampleModel`:</span><span class="sxs-lookup"><span data-stu-id="18c03-130">The following `Starship` type defines validation logic using a larger set of properties and data annotations than the earlier `ExampleModel`:</span></span>

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

<span data-ttu-id="18c03-131">В предыдущем примере `Description` является необязательным, так как заметки к данным отсутствуют.</span><span class="sxs-lookup"><span data-stu-id="18c03-131">In the preceding example, `Description` is optional because no data annotations are present.</span></span>

<span data-ttu-id="18c03-132">Следующая форма проверяет ввод пользователя с помощью проверки, определенной в модели `Starship`:</span><span class="sxs-lookup"><span data-stu-id="18c03-132">The following form validates user input using the validation defined in the `Starship` model:</span></span>

```razor
@page "/FormsValidation"

<h1>Starfleet Starship Database</h1>

<h2>New Ship Entry Form</h2>

<EditForm Model="@_starship" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label>
            Identifier:
            <InputText @bind-Value="_starship.Identifier" />
        </label>
    </p>
    <p>
        <label>
            Description (optional):
            <InputTextArea @bind-Value="_starship.Description" />
        </label>
    </p>
    <p>
        <label>
            Primary Classification:
            <InputSelect @bind-Value="_starship.Classification">
                <option value="">Select classification ...</option>
                <option value="Exploration">Exploration</option>
                <option value="Diplomacy">Diplomacy</option>
                <option value="Defense">Defense</option>
            </InputSelect>
        </label>
    </p>
    <p>
        <label>
            Maximum Accommodation:
            <InputNumber @bind-Value="_starship.MaximumAccommodation" />
        </label>
    </p>
    <p>
        <label>
            Engineering Approval:
            <InputCheckbox @bind-Value="_starship.IsValidatedDesign" />
        </label>
    </p>
    <p>
        <label>
            Production Date:
            <InputDate @bind-Value="_starship.ProductionDate" />
        </label>
    </p>

    <button type="submit">Submit</button>

    <p>
        <a href="http://www.startrek.com/">Star Trek</a>, 
        &copy;1966-2019 CBS Studios, Inc. and 
        <a href="https://www.paramount.com">Paramount Pictures</a>
    </p>
</EditForm>

@code {
    private Starship _starship = new Starship();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

<span data-ttu-id="18c03-133">`EditForm` создает `EditContext` в виде [каскадного значения](xref:blazor/components#cascading-values-and-parameters) , отслеживающего метаданные о процессе редактирования, включая поля, которые были изменены, и текущие сообщения проверки.</span><span class="sxs-lookup"><span data-stu-id="18c03-133">The `EditForm` creates an `EditContext` as a [cascading value](xref:blazor/components#cascading-values-and-parameters) that tracks metadata about the edit process, including which fields have been modified and the current validation messages.</span></span> <span data-ttu-id="18c03-134">`EditForm` также предоставляет удобные события для допустимых и недопустимых отправок (`OnValidSubmit`, `OnInvalidSubmit`).</span><span class="sxs-lookup"><span data-stu-id="18c03-134">The `EditForm` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="18c03-135">Кроме того, можно использовать `OnSubmit`, чтобы активировать значения полей проверки и проверки с помощью пользовательского кода проверки.</span><span class="sxs-lookup"><span data-stu-id="18c03-135">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

<span data-ttu-id="18c03-136">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="18c03-136">In the following example:</span></span>

* <span data-ttu-id="18c03-137">Метод `HandleSubmit` выполняется при выборе кнопки **Отправить** .</span><span class="sxs-lookup"><span data-stu-id="18c03-137">The `HandleSubmit` method runs when the **Submit** button is selected.</span></span>
* <span data-ttu-id="18c03-138">Форма проверяется с помощью `EditContext`формы.</span><span class="sxs-lookup"><span data-stu-id="18c03-138">The form is validated using the form's `EditContext`.</span></span>
* <span data-ttu-id="18c03-139">Форма проверяется с помощью передачи `EditContext` методу `ServerValidate`, который вызывает конечную точку веб-API на сервере (*не показано*).</span><span class="sxs-lookup"><span data-stu-id="18c03-139">The form is further validated by passing the `EditContext` to the `ServerValidate` method that calls a web API endpoint on the server (*not shown*).</span></span>
* <span data-ttu-id="18c03-140">Дополнительный код выполняется в зависимости от результатов проверки на стороне клиента и сервера путем проверки `isValid`.</span><span class="sxs-lookup"><span data-stu-id="18c03-140">Additional code is run depending on the result of the client- and server-side validation by checking `isValid`.</span></span>

```razor
<EditForm EditContext="@_editContext" OnSubmit="@HandleSubmit">

    ...

    <button type="submit">Submit</button>
</EditForm>

@code {
    private Starship _starship = new Starship();
    private EditContext _editContext;

    protected override void OnInitialized()
    {
        _editContext = new EditContext(_starship);
    }

    private async Task HandleSubmit()
    {
        var isValid = _editContext.Validate() && 
            await ServerValidate(_editContext);

        if (isValid)
        {
            ...
        }
        else
        {
            ...
        }
    }

    private async Task<bool> ServerValidate(EditContext editContext)
    {
        var serverChecksValid = ...

        return serverChecksValid;
    }
}
```

## <a name="inputtext-based-on-the-input-event"></a><span data-ttu-id="18c03-141">Инпуттекст на основе события ввода</span><span class="sxs-lookup"><span data-stu-id="18c03-141">InputText based on the input event</span></span>

<span data-ttu-id="18c03-142">Используйте компонент `InputText`, чтобы создать пользовательский компонент, использующий событие `input`, а не событие `change`.</span><span class="sxs-lookup"><span data-stu-id="18c03-142">Use the `InputText` component to create a custom component that uses the `input` event instead of the `change` event.</span></span>

<span data-ttu-id="18c03-143">Создайте компонент со следующей разметкой и используйте компонент так же, как `InputText` используется:</span><span class="sxs-lookup"><span data-stu-id="18c03-143">Create a component with the following markup, and use the component just as `InputText` is used:</span></span>

```razor
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="work-with-radio-buttons"></a><span data-ttu-id="18c03-144">Работа с переключателями</span><span class="sxs-lookup"><span data-stu-id="18c03-144">Work with radio buttons</span></span>

<span data-ttu-id="18c03-145">При работе с переключателями в форме привязка данных обрабатывается иначе, чем другие элементы, так как переключатели оцениваются как группа.</span><span class="sxs-lookup"><span data-stu-id="18c03-145">When working with radio buttons in a form, data binding is handled differently than other elements because radio buttons are evaluated as a group.</span></span> <span data-ttu-id="18c03-146">Значение каждого переключателя является фиксированным, но значение группы переключателей является значением выбранного переключателя.</span><span class="sxs-lookup"><span data-stu-id="18c03-146">The value of each radio button is fixed, but the value of the radio button group is the value of the selected radio button.</span></span> <span data-ttu-id="18c03-147">В приведенном ниже примере показано, как выполнить следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="18c03-147">The following example shows how to:</span></span>

* <span data-ttu-id="18c03-148">Обрабатывает привязку данных для группы переключателей.</span><span class="sxs-lookup"><span data-stu-id="18c03-148">Handle data binding for a radio button group.</span></span>
* <span data-ttu-id="18c03-149">Поддержка проверки с помощью пользовательского компонента `InputRadio`.</span><span class="sxs-lookup"><span data-stu-id="18c03-149">Support validation using a custom `InputRadio` component.</span></span>

```razor
@using System.Globalization
@typeparam TValue
@inherits InputBase<TValue>

<input @attributes="AdditionalAttributes" type="radio" value="@SelectedValue" 
       checked="@(SelectedValue.Equals(Value))" @onchange="OnChange" />

@code {
    [Parameter]
    public TValue SelectedValue { get; set; }

    private void OnChange(ChangeEventArgs args)
    {
        CurrentValueAsString = args.Value.ToString();
    }

    protected override bool TryParseValueFromString(string value, 
        out TValue result, out string errorMessage)
    {
        var success = BindConverter.TryConvertTo<TValue>(
            value, CultureInfo.CurrentCulture, out var parsedValue);
        if (success)
        {
            result = parsedValue;
            errorMessage = null;

            return true;
        }
        else
        {
            result = default;
            errorMessage = $"{FieldIdentifier.FieldName} field isn't valid.";

            return false;
        }
    }
}
```

<span data-ttu-id="18c03-150">Следующий `EditForm` использует предыдущий компонент `InputRadio` для получения и проверки рейтинга у пользователя:</span><span class="sxs-lookup"><span data-stu-id="18c03-150">The following `EditForm` uses the preceding `InputRadio` component to obtain and validate a rating from the user:</span></span>

```razor
@page "/RadioButtonExample"
@using System.ComponentModel.DataAnnotations

<h1>Radio Button Group Test</h1>

<EditForm Model="_model" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    @for (int i = 1; i <= 5; i++)
    {
        <label>
            <InputRadio name="rate" SelectedValue="i" @bind-Value="_model.Rating" />
            @i
        </label>
    }

    <button type="submit">Submit</button>
</EditForm>

<p>You chose: @_model.Rating</p>

@code {
    private Model _model = new Model();

    private void HandleValidSubmit()
    {
        Console.WriteLine("valid");
    }

    public class Model
    {
        [Range(1, 5)]
        public int Rating { get; set; }
    }
}
```

## <a name="validation-support"></a><span data-ttu-id="18c03-151">Поддержка проверки</span><span class="sxs-lookup"><span data-stu-id="18c03-151">Validation support</span></span>

<span data-ttu-id="18c03-152">Компонент `DataAnnotationsValidator` прикрепляет поддержку проверки, используя заметки к данным для каскадных `EditContext`.</span><span class="sxs-lookup"><span data-stu-id="18c03-152">The `DataAnnotationsValidator` component attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="18c03-153">Для включения поддержки проверки с помощью заметок к данным требуется этот явный жест.</span><span class="sxs-lookup"><span data-stu-id="18c03-153">Enabling support for validation using data annotations requires this explicit gesture.</span></span> <span data-ttu-id="18c03-154">Чтобы использовать другую систему проверки по сравнению с заметками данных, замените `DataAnnotationsValidator` пользовательской реализацией.</span><span class="sxs-lookup"><span data-stu-id="18c03-154">To use a different validation system than data annotations, replace the `DataAnnotationsValidator` with a custom implementation.</span></span> <span data-ttu-id="18c03-155">Реализация ASP.NET Core доступна для проверки в источнике ссылки: [датааннотатионсвалидатор](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[адддатааннотатионсвалидатион](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="18c03-155">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span></span>

Blazor<span data-ttu-id="18c03-156"> выполняет два типа проверки:</span><span class="sxs-lookup"><span data-stu-id="18c03-156"> performs two types of validation:</span></span>

* <span data-ttu-id="18c03-157">*Проверка полей* выполняется, когда пользователь выходит за пределы поля.</span><span class="sxs-lookup"><span data-stu-id="18c03-157">*Field validation* is performed when the user tabs out of a field.</span></span> <span data-ttu-id="18c03-158">Во время проверки поля компонент `DataAnnotationsValidator` связывает все результаты проверки с полем.</span><span class="sxs-lookup"><span data-stu-id="18c03-158">During field validation, the `DataAnnotationsValidator` component associates all reported validation results with the field.</span></span>
* <span data-ttu-id="18c03-159">*Проверка модели* выполняется, когда пользователь отправляет форму.</span><span class="sxs-lookup"><span data-stu-id="18c03-159">*Model validation* is performed when the user submits the form.</span></span> <span data-ttu-id="18c03-160">Во время проверки модели компонент `DataAnnotationsValidator` пытается определить поле на основе имени члена, полученного отчетом о результатах проверки.</span><span class="sxs-lookup"><span data-stu-id="18c03-160">During model validation, the `DataAnnotationsValidator` component attempts to determine the field based on the member name that the validation result reports.</span></span> <span data-ttu-id="18c03-161">Результаты проверки, не связанные с отдельным элементом, связаны с моделью, а не с полем.</span><span class="sxs-lookup"><span data-stu-id="18c03-161">Validation results that aren't associated with an individual member are associated with the model rather than a field.</span></span>

### <a name="validation-summary-and-validation-message-components"></a><span data-ttu-id="18c03-162">Компоненты для сообщений о проверке и сообщения проверки</span><span class="sxs-lookup"><span data-stu-id="18c03-162">Validation Summary and Validation Message components</span></span>

<span data-ttu-id="18c03-163">Компонент `ValidationSummary` суммирует все сообщения проверки, которые похожи на [вспомогательную функцию тега сводки проверки](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="18c03-163">The `ValidationSummary` component summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):</span></span>

```razor
<ValidationSummary />
```

<span data-ttu-id="18c03-164">Выходные сообщения проверки для конкретной модели с параметром `Model`:</span><span class="sxs-lookup"><span data-stu-id="18c03-164">Output validation messages for a specific model with the `Model` parameter:</span></span>
  
```razor
<ValidationSummary Model="@_starship" />
```

<span data-ttu-id="18c03-165">Компонент `ValidationMessage` отображает сообщения проверки для определенного поля, что аналогично [вспомогательной функции тега сообщения о проверке](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="18c03-165">The `ValidationMessage` component displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="18c03-166">Укажите поле для проверки с помощью атрибута `For` и лямбда-выражения с именем свойства модели:</span><span class="sxs-lookup"><span data-stu-id="18c03-166">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```razor
<ValidationMessage For="@(() => _starship.MaximumAccommodation)" />
```

<span data-ttu-id="18c03-167">Компоненты `ValidationMessage` и `ValidationSummary` поддерживают произвольные атрибуты.</span><span class="sxs-lookup"><span data-stu-id="18c03-167">The `ValidationMessage` and `ValidationSummary` components support arbitrary attributes.</span></span> <span data-ttu-id="18c03-168">Любой атрибут, который не соответствует параметру компонента, добавляется в созданный `<div>` или `<ul>` элемент.</span><span class="sxs-lookup"><span data-stu-id="18c03-168">Any attribute that doesn't match a component parameter is added to the generated `<div>` or `<ul>` element.</span></span>

### <a name="custom-validation-attributes"></a><span data-ttu-id="18c03-169">Настраиваемые атрибуты проверки</span><span class="sxs-lookup"><span data-stu-id="18c03-169">Custom validation attributes</span></span>

<span data-ttu-id="18c03-170">Чтобы убедиться, что результат проверки правильно связан с полем при использовании [настраиваемого атрибута проверки](xref:mvc/models/validation#custom-attributes), передайте <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> контекста проверки при создании <xref:System.ComponentModel.DataAnnotations.ValidationResult>:</span><span class="sxs-lookup"><span data-stu-id="18c03-170">To ensure that a validation result is correctly associated with a field when using a [custom validation attribute](xref:mvc/models/validation#custom-attributes), pass the validation context's <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> when creating the <xref:System.ComponentModel.DataAnnotations.ValidationResult>:</span></span>

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

### <a name="opno-locblazor-data-annotations-validation-package"></a><span data-ttu-id="18c03-171">пакет проверки заметок к данным Blazor</span><span class="sxs-lookup"><span data-stu-id="18c03-171">Blazor data annotations validation package</span></span>

<span data-ttu-id="18c03-172">Объект [Microsoft.AspNetCore.Blazor.Аннотации.Проверка](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) — это пакет, который выполняет проверку пропусков проверки с помощью компонента `DataAnnotationsValidator`.</span><span class="sxs-lookup"><span data-stu-id="18c03-172">The [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) is a package that fills validation experience gaps using the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="18c03-173">В настоящее время пакет *эксперименталь*.</span><span class="sxs-lookup"><span data-stu-id="18c03-173">The package is currently *experimental*.</span></span>

### <a name="compareproperty-attribute"></a><span data-ttu-id="18c03-174">Атрибут [Компарепроперти]</span><span class="sxs-lookup"><span data-stu-id="18c03-174">[CompareProperty] attribute</span></span>

<span data-ttu-id="18c03-175"><xref:System.ComponentModel.DataAnnotations.CompareAttribute> плохо работает с компонентом `DataAnnotationsValidator`, так как он не связывает результат проверки с конкретным элементом.</span><span class="sxs-lookup"><span data-stu-id="18c03-175">The <xref:System.ComponentModel.DataAnnotations.CompareAttribute> doesn't work well with the `DataAnnotationsValidator` component because it doesn't associate the validation result with a specific member.</span></span> <span data-ttu-id="18c03-176">Это может привести к несогласованному поведению при проверке на уровне полей и при проверке всей модели при отправке.</span><span class="sxs-lookup"><span data-stu-id="18c03-176">This can result in inconsistent behavior between field-level validation and when the entire model is validated on a submit.</span></span> <span data-ttu-id="18c03-177">Объект [Microsoft.AspNetCore.Blazor.Аннотации данных.](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) *экспериментальный* пакет проверки вводит дополнительный атрибут проверки `ComparePropertyAttribute`, который обходит эти ограничения.</span><span class="sxs-lookup"><span data-stu-id="18c03-177">The [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) *experimental* package introduces an additional validation attribute, `ComparePropertyAttribute`, that works around these limitations.</span></span> <span data-ttu-id="18c03-178">В Blazor приложении `[CompareProperty]` является непосредственной заменой атрибута `[Compare]`.</span><span class="sxs-lookup"><span data-stu-id="18c03-178">In a Blazor app, `[CompareProperty]` is a direct replacement for the `[Compare]` attribute.</span></span>

### <a name="nested-models-collection-types-and-complex-types"></a><span data-ttu-id="18c03-179">Вложенные модели, типы коллекций и сложные типы</span><span class="sxs-lookup"><span data-stu-id="18c03-179">Nested models, collection types, and complex types</span></span>

Blazor<span data-ttu-id="18c03-180"> обеспечивает поддержку проверки входных данных формы с помощью заметок к данным со встроенными `DataAnnotationsValidator`.</span><span class="sxs-lookup"><span data-stu-id="18c03-180"> provides support for validating form input using data annotations with the built-in `DataAnnotationsValidator`.</span></span> <span data-ttu-id="18c03-181">Однако `DataAnnotationsValidator` проверяет только свойства верхнего уровня модели, привязанные к форме, которая не имеет свойств сбора или сложного типа.</span><span class="sxs-lookup"><span data-stu-id="18c03-181">However, the `DataAnnotationsValidator` only validates top-level properties of the model bound to the form that aren't collection- or complex-type properties.</span></span>

<span data-ttu-id="18c03-182">Чтобы проверить все графы объектов привязанной модели, в том числе свойства сбора и сложного типа, используйте `ObjectGraphDataAnnotationsValidator`, предоставляемый *экспериментальным* [Microsoft.AspNetCore.Blazor.Аннотация.пакет проверки](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) :</span><span class="sxs-lookup"><span data-stu-id="18c03-182">To validate the bound model's entire object graph, including collection- and complex-type properties, use the `ObjectGraphDataAnnotationsValidator` provided by the *experimental* [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) package:</span></span>

```razor
<EditForm Model="@_model" OnValidSubmit="HandleValidSubmit">
    <ObjectGraphDataAnnotationsValidator />
    ...
</EditForm>
```

<span data-ttu-id="18c03-183">Закомментировать свойства модели с помощью `[ValidateComplexType]`.</span><span class="sxs-lookup"><span data-stu-id="18c03-183">Annotate model properties with `[ValidateComplexType]`.</span></span> <span data-ttu-id="18c03-184">В следующих классах модели `ShipDescription` класс содержит дополнительные заметки к данным для проверки при привязке модели к форме:</span><span class="sxs-lookup"><span data-stu-id="18c03-184">In the following model classes, the `ShipDescription` class contains additional data annotations to validate when the model is bound to the form:</span></span>

<span data-ttu-id="18c03-185">*Starship.CS*:</span><span class="sxs-lookup"><span data-stu-id="18c03-185">*Starship.cs*:</span></span>

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

<span data-ttu-id="18c03-186">*ShipDescription.CS*:</span><span class="sxs-lookup"><span data-stu-id="18c03-186">*ShipDescription.cs*:</span></span>

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

### <a name="enable-the-submit-button-based-on-form-validation"></a><span data-ttu-id="18c03-187">Включить кнопку "Отправить" на основе проверки формы</span><span class="sxs-lookup"><span data-stu-id="18c03-187">Enable the submit button based on form validation</span></span>

<span data-ttu-id="18c03-188">Включение и отключение кнопки "Отправить" на основе проверки формы:</span><span class="sxs-lookup"><span data-stu-id="18c03-188">To enable and disable the submit button based on form validation:</span></span>

* <span data-ttu-id="18c03-189">Используйте `EditContext` формы, чтобы назначить модель при инициализации компонента.</span><span class="sxs-lookup"><span data-stu-id="18c03-189">Use the form's `EditContext` to assign the model when the component is initialized.</span></span>
* <span data-ttu-id="18c03-190">Проверьте форму в обратном вызове `OnFieldChanged` контекста, чтобы включить и отключить кнопку "Отправить".</span><span class="sxs-lookup"><span data-stu-id="18c03-190">Validate the form in the context's `OnFieldChanged` callback to enable and disable the submit button.</span></span>

```razor
<EditForm EditContext="@_editContext">
    <DataAnnotationsValidator />
    <ValidationSummary />

    ...

    <button type="submit" disabled="@_formInvalid">Submit</button>
</EditForm>

@code {
    private Starship _starship = new Starship();
    private bool _formInvalid = true;
    private EditContext _editContext;

    protected override void OnInitialized()
    {
        _editContext = new EditContext(_starship);

        _editContext.OnFieldChanged += (_, __) =>
        {
            _formInvalid = !_editContext.Validate();
            StateHasChanged();
        };
    }
}
```

<span data-ttu-id="18c03-191">В предыдущем примере задайте для `_formInvalid` значение `false`, если:</span><span class="sxs-lookup"><span data-stu-id="18c03-191">In the preceding example, set `_formInvalid` to `false` if:</span></span>

* <span data-ttu-id="18c03-192">Форма предварительно загружена с допустимыми значениями по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="18c03-192">The form is preloaded with valid default values.</span></span>
* <span data-ttu-id="18c03-193">Вы хотите, чтобы кнопка отправки была включена при загрузке формы.</span><span class="sxs-lookup"><span data-stu-id="18c03-193">You want the submit button enabled when the form loads.</span></span>

<span data-ttu-id="18c03-194">Побочным результатом предыдущего подхода является то, что `ValidationSummary` компонент заполняется недопустимыми полями после того, как пользователь взаимодействует с одним полем.</span><span class="sxs-lookup"><span data-stu-id="18c03-194">A side effect of the preceding approach is that a `ValidationSummary` component is populated with invalid fields after the user interacts with any one field.</span></span> <span data-ttu-id="18c03-195">Этот сценарий можно решить одним из следующих способов.</span><span class="sxs-lookup"><span data-stu-id="18c03-195">This scenario can be addressed in either of the following ways:</span></span>

* <span data-ttu-id="18c03-196">Не используйте компонент `ValidationSummary` в форме.</span><span class="sxs-lookup"><span data-stu-id="18c03-196">Don't use a `ValidationSummary` component on the form.</span></span>
* <span data-ttu-id="18c03-197">Сделать компонент `ValidationSummary` видимым при выборе кнопки отправки (например, в методе `HandleValidSubmit`).</span><span class="sxs-lookup"><span data-stu-id="18c03-197">Make the `ValidationSummary` component visible when the submit button is selected (for example, in a `HandleValidSubmit` method).</span></span>

```razor
<EditForm EditContext="@_editContext" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary style="@_displaySummary" />

    ...

    <button type="submit" disabled="@_formInvalid">Submit</button>
</EditForm>

@code {
    private string _displaySummary = "display:none";

    ...

    private void HandleValidSubmit()
    {
        _displaySummary = "display:block";
    }
}
```
