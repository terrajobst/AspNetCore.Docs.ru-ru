---
title: Вызов методов .NET из функций JavaScript в ASP.NET Core Blazor
author: guardrex
description: Узнайте, как вызывать методы .NET из функций JavaScript в приложениях Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/19/2020
no-loc:
- Blazor
- SignalR
uid: blazor/call-dotnet-from-javascript
ms.openlocfilehash: f4964341e261c65269eedafbbd6e676c1054f427
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78647410"
---
# <a name="call-net-methods-from-javascript-functions-in-aspnet-core-opno-locblazor"></a>Вызов методов .NET из функций JavaScript в ASP.NET Core Blazor

Авторы: [Хавьер Кальварро Нельсон](https://github.com/javiercn) (Javier Calvarro Nelson), [Дэниэл Рот](https://github.com/danroth27) (Daniel Roth) и [Люк Латэм](https://github.com/guardrex) (Luke Latham)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Приложение Blazor может вызывать функции JavaScript из методов .NET и методы .NET из функций JavaScript. Такой подход называется *взаимодействием с JavaScript* (*JS*).

В этой статье рассматривается вызов методов .NET из JavaScript. Сведения о том, как вызывать функции JavaScript из .NET, см. в разделе <xref:blazor/call-javascript-from-dotnet>.

[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="static-net-method-call"></a>Вызов статического метода .NET

Чтобы вызвать статический метод .NET из JavaScript, используйте функции `DotNet.invokeMethod` или `DotNet.invokeMethodAsync`. Передайте идентификатор статического метода, который необходимо вызвать, имя сборки, содержащей функцию, и любые аргументы. Асинхронная версия является предпочтительной для поддержки сценариев Blazor Server. Метод .NET должен быть открытым, статическим и иметь атрибут `[JSInvokable]`. Вызов открытых универсальных методов в настоящее время не поддерживается.

Пример приложения включает метод C#, возвращающий массив `int`. К методу применяется атрибут `JSInvokable`.

*Pages/JsInterop.razor*:

```razor
<button type="button" class="btn btn-primary"
        onclick="exampleJsFunctions.returnArrayAsyncJs()">
    Trigger .NET static method ReturnArrayAsync
</button>

@code {
    [JSInvokable]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

JavaScript, предоставляемый клиенту, вызывает метод C# .NET.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

Нажмите кнопку **Trigger .NET static method ReturnArrayAsync** (Вызвать статический метод .NET ReturnArrayAsync) и проверьте выходные данные консоли в средствах веб-разработчика браузера.

Выходные данные консоли:

```console
Array(4) [ 1, 2, 3, 4 ]
```

Четвертое значение массива помещается в массив (`data.push(4);`), возвращаемый методом `ReturnArrayAsync`.

По умолчанию идентификатором метода является имя метода, но можно указать другой идентификатор с помощью конструктора `JSInvokableAttribute`:

```csharp
@code {
    [JSInvokable("DifferentMethodName")]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

В файле JavaScript на стороне клиента:

```javascript
returnArrayAsyncJs: function () {
  DotNet.invokeMethodAsync('BlazorSample', 'DifferentMethodName')
    .then(data => {
      data.push(4);
      console.log(data);
    });
}
```

## <a name="instance-method-call"></a>Вызов метода экземпляра

Кроме того, из JavaScript можно вызывать методы экземпляра .NET. Для этого необходимо выполнить следующие действия.

* Передайте в JavaScript экземпляр .NET по ссылке:
  * Выполните статический вызов `DotNetObjectReference.Create`.
  * Заключите экземпляр в экземпляр `DotNetObjectReference` и вызовите метод `Create` в экземпляре `DotNetObjectReference`. Удалите объекты `DotNetObjectReference` (пример приведен далее в этом разделе).
* Вызовите методы экземпляра .NET в экземпляре с помощью функций `invokeMethod` или `invokeMethodAsync`. Экземпляр .NET можно также передать в качестве аргумента при вызове других методов .NET из JavaScript.

> [!NOTE]
> Пример приложения записывает сообщения в консоль на стороне клиента. В следующих примерах, продемонстрированных в учебном приложении, изучите выходные данные консоли браузера в средствах разработчика браузера.

При нажатии на кнопку **Trigger .NET instance method HelloHelper.SayHello** (Вызвать метод экземпляра .NET HelloHelper.SayHello) вызывается метод `ExampleJsInterop.CallHelloHelperSayHello` и в метод передается имя `Blazor`.

*Pages/JsInterop.razor*:

```razor
<button type="button" class="btn btn-primary" @onclick="TriggerNetInstanceMethod">
    Trigger .NET instance method HelloHelper.SayHello
</button>

@code {
    public async Task TriggerNetInstanceMethod()
    {
        var exampleJsInterop = new ExampleJsInterop(JSRuntime);
        await exampleJsInterop.CallHelloHelperSayHello("Blazor");
    }
}
```

Метод `CallHelloHelperSayHello` вызывает функцию JavaScript `sayHello` с новым экземпляром `HelloHelper`.

*JsInteropClasses/ExampleJsInterop.cs*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

Имя передается в конструктор `HelloHelper`, который задает свойство `HelloHelper.Name`. При выполнении функции JavaScript `sayHello` метод `HelloHelper.SayHello` возвращает сообщение `Hello, {Name}!`, которое записывается в консоль функцией JavaScript.

*JsInteropClasses/HelloHelper.cs*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Выходные данные консоли в средствах веб-разработчика браузера:

```console
Hello, Blazor!
```

Чтобы избежать утечки памяти и разрешить сборку мусора для компонента, который создает метод `DotNetObjectReference`, удалите объект в классе, который создал экземпляр `DotNetObjectReference`:

```csharp
public class ExampleJsInterop : IDisposable
{
    private readonly IJSRuntime _jsRuntime;
    private DotNetObjectReference<HelloHelper> _objRef;

    public ExampleJsInterop(IJSRuntime jsRuntime)
    {
        _jsRuntime = jsRuntime;
    }

    public ValueTask<string> CallHelloHelperSayHello(string name)
    {
        _objRef = DotNetObjectReference.Create(new HelloHelper(name));

        return _jsRuntime.InvokeAsync<string>(
            "exampleJsFunctions.sayHello",
            _objRef);
    }

    public void Dispose()
    {
        _objRef?.Dispose();
    }
}
```
  
Предыдущий шаблон, показанный в классе `ExampleJsInterop`, также можно реализовать в компоненте:
  
```razor
@page "/JSInteropComponent"
@using BlazorSample.JsInteropClasses
@implements IDisposable
@inject IJSRuntime JSRuntime

<h1>JavaScript Interop</h1>

<button type="button" class="btn btn-primary" @onclick="TriggerNetInstanceMethod">
    Trigger .NET instance method HelloHelper.SayHello
</button>

@code {
    private DotNetObjectReference<HelloHelper> _objRef;

    public async Task TriggerNetInstanceMethod()
    {
        _objRef = DotNetObjectReference.Create(new HelloHelper("Blazor"));

        await JSRuntime.InvokeAsync<string>(
            "exampleJsFunctions.sayHello",
            _objRef);
    }

    public void Dispose()
    {
        _objRef?.Dispose();
    }
}
```

[!INCLUDE[Share interop code in a class library](~/includes/blazor-share-interop-code.md)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:blazor/call-javascript-from-dotnet>
* [Пример InteropComponent.razor (репозиторий GitHub dotnet/AspNetCore, ветвь выпуска 3.1)](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
* [Выполнение передачи больших объемов данных в приложениях Blazor Server](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)
