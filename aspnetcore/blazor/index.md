---
title: Общие сведения об использовании Blazor в ASP.NET Core
author: guardrex
description: Узнайте больше об использовании Blazor в ASP.NET Core для создания интерактивного клиентского веб-интерфейса с помощью .NET в приложении ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 09/05/2019
uid: blazor/index
ms.openlocfilehash: 767ec8f106bebb92cf13a10eb63fab4905715d3d
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2019
ms.locfileid: "70964110"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="21cc1-103">Введение в Blazor</span><span class="sxs-lookup"><span data-stu-id="21cc1-103">Introduction to Blazor</span></span>

<span data-ttu-id="21cc1-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="21cc1-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="21cc1-105">*Добро пожаловать в Blazor!*</span><span class="sxs-lookup"><span data-stu-id="21cc1-105">*Welcome to Blazor!*</span></span>

<span data-ttu-id="21cc1-106">Платформа Blazor предназначена для создания интерактивного веб-интерфейса на стороне клиента для .NET и предлагает следующие возможности:</span><span class="sxs-lookup"><span data-stu-id="21cc1-106">Blazor is a framework for building interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="21cc1-107">создание многофункциональных интерактивных пользовательских интерфейсов на C# вместо JavaScript;</span><span class="sxs-lookup"><span data-stu-id="21cc1-107">Create rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="21cc1-108">совместное использование серверной и клиентской логик приложений, написанных с помощью .NET;</span><span class="sxs-lookup"><span data-stu-id="21cc1-108">Share server-side and client-side app logic written in .NET.</span></span>
* <span data-ttu-id="21cc1-109">отображение пользовательского интерфейса в виде HTML-страницы с CSS для широкой поддержки браузеров, в том числе для мобильных устройств.</span><span class="sxs-lookup"><span data-stu-id="21cc1-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="21cc1-110">Использование .NET для разработки веб-приложений на стороне клиента предоставляет следующие преимущества:</span><span class="sxs-lookup"><span data-stu-id="21cc1-110">Using .NET for client-side web development offers the following advantages:</span></span>

* <span data-ttu-id="21cc1-111">создавайте код на C#, а не на JavaScript.</span><span class="sxs-lookup"><span data-stu-id="21cc1-111">Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="21cc1-112">возможность использования существующей экосистемы .NET с библиотеками .NET;</span><span class="sxs-lookup"><span data-stu-id="21cc1-112">Leverage the existing .NET ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="21cc1-113">сохранение единой логики приложений для сервера и клиента;</span><span class="sxs-lookup"><span data-stu-id="21cc1-113">Share app logic across server and client.</span></span>
* <span data-ttu-id="21cc1-114">высокая производительность, надежность и безопасность платформы .NET;</span><span class="sxs-lookup"><span data-stu-id="21cc1-114">Benefit from .NET's performance, reliability, and security.</span></span>
* <span data-ttu-id="21cc1-115">сохраняйте высокий уровень продуктивности с помощью Visual Studio в Windows, Linux и macOS.</span><span class="sxs-lookup"><span data-stu-id="21cc1-115">Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="21cc1-116">создавайте приложения на основе распространенных языков, платформ и инструментов, которые отличаются стабильностью, широким набором функций и простотой в использовании.</span><span class="sxs-lookup"><span data-stu-id="21cc1-116">Build on a common set of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

## <a name="components"></a><span data-ttu-id="21cc1-117">Компоненты</span><span class="sxs-lookup"><span data-stu-id="21cc1-117">Components</span></span>

<span data-ttu-id="21cc1-118">Приложения Blazor основаны на *компонентах*.</span><span class="sxs-lookup"><span data-stu-id="21cc1-118">Blazor apps are based on *components*.</span></span> <span data-ttu-id="21cc1-119">Компонентом Blazor называется любой элемент пользовательского интерфейса, например страница, диалоговое окно или форма ввода данных.</span><span class="sxs-lookup"><span data-stu-id="21cc1-119">A component in Blazor is an element of UI, such as a page, dialog, or data entry form.</span></span>

