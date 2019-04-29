---
title: Общие сведения об использовании Blazor в ASP.NET Core
author: guardrex
description: Узнайте больше об использовании Blazor в ASP.NET Core для создания интерактивного клиентского веб-интерфейса с помощью .NET в приложении ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/18/2019
uid: blazor/index
ms.openlocfilehash: 74eeb003c249fc9ff8267ac855455f875806ccd9
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983001"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="2de70-103">Введение в Blazor</span><span class="sxs-lookup"><span data-stu-id="2de70-103">Introduction to Blazor</span></span>

<span data-ttu-id="2de70-104">Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2de70-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2de70-105">Добро пожаловать в Blazor!</span><span class="sxs-lookup"><span data-stu-id="2de70-105">Welcome to Blazor!</span></span>

<span data-ttu-id="2de70-106">Создавайте интерактивные клиентские веб-интерфейсы с использованием .NET:</span><span class="sxs-lookup"><span data-stu-id="2de70-106">Build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="2de70-107">создание многофункциональных интерактивных пользовательских интерфейсов с помощью C# (а не JavaScript);</span><span class="sxs-lookup"><span data-stu-id="2de70-107">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="2de70-108">совместное использование серверной и клиентской логик приложений, написанных с помощью .NET;</span><span class="sxs-lookup"><span data-stu-id="2de70-108">Share server-side and client-side app logic written with .NET.</span></span>
* <span data-ttu-id="2de70-109">отображение пользовательского интерфейса в виде HTML-страницы с CSS для широкой поддержки браузеров, в том числе для мобильных устройств.</span><span class="sxs-lookup"><span data-stu-id="2de70-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="2de70-110">Blazor поддерживает базовые сценарии, которые требуются для большинства приложений:</span><span class="sxs-lookup"><span data-stu-id="2de70-110">Blazor supports core scenarios required by most apps:</span></span>

* <span data-ttu-id="2de70-111">Параметры</span><span class="sxs-lookup"><span data-stu-id="2de70-111">Parameters</span></span>
* <span data-ttu-id="2de70-112">Обработка событий</span><span class="sxs-lookup"><span data-stu-id="2de70-112">Event handling</span></span>
* <span data-ttu-id="2de70-113">привязка данных,</span><span class="sxs-lookup"><span data-stu-id="2de70-113">Data binding</span></span>
* <span data-ttu-id="2de70-114">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="2de70-114">Routing</span></span>
* <span data-ttu-id="2de70-115">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="2de70-115">Dependency injection</span></span>
* <span data-ttu-id="2de70-116">Макеты</span><span class="sxs-lookup"><span data-stu-id="2de70-116">Layouts</span></span>
* <span data-ttu-id="2de70-117">Шаблоны</span><span class="sxs-lookup"><span data-stu-id="2de70-117">Templates</span></span>
* <span data-ttu-id="2de70-118">Каскадные значения</span><span class="sxs-lookup"><span data-stu-id="2de70-118">Cascading values</span></span>

## <a name="components"></a><span data-ttu-id="2de70-119">Компоненты</span><span class="sxs-lookup"><span data-stu-id="2de70-119">Components</span></span>

<span data-ttu-id="2de70-120">*Компонент* Blazor — это элемент пользовательского интерфейса, например страница, диалоговое окно или форма ввода данных.</span><span class="sxs-lookup"><span data-stu-id="2de70-120">A *component* in Blazor is an element of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="2de70-121">Компоненты обрабатывают для пользователей события и определяют гибкую логику отображения пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="2de70-121">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="2de70-122">Их можно вкладывать друг в друга и использовать повторно.</span><span class="sxs-lookup"><span data-stu-id="2de70-122">Components can be nested and reused.</span></span>

<span data-ttu-id="2de70-123">Компоненты — это классы .NET, встроенные в сборки .NET, которые можно совместно использовать и распространять в виде пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="2de70-123">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="2de70-124">Класс компонента обычно записывается в виде страницы разметки Razor с расширением файла *.razor*.</span><span class="sxs-lookup"><span data-stu-id="2de70-124">The component class is usually written in the form of a Razor markup page with a *.razor* file extension.</span></span> <span data-ttu-id="2de70-125">Компоненты в Blazor иногда называются компонентами Razor.</span><span class="sxs-lookup"><span data-stu-id="2de70-125">Components in Blazor are sometimes referred to as Razor components.</span></span>

