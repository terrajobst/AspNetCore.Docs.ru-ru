---
title: Введение в Blazor
author: guardrex
description: Blazor в ASP.NET Core предлагает новый способ создания интерактивных клиентских приложений с использованием .NET, которые выполняются в браузере с поддержкой WebAssembly.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: spa/blazor/index
ms.openlocfilehash: be8fdb7bcbf9ce8c80bc6e21be455dfbfcaf404b
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468615"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="94d23-103">Введение в Blazor</span><span class="sxs-lookup"><span data-stu-id="94d23-103">Introduction to Blazor</span></span>

<span data-ttu-id="94d23-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="94d23-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="94d23-105">Blazor — это платформа одностраничных приложений для создания интерактивных клиентских веб-приложений с использованием .NET.</span><span class="sxs-lookup"><span data-stu-id="94d23-105">Blazor is a single-page app framework for building interactive client-side Web apps with .NET.</span></span> <span data-ttu-id="94d23-106">Blazor использует открытые веб-стандарты без подключаемых модулей или транспиляции кода.</span><span class="sxs-lookup"><span data-stu-id="94d23-106">Blazor uses open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="94d23-107">Blazor работает во всех современных веб-браузерах, включая браузеры мобильных устройств.</span><span class="sxs-lookup"><span data-stu-id="94d23-107">Blazor works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="94d23-108">Использование .NET в браузере для веб-разработки на стороне клиента дает множество преимуществ:</span><span class="sxs-lookup"><span data-stu-id="94d23-108">Using .NET in the browser for client-side web development offers many advantages:</span></span>

* <span data-ttu-id="94d23-109">**Язык C#**: создавайте код на C#, а не на JavaScript.</span><span class="sxs-lookup"><span data-stu-id="94d23-109">**C# language**: Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="94d23-110">**Экосистема .NET**: используйте существующую экосистему библиотек .NET.</span><span class="sxs-lookup"><span data-stu-id="94d23-110">**.NET Ecosystem**: Leverage the existing ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="94d23-111">**Полностековая разработка**: совместно используйте серверную и клиентскую логики.</span><span class="sxs-lookup"><span data-stu-id="94d23-111">**Full-stack development**: Share server and client-side logic.</span></span>
* <span data-ttu-id="94d23-112">**Скорость и масштабируемость**: NET обеспечивает производительность, надежность и безопасность.</span><span class="sxs-lookup"><span data-stu-id="94d23-112">**Speed and scalability**: .NET was built for performance, reliability, and security.</span></span>
* <span data-ttu-id="94d23-113">**Ведущее в отрасли средства**: сохраняйте высокий уровень продуктивности с помощью Visual Studio в Windows, Linux и macOS.</span><span class="sxs-lookup"><span data-stu-id="94d23-113">**Industry-leading tools**: Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="94d23-114">**Стабильность и согласованность**:  в основе лежит общий набор языков, платформ и инструментов, которые отличаются стабильностью, многофункциональностью и простотой использования.</span><span class="sxs-lookup"><span data-stu-id="94d23-114">**Stability and consistency**:  Build on a commonset of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