<span data-ttu-id="21cc1-120">Компоненты являются классами .NET, встроенными в сборки .NET, которые:</span><span class="sxs-lookup"><span data-stu-id="21cc1-120">Components are .NET classes built into .NET assemblies that:</span></span>

* <span data-ttu-id="21cc1-121">определяют гибкую логику визуализации пользовательского интерфейса;</span><span class="sxs-lookup"><span data-stu-id="21cc1-121">Define flexible UI rendering logic.</span></span>
* <span data-ttu-id="21cc1-122">обрабатывают действия пользователя;</span><span class="sxs-lookup"><span data-stu-id="21cc1-122">Handle user events.</span></span>
* <span data-ttu-id="21cc1-123">можно вкладывать друг в друга и использовать повторно;</span><span class="sxs-lookup"><span data-stu-id="21cc1-123">Can be nested and reused.</span></span>
* <span data-ttu-id="21cc1-124">можно совместно использовать и распространять в виде [библиотек классов Razor](xref:razor-pages/ui-class) или [пакетов NuGet](/nuget/what-is-nuget).</span><span class="sxs-lookup"><span data-stu-id="21cc1-124">Can be shared and distributed as [Razor class libraries](xref:razor-pages/ui-class) or [NuGet packages](/nuget/what-is-nuget).</span></span>

<span data-ttu-id="21cc1-125">Класс компонента обычно записывается в виде страницы разметки [Razor](xref:mvc/views/razor) с расширением файла *.razor*.</span><span class="sxs-lookup"><span data-stu-id="21cc1-125">The component class is usually written in the form of a [Razor](xref:mvc/views/razor) markup page with a *.razor* file extension.</span></span> <span data-ttu-id="21cc1-126">Обычно компоненты в Blazor называются *компонентами Razor*.</span><span class="sxs-lookup"><span data-stu-id="21cc1-126">Components in Blazor are formally referred to as *Razor components*.</span></span> <span data-ttu-id="21cc1-127">Синтаксис Razor сочетает разметку HTML с кодом C#, позволяя повысить производительность разработчиков.</span><span class="sxs-lookup"><span data-stu-id="21cc1-127">Razor is a syntax for combining HTML markup with C# code designed for developer productivity.</span></span> <span data-ttu-id="21cc1-128">Razor позволяет переключаться между разметкой HTML и кодом C# в одном файле с поддержкой [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="21cc1-128">Razor allows you to switch between HTML markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="21cc1-129">Синтаксис Razor применяется также для Razor Pages и MVC.</span><span class="sxs-lookup"><span data-stu-id="21cc1-129">Razor Pages and MVC also use Razor.</span></span> <span data-ttu-id="21cc1-130">В отличие от Razor Pages и MVC, которые построены по модели "запрос-ответ", компоненты используются специально для логики пользовательского интерфейса на стороне клиента и для компоновки.</span><span class="sxs-lookup"><span data-stu-id="21cc1-130">Unlike Razor Pages and MVC, which are built around a request/response model, components are used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="21cc1-131">Следующая разметка Razor определяет компонент *Dialog.razor*, который может быть вложен в другой компонент:</span><span class="sxs-lookup"><span data-stu-id="21cc1-131">The following Razor markup demonstrates a component (*Dialog.razor*), which can be nested within another component:</span></span>

```cshtml
<div>
    <h1>@Title</h1>

    @ChildContent

    <button @onclick="OnYes">Yes!</button>
</div>

@code {
    [Parameter]
    public string Title { get; set; }

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    private void OnYes()
    {
        Console.WriteLine("Write to the console in C#! 'Yes' button was selected.");
    }
}
```

<span data-ttu-id="21cc1-132">Содержимое текста (`ChildContent`) и заголовка (`Title`) диалогового окна предоставляются компонентом, который использует этот компонент в пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="21cc1-132">The dialog's body content (`ChildContent`) and title (`Title`) are provided by the component that uses this component in its UI.</span></span> <span data-ttu-id="21cc1-133">Метод C# `OnYes` инициируется событием кнопки `onclick`.</span><span class="sxs-lookup"><span data-stu-id="21cc1-133">`OnYes` is a C# method triggered by the button's `onclick` event.</span></span>

