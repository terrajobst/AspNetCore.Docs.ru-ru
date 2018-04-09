---
title: Используйте JavaScriptServices для создания приложений на одной странице в ASP.NET Core
author: scottaddie
description: Узнайте о преимуществах использования JavaScriptServices для создания одной страницы приложений (SPA) поддерживаемый ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/spa-services
ms.openlocfilehash: 05b0d7f31e167e620f2d168109ffd907ba120a49
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="10595-103">Используйте JavaScriptServices для создания приложений на одной странице в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="10595-103">Use JavaScriptServices to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="10595-104">По [Scott Addie](https://github.com/scottaddie) и [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="10595-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="10595-105">Приложение одной странице (SPA) является типом популярных веб-приложения из-за его присущие многофункциональном пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="10595-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="10595-106">Интеграция клиентского SPA платформы или библиотеки, такие как [Вращательный](https://angular.io/) или [реагировать](https://facebook.github.io/react/), с серверные платформы, такие как ASP.NET Core могут возникнуть трудности.</span><span class="sxs-lookup"><span data-stu-id="10595-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="10595-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) разработанного для уменьшения трения в процессе интеграции.</span><span class="sxs-lookup"><span data-stu-id="10595-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="10595-108">Он позволяет работать между клиента и сервера технологии стеки.</span><span class="sxs-lookup"><span data-stu-id="10595-108">It enables seamless operation between the different client and server technology stacks.</span></span>

<span data-ttu-id="10595-109">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="10595-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="10595-110">Что такое JavaScriptServices?</span><span class="sxs-lookup"><span data-stu-id="10595-110">What is JavaScriptServices?</span></span>

<span data-ttu-id="10595-111">JavaScriptServices — это совокупность технологий клиентские ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10595-111">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="10595-112">Его задача — размещать в качестве разработчиков предпочтительный серверную платформу для построения SPAs ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10595-112">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="10595-113">JavaScriptServices состоит из трех различных пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="10595-113">JavaScriptServices consists of three distinct NuGet packages:</span></span>
* <span data-ttu-id="10595-114">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="10595-114">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="10595-115">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="10595-115">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="10595-116">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="10595-116">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="10595-117">Эти пакеты полезны, если вы:</span><span class="sxs-lookup"><span data-stu-id="10595-117">These packages are useful if you:</span></span>
* <span data-ttu-id="10595-118">Запускать JavaScript на сервере</span><span class="sxs-lookup"><span data-stu-id="10595-118">Run JavaScript on the server</span></span>
* <span data-ttu-id="10595-119">Использовать SPA framework или в библиотеке</span><span class="sxs-lookup"><span data-stu-id="10595-119">Use a SPA framework or library</span></span>
* <span data-ttu-id="10595-120">Построение клиентского средства с Webpack</span><span class="sxs-lookup"><span data-stu-id="10595-120">Build client-side assets with Webpack</span></span>

<span data-ttu-id="10595-121">В этой статье внимание следует уделять помещается в с помощью пакета SpaServices.</span><span class="sxs-lookup"><span data-stu-id="10595-121">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="10595-122">Что такое SpaServices?</span><span class="sxs-lookup"><span data-stu-id="10595-122">What is SpaServices?</span></span>

<span data-ttu-id="10595-123">SpaServices был создан для позиционирования в качестве разработчиков предпочтительный серверную платформу для построения SPAs ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10595-123">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="10595-124">Для разработки SPAs с ASP.NET Core необязательно SpaServices и он не блокирует в платформу конкретного клиента.</span><span class="sxs-lookup"><span data-stu-id="10595-124">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="10595-125">SpaServices предоставляет полезные инфраструктуры, такие как:</span><span class="sxs-lookup"><span data-stu-id="10595-125">SpaServices provides useful infrastructure such as:</span></span>
* [<span data-ttu-id="10595-126">Серверные предварительной визуализации</span><span class="sxs-lookup"><span data-stu-id="10595-126">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="10595-127">По промежуточного слоя Webpack разработки</span><span class="sxs-lookup"><span data-stu-id="10595-127">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="10595-128">Модуль горячей замены</span><span class="sxs-lookup"><span data-stu-id="10595-128">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="10595-129">Помощники маршрутизации</span><span class="sxs-lookup"><span data-stu-id="10595-129">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="10595-130">В совокупности эти компоненты инфраструктуры расширяют рабочий процесс разработки и среду выполнения.</span><span class="sxs-lookup"><span data-stu-id="10595-130">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="10595-131">Компоненты, может быть использована по отдельности.</span><span class="sxs-lookup"><span data-stu-id="10595-131">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="10595-132">Предварительные требования для использования SpaServices</span><span class="sxs-lookup"><span data-stu-id="10595-132">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="10595-133">Для работы с SpaServices, установите следующее:</span><span class="sxs-lookup"><span data-stu-id="10595-133">To work with SpaServices, install the following:</span></span>
* <span data-ttu-id="10595-134">[Node.js](https://nodejs.org/) (версии 6 или более поздней версии) с npm</span><span class="sxs-lookup"><span data-stu-id="10595-134">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
  * <span data-ttu-id="10595-135">Чтобы проверить эти компоненты установлены и может быть найден, выполните следующую из командной строки:</span><span class="sxs-lookup"><span data-stu-id="10595-135">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="10595-136">Примечание: При развертывании веб-сайте Azure не нужно выполнять никаких действий, &mdash; Node.js установлена и доступна в серверных средах.</span><span class="sxs-lookup"><span data-stu-id="10595-136">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * <span data-ttu-id="10595-137">Если вы используете Windows с помощью Visual Studio 2017 г., выбрав установлен пакет SDK **кросс платформенной разработки .NET Core** рабочей нагрузки.</span><span class="sxs-lookup"><span data-stu-id="10595-137">If you're on Windows using Visual Studio 2017, the SDK is installed by selecting the **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="10595-138">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span><span class="sxs-lookup"><span data-stu-id="10595-138">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="10595-139">Серверные предварительной визуализации</span><span class="sxs-lookup"><span data-stu-id="10595-139">Server-side prerendering</span></span>

<span data-ttu-id="10595-140">Универсальное приложение (также известный как isomorphic) — это приложение JavaScript, может запускать как на сервере и клиенте.</span><span class="sxs-lookup"><span data-stu-id="10595-140">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="10595-141">Угловая, реагирование на них и других популярных платформ предоставляют универсальной платформы для этого стиля разработки приложения.</span><span class="sxs-lookup"><span data-stu-id="10595-141">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="10595-142">Основная идея — сначала отрисовки компоненты framework на сервере через Node.js, а затем делегирует дальнейшего выполнения клиенту.</span><span class="sxs-lookup"><span data-stu-id="10595-142">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="10595-143">ASP.NET Core [вспомогательных функций тегов](xref:mvc/views/tag-helpers/intro) предоставляемые SpaServices упрощения реализации предварительной визуализации серверные путем вызова функций JavaScript на сервере.</span><span class="sxs-lookup"><span data-stu-id="10595-143">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="10595-144">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="10595-144">Prerequisites</span></span>

<span data-ttu-id="10595-145">Установите следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="10595-145">Install the following:</span></span>
* <span data-ttu-id="10595-146">[предварительной визуализации ASPNET](https://www.npmjs.com/package/aspnet-prerendering) пакета npm:</span><span class="sxs-lookup"><span data-stu-id="10595-146">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="10595-147">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="10595-147">Configuration</span></span>

<span data-ttu-id="10595-148">Вспомогательных функций тегов вносятся найти через регистрации пространства имен в проекте *_ViewImports.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="10595-148">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="10595-149">Эти вспомогательных функций тегов абстрагируется от особенностей взаимодействовать непосредственно с низкоуровневые интерфейсы API, используя синтаксис, подобный HTML внутри представления Razor:</span><span class="sxs-lookup"><span data-stu-id="10595-149">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="10595-150">`asp-prerender-module` Тег вспомогательный</span><span class="sxs-lookup"><span data-stu-id="10595-150">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="10595-151">`asp-prerender-module` Вспомогательный тег, используемый в предыдущем примере кода выполняется *ClientApp/dist/main-server.js* на сервере через Node.js.</span><span class="sxs-lookup"><span data-stu-id="10595-151">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="10595-152">Для ясности *main server.js* файл — это артефакт задачи transpilation TypeScript для JavaScript в [Webpack](http://webpack.github.io/) процесса построения.</span><span class="sxs-lookup"><span data-stu-id="10595-152">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="10595-153">Webpack определяет псевдоним точки входа `main-server`; и начинается обход граф зависимостей для данного псевдонима *ClientApp, загрузки server.ts* файла:</span><span class="sxs-lookup"><span data-stu-id="10595-153">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="10595-154">В следующем примере углового *ClientApp, загрузки server.ts* использует файл `createServerRenderer` функции и `RenderResult` тип `aspnet-prerendering` пакет npm для настройки подготовки сервера отчетов через Node.js.</span><span class="sxs-lookup"><span data-stu-id="10595-154">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="10595-155">HTML-разметка, предназначенные для отрисовки на стороне сервера, переданного в вызов функции разрешения, который помещается в JavaScript строго типизированных `Promise` объекта.</span><span class="sxs-lookup"><span data-stu-id="10595-155">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="10595-156">`Promise` Точность объекта — он асинхронно передает разметку HTML для страницы для вставки в элементе DOM заполнителя.</span><span class="sxs-lookup"><span data-stu-id="10595-156">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="10595-157">`asp-prerender-data` Тег вспомогательный</span><span class="sxs-lookup"><span data-stu-id="10595-157">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="10595-158">При работе с `asp-prerender-module` вспомогательный тег `asp-prerender-data` вспомогательный тега может использоваться для передачи контекстных данных из представления Razor JavaScript на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="10595-158">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="10595-159">Например, приведенный ниже код передает пользовательских данных для `main-server` модуля:</span><span class="sxs-lookup"><span data-stu-id="10595-159">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="10595-160">Данных, полученных `UserName` аргумент сериализуется с использованием как встроенный сериализатор JSON и хранится в `params.data` объекта.</span><span class="sxs-lookup"><span data-stu-id="10595-160">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="10595-161">В следующем примере углового данных используется для создания индивидуальное приветствие в `h1` элемента:</span><span class="sxs-lookup"><span data-stu-id="10595-161">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="10595-162">Примечание: Имена свойств, переданный вспомогательных функций тегов, были представлены **PascalCase** нотации.</span><span class="sxs-lookup"><span data-stu-id="10595-162">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="10595-163">Сравните это JavaScript, где представлены те же имена свойств с **camelCase**.</span><span class="sxs-lookup"><span data-stu-id="10595-163">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="10595-164">Конфигурация по умолчанию для сериализации JSON отвечает за это различие.</span><span class="sxs-lookup"><span data-stu-id="10595-164">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="10595-165">Для расширения в предыдущем примере кода, данные могут передаваться с сервера к представлению, hydrating `globals` для свойства `resolve` функции:</span><span class="sxs-lookup"><span data-stu-id="10595-165">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="10595-166">`postList` Массива, определенные внутри `globals` присоединен объект в браузере глобального `window` объекта.</span><span class="sxs-lookup"><span data-stu-id="10595-166">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="10595-167">Этой переменной подъем в глобальной области видимости исключает двойной работы, особенно относится к загрузке те же данные один раз на сервере и повторно на клиенте.</span><span class="sxs-lookup"><span data-stu-id="10595-167">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![глобальная переменная postList, присоединенные к объекту окна](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="10595-169">По промежуточного слоя Webpack разработки</span><span class="sxs-lookup"><span data-stu-id="10595-169">Webpack Dev Middleware</span></span>

<span data-ttu-id="10595-170">[По промежуточного слоя разработки Webpack](https://webpack.github.io/docs/webpack-dev-middleware.html) предлагает рабочий процесс разработки, при котором Webpack создает ресурсы по требованию.</span><span class="sxs-lookup"><span data-stu-id="10595-170">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="10595-171">По промежуточного слоя автоматически компилирует и выполняет ресурсами на стороне клиента при перезагрузке страницы в браузере.</span><span class="sxs-lookup"><span data-stu-id="10595-171">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="10595-172">Альтернативный подход заключается в вручную вызывать Webpack через скрипт построения проекта npm при изменении зависимости сторонних разработчиков или пользовательский код.</span><span class="sxs-lookup"><span data-stu-id="10595-172">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="10595-173">Npm создания скрипта *package.json* файл показан в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="10595-173">An npm build script in the *package.json* file is shown in the following example:</span></span>

[!code-json[](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a><span data-ttu-id="10595-174">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="10595-174">Prerequisites</span></span>

<span data-ttu-id="10595-175">Установите следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="10595-175">Install the following:</span></span>
* <span data-ttu-id="10595-176">[ASPNET webpack](https://www.npmjs.com/package/aspnet-webpack) пакета npm:</span><span class="sxs-lookup"><span data-stu-id="10595-176">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="10595-177">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="10595-177">Configuration</span></span>

<span data-ttu-id="10595-178">По промежуточного слоя разработки Webpack регистрируется в конвейер запросов HTTP через следующий код в *файла Startup.cs* файла `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="10595-178">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

<span data-ttu-id="10595-179">`UseWebpackDevMiddleware` Метод расширения должен быть вызван перед [регистрация статического файла размещения](xref:fundamentals/static-files) через `UseStaticFiles` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="10595-179">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="10595-180">По соображениям безопасности зарегистрируйте по промежуточного слоя, только в том случае, когда приложение запускается в режиме разработки.</span><span class="sxs-lookup"><span data-stu-id="10595-180">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="10595-181">*Webpack.config.js* файла `output.publicPath` свойство сообщает по промежуточного слоя для отслеживания `dist` папку для изменения:</span><span class="sxs-lookup"><span data-stu-id="10595-181">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="10595-182">Модуль горячей замены</span><span class="sxs-lookup"><span data-stu-id="10595-182">Hot Module Replacement</span></span>

<span data-ttu-id="10595-183">Можно представить в Webpack [горячей замены модуля](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) компоненте (HMR) с развитием [Webpack разработки по промежуточного слоя](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="10595-183">Think of Webpack's [Hot Module Replacement](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="10595-184">HMR представляет те же преимущества, но дополнительно упрощает рабочего процесса разработки автоматически обновит содержимое страницы после компиляции изменений.</span><span class="sxs-lookup"><span data-stu-id="10595-184">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="10595-185">Не следует путать с обновление браузера, который может повлиять на текущее состояние в памяти и SPA сеанс отладки.</span><span class="sxs-lookup"><span data-stu-id="10595-185">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="10595-186">Нет активную связь между службой Webpack разработки по промежуточного слоя и браузер, это означает, что изменения передаются в браузер.</span><span class="sxs-lookup"><span data-stu-id="10595-186">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="10595-187">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="10595-187">Prerequisites</span></span>

<span data-ttu-id="10595-188">Установите следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="10595-188">Install the following:</span></span>
* <span data-ttu-id="10595-189">[активные промежуточное webpack](https://www.npmjs.com/package/webpack-hot-middleware) пакета npm:</span><span class="sxs-lookup"><span data-stu-id="10595-189">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="10595-190">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="10595-190">Configuration</span></span>

<span data-ttu-id="10595-191">Компонент HMR должен быть зарегистрирован в конвейер запросов HTTP MVC в `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="10595-191">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="10595-192">Как и в случае имел значение true, если с [Webpack разработки по промежуточного слоя](#webpack-dev-middleware), `UseWebpackDevMiddleware` метод расширения должен быть вызван перед `UseStaticFiles` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="10595-192">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="10595-193">По соображениям безопасности зарегистрируйте по промежуточного слоя, только в том случае, когда приложение запускается в режиме разработки.</span><span class="sxs-lookup"><span data-stu-id="10595-193">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="10595-194">*Webpack.config.js* необходимо определить файл `plugins` массив, даже если он остается пустым:</span><span class="sxs-lookup"><span data-stu-id="10595-194">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="10595-195">После загрузки приложения в браузере, вкладку средства разработчика консоли предоставляет подтверждения активации HMR:</span><span class="sxs-lookup"><span data-stu-id="10595-195">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Сообщение о подключении горячей замены модуля](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="10595-197">Помощники маршрутизации</span><span class="sxs-lookup"><span data-stu-id="10595-197">Routing helpers</span></span>

<span data-ttu-id="10595-198">В большинстве ASP.NET Core-SPAs будет необходимо клиентские маршрутизации Помимо маршрутизации на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="10595-198">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="10595-199">Системы маршрутизации SPA и MVC могут работать независимо, без помех.</span><span class="sxs-lookup"><span data-stu-id="10595-199">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="10595-200">, Однако регистр дают один край решить проблемы: определение 404 HTTP-ответов.</span><span class="sxs-lookup"><span data-stu-id="10595-200">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="10595-201">Рассмотрим сценарий, в котором без расширений маршрут `/some/page` используется.</span><span class="sxs-lookup"><span data-stu-id="10595-201">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="10595-202">Предполагается запроса не фильтрует маршрута серверных, но его шаблоном соответствует маршруту клиентские.</span><span class="sxs-lookup"><span data-stu-id="10595-202">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="10595-203">Теперь рассмотрим входящий запрос `/images/user-512.png`, который обычно ожидает найти файл изображения на сервере.</span><span class="sxs-lookup"><span data-stu-id="10595-203">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="10595-204">Если этот путь запрашиваемого ресурса не соответствует любой маршрут на стороне сервера или статический файл, маловероятно, что клиентское приложение будет обрабатывать — обычно требуется возвращает код состояния HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="10595-204">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="10595-205">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="10595-205">Prerequisites</span></span>

<span data-ttu-id="10595-206">Установите следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="10595-206">Install the following:</span></span>
* <span data-ttu-id="10595-207">Клиентские маршрутизации пакета npm.</span><span class="sxs-lookup"><span data-stu-id="10595-207">The client-side routing npm package.</span></span> <span data-ttu-id="10595-208">В примере с момента:</span><span class="sxs-lookup"><span data-stu-id="10595-208">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="10595-209">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="10595-209">Configuration</span></span>

<span data-ttu-id="10595-210">Метод расширения с именем `MapSpaFallbackRoute` используется в `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="10595-210">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

<span data-ttu-id="10595-211">Совет: Маршруты, вычисляются в порядке, в котором они настроены.</span><span class="sxs-lookup"><span data-stu-id="10595-211">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="10595-212">Следовательно `default` маршрут в предыдущем примере сначала используется сопоставление по шаблону.</span><span class="sxs-lookup"><span data-stu-id="10595-212">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="10595-213">Создание нового проекта</span><span class="sxs-lookup"><span data-stu-id="10595-213">Creating a new project</span></span>

<span data-ttu-id="10595-214">JavaScriptServices предоставляет шаблоны предварительно настроенное приложение.</span><span class="sxs-lookup"><span data-stu-id="10595-214">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="10595-215">SpaServices используется в этих шаблонов в сочетании с помощью различных платформ и библиотек, например угловая, Aurelia, Knockout, реагирование на них и Vue.</span><span class="sxs-lookup"><span data-stu-id="10595-215">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, Aurelia, Knockout, React, and Vue.</span></span>

<span data-ttu-id="10595-216">Эти шаблоны может быть выполнена с помощью .NET Core CLI, выполнив следующую команду:</span><span class="sxs-lookup"><span data-stu-id="10595-216">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="10595-217">Отображается список доступных шаблонов SPA:</span><span class="sxs-lookup"><span data-stu-id="10595-217">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="10595-218">Шаблоны</span><span class="sxs-lookup"><span data-stu-id="10595-218">Templates</span></span>                                 | <span data-ttu-id="10595-219">Краткое имя</span><span class="sxs-lookup"><span data-stu-id="10595-219">Short Name</span></span> | <span data-ttu-id="10595-220">Язык</span><span class="sxs-lookup"><span data-stu-id="10595-220">Language</span></span> | <span data-ttu-id="10595-221">Теги</span><span class="sxs-lookup"><span data-stu-id="10595-221">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="10595-222">Ядро ASP.NET MVC с углового</span><span class="sxs-lookup"><span data-stu-id="10595-222">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="10595-223">angular</span><span class="sxs-lookup"><span data-stu-id="10595-223">angular</span></span>    | <span data-ttu-id="10595-224">[C#]</span><span class="sxs-lookup"><span data-stu-id="10595-224">[C#]</span></span>     | <span data-ttu-id="10595-225">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="10595-225">Web/MVC/SPA</span></span> |
| <span data-ttu-id="10595-226">Ядро ASP.NET MVC с Aurelia</span><span class="sxs-lookup"><span data-stu-id="10595-226">MVC ASP.NET Core with Aurelia</span></span>             | <span data-ttu-id="10595-227">aurelia</span><span class="sxs-lookup"><span data-stu-id="10595-227">aurelia</span></span>    | <span data-ttu-id="10595-228">[C#]</span><span class="sxs-lookup"><span data-stu-id="10595-228">[C#]</span></span>     | <span data-ttu-id="10595-229">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="10595-229">Web/MVC/SPA</span></span> |
| <span data-ttu-id="10595-230">Ядро ASP.NET MVC с Knockout.js</span><span class="sxs-lookup"><span data-stu-id="10595-230">MVC ASP.NET Core with Knockout.js</span></span>         | <span data-ttu-id="10595-231">маскирования</span><span class="sxs-lookup"><span data-stu-id="10595-231">knockout</span></span>   | <span data-ttu-id="10595-232">[C#]</span><span class="sxs-lookup"><span data-stu-id="10595-232">[C#]</span></span>     | <span data-ttu-id="10595-233">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="10595-233">Web/MVC/SPA</span></span> |
| <span data-ttu-id="10595-234">Ядро ASP.NET MVC с React.js</span><span class="sxs-lookup"><span data-stu-id="10595-234">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="10595-235">react</span><span class="sxs-lookup"><span data-stu-id="10595-235">react</span></span>      | <span data-ttu-id="10595-236">[C#]</span><span class="sxs-lookup"><span data-stu-id="10595-236">[C#]</span></span>     | <span data-ttu-id="10595-237">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="10595-237">Web/MVC/SPA</span></span> |
| <span data-ttu-id="10595-238">MVC ASP.NET Core с React.js и локализации</span><span class="sxs-lookup"><span data-stu-id="10595-238">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="10595-239">reactredux</span><span class="sxs-lookup"><span data-stu-id="10595-239">reactredux</span></span> | <span data-ttu-id="10595-240">[C#]</span><span class="sxs-lookup"><span data-stu-id="10595-240">[C#]</span></span>     | <span data-ttu-id="10595-241">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="10595-241">Web/MVC/SPA</span></span> |
| <span data-ttu-id="10595-242">Ядро ASP.NET MVC с Vue.js</span><span class="sxs-lookup"><span data-stu-id="10595-242">MVC ASP.NET Core with Vue.js</span></span>              | <span data-ttu-id="10595-243">VUE</span><span class="sxs-lookup"><span data-stu-id="10595-243">vue</span></span>        | <span data-ttu-id="10595-244">[C#]</span><span class="sxs-lookup"><span data-stu-id="10595-244">[C#]</span></span>     | <span data-ttu-id="10595-245">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="10595-245">Web/MVC/SPA</span></span> | 

<span data-ttu-id="10595-246">Чтобы создать новый проект с помощью одного из шаблонов SPA, включают **короткое имя** шаблона в [dotnet новый](/dotnet/core/tools/dotnet-new) команды.</span><span class="sxs-lookup"><span data-stu-id="10595-246">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="10595-247">Следующая команда создает углового приложения с ASP.NET MVC Core настроен на стороне сервера:</span><span class="sxs-lookup"><span data-stu-id="10595-247">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="10595-248">Установите режим среды выполнения конфигурации</span><span class="sxs-lookup"><span data-stu-id="10595-248">Set the runtime configuration mode</span></span>

<span data-ttu-id="10595-249">Существует два режима конфигурации основной среды выполнения:</span><span class="sxs-lookup"><span data-stu-id="10595-249">Two primary runtime configuration modes exist:</span></span>
* <span data-ttu-id="10595-250">**Разработка**:</span><span class="sxs-lookup"><span data-stu-id="10595-250">**Development**:</span></span>
    * <span data-ttu-id="10595-251">Содержит исходное сопоставление в целях отладки.</span><span class="sxs-lookup"><span data-stu-id="10595-251">Includes source maps to ease debugging.</span></span>
    * <span data-ttu-id="10595-252">Не Оптимизируйте код на стороне клиента для повышения производительности.</span><span class="sxs-lookup"><span data-stu-id="10595-252">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="10595-253">**Производство**:</span><span class="sxs-lookup"><span data-stu-id="10595-253">**Production**:</span></span>
    * <span data-ttu-id="10595-254">Исключает исходное сопоставление.</span><span class="sxs-lookup"><span data-stu-id="10595-254">Excludes source maps.</span></span>
    * <span data-ttu-id="10595-255">Оптимизирует код на стороне клиента через объединение и Минификация.</span><span class="sxs-lookup"><span data-stu-id="10595-255">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="10595-256">ASP.NET Core использует переменную среды с именем `ASPNETCORE_ENVIRONMENT` для хранения режим конфигурации.</span><span class="sxs-lookup"><span data-stu-id="10595-256">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="10595-257">В разделе **[параметр среды](xref:fundamentals/environments#setting-the-environment)** для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="10595-257">See **[Setting the environment](xref:fundamentals/environments#setting-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="10595-258">Запуск с .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="10595-258">Running with .NET Core CLI</span></span>

<span data-ttu-id="10595-259">Восстановление требуется NuGet и пакета npm, выполнив следующую команду в корне проекта.</span><span class="sxs-lookup"><span data-stu-id="10595-259">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="10595-260">Построение и запуск приложения.</span><span class="sxs-lookup"><span data-stu-id="10595-260">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="10595-261">Приложение запускается на localhost согласно [режим среды выполнения конфигурации](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="10595-261">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="10595-262">Переход к `http://localhost:5000` в браузере отображается главная страница.</span><span class="sxs-lookup"><span data-stu-id="10595-262">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="10595-263">Работает с Visual Studio 2017 г.</span><span class="sxs-lookup"><span data-stu-id="10595-263">Running with Visual Studio 2017</span></span>

<span data-ttu-id="10595-264">Откройте *.csproj* файл, созданный [dotnet новый](/dotnet/core/tools/dotnet-new) команды.</span><span class="sxs-lookup"><span data-stu-id="10595-264">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="10595-265">Необходимые пакеты NuGet и npm, автоматически восстанавливаются при открытии проекта.</span><span class="sxs-lookup"><span data-stu-id="10595-265">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="10595-266">Этот процесс восстановления может занять несколько минут, и приложение готово к запуску после ее завершения.</span><span class="sxs-lookup"><span data-stu-id="10595-266">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="10595-267">Нажмите зеленую кнопку для запуска или нажмите клавишу `Ctrl + F5`, и в браузере будет открыта на главную страницу приложения.</span><span class="sxs-lookup"><span data-stu-id="10595-267">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="10595-268">Приложение будет выполняться на localhost согласно [режим среды выполнения конфигурации](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="10595-268">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> 

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="10595-269">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="10595-269">Testing the app</span></span>

<span data-ttu-id="10595-270">Выполнять тесты на стороне клиента, с помощью шаблонов SpaServices предварительно настроены [Karma](https://karma-runner.github.io/1.0/index.html) и [Jasmine](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="10595-270">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="10595-271">Jasmine является популярных платформа модульного тестирования для JavaScript, тогда как Karma тестов для этих тестов.</span><span class="sxs-lookup"><span data-stu-id="10595-271">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="10595-272">Karma настроен для работы с [Webpack разработки по промежуточного слоя](#webpack-dev-middleware) таким образом, разработчику не требуется остановка и запуск теста, каждый раз при внесении изменений.</span><span class="sxs-lookup"><span data-stu-id="10595-272">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="10595-273">Является ли код запуска для тестового случая или сам тестовый случай, тест будет выполняться автоматически.</span><span class="sxs-lookup"><span data-stu-id="10595-273">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="10595-274">С помощью угловых приложения в качестве примера, два тестовых случаев Jasmine уже предоставляются для `CounterComponent` в *counter.component.spec.ts* файла:</span><span class="sxs-lookup"><span data-stu-id="10595-274">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="10595-275">Откройте окно командной строки, в корне проекта и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="10595-275">Open the command prompt at the project root, and run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="10595-276">Скрипт запускает средство запуска тестов Karma, который считывает параметры, определенные в *karma.conf.js* файла.</span><span class="sxs-lookup"><span data-stu-id="10595-276">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="10595-277">Вместе с другими параметрами *karma.conf.js* определяет файлы теста для выполнения через его `files` массива:</span><span class="sxs-lookup"><span data-stu-id="10595-277">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="10595-278">Публикация приложения</span><span class="sxs-lookup"><span data-stu-id="10595-278">Publishing the application</span></span>

<span data-ttu-id="10595-279">Объединение созданный клиентских средств и опубликованные артефакты ASP.NET Core в готовности развернуть пакет может быть очень обременительным.</span><span class="sxs-lookup"><span data-stu-id="10595-279">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="10595-280">К счастью, SpaServices управляет всей публикации процесса с пользовательской целевой объект MSBuild, с именем `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="10595-280">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="10595-281">Целевой объект MSBuild должен выполнить следующие действия:</span><span class="sxs-lookup"><span data-stu-id="10595-281">The MSBuild target has the following responsibilities:</span></span>
1. <span data-ttu-id="10595-282">Восстановление пакета npm</span><span class="sxs-lookup"><span data-stu-id="10595-282">Restore the npm packages</span></span>
1. <span data-ttu-id="10595-283">Создание сборки уровня рабочей сторонних разработчиков, клиентских средств</span><span class="sxs-lookup"><span data-stu-id="10595-283">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="10595-284">Создание уровня рабочей сборки пользовательских клиентских средств</span><span class="sxs-lookup"><span data-stu-id="10595-284">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="10595-285">Копирование ресурсов Webpack создается в папке публикации</span><span class="sxs-lookup"><span data-stu-id="10595-285">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="10595-286">Целевой объект MSBuild вызывается при выполнении:</span><span class="sxs-lookup"><span data-stu-id="10595-286">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="10595-287">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="10595-287">Additional resources</span></span>

* [<span data-ttu-id="10595-288">Угловое документы</span><span class="sxs-lookup"><span data-stu-id="10595-288">Angular Docs</span></span>](https://angular.io/docs)