<span data-ttu-id="2de70-126">[Razor](xref:mvc/views/razor) — это синтаксис для объединения разметки HTML с кодом C#.</span><span class="sxs-lookup"><span data-stu-id="2de70-126">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="2de70-127">Razor предназначен для повышения производительности разработчика и позволяет разработчику переключаться между разметкой и C# в одном файле с поддержкой [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="2de70-127">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="2de70-128">Razor Pages и представления MVC также используют Razor.</span><span class="sxs-lookup"><span data-stu-id="2de70-128">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="2de70-129">В отличие от Razor Pages и представлений MVC, которые созданы на базе модели "запрос — ответ", компоненты используются исключительно для обработки композиции пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="2de70-129">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="2de70-130">Компоненты Razor можно использовать для клиентской логики и формирования пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="2de70-130">Razor components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="2de70-131">Следующая разметка является примером настраиваемого диалогового компонента:</span><span class="sxs-lookup"><span data-stu-id="2de70-131">The following markup is an example of a custom dialog component:</span></span>

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

<span data-ttu-id="2de70-132">Если этот компонент используется в другом месте в приложении, IntelliSense в [Visual Studio](https://visualstudio.microsoft.com/vs/) позволяет ускорить разработку за счет дополнения элементов синтаксиса и параметров.</span><span class="sxs-lookup"><span data-stu-id="2de70-132">When this component is used elsewhere in the app, IntelliSense in [Visual Studio](https://visualstudio.microsoft.com/vs/) speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="2de70-133">Компоненты преобразуются в представление DOM браузера в памяти, которое называется *деревом визуализации*. Его затем можно использовать для гибкого и эффективного обновления пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="2de70-133">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="blazor-server-side"></a><span data-ttu-id="2de70-134">Blazor на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="2de70-134">Blazor server-side</span></span>

<span data-ttu-id="2de70-135">Blazor отделяет логику отображения компонентов от того, как применяются обновления пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="2de70-135">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="2de70-136">Blazor на стороне сервера предоставляет поддержку размещения компонентов Razor на сервере в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2de70-136">Blazor server-side provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="2de70-137">Обновления пользовательского интерфейса обрабатываются через подключение SignalR.</span><span class="sxs-lookup"><span data-stu-id="2de70-137">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="2de70-138">Среда выполнения:</span><span class="sxs-lookup"><span data-stu-id="2de70-138">The runtime:</span></span>

* <span data-ttu-id="2de70-139">обрабатывает отправку событий пользовательского интерфейса из браузера на сервер;</span><span class="sxs-lookup"><span data-stu-id="2de70-139">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="2de70-140">применяет обновления пользовательского интерфейса, отправленные сервером браузеру после выполнения компонентов.</span><span class="sxs-lookup"><span data-stu-id="2de70-140">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="2de70-141">Подключение, используемое Blazor на стороне сервера для обмена данными с браузером, также применяется для обработки вызовов взаимодействия JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2de70-141">The connection used by Blazor server-side to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Blazor на стороне сервера выполняет код .NET на сервере и взаимодействует с моделью DOM на стороне клиента через подключение SignalR.](index/_static/blazor-server-side.png)

<span data-ttu-id="2de70-143">Для получения дополнительной информации см. <xref:blazor/hosting-models#server-side>.</span><span class="sxs-lookup"><span data-stu-id="2de70-143">For more information, see <xref:blazor/hosting-models#server-side>.</span></span>

## <a name="blazor-client-side"></a><span data-ttu-id="2de70-144">Blazor на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="2de70-144">Blazor client-side</span></span>

<span data-ttu-id="2de70-145">Blazor на стороне клиента — это платформа одностраничных приложений для создания интерактивных клиентских веб-приложений с использованием .NET.</span><span class="sxs-lookup"><span data-stu-id="2de70-145">Blazor client-side is a single-page app framework for building interactive client-side Web apps with .NET.</span></span> <span data-ttu-id="2de70-146">Blazor на стороне клиента использует открытые веб-стандарты без подключаемых модулей или преобразования кода в код на другом языке программирования.</span><span class="sxs-lookup"><span data-stu-id="2de70-146">Blazor client-side uses open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="2de70-147">Blazor на стороне клиента работает во всех современных веб-браузерах, включая браузеры мобильных устройств.</span><span class="sxs-lookup"><span data-stu-id="2de70-147">Blazor client-side works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="2de70-148">Использование .NET в браузере для веб-разработки на стороне клиента дает множество преимуществ:</span><span class="sxs-lookup"><span data-stu-id="2de70-148">Using .NET in the browser for client-side web development offers many advantages:</span></span>

* <span data-ttu-id="2de70-149">**Язык C#**: создавайте код на C#, а не на JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2de70-149">**C# language**: Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="2de70-150">**Экосистема .NET**: используйте существующую экосистему библиотек .NET.</span><span class="sxs-lookup"><span data-stu-id="2de70-150">**.NET Ecosystem**: Leverage the existing ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="2de70-151">**Полностековая разработка**: совместно используйте серверную и клиентскую логики.</span><span class="sxs-lookup"><span data-stu-id="2de70-151">**Full-stack development**: Share server and client-side logic.</span></span>
* <span data-ttu-id="2de70-152">**Скорость и масштабируемость**: NET обеспечивает производительность, надежность и безопасность.</span><span class="sxs-lookup"><span data-stu-id="2de70-152">**Speed and scalability**: .NET was built for performance, reliability, and security.</span></span>
* <span data-ttu-id="2de70-153">**Ведущее в отрасли средства**: сохраняйте высокий уровень продуктивности с помощью Visual Studio в Windows, Linux и macOS.</span><span class="sxs-lookup"><span data-stu-id="2de70-153">**Industry-leading tools**: Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="2de70-154">**Стабильность и согласованность**:  создавайте приложения на основе распространенных языков, платформ и инструментов, которые отличаются стабильностью, широким набором функций и простотой в использовании.</span><span class="sxs-lookup"><span data-stu-id="2de70-154">**Stability and consistency**:  Build on a common set of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

<span data-ttu-id="2de70-155">Выполнение кода .NET в веб-браузерах становится возможным благодаря технологии [WebAssembly](http://webassembly.org) (сокращенно *wasm*).</span><span class="sxs-lookup"><span data-stu-id="2de70-155">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="2de70-156">WebAssembly — это открытый веб-стандарт, который поддерживается в веб-браузерах без подключаемых модулей.</span><span class="sxs-lookup"><span data-stu-id="2de70-156">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="2de70-157">WebAssembly — это компактный формат байт-кода, оптимизированный для быстрой загрузки и максимального быстродействия.</span><span class="sxs-lookup"><span data-stu-id="2de70-157">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="2de70-158">Код WebAssembly может использовать все возможности браузера с помощью взаимодействия с JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2de70-158">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="2de70-159">В то же время код .NET, запускаемый с использованием WebAssembly, выполняется в той же доверенной песочнице, что и код JavaScript, для предотвращения вредоносных действий на клиентском компьютере.</span><span class="sxs-lookup"><span data-stu-id="2de70-159">At the same time, .NET code executed via WebAssembly runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![Blazor на стороне клиента выполняет код .NET в браузере с WebAssembly.](index/_static/blazor-client-side.png)

<span data-ttu-id="2de70-161">Когда клиентское приложение Blazor создается и запускается в браузере:</span><span class="sxs-lookup"><span data-stu-id="2de70-161">When a Blazor client-side app is built and run in a browser:</span></span>

* <span data-ttu-id="2de70-162">Файлы кода C# и файлы Razor компилируются в сборки .NET.</span><span class="sxs-lookup"><span data-stu-id="2de70-162">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="2de70-163">Сборки и среда выполнения .NET загружаются в браузер.</span><span class="sxs-lookup"><span data-stu-id="2de70-163">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="2de70-164">Blazor на стороне клиента осуществляет начальную загрузку среды выполнения .NET и настраивает ее для загрузки сборок для приложения.</span><span class="sxs-lookup"><span data-stu-id="2de70-164">Blazor client-side bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="2de70-165">Операции с моделью DOM и вызовы API браузера обрабатываются средой выполнения Blazor на стороне клиента через взаимодействие с JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2de70-165">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor client-side runtime via JavaScript interop.</span></span>

<span data-ttu-id="2de70-166">Чтобы уменьшить размер скачиваемого приложения, [компоновщик промежуточного языка (IL)](xref:host-and-deploy/blazor/configure-linker) отсекает неиспользуемый код от приложения при его публикации.</span><span class="sxs-lookup"><span data-stu-id="2de70-166">To reduce the size of the downloaded app, unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>

<span data-ttu-id="2de70-167">Blazor предполагает размещение на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="2de70-167">Blazor client-side is a client-side hosting model.</span></span> <span data-ttu-id="2de70-168">Так как Blazor отделяет логику отображения компонента от способов применения обновлений пользовательского интерфейса, Blazor можно размещать разными способами.</span><span class="sxs-lookup"><span data-stu-id="2de70-168">Because Blazor decouples a component's rendering logic from how UI updates are applied, there's flexibility in how Blazor can be hosted.</span></span> <span data-ttu-id="2de70-169">Используйте [Blazor на стороне сервера](#blazor-server-side) для размещения Blazor на сервере в приложении ASP.NET Core с возможностью обработки обновлений пользовательского интерфейса через подключение SignalR.</span><span class="sxs-lookup"><span data-stu-id="2de70-169">Use [Blazor server-side](#blazor-server-side) to host Blazor on the server in an ASP.NET Core app where UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="2de70-170">Для получения дополнительной информации см. <xref:blazor/hosting-models#server-side>.</span><span class="sxs-lookup"><span data-stu-id="2de70-170">For more information, see <xref:blazor/hosting-models#server-side>.</span></span> 

<span data-ttu-id="2de70-171">Размер полезных данных критически важен для производительности и, как следствие, удобства использования приложения.</span><span class="sxs-lookup"><span data-stu-id="2de70-171">Payload size is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="2de70-172">Blazor на стороне клиента оптимизирует размер полезных данных, чтобы сократить время скачивания:</span><span class="sxs-lookup"><span data-stu-id="2de70-172">Blazor client-side optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="2de70-173">неиспользуемые части сборок .NET удаляются в ходе компиляции;</span><span class="sxs-lookup"><span data-stu-id="2de70-173">Unused parts of .NET assemblies are removed during the build process.</span></span>
* <span data-ttu-id="2de70-174">HTTP-ответы сжимаются;</span><span class="sxs-lookup"><span data-stu-id="2de70-174">HTTP responses are compressed.</span></span>
* <span data-ttu-id="2de70-175">среда выполнения и сборки .NET кэшируются в браузере.</span><span class="sxs-lookup"><span data-stu-id="2de70-175">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="2de70-176">[Blazor на стороне сервера](#blazor-server-side) предоставляет полезные данные меньшего размера по сравнению с Blazor на стороне клиента, поддерживая сборки .NET, сборку приложения и среду выполнения на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="2de70-176">[Blazor server-side](#blazor-server-side) provides a smaller payload size than Blazor client-side by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="2de70-177">Серверные приложения Blazor передают клиентам только файлы разметки и статическое содержимое.</span><span class="sxs-lookup"><span data-stu-id="2de70-177">Blazor server-side apps only serve markup files and static assets to clients.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="2de70-178">Взаимодействие с JavaScript</span><span class="sxs-lookup"><span data-stu-id="2de70-178">JavaScript interop</span></span>

<span data-ttu-id="2de70-179">Для приложений, требующих сторонние библиотеки JavaScript и API браузера, компоненты взаимодействуют с JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2de70-179">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="2de70-180">Компоненты могут использовать любую библиотеку или API, которые может использовать JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2de70-180">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="2de70-181">Код C# может вызывать код JavaScript, а код JavaScript — код C#.</span><span class="sxs-lookup"><span data-stu-id="2de70-181">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="2de70-182">Дополнительные сведения см. в разделе [Взаимодействие с JavaScript](xref:blazor/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="2de70-182">For more information, see [JavaScript interop](xref:blazor/javascript-interop).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="2de70-183">Совместное использование кода и .NET Standard</span><span class="sxs-lookup"><span data-stu-id="2de70-183">Code sharing and .NET Standard</span></span>

<span data-ttu-id="2de70-184">Приложения могут ссылаться и использовать существующие библиотеки [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="2de70-184">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="2de70-185">.NET Standard — это формальная спецификация API-интерфейсов .NET, которые доступны во всех реализациях .NET.</span><span class="sxs-lookup"><span data-stu-id="2de70-185">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="2de70-186">Blazor реализует .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="2de70-186">Blazor implements .NET Standard 2.0.</span></span> <span data-ttu-id="2de70-187">API, которые нельзя использовать в веб-браузере (например, для доступа к файловой системе, открытия сокета, работы с потоками и других операций), выдают исключение <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="2de70-187">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw <xref:System.PlatformNotSupportedException>.</span></span> <span data-ttu-id="2de70-188">Библиотеки классов .NET Standard можно использовать на разных платформах .NET, таких как Blazor, .NET Framework, .NET Core, Xamarin, Mono и Unity.</span><span class="sxs-lookup"><span data-stu-id="2de70-188">.NET Standard class libraries can be shared across different .NET platforms, such as Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2de70-189">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="2de70-189">Additional resources</span></span>

* [<span data-ttu-id="2de70-190">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="2de70-190">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="2de70-191">Руководство по языку C#</span><span class="sxs-lookup"><span data-stu-id="2de70-191">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="2de70-192">HTML</span><span class="sxs-lookup"><span data-stu-id="2de70-192">HTML</span></span>](https://www.w3.org/html/)