<span data-ttu-id="94d23-115">Выполнение кода .NET в веб-браузерах становится возможным благодаря технологии [WebAssembly](http://webassembly.org) (сокращенно *wasm*).</span><span class="sxs-lookup"><span data-stu-id="94d23-115">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="94d23-116">WebAssembly — это открытый веб-стандарт, который поддерживается в веб-браузерах без подключаемых модулей.</span><span class="sxs-lookup"><span data-stu-id="94d23-116">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="94d23-117">WebAssembly — это компактный формат байт-кода, оптимизированный для быстрой загрузки и максимального быстродействия.</span><span class="sxs-lookup"><span data-stu-id="94d23-117">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="94d23-118">Код WebAssembly может использовать все возможности браузера с помощью взаимодействия с JavaScript.</span><span class="sxs-lookup"><span data-stu-id="94d23-118">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="94d23-119">В то же время код .NET, запускаемый с использованием WebAssembly, выполняется в той же доверенной песочнице, что и код JavaScript, для предотвращения вредоносных действий на клиентском компьютере.</span><span class="sxs-lookup"><span data-stu-id="94d23-119">At the same time, .NET code executed via WebAssembly runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![Blazor выполняет код .NET в браузере с WebAssembly.](index/_static/blazor.png)

<span data-ttu-id="94d23-121">Когда приложение Blazor создано и запущено в браузере:</span><span class="sxs-lookup"><span data-stu-id="94d23-121">When a Blazor app is built and run in a browser:</span></span>

* <span data-ttu-id="94d23-122">Файлы кода C# и файлы Razor компилируются в сборки .NET.</span><span class="sxs-lookup"><span data-stu-id="94d23-122">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="94d23-123">Сборки и среда выполнения .NET загружаются в браузер.</span><span class="sxs-lookup"><span data-stu-id="94d23-123">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="94d23-124">Blazor осуществляет начальную загрузку среды выполнения .NET и настраивает ее для загрузки сборок для приложения.</span><span class="sxs-lookup"><span data-stu-id="94d23-124">Blazor bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="94d23-125">Операции с моделью DOM и вызовы API браузера обрабатываются средой выполнения Blazor через взаимодействие с JavaScript.</span><span class="sxs-lookup"><span data-stu-id="94d23-125">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor runtime via JavaScript interop.</span></span>

<span data-ttu-id="94d23-126">Blazor поддерживает базовые функции, в которых нуждаются большинство приложений, в том числе такие:</span><span class="sxs-lookup"><span data-stu-id="94d23-126">Blazor supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="94d23-127">Параметры</span><span class="sxs-lookup"><span data-stu-id="94d23-127">Parameters</span></span>
* <span data-ttu-id="94d23-128">Обработка событий</span><span class="sxs-lookup"><span data-stu-id="94d23-128">Event handling</span></span>
* <span data-ttu-id="94d23-129">привязка данных,</span><span class="sxs-lookup"><span data-stu-id="94d23-129">Data binding</span></span>
* <span data-ttu-id="94d23-130">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="94d23-130">Routing</span></span>
* <span data-ttu-id="94d23-131">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="94d23-131">Dependency injection</span></span>
* <span data-ttu-id="94d23-132">Макеты</span><span class="sxs-lookup"><span data-stu-id="94d23-132">Layouts</span></span>
* <span data-ttu-id="94d23-133">Использование шаблонов</span><span class="sxs-lookup"><span data-stu-id="94d23-133">Templating</span></span>
* <span data-ttu-id="94d23-134">Каскадные значения</span><span class="sxs-lookup"><span data-stu-id="94d23-134">Cascading values</span></span>

<span data-ttu-id="94d23-135">Чтобы уменьшить размер скачиваемого приложения, [компоновщик промежуточного языка (IL)](xref:host-and-deploy/razor-components-blazor/configure-linker) отсекает неиспользуемый код от приложения при его публикации.</span><span class="sxs-lookup"><span data-stu-id="94d23-135">To reduce the size of the downloaded app, unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/razor-components-blazor/configure-linker).</span></span>

<span data-ttu-id="94d23-136">Blazor — это модель размещения на стороне клиента для компонентов Razor.</span><span class="sxs-lookup"><span data-stu-id="94d23-136">Blazor is the client-side hosting model for Razor Components.</span></span> <span data-ttu-id="94d23-137">Так как компоненты Razor отделяют логику отображения компонента от способов применения обновлений пользовательского интерфейса, эти компоненты можно размещать по-разному.</span><span class="sxs-lookup"><span data-stu-id="94d23-137">Because Razor Components decouple a component's rendering logic from how UI updates are applied, there's flexibility in how Razor Components can be hosted.</span></span> <span data-ttu-id="94d23-138">Используйте компоненты Razor в ASP.NET Core, чтобы разместить их на сервере в приложении ASP.NET Core, для которого обновления пользовательского интерфейса обрабатываются через подключение SignalR.</span><span class="sxs-lookup"><span data-stu-id="94d23-138">Use ASP.NET Core Razor Components to host Razor Components on the server in an ASP.NET Core app where UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="94d23-139">Дополнительные сведения см. в разделах <xref:razor-components/index> и <xref:razor-components/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="94d23-139">For more information, see <xref:razor-components/index> and <xref:razor-components/hosting-models#server-side-hosting-model>.</span></span> 