<span data-ttu-id="21cc1-134">Blazor использует естественные теги HTML для компоновки пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="21cc1-134">Blazor uses natural HTML tags for UI composition.</span></span> <span data-ttu-id="21cc1-135">Элементы HTML определяют компоненты, а атрибуты тегов передают значения для свойств компонентов.</span><span class="sxs-lookup"><span data-stu-id="21cc1-135">HTML elements specify components, and a tag's attributes pass values to a component's properties.</span></span>

<span data-ttu-id="21cc1-136">В следующем примере компонент `Index` использует компонент `Dialog`.</span><span class="sxs-lookup"><span data-stu-id="21cc1-136">In the following example, the `Index` component uses the `Dialog` component.</span></span> <span data-ttu-id="21cc1-137">Значения `ChildContent` и `Title` определяются атрибутами и содержимым элемента `<Dialog>`.</span><span class="sxs-lookup"><span data-stu-id="21cc1-137">`ChildContent` and `Title` are set by the attributes and content of the `<Dialog>` element.</span></span>

<span data-ttu-id="21cc1-138">*Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="21cc1-138">*Index.razor*:</span></span>

```cshtml
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

<span data-ttu-id="21cc1-139">Диалоговое окно отображается, когда к родительскому компоненту (*Index.razor*) осуществляется доступ в браузере:</span><span class="sxs-lookup"><span data-stu-id="21cc1-139">The dialog is rendered when the parent (*Index.razor*) is accessed in a browser:</span></span>

![Компонент диалогового окна, отображаемый в браузере](index/_static/dialog.png)

<span data-ttu-id="21cc1-141">Если этот компонент используется в приложении, технология IntelliSense в [Visual Studio](/visualstudio/ide/using-intellisense) и [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) ускоряет разработку благодаря завершению синтаксиса и параметров.</span><span class="sxs-lookup"><span data-stu-id="21cc1-141">When this component is used in the app, IntelliSense in [Visual Studio](/visualstudio/ide/using-intellisense) and [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="21cc1-142">Компоненты преобразуются в хранящееся в памяти представление модели DOM для браузера, которое называется *деревом отображения*, позволяя гибко и эффективно обновлять пользовательский интерфейс.</span><span class="sxs-lookup"><span data-stu-id="21cc1-142">Components render into an in-memory representation of the browser's Document Object Model (DOM) called a *render tree*, which is used to update the UI in a flexible and efficient way.</span></span>

## <a name="blazor-webassembly"></a><span data-ttu-id="21cc1-143">Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="21cc1-143">Blazor WebAssembly</span></span>

<span data-ttu-id="21cc1-144">Blazor WebAssembly — это платформа для одностраничных приложений, которая позволяет создавать интерактивные клиентские веб-приложения с помощью .NET.</span><span class="sxs-lookup"><span data-stu-id="21cc1-144">Blazor WebAssembly is a single-page app framework for building interactive client-side web apps with .NET.</span></span> <span data-ttu-id="21cc1-145">Blazor WebAssembly использует открытые веб-стандарты без подключаемых модулей или транскомпиляции кода. Эта платформа работает во всех современных веб-браузерах, в том числе на мобильных устройствах.</span><span class="sxs-lookup"><span data-stu-id="21cc1-145">Blazor WebAssembly uses open web standards without plugins or code transpilation and works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="21cc1-146">Выполнение кода .NET в веб-браузерах становится возможным благодаря технологии [WebAssembly](https://webassembly.org) (сокращенно *wasm*).</span><span class="sxs-lookup"><span data-stu-id="21cc1-146">Running .NET code inside web browsers is made possible by [WebAssembly](https://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="21cc1-147">WebAssembly — это компактный формат байт-кода, оптимизированный для быстрой загрузки и максимального быстродействия.</span><span class="sxs-lookup"><span data-stu-id="21cc1-147">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span> <span data-ttu-id="21cc1-148">WebAssembly — это открытый веб-стандарт, который поддерживается в веб-браузерах без подключаемых модулей.</span><span class="sxs-lookup"><span data-stu-id="21cc1-148">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span>

<span data-ttu-id="21cc1-149">Код WebAssembly может обращаться к любым функциям браузера через JavaScript благодаря поддержке *взаимодействия с JavaScript* *.*</span><span class="sxs-lookup"><span data-stu-id="21cc1-149">WebAssembly code can access the full functionality of the browser via JavaScript, called *JavaScript interoperability* (or *JavaScript interop*).</span></span> <span data-ttu-id="21cc1-150">Код .NET, который обрабатывается через WebAssembly в браузере, выполняется в песочнице для JavaScript этого браузера, которая включает средства защиты от вредоносных действий на клиентском компьютере.</span><span class="sxs-lookup"><span data-stu-id="21cc1-150">.NET code executed via WebAssembly in the browser runs in the browser's JavaScript sandbox with the protections that the sandbox provides against malicious actions on the client machine.</span></span>

![Blazor WebAssembly выполняет код .NET в браузере с WebAssembly.](index/_static/blazor-webassembly.png)

<span data-ttu-id="21cc1-152">Когда приложение Blazor WebAssembly создается и запускается в браузере:</span><span class="sxs-lookup"><span data-stu-id="21cc1-152">When a Blazor WebAssembly app is built and run in a browser:</span></span>

* <span data-ttu-id="21cc1-153">Файлы кода C# и файлы Razor компилируются в сборки .NET.</span><span class="sxs-lookup"><span data-stu-id="21cc1-153">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="21cc1-154">Сборки и среда выполнения .NET загружаются в браузер.</span><span class="sxs-lookup"><span data-stu-id="21cc1-154">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="21cc1-155">Blazor WebAssembly осуществляет начальную загрузку среды выполнения .NET и настраивает ее для загрузки сборок для приложения.</span><span class="sxs-lookup"><span data-stu-id="21cc1-155">Blazor WebAssembly bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="21cc1-156">Среда выполнения Blazor WebAssembly использует взаимодействие с JavaScript для обработки операций с моделью DOM и вызовов API браузера.</span><span class="sxs-lookup"><span data-stu-id="21cc1-156">The Blazor WebAssembly runtime uses JavaScript interop to handle DOM manipulation and browser API calls.</span></span>

<span data-ttu-id="21cc1-157">Размер опубликованного приложения, известный как *размер полезных данных*, является критически важным фактором производительности для удобства работы с приложением.</span><span class="sxs-lookup"><span data-stu-id="21cc1-157">The size of the published app, its *payload size*, is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="21cc1-158">Крупное приложение скачивается в браузере довольно долго, что ухудшает взаимодействие с пользователем.</span><span class="sxs-lookup"><span data-stu-id="21cc1-158">A large app takes a relatively long time to download to a browser, which diminishes the user experience.</span></span> <span data-ttu-id="21cc1-159">Blazor WebAssembly оптимизирует размер полезных данных, чтобы сократить время скачивания:</span><span class="sxs-lookup"><span data-stu-id="21cc1-159">Blazor WebAssembly optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="21cc1-160">При публикации неиспользуемый код исключается из приложения [компоновщиком промежуточного языка](xref:host-and-deploy/blazor/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="21cc1-160">Unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>
* <span data-ttu-id="21cc1-161">HTTP-ответы сжимаются;</span><span class="sxs-lookup"><span data-stu-id="21cc1-161">HTTP responses are compressed.</span></span>
* <span data-ttu-id="21cc1-162">среда выполнения и сборки .NET кэшируются в браузере.</span><span class="sxs-lookup"><span data-stu-id="21cc1-162">The .NET runtime and assemblies are cached in the browser.</span></span>

## <a name="blazor-server"></a><span data-ttu-id="21cc1-163">Blazor Server</span><span class="sxs-lookup"><span data-stu-id="21cc1-163">Blazor Server</span></span>

<span data-ttu-id="21cc1-164">Blazor отделяет логику отображения компонентов от того, как применяются обновления пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="21cc1-164">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="21cc1-165">Blazor Server предоставляет поддержку размещения компонентов Razor на сервере в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="21cc1-165">Blazor Server provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="21cc1-166">Обновления пользовательского интерфейса передаются через подключение [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="21cc1-166">UI updates are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="21cc1-167">Среда выполнения обрабатывает отправку событий пользовательского интерфейса из браузера на сервер и применяет полученные от сервера обновления пользовательского интерфейса в браузере после выполнения компонентов.</span><span class="sxs-lookup"><span data-stu-id="21cc1-167">The runtime handles sending UI events from the browser to the server and applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="21cc1-168">Подключение, используемое Blazor Server для обмена данными с браузером, также применяется для обработки вызовов взаимодействия JavaScript.</span><span class="sxs-lookup"><span data-stu-id="21cc1-168">The connection used by Blazor Server to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Blazor Server выполняет код .NET на сервере и взаимодействует с моделью DOM на стороне клиента через подключение SignalR.](index/_static/blazor-server.png)

## <a name="javascript-interop"></a><span data-ttu-id="21cc1-170">Взаимодействие с JavaScript</span><span class="sxs-lookup"><span data-stu-id="21cc1-170">JavaScript interop</span></span>

<span data-ttu-id="21cc1-171">Для приложений, которым требуются сторонние библиотеки JavaScript и доступ к API браузера, компоненты взаимодействуют с JavaScript.</span><span class="sxs-lookup"><span data-stu-id="21cc1-171">For apps that require third-party JavaScript libraries and access to browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="21cc1-172">Компоненты могут использовать любую библиотеку или API, которые может использовать JavaScript.</span><span class="sxs-lookup"><span data-stu-id="21cc1-172">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="21cc1-173">Код C# может вызывать код JavaScript, а код JavaScript — код C#.</span><span class="sxs-lookup"><span data-stu-id="21cc1-173">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="21cc1-174">Дополнительные сведения можно найти по адресу: <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="21cc1-174">For more information, see <xref:blazor/javascript-interop>.</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="21cc1-175">Совместное использование кода и .NET Standard</span><span class="sxs-lookup"><span data-stu-id="21cc1-175">Code sharing and .NET Standard</span></span>

<span data-ttu-id="21cc1-176">Blazor реализует [.NET Standard 2.0](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="21cc1-176">Blazor implements [.NET Standard 2.0](/dotnet/standard/net-standard).</span></span> <span data-ttu-id="21cc1-177">.NET Standard — это формальная спецификация API-интерфейсов .NET, которые доступны во всех реализациях .NET.</span><span class="sxs-lookup"><span data-stu-id="21cc1-177">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="21cc1-178">Библиотеки классов .NET Standard можно использовать на разных платформах .NET, таких как Blazor, .NET Framework, .NET Core, Xamarin, Mono и Unity.</span><span class="sxs-lookup"><span data-stu-id="21cc1-178">.NET Standard class libraries can be shared across different .NET platforms, such as Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

<span data-ttu-id="21cc1-179">API, которые не используются в веб-браузере (например, для доступа к файловой системе, открытия сокетов и работы с потоками), создают исключение <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="21cc1-179">APIs that aren't applicable inside of a web browser (for example, accessing the file system, opening a socket, and threading) throw a <xref:System.PlatformNotSupportedException>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="21cc1-180">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="21cc1-180">Additional resources</span></span>

* [<span data-ttu-id="21cc1-181">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="21cc1-181">WebAssembly</span></span>](https://webassembly.org/)
* <xref:blazor/hosting-models>
* [<span data-ttu-id="21cc1-182">Руководство по языку C#</span><span class="sxs-lookup"><span data-stu-id="21cc1-182">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="21cc1-183">HTML</span><span class="sxs-lookup"><span data-stu-id="21cc1-183">HTML</span></span>](https://www.w3.org/html/)
