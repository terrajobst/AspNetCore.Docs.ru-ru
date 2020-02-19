---
title: Создание и использование компонентов ASP.NET Core Razor
author: guardrex
description: Узнайте, как создавать и использовать компоненты Razor, включая способы привязки к данным, обработки событий и управления жизненным циклом компонентов.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/components
ms.openlocfilehash: f9b4eab29fafe8113528062f57d28dadd0f57577
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447104"
---
# <a name="create-and-use-aspnet-core-razor-components"></a>Создание и использование компонентов ASP.NET Core Razor

[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([как скачивать](xref:index#how-to-download-a-sample))

Blazor приложения создаются с помощью *компонентов*. Компонент — это автономный фрагмент пользовательского интерфейса, например страница, диалоговое окно или форма. Компонент включает разметку HTML и логику обработки, необходимую для вставки данных или реагирования на события пользовательского интерфейса. Компоненты являются гибкими и легковесными. Они могут быть вложенными, повторно использованы и совместно использоваться проектами.

## <a name="component-classes"></a>Классы компонентов

Компоненты реализуются в файлах компонентов [Razor](xref:mvc/views/razor) ( *. Razor*) с помощью комбинации C# и HTML-разметки. Компонент в Blazor формально называется *компонентом Razor*.

Имя компонента должно начинаться с символа верхнего регистра. Например, *микулкомпонент. Razor* является допустимым, а *микулкомпонент. Razor* является недопустимым.

Пользовательский интерфейс для компонента определяется с помощью HTML. Логика динамического отображения (например выражения, циклы и условные выражения) добавляется с помощью встроенного синтаксиса C# под названием [Razor](xref:mvc/views/razor). При компиляции приложения логика разметки и C# отрисовки HTML преобразуется в класс компонента. Имя созданного класса соответствует имени файла.

Элементы класса компонента определяются в блоке `@code`. В блоке `@code` состояние компонента (свойства, поля) указывается с помощью методов обработки событий или для определения другой логики компонента. Допускается более одного блока `@code`.

Члены компонента можно использовать как часть логики визуализации компонента с помощью C# выражений, начинающихся с `@`. Например, C# поле подготавливается к просмотру путем добавления `@` к имени поля. В следующем примере вычисляется и выводится следующее:

* `_headingFontStyle` значение свойства CSS для `font-style`.
* `_headingText` содержимое элемента `<h1>`.

```razor
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

После первоначального отображения компонента Компонент повторно создает дерево рендеринга в ответ на события. Blazor затем сравнивает новое дерево отрисовки с предыдущим и применяет любые изменения в модель DOM (DOM) браузера.

Компоненты являются обычными C# классами и могут быть помещены в любое место внутри проекта. Компоненты, создающие веб-страницы, обычно находятся в папке *страниц* . Компоненты, не являющиеся страницами, часто помещаются в *общую* папку или пользовательскую папку, добавленную в проект.

Как правило, пространство имен компонента является производным от корневого пространства имен приложения и расположения компонента (папки) в приложении. Если корневое пространство имен приложения `BlazorApp` и компонент `Counter` находится в папке *pages* :

* Пространство имен компонента `Counter` `BlazorApp.Pages`.
* Полное имя типа компонента — `BlazorApp.Pages.Counter`.

Дополнительные сведения см. в разделе [Импорт компонентов](#import-components) .

Чтобы использовать пользовательскую папку, добавьте пространство имен пользовательской папки либо в родительский компонент, либо в файл *_Imports. Razor* приложения. Например, следующее пространство имен делает компоненты в папке *Components* доступной при `BlazorApp`корневого пространства имен приложения:

```razor
@using BlazorApp.Components
```

## <a name="tag-helpers-arent-used-in-components"></a>Вспомогательные функции тегов не используются в компонентах

[Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) не поддерживаются в компонентах Razor (файлы *. Razor* ). Чтобы обеспечить функциональные возможности тегов в Blazor, создайте компонент с теми же функциональными возможностями, что и вспомогательная функция тега, и используйте вместо него компонент.

## <a name="use-components"></a>Использование компонентов

Компоненты могут включать другие компоненты, объявляя их с помощью синтаксиса HTML-элементов. Разметка для использования компонента выглядит как тег HTML с именем, соответствующем типу компонента.

Привязка атрибута чувствительна к регистру. Например, `@bind` является допустимым, а `@Bind` является недопустимым.

Следующая разметка в *index. Razor* визуализирует `HeadingComponent` экземпляр:

```razor
<HeadingComponent />
```

*Components/хеадингкомпонент. Razor*:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

Если компонент содержит HTML-элемент с первой буквой в верхнем регистре, который не соответствует имени компонента, выдается предупреждение, указывающее, что элемент имеет непредвиденное имя. Добавление директивы `@using` для пространства имен компонента делает компонент доступным, что позволяет устранить это предупреждение.

## <a name="routing"></a>Маршрутизация

Маршрутизация в Blazor достигается путем предоставления шаблона маршрута для каждого доступного компонента в приложении.

При компиляции файла Razor с директивой `@page` созданному классу присваивается <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> с указанием шаблона маршрута. Во время выполнения маршрутизатор ищет классы компонентов с `RouteAttribute` и отображает, какой из компонентов имеет шаблон маршрута, соответствующий запрошенному URL-адресу.

```razor
@page "/ParentComponent"

...
```

Дополнительные сведения см. в разделе <xref:blazor/routing>.

## <a name="parameters"></a>Параметры

### <a name="route-parameters"></a>Параметры маршрута

Компоненты могут принимать параметры маршрута из шаблона маршрута, указанного в директиве `@page`. Маршрутизатор использует параметры маршрута для заполнения соответствующих параметров компонента.

*Pages/раутепараметер. Razor*:

[!code-razor[](components/samples_snapshot/RouteParameter.razor?highlight=2,7-8)]

Необязательные параметры не поддерживаются, поэтому в предыдущем примере применяются две директивы `@page`. Первый позволяет переходить к компоненту без параметра. Вторая директива `@page` получает параметр `{text}` маршрута и присваивает значение свойству `Text`.

Синтаксис параметра *Catch-All* (`*`/`**`), который захватывает путь для нескольких папок, **не** поддерживается в компонентах Razor ( *. Razor*).

### <a name="component-parameters"></a>Параметры компонентов

Компоненты могут иметь *Параметры компонента*, которые определяются с помощью общедоступных свойств класса Component с атрибутом `[Parameter]`. Используйте атрибуты, чтобы указать аргументы для компонента в разметке.

*Components/чилдкомпонент. Razor*:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=2,11-12)]

В следующем примере из примера приложения `ParentComponent` задает значение свойства `Title` `ChildComponent`.

*Pages/паренткомпонент. Razor*:

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=5-6)]

## <a name="child-content"></a>Дочернее содержимое

Компоненты могут устанавливать содержимое другого компонента. Назначение компонента предоставляет содержимое между тегами, задающих принимающий компонент.

В следующем примере `ChildComponent` имеет свойство `ChildContent`, представляющее `RenderFragment`, которое представляет сегмент пользовательского интерфейса для отрисовки. Значение `ChildContent` размещается в разметке компонента, где должно быть визуализировано содержимое. Значение `ChildContent` получено от родительского компонента и отображается внутри `panel-body`панели начальной загрузки.

*Components/чилдкомпонент. Razor*:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> Свойству, получающему `RenderFragment`ное содержимое, необходимо присвоить имя `ChildContent` по соглашению.

`ParentComponent` в примере приложения может предоставить содержимое для подготовки к просмотру `ChildComponent`, поместив содержимое в теги `<ChildComponent>`.

*Pages/паренткомпонент. Razor*:

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a>Атрибуты Сплаттинг и произвольные параметры

Компоненты могут записывать и отображать дополнительные атрибуты в дополнение к объявленным параметрам компонента. Дополнительные атрибуты можно записать в словарь, а затем *сплаттед* на элемент при подготовке компонента к просмотру с помощью директивы [`@attributes`](xref:mvc/views/razor#attributes) Razor. Этот сценарий полезен при определении компонента, который создает элемент разметки, поддерживающий разнообразные настройки. Например, может быть утомительно определять атрибуты отдельно для `<input>`, который поддерживает много параметров.

В следующем примере первый элемент `<input>` (`id="useIndividualParams"`) использует параметры отдельных компонентов, а второй элемент `<input>` (`id="useAttributesDict"`) использует атрибут Сплаттинг:

```razor
<input id="useIndividualParams"
       maxlength="@Maxlength"
       placeholder="@Placeholder"
       required="@Required"
       size="@Size" />

<input id="useAttributesDict"
       @attributes="InputAttributes" />

@code {
    [Parameter]
    public string Maxlength { get; set; } = "10";

    [Parameter]
    public string Placeholder { get; set; } = "Input placeholder text";

    [Parameter]
    public string Required { get; set; } = "required";

    [Parameter]
    public string Size { get; set; } = "50";

    [Parameter]
    public Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "required" },
            { "size", "50" }
        };
}
```

Тип параметра должен реализовывать `IEnumerable<KeyValuePair<string, object>>` со строковыми ключами. Использование `IReadOnlyDictionary<string, object>` также является параметром в этом сценарии.

Отображаемые `<input>` элементы, использующие оба подхода, идентичны:

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">
```

Чтобы принять произвольные атрибуты, определите параметр компонента с помощью атрибута `[Parameter]` со свойством `CaptureUnmatchedValues`, для которого задано значение `true`.

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

Свойство `CaptureUnmatchedValues` в `[Parameter]` позволяет параметру сопоставлять все атрибуты, не соответствующие ни одному другому параметру. Компонент может определять только один параметр с `CaptureUnmatchedValues`. Тип свойства, используемый с `CaptureUnmatchedValues`, должен быть назначен из `Dictionary<string, object>` с строковыми ключами. в этом сценарии также используются параметры `IEnumerable<KeyValuePair<string, object>>` и `IReadOnlyDictionary<string, object>`.

Расположение `@attributes` относительно положения атрибутов элемента важно. Когда `@attributes` сплаттед для элемента, атрибуты обрабатываются справа налево (последний — первый). Рассмотрим следующий пример компонента, использующего компонент `Child`:

*Паренткомпонент. Razor*:

```razor
<ChildComponent extra="10" />
```

*Чилдкомпонент. Razor*:

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

Атрибут `extra` `Child` компонента установлен справа от `@attributes`. `<div>`, отображаемый компонентом `Parent`, содержит `extra="5"` при передаче через дополнительный атрибут, так как атрибуты обрабатываются справа налево (последний — первый):

```html
<div extra="5" />
```

В следующем примере порядок `extra` и `@attributes` отменяется в `<div>`е `Child` компонента:

*Паренткомпонент. Razor*:

```razor
<ChildComponent extra="10" />
```

*Чилдкомпонент. Razor*:

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

Отображаемые `<div>` в компоненте `Parent` содержат `extra="10"` при передаче через дополнительный атрибут:

```html
<div extra="10" />
```

## <a name="capture-references-to-components"></a>Запись ссылок на компоненты

Ссылки на компоненты предоставляют способ ссылки на экземпляр компонента, чтобы можно было выполнять команды для этого экземпляра, например `Show` или `Reset`. Чтобы записать ссылку на компонент, сделайте следующее:

* Добавьте атрибут [`@ref`](xref:mvc/views/razor#ref) в дочерний компонент.
* Определите поле с тем же типом, что и у дочернего компонента.

```razor
<MyLoginDialog @ref="_loginDialog" ... />

@code {
    private MyLoginDialog _loginDialog;

    private void OnSomething()
    {
        _loginDialog.Show();
    }
}
```

При подготовке компонента к просмотру `_loginDialog` поле заполняется экземпляром `MyLoginDialog` дочернего компонента. Затем можно вызывать методы .NET в экземпляре компонента.

> [!IMPORTANT]
> `_loginDialog` переменная заполняется только после подготовки компонента, а ее выходные данные включают элемент `MyLoginDialog`. До этого момента нет никаких ссылок на. Чтобы управлять ссылками на компоненты после завершения подготовки компонента к просмотру, используйте [методы онафтеррендерасинк или онафтеррендер](xref:blazor/lifecycle#after-component-render).

При захвате ссылок на компоненты используется аналогичный синтаксис для [записи ссылок на элементы](xref:blazor/javascript-interop#capture-references-to-elements), но это не функция [взаимодействия JavaScript](xref:blazor/javascript-interop) . Ссылки на компоненты не передаются в код JavaScript&mdash;они используются только в коде .NET.

> [!NOTE]
> **Не** используйте ссылки на компоненты для изменения состояния дочерних компонентов. Вместо этого используйте обычные декларативные параметры для передачи данных дочерним компонентам. Использование обычных декларативных параметров приводит к тому, что дочерние компоненты автоматически визуализируются в нужное время.

## <a name="invoke-component-methods-externally-to-update-state"></a>Вызывать методы компонента извне для обновления состояния

Blazor использует `SynchronizationContext` для принудительного применения одного логического потока выполнения. В этом `SynchronizationContext`выполняются [методы жизненного цикла](xref:blazor/lifecycle) компонента и любые обратные вызовы событий, вызванные Blazor. В случае, если компонент должен быть обновлен на основе внешнего события, такого как таймер или другие уведомления, используйте метод `InvokeAsync`, который будет отправляться в `SynchronizationContext`Blazor.

Например, рассмотрим *службу уведомления* , которая может уведомлять любой компонент, выполняющий прослушивание, в обновленном состоянии:

```csharp
public class NotifierService
{
    // Can be called from anywhere
    public async Task Update(string key, int value)
    {
        if (Notify != null)
        {
            await Notify.Invoke(key, value);
        }
    }

    public event Func<string, int, Task> Notify;
}
```

Зарегистрируйте `NotifierService` как синглетион:

* В Blazor сборки Зарегистрируйте службу в `Program.Main`:

  ```csharp
  builder.Services.AddSingleton<NotifierService>();
  ```

* В Blazor Server Зарегистрируйте службу в `Startup.ConfigureServices`:

  ```csharp
  services.AddSingleton<NotifierService>();
  ```

Используйте `NotifierService` для обновления компонента:

```razor
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @_lastNotification.key = @_lastNotification.value</p>

@code {
    private (string key, int value) _lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            _lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

В предыдущем примере `NotifierService` вызывает метод `OnNotify` компонента за пределами Blazor`SynchronizationContext`. `InvokeAsync` используется для переключения на правильный контекст и постановка в очередь визуализации.

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a>Использование ключа \@для управления сохранением элементов и компонентов

При последующем изменении списка элементов или компонентов, а также элементов или компонентов алгоритм сравнения Blazorдолжен решить, какие из предыдущих элементов или компонентов могут быть сохранены и как объекты модели должны сопоставляться с ними. Обычно этот процесс выполняется автоматически и его можно игнорировать, но в некоторых случаях может потребоваться управление процессом.

Рассмотрим следующий пример:

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

Содержимое коллекции `People` может изменяться при вставке, удалении или повторном упорядочении записей. Когда компонент перерисовывается, компонент `<DetailsEditor>` может измениться на получение различных значений параметров `Details`. Это может привести к более сложной визуализации, чем ожидалось. В некоторых случаях перерисовка может привести к появлению видимых различий в поведении, таких как потеря фокуса элементов.

Процесс сопоставления можно контролировать с помощью атрибута директивы `@key`. `@key` заставляет алгоритм сравнения гарантировать сохранение элементов или компонентов на основе значения ключа:

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="person" Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

При изменении коллекции `People` алгоритм сравнения удерживает связи между экземплярами `<DetailsEditor>` и экземплярами `person`.

* Если `Person` удаляется из списка `People`, то из пользовательского интерфейса удаляется только соответствующий экземпляр `<DetailsEditor>`. Другие экземпляры остаются без изменений.
* Если в какой-либо позиции в списке вставляется `Person`, то в соответствующей позиции вставляется один новый экземпляр `<DetailsEditor>`. Другие экземпляры остаются без изменений.
* При повторном упорядочении записей `Person` соответствующие экземпляры `<DetailsEditor>` сохраняются и переупорядочиваются в пользовательском интерфейсе.

В некоторых сценариях использование `@key` уменьшает сложность повторного отображения и позволяет избежать потенциальных проблем с элементами, изменяющимися с отслеживанием состояния DOM, например положением фокуса.

> [!IMPORTANT]
> Ключи являются локальными для каждого элемента или компонента контейнера. Ключи не сравниваются глобально по всему документу.

### <a name="when-to-use-key"></a>Когда следует использовать ключ \@

Как правило, имеет смысл использовать `@key` всякий раз при подготовке списка (например, в блоке `@foreach`) и при наличии подходящего значения для определения `@key`.

Можно также использовать `@key`, чтобы запретить Blazor сохранение поддерева элемента или компонента при изменении объекта.

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

Если `@currentPerson` меняется, директива атрибута `@key` заставляет Blazor отбросить все `<div>` и его потомков и перестроить поддерево в пользовательском интерфейсе с помощью новых элементов и компонентов. Это может быть полезно, если необходимо гарантировать, что при `@currentPerson` изменений состояние пользовательского интерфейса не сохраняется.

### <a name="when-not-to-use-key"></a>Когда не следует использовать ключ \@

При использовании сравнения с `@key`ми возникают затраты на производительность. Затраты на производительность не слишком велики, но указываются только `@key` если управление правилами сохранения элементов или компонентов выгодно для приложения.

Даже если `@key` не используется, Blazor сохраняет как можно больше экземпляров дочернего элемента и компонента. Единственным преимуществом использования `@key` является управление *тем, как* экземпляры модели сопоставляются с сохраненными экземплярами компонента, а не алгоритмом сравнения, который выбирает сопоставление.

### <a name="what-values-to-use-for-key"></a>Какие значения следует использовать для ключа \@

Как правило, имеет смысл указать одно из следующих значений для `@key`:

* Экземпляры объектов Model (например, экземпляр `Person`, как в предыдущем примере). Это гарантирует сохранение на основе равенства ссылок на объекты.
* Уникальные идентификаторы (например, значения первичного ключа типа `int`, `string`или `Guid`).

Убедитесь, что значения, используемые для `@key`, не конфликтуют. Если конфликтные значения обнаруживаются в одном родительском элементе, Blazor создает исключение, поскольку оно не может детерминированно сопоставлять старые элементы или компоненты с новыми элементами или компонентами. Используйте только уникальные значения, такие как экземпляры объекта или значения первичного ключа.

## <a name="partial-class-support"></a>Поддержка разделяемых классов

Компоненты Razor создаются как разделяемые классы. Компоненты Razor создаются с помощью любого из следующих подходов:

* C#код определяется в [`@code`](xref:mvc/views/razor#code) блоке с разметкой HTML и кодом Razor в одном файле. с помощью этого подхода шаблоны Blazor определяют свои компоненты Razor.
* C#код помещается в файл кода программной части, определенный как разделяемый класс.

В следующем примере показан компонент `Counter` по умолчанию с блоком `@code` в приложении, созданном из шаблона Blazor. Разметка HTML, код Razor и C# код находятся в одном файле:

*Counter. Razor*:

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int _currentCount = 0;

    void IncrementCount()
    {
        _currentCount++;
    }
}
```

`Counter` компонент можно также создать с помощью файла кода программной части с разделяемым классом:

*Counter. Razor*:

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

*Counter.Razor.CS*:

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        private int _currentCount = 0;

        void IncrementCount()
        {
            _currentCount++;
        }
    }
}
```

При необходимости добавьте необходимые пространства имен в файл разделяемого класса. К типичным пространствам имен, используемым компонентами Razor, относятся:

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

## <a name="specify-a-base-class"></a>Укажите базовый класс

Директиву [`@inherits`](xref:mvc/views/razor#inherits) можно использовать, чтобы указать базовый класс для компонента. В следующем примере показано, как компонент может наследовать базовый класс `BlazorRocksBase`, чтобы предоставить свойства и методы компонента. Базовый класс должен быть производным от `ComponentBase`.

*Pages/блазорроккс. Razor*:

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

*BlazorRocksBase.CS*:

```csharp
using Microsoft.AspNetCore.Components;

namespace BlazorSample
{
    public class BlazorRocksBase : ComponentBase
    {
        public string BlazorRocksText { get; set; } = 
            "Blazor rocks the browser!";
    }
}
```

## <a name="specify-an-attribute"></a>Укажите атрибут

Атрибуты можно указать в компонентах Razor с помощью директивы [`@attribute`](xref:mvc/views/razor#attribute) . В следующем примере атрибут `[Authorize]` применяется к классу Component:

```razor
@page "/"
@attribute [Authorize]
```

## <a name="import-components"></a>Импорт компонентов

Пространство имен компонента, созданного с помощью Razor, основано на (в порядке приоритета):

* [`@namespace`е](xref:mvc/views/razor#namespace) обозначение в разметке файла Razor ( *. Razor*) (`@namespace BlazorSample.MyNamespace`).
* `RootNamespace` проекта в файле проекта (`<RootNamespace>BlazorSample</RootNamespace>`).
* Имя проекта, полученное из имени файла проекта (*CSPROJ*), и путь из корневого каталога проекта к компоненту. Например, платформа разрешает *{root Project}/Пажес/индекс.Разор* (*блазорсампле. csproj*) к пространству имен `BlazorSample.Pages`. Компоненты следуют C# правилам привязки имен. Для компонента `Index` в этом примере компоненты в области являются всеми компонентами:
  * В той же папке *страницы*.
  * Компоненты в корне проекта, которые не задают явно другое пространство имен.

Компоненты, определенные в другом пространстве имен, помещаются в область с помощью директивы [`@using`](xref:mvc/views/razor#using) Razor.

Если другой компонент, `NavMenu.razor`, существует в папке *блазорсампле/Shared/* Folder, компонент можно использовать в `Index.razor` со следующей инструкцией `@using`:

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

На компоненты также можно ссылаться с помощью полных имен, для которых не требуется директива [`@using`](xref:mvc/views/razor#using) :

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> Квалификация `global::` не поддерживается.
>
> Импорт компонентов с псевдонимами `using` операторы (например, `@using Foo = Bar`) не поддерживается.
>
> Частично определенные имена не поддерживаются. Например, добавление `@using BlazorSample` и ссылки на `NavMenu.razor` с `<Shared.NavMenu></Shared.NavMenu>` не поддерживается.

## <a name="conditional-html-element-attributes"></a>Атрибуты условного HTML-элемента

Атрибуты HTML-элемента условно подготавливаются в зависимости от значения .NET. Если значение равно `false` или `null`, то атрибут не отображается. Если значение равно `true`, атрибут отображается в виде сворачивания.

В следующем примере `IsCompleted` определяет, отображается ли `checked` в разметке элемента:

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

Если `IsCompleted` `true`, флажок отображается следующим образом:

```html
<input type="checkbox" checked />
```

Если `IsCompleted` `false`, флажок отображается следующим образом:

```html
<input type="checkbox" />
```

Дополнительные сведения см. в разделе <xref:mvc/views/razor>.

> [!WARNING]
> Некоторые атрибуты HTML, такие как [ARIA-Pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), не работают должным образом, если тип .net является `bool`ом. В этих случаях используйте `string` тип вместо `bool`.

## <a name="raw-html"></a>Необработанный HTML

Строки обычно подготавливаются с помощью текстовых узлов DOM, что означает, что любая разметка, которую они могут содержать, игнорируется и обрабатывается как литеральный текст. Для отрисовки необработанного HTML-кода заключите содержимое HTML в `MarkupString` значение. Значение анализируется как HTML или SVG и вставляется в модель DOM.

> [!WARNING]
> Визуализация необработанного HTML-кода, созданного из любого ненадежного источника, является **угрозой безопасности** , и ее следует избегать.

В следующем примере показано использование типа `MarkupString` для добавления блока статического HTML-содержимого в отображаемые выходные данные компонента:

```html
@((MarkupString)_myMarkup)

@code {
    private string _myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="cascading-values-and-parameters"></a>Каскадные значения и параметры

В некоторых сценариях неудобно передавать данные из компонента-предка в дочерний компонент с помощью [параметров компонента](#component-parameters), особенно при наличии нескольких слоев компонентов. Каскадные значения и параметры позволяют решить эту проблему, предоставляя удобный способ, позволяющий компоненту-предку предоставить значение всем его дочерним компонентам. Каскадные значения и параметры также предоставляют подход для координации компонентов.

### <a name="theme-example"></a>Пример темы

В следующем примере из примера приложения класс `ThemeInfo` указывает сведения о теме для перетекания иерархии компонентов, чтобы все кнопки в пределах данной части приложения совпадали с тем же стилем.

*Уисемеклассес/themeinfo указывает расположение. CS*:

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

Компонент-предок может предоставлять каскадное значение с помощью компонента каскадного значения. Компонент `CascadingValue` упаковывает поддерево иерархии компонентов и предоставляет одно значение всем компонентам в этом поддереве.

Например, в примере приложения указываются сведения о теме (`ThemeInfo`) в одном из макетов приложения в виде каскадного параметра для всех компонентов, составляющих текст макета свойства `@Body`. `ButtonClass` присваивается значение `btn-success` в компоненте макета. Любой компонент-потомок может использовать это свойство через `ThemeInfo` каскадный объект.

`CascadingValuesParametersLayout` компонент:

```razor
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="_theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo _theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

Чтобы использовать каскадные значения, компоненты объявляют каскадные параметры с помощью атрибута `[CascadingParameter]`. Каскадные значения привязываются к каскадным параметрам по типу.

В примере приложения компонент `CascadingValuesParametersTheme` привязывает `ThemeInfo` каскадное значение к каскадному параметру. Параметр используется для задания класса CSS для одной из кнопок, отображаемых компонентом.

`CascadingValuesParametersTheme` компонент:

```razor
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @_currentCount</p>

<p>
    <button class="btn" @onclick="IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" @onclick="IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@code {
    private int _currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        _currentCount++;
    }
}
```

Чтобы вычислить несколько значений одного типа в одном поддереве, укажите уникальную строку `Name` для каждого компонента `CascadingValue` и соответствующего `CascadingParameter`. В следующем примере два `CascadingValue` компонентов каскадом разворачивают разные экземпляры `MyCascadingType` по имени:

```razor
<CascadingValue Value=@_parentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType _parentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

В компоненте-потомках каскадные параметры получают значения из соответствующих каскадных значений в компоненте-предке по имени:

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a>Пример Табсет

Каскадные параметры также позволяют компонентам работать в иерархии компонентов. Например, рассмотрим следующий пример *табсет* в примере приложения.

В примере приложения имеется интерфейс `ITab`, который реализуется вкладками:

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

Компонент `CascadingValuesParametersTabSet` использует компонент `TabSet`, который содержит несколько `Tab` компонентов:

```razor
<TabSet>
    <Tab Title="First tab">
        <h4>Greetings from the first tab!</h4>

        <label>
            <input type="checkbox" @bind="showThirdTab" />
            Toggle third tab
        </label>
    </Tab>
    <Tab Title="Second tab">
        <h4>The second tab says Hello World!</h4>
    </Tab>

    @if (showThirdTab)
    {
        <Tab Title="Third tab">
            <h4>Welcome to the disappearing third tab!</h4>
            <p>Toggle this tab from the first tab.</p>
        </Tab>
    }
</TabSet>
```

Дочерние `Tab` компоненты явно не передаются в качестве параметров в `TabSet`. Вместо этого дочерние `Tab` компоненты являются частью дочернего содержимого `TabSet`. Однако `TabSet` по-прежнему необходимо узнать о каждом компоненте `Tab`, чтобы он мог отобразить заголовки и активную вкладку. Чтобы обеспечить эту координацию без необходимости дополнительного кода, компонент `TabSet` *может предоставить себя как каскадное значение* , которое затем будет выбиралось компонентами-потомками `Tab`.

`TabSet` компонент:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

Дочерние `Tab` компоненты захватывают содержащий `TabSet` в виде каскадного параметра, поэтому `Tab` компоненты добавляются в `TabSet` и координирует, какая вкладка активна.

`Tab` компонент:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a>Шаблоны Razor

Фрагменты визуализации можно определить с помощью синтаксиса шаблона Razor. Шаблоны Razor позволяют определить фрагмент пользовательского интерфейса и принять следующий формат:

```razor
@<{HTML tag}>...</{HTML tag}>
```

В следующем примере показано, как указать значения `RenderFragment` и `RenderFragment<T>` и визуализировать шаблоны непосредственно в компоненте. Фрагменты рендеринга также могут передаваться в качестве аргументов в [шаблонные компоненты](xref:blazor/templated-components).

```razor
@_timeTemplate

@_petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment _timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> _petTemplate = (pet) => @<p>Pet: @pet.Name</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

Отображаемые выходные данные предыдущего кода:

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Pet: Rex</p>
```

## <a name="scalable-vector-graphics-svg-images"></a>Масштабируемые изображения векторной графики (SVG)

Поскольку Blazor отображает HTML-файлы, поддерживаемые браузером изображения, включая масштабируемые векторные графики (SVG *),* поддерживаются с помощью тега `<img>`:

```html
<img alt="Example image" src="some-image.svg" />
```

Аналогичным образом SVG-изображения поддерживаются в правилах CSS файла стилей ( *. CSS*):

```css
.my-element {
    background-image: url("some-image.svg");
}
```

Однако встроенная разметка SVG не поддерживается во всех сценариях. Если поместить тег `<svg>` непосредственно в файл компонента (*Razor*), то поддерживается базовая визуализация изображений, но многие расширенные сценарии пока не поддерживаются. Например, `<use>` теги в настоящее время не учитываются, и `@bind` нельзя использовать с некоторыми тегами SVG. В будущем выпуске мы планируем устранить эти ограничения.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:security/blazor/server> &ndash; содержит рекомендации по созданию серверных приложений Blazor, которые должны конкурировать с нехваткой ресурсов.