## <a name="components"></a><span data-ttu-id="94d23-140">Компоненты</span><span class="sxs-lookup"><span data-stu-id="94d23-140">Components</span></span>

<span data-ttu-id="94d23-141">*Компонент Razor* — это часть пользовательского интерфейса, например страница, диалоговое окно или форма ввода данных.</span><span class="sxs-lookup"><span data-stu-id="94d23-141">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="94d23-142">Компоненты обрабатывают для пользователей события и определяют гибкую логику отображения пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="94d23-142">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="94d23-143">Их можно вкладывать друг в друга и использовать повторно.</span><span class="sxs-lookup"><span data-stu-id="94d23-143">Components can be nested and reused.</span></span>

<span data-ttu-id="94d23-144">Компоненты — это классы .NET, встроенные в сборки .NET, которые можно совместно использовать и распространять в виде пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="94d23-144">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="94d23-145">Этот класс может записываться в виде страницы разметки Razor (*.cshtml*) или в виде класса C# (*.cs*).</span><span class="sxs-lookup"><span data-stu-id="94d23-145">The class can either be written in the form of a Razor markup page (*.cshtml*) or as a C# class (*.cs*).</span></span>

<span data-ttu-id="94d23-146">[Razor](xref:mvc/views/razor) — это синтаксис для объединения разметки HTML с кодом C#.</span><span class="sxs-lookup"><span data-stu-id="94d23-146">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="94d23-147">Razor предназначен для повышения производительности разработчика и позволяет разработчику переключаться между разметкой и C# в одном файле с поддержкой [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="94d23-147">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="94d23-148">Razor Pages и представления MVC также используют Razor.</span><span class="sxs-lookup"><span data-stu-id="94d23-148">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="94d23-149">В отличие от Razor Pages и представлений MVC, которые созданы на базе модели "запрос — ответ", компоненты используются исключительно для обработки композиции пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="94d23-149">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="94d23-150">Компоненты Razor можно использовать исключительно для клиентской логики и композиции пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="94d23-150">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="94d23-151">Следующая разметка является примером настраиваемого диалогового компонента в файле Razor (*DialogComponent.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="94d23-151">The following markup is an example of a custom dialog component in a Razor file (*DialogComponent.cshtml*):</span></span>

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick="@OnOK">OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

<span data-ttu-id="94d23-152">Когда этот компонент используется в другом месте в приложении, IntelliSense ускоряет разработку с помощью выполнения синтаксиса и параметров.</span><span class="sxs-lookup"><span data-stu-id="94d23-152">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="94d23-153">Компоненты преобразуются в представление DOM браузера в памяти, которое называется *деревом визуализации*. Его затем можно использовать для гибкого и эффективного обновления пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="94d23-153">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="94d23-154">Взаимодействие с JavaScript</span><span class="sxs-lookup"><span data-stu-id="94d23-154">JavaScript interop</span></span>

