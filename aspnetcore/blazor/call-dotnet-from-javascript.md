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
# <a name="call-net-methods-from-javascript-functions-in-aspnet-core-opno-locblazor"></a><span data-ttu-id="a67fb-103">Вызов методов .NET из функций JavaScript в ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="a67fb-103">Call .NET methods from JavaScript functions in ASP.NET Core Blazor</span></span>

<span data-ttu-id="a67fb-104">Авторы: [Хавьер Кальварро Нельсон](https://github.com/javiercn) (Javier Calvarro Nelson), [Дэниэл Рот](https://github.com/danroth27) (Daniel Roth) и [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="a67fb-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="a67fb-105">Приложение Blazor может вызывать функции JavaScript из методов .NET и методы .NET из функций JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a67fb-105">A Blazor app can invoke JavaScript functions from .NET methods and .NET methods from JavaScript functions.</span></span> <span data-ttu-id="a67fb-106">Такой подход называется *взаимодействием с JavaScript* (*JS*).</span><span class="sxs-lookup"><span data-stu-id="a67fb-106">These scenarios are called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="a67fb-107">В этой статье рассматривается вызов методов .NET из JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a67fb-107">This article covers invoking .NET methods from JavaScript.</span></span> <span data-ttu-id="a67fb-108">Сведения о том, как вызывать функции JavaScript из .NET, см. в разделе <xref:blazor/call-javascript-from-dotnet>.</span><span class="sxs-lookup"><span data-stu-id="a67fb-108">For information on how to call JavaScript functions from .NET, see <xref:blazor/call-javascript-from-dotnet>.</span></span>

<span data-ttu-id="a67fb-109">[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a67fb-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="static-net-method-call"></a><span data-ttu-id="a67fb-110">Вызов статического метода .NET</span><span class="sxs-lookup"><span data-stu-id="a67fb-110">Static .NET method call</span></span>

<span data-ttu-id="a67fb-111">Чтобы вызвать статический метод .NET из JavaScript, используйте функции `DotNet.invokeMethod` или `DotNet.invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="a67fb-111">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="a67fb-112">Передайте идентификатор статического метода, который необходимо вызвать, имя сборки, содержащей функцию, и любые аргументы.</span><span class="sxs-lookup"><span data-stu-id="a67fb-112">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="a67fb-113">Асинхронная версия является предпочтительной для поддержки сценариев Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="a67fb-113">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="a67fb-114">Метод .NET должен быть открытым, статическим и иметь атрибут `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="a67fb-114">The .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="a67fb-115">Вызов открытых универсальных методов в настоящее время не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="a67fb-115">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="a67fb-116">Пример приложения включает метод C#, возвращающий массив `int`.</span><span class="sxs-lookup"><span data-stu-id="a67fb-116">The sample app includes a C# method to return an `int` array.</span></span> <span data-ttu-id="a67fb-117">К методу применяется атрибут `JSInvokable`.</span><span class="sxs-lookup"><span data-stu-id="a67fb-117">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="a67fb-118">*Pages/JsInterop.razor*:</span><span class="sxs-lookup"><span data-stu-id="a67fb-118">*Pages/JsInterop.razor*:</span></span>

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

<span data-ttu-id="a67fb-119">JavaScript, предоставляемый клиенту, вызывает метод C# .NET.</span><span class="sxs-lookup"><span data-stu-id="a67fb-119">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="a67fb-120">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="a67fb-120">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="a67fb-121">Нажмите кнопку **Trigger .NET static method ReturnArrayAsync** (Вызвать статический метод .NET ReturnArrayAsync) и проверьте выходные данные консоли в средствах веб-разработчика браузера.</span><span class="sxs-lookup"><span data-stu-id="a67fb-121">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="a67fb-122">Выходные данные консоли:</span><span class="sxs-lookup"><span data-stu-id="a67fb-122">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="a67fb-123">Четвертое значение массива помещается в массив (`data.push(4);`), возвращаемый методом `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="a67fb-123">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

<span data-ttu-id="a67fb-124">По умолчанию идентификатором метода является имя метода, но можно указать другой идентификатор с помощью конструктора `JSInvokableAttribute`:</span><span class="sxs-lookup"><span data-stu-id="a67fb-124">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor:</span></span>

```csharp
@code {
    [JSInvokable("DifferentMethodName")]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

<span data-ttu-id="a67fb-125">В файле JavaScript на стороне клиента:</span><span class="sxs-lookup"><span data-stu-id="a67fb-125">In the client-side JavaScript file:</span></span>

```javascript
returnArrayAsyncJs: function () {
  DotNet.invokeMethodAsync('BlazorSample', 'DifferentMethodName')
    .then(data => {
      data.push(4);
      console.log(data);
    });
}
```

## <a name="instance-method-call"></a><span data-ttu-id="a67fb-126">Вызов метода экземпляра</span><span class="sxs-lookup"><span data-stu-id="a67fb-126">Instance method call</span></span>

<span data-ttu-id="a67fb-127">Кроме того, из JavaScript можно вызывать методы экземпляра .NET.</span><span class="sxs-lookup"><span data-stu-id="a67fb-127">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="a67fb-128">Для этого необходимо выполнить следующие действия.</span><span class="sxs-lookup"><span data-stu-id="a67fb-128">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="a67fb-129">Передайте в JavaScript экземпляр .NET по ссылке:</span><span class="sxs-lookup"><span data-stu-id="a67fb-129">Pass the .NET instance by reference to JavaScript:</span></span>
  * <span data-ttu-id="a67fb-130">Выполните статический вызов `DotNetObjectReference.Create`.</span><span class="sxs-lookup"><span data-stu-id="a67fb-130">Make a static call to `DotNetObjectReference.Create`.</span></span>
  * <span data-ttu-id="a67fb-131">Заключите экземпляр в экземпляр `DotNetObjectReference` и вызовите метод `Create` в экземпляре `DotNetObjectReference`.</span><span class="sxs-lookup"><span data-stu-id="a67fb-131">Wrap the instance in a `DotNetObjectReference` instance and call `Create` on the `DotNetObjectReference` instance.</span></span> <span data-ttu-id="a67fb-132">Удалите объекты `DotNetObjectReference` (пример приведен далее в этом разделе).</span><span class="sxs-lookup"><span data-stu-id="a67fb-132">Dispose of `DotNetObjectReference` objects (an example appears later in this section).</span></span>
* <span data-ttu-id="a67fb-133">Вызовите методы экземпляра .NET в экземпляре с помощью функций `invokeMethod` или `invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="a67fb-133">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="a67fb-134">Экземпляр .NET можно также передать в качестве аргумента при вызове других методов .NET из JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a67fb-134">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="a67fb-135">Пример приложения записывает сообщения в консоль на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="a67fb-135">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="a67fb-136">В следующих примерах, продемонстрированных в учебном приложении, изучите выходные данные консоли браузера в средствах разработчика браузера.</span><span class="sxs-lookup"><span data-stu-id="a67fb-136">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="a67fb-137">При нажатии на кнопку **Trigger .NET instance method HelloHelper.SayHello** (Вызвать метод экземпляра .NET HelloHelper.SayHello) вызывается метод `ExampleJsInterop.CallHelloHelperSayHello` и в метод передается имя `Blazor`.</span><span class="sxs-lookup"><span data-stu-id="a67fb-137">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="a67fb-138">*Pages/JsInterop.razor*:</span><span class="sxs-lookup"><span data-stu-id="a67fb-138">*Pages/JsInterop.razor*:</span></span>

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

<span data-ttu-id="a67fb-139">Метод `CallHelloHelperSayHello` вызывает функцию JavaScript `sayHello` с новым экземпляром `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="a67fb-139">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="a67fb-140">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="a67fb-140">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="a67fb-141">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="a67fb-141">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="a67fb-142">Имя передается в конструктор `HelloHelper`, который задает свойство `HelloHelper.Name`.</span><span class="sxs-lookup"><span data-stu-id="a67fb-142">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="a67fb-143">При выполнении функции JavaScript `sayHello` метод `HelloHelper.SayHello` возвращает сообщение `Hello, {Name}!`, которое записывается в консоль функцией JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a67fb-143">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="a67fb-144">*JsInteropClasses/HelloHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="a67fb-144">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="a67fb-145">Выходные данные консоли в средствах веб-разработчика браузера:</span><span class="sxs-lookup"><span data-stu-id="a67fb-145">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

<span data-ttu-id="a67fb-146">Чтобы избежать утечки памяти и разрешить сборку мусора для компонента, который создает метод `DotNetObjectReference`, удалите объект в классе, который создал экземпляр `DotNetObjectReference`:</span><span class="sxs-lookup"><span data-stu-id="a67fb-146">To avoid a memory leak and allow garbage collection on a component that creates a `DotNetObjectReference`, dispose of the object in the class that created the `DotNetObjectReference` instance:</span></span>

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
  
<span data-ttu-id="a67fb-147">Предыдущий шаблон, показанный в классе `ExampleJsInterop`, также можно реализовать в компоненте:</span><span class="sxs-lookup"><span data-stu-id="a67fb-147">The preceding pattern shown in the `ExampleJsInterop` class can also be implemented in a component:</span></span>
  
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

## <a name="additional-resources"></a><span data-ttu-id="a67fb-148">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a67fb-148">Additional resources</span></span>

* <xref:blazor/call-javascript-from-dotnet>
* [<span data-ttu-id="a67fb-149">Пример InteropComponent.razor (репозиторий GitHub dotnet/AspNetCore, ветвь выпуска 3.1)</span><span class="sxs-lookup"><span data-stu-id="a67fb-149">InteropComponent.razor example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
* <span data-ttu-id="a67fb-150">[Выполнение передачи больших объемов данных в приложениях Blazor Server](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)</span><span class="sxs-lookup"><span data-stu-id="a67fb-150">[Perform large data transfers in Blazor Server apps](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)</span></span>