<span data-ttu-id="94d23-155">Для приложений, требующих сторонние библиотеки JavaScript и API браузера, Blazor взаимодействует с JavaScript.</span><span class="sxs-lookup"><span data-stu-id="94d23-155">For apps that require third-party JavaScript libraries and browser APIs, Blazor interoperates with JavaScript.</span></span> <span data-ttu-id="94d23-156">Компоненты могут использовать любую библиотеку или API, которые может использовать JavaScript.</span><span class="sxs-lookup"><span data-stu-id="94d23-156">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="94d23-157">Код C# может вызывать код JavaScript, а код JavaScript — код C#.</span><span class="sxs-lookup"><span data-stu-id="94d23-157">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="94d23-158">Дополнительные сведения см. в разделе [Взаимодействие с JavaScript](xref:razor-components/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="94d23-158">For more information, see [JavaScript interop](xref:razor-components/javascript-interop).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="94d23-159">Совместное использование кода и .NET Standard</span><span class="sxs-lookup"><span data-stu-id="94d23-159">Code sharing and .NET Standard</span></span>

<span data-ttu-id="94d23-160">Приложения могут ссылаться и использовать существующие библиотеки [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="94d23-160">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="94d23-161">.NET Standard — это формальная спецификация API-интерфейсов .NET, которые доступны во всех реализациях .NET.</span><span class="sxs-lookup"><span data-stu-id="94d23-161">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="94d23-162">Blazor реализует .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="94d23-162">Blazor implements .NET Standard 2.0.</span></span> <span data-ttu-id="94d23-163">API, которые нельзя использовать в веб-браузере (например, для доступа к файловой системе, открытия сокета, работы с потоками и других операций), выдают исключение <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="94d23-163">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw <xref:System.PlatformNotSupportedException>.</span></span> <span data-ttu-id="94d23-164">Библиотеки классов .NET Standard можно использовать на разных платформах .NET, включая Blazor, .NET Framework, .NET Core, Mono, Xamarin и Unity.</span><span class="sxs-lookup"><span data-stu-id="94d23-164">.NET Standard class libraries can be shared across different .NET platforms, like Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

## <a name="optimization"></a><span data-ttu-id="94d23-165">Оптимизация</span><span class="sxs-lookup"><span data-stu-id="94d23-165">Optimization</span></span>

<span data-ttu-id="94d23-166">Размер полезных данных критически важен для производительности и, как следствие, удобства использования приложения.</span><span class="sxs-lookup"><span data-stu-id="94d23-166">Payload size is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="94d23-167">Blazor оптимизирует размер полезных данных, чтобы сократить время скачивания:</span><span class="sxs-lookup"><span data-stu-id="94d23-167">Blazor optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="94d23-168">неиспользуемые части сборок .NET удаляются в ходе компиляции;</span><span class="sxs-lookup"><span data-stu-id="94d23-168">Unused parts of .NET assemblies are removed during the build process.</span></span>
* <span data-ttu-id="94d23-169">HTTP-ответы сжимаются;</span><span class="sxs-lookup"><span data-stu-id="94d23-169">HTTP responses are compressed.</span></span>
* <span data-ttu-id="94d23-170">среда выполнения и сборки .NET кэшируются в браузере.</span><span class="sxs-lookup"><span data-stu-id="94d23-170">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="94d23-171">[Серверные компоненты Razor](xref:razor-components/index) предоставляют даже меньший размер полезных данных, чем Blazor, благодаря размещению сборок .NET, сборок приложений и среды выполнения на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="94d23-171">[Server-side Razor Components](xref:razor-components/index) provides an even smaller payload size than Blazor by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="94d23-172">Приложения компонентов Razor передают клиентам только файлы разметки и статическое содержимое.</span><span class="sxs-lookup"><span data-stu-id="94d23-172">Razor Components apps only serve markup files and static assets to clients.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="94d23-173">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="94d23-173">Additional resources</span></span>

* <xref:razor-components/index>
* [<span data-ttu-id="94d23-174">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="94d23-174">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="94d23-175">Руководство по языку C#</span><span class="sxs-lookup"><span data-stu-id="94d23-175">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="94d23-176">HTML</span><span class="sxs-lookup"><span data-stu-id="94d23-176">HTML</span></span>](https://www.w3.org/html/)
