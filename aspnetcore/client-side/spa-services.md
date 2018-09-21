---
title: Использование JavaScriptServices для создания одностраничных приложений ASP.NET Core
author: scottaddie
description: Узнайте о преимуществах создания одностраничных приложений (SPA) с ASP.NET Core с помощью JavaScriptServices.
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
uid: client-side/spa-services
ms.openlocfilehash: 6d6a92427d5d4b853248e60a12625573c4375515
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523302"
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="0f430-103">Использование JavaScriptServices для создания одностраничных приложений ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0f430-103">Use JavaScriptServices to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="0f430-104">По [Scott Addie](https://github.com/scottaddie) и [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="0f430-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="0f430-105">Одностраничное приложение (SPA) — это популярный тип веб-приложения из-за его присущие многофункциональном пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="0f430-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="0f430-106">Интеграция клиентские платформы одностраничных ПРИЛОЖЕНИЙ или библиотек, таких как [Angular](https://angular.io/) или [React](https://facebook.github.io/react/), с помощью платформы на стороне сервера, такие как ASP.NET Core довольно сложно.</span><span class="sxs-lookup"><span data-stu-id="0f430-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="0f430-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) призвано трудностей, в процессе интеграции.</span><span class="sxs-lookup"><span data-stu-id="0f430-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="0f430-108">Он позволяет работать между различными клиентскими и стеков технологий сервера.</span><span class="sxs-lookup"><span data-stu-id="0f430-108">It enables seamless operation between the different client and server technology stacks.</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="0f430-109">Что такое JavaScriptServices</span><span class="sxs-lookup"><span data-stu-id="0f430-109">What is JavaScriptServices</span></span>

<span data-ttu-id="0f430-110">JavaScriptServices — это совокупность технологий на стороне клиента для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0f430-110">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="0f430-111">Наша цель — для размещения ASP.NET Core разработчиков предпочтительной платформой на стороне сервера для построения одностраничных приложений.</span><span class="sxs-lookup"><span data-stu-id="0f430-111">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="0f430-112">JavaScriptServices состоит из трех различных пакетов NuGet:</span><span class="sxs-lookup"><span data-stu-id="0f430-112">JavaScriptServices consists of three distinct NuGet packages:</span></span>

* <span data-ttu-id="0f430-113">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="0f430-113">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="0f430-114">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="0f430-114">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="0f430-115">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="0f430-115">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="0f430-116">Эти пакеты полезны, если вы:</span><span class="sxs-lookup"><span data-stu-id="0f430-116">These packages are useful if you:</span></span>

* <span data-ttu-id="0f430-117">Выполните на сервере JavaScript</span><span class="sxs-lookup"><span data-stu-id="0f430-117">Run JavaScript on the server</span></span>
* <span data-ttu-id="0f430-118">Использование библиотеки или платформы одностраничных ПРИЛОЖЕНИЙ</span><span class="sxs-lookup"><span data-stu-id="0f430-118">Use a SPA framework or library</span></span>
* <span data-ttu-id="0f430-119">Создание активов на стороне клиента с помощью Webpack</span><span class="sxs-lookup"><span data-stu-id="0f430-119">Build client-side assets with Webpack</span></span>

<span data-ttu-id="0f430-120">В этой статье внимание следует уделять помещается в с помощью пакета SpaServices.</span><span class="sxs-lookup"><span data-stu-id="0f430-120">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="0f430-121">Что такое SpaServices</span><span class="sxs-lookup"><span data-stu-id="0f430-121">What is SpaServices</span></span>

<span data-ttu-id="0f430-122">SpaServices был создан для размещения ASP.NET Core разработчиков предпочтительной платформой на стороне сервера для построения одностраничных приложений.</span><span class="sxs-lookup"><span data-stu-id="0f430-122">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="0f430-123">SpaServices не обязательно должна разработать одностраничных приложений с помощью ASP.NET Core, и он не блокирует в платформу, конкретного клиента.</span><span class="sxs-lookup"><span data-stu-id="0f430-123">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="0f430-124">SpaServices предоставляет полезные инфраструктуры, например:</span><span class="sxs-lookup"><span data-stu-id="0f430-124">SpaServices provides useful infrastructure such as:</span></span>

* [<span data-ttu-id="0f430-125">Предварительной визуализации на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="0f430-125">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="0f430-126">По промежуточного слоя Webpack разработки</span><span class="sxs-lookup"><span data-stu-id="0f430-126">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="0f430-127">Модуль горячей замены</span><span class="sxs-lookup"><span data-stu-id="0f430-127">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="0f430-128">Вспомогательные функции маршрутизации</span><span class="sxs-lookup"><span data-stu-id="0f430-128">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="0f430-129">В совокупности эти компоненты инфраструктуры улучшить рабочий процесс разработки и возможности среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="0f430-129">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="0f430-130">Компоненты, может быть использована по отдельности.</span><span class="sxs-lookup"><span data-stu-id="0f430-130">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="0f430-131">Предварительные требования для использования SpaServices</span><span class="sxs-lookup"><span data-stu-id="0f430-131">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="0f430-132">Для работы с SpaServices, установите следующее:</span><span class="sxs-lookup"><span data-stu-id="0f430-132">To work with SpaServices, install the following:</span></span>

* <span data-ttu-id="0f430-133">[Node.js](https://nodejs.org/) (версия 6 или более поздней версии) с помощью npm</span><span class="sxs-lookup"><span data-stu-id="0f430-133">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
  * <span data-ttu-id="0f430-134">Чтобы проверить эти компоненты устанавливаются и может быть найден, выполните следующую команду из командной строки:</span><span class="sxs-lookup"><span data-stu-id="0f430-134">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="0f430-135">Примечание: Если вы развертываете веб-сайте Azure, не нужно выполнять никаких действий, &mdash; Node.js установлена и доступна в серверных средах.</span><span class="sxs-lookup"><span data-stu-id="0f430-135">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * <span data-ttu-id="0f430-136">Если вы используете Windows, с помощью Visual Studio 2017, пакет SDK установлен, выбрав **кросс Платформенная разработка .NET Core** рабочей нагрузки.</span><span class="sxs-lookup"><span data-stu-id="0f430-136">If you're on Windows using Visual Studio 2017, the SDK is installed by selecting the **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="0f430-137">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) пакета NuGet</span><span class="sxs-lookup"><span data-stu-id="0f430-137">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="0f430-138">Предварительной визуализации на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="0f430-138">Server-side prerendering</span></span>

<span data-ttu-id="0f430-139">Универсальное приложение (также известный как isomorphic) — это приложение JavaScript, может запускать как на сервере и на клиенте.</span><span class="sxs-lookup"><span data-stu-id="0f430-139">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="0f430-140">Angular, React и других популярных платформ предоставляют универсальной платформы для этого стиля разработки приложения.</span><span class="sxs-lookup"><span data-stu-id="0f430-140">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="0f430-141">Идея состоит в том, чтобы сначала отрисовки компоненты платформы на сервере с помощью Node.js, а затем делегирует дальнейшего выполнения клиенту.</span><span class="sxs-lookup"><span data-stu-id="0f430-141">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="0f430-142">ASP.NET Core [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) предоставляемые SpaServices упрощают реализацию предварительной визуализации на сервере путем вызова функции JavaScript на сервере.</span><span class="sxs-lookup"><span data-stu-id="0f430-142">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="0f430-143">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="0f430-143">Prerequisites</span></span>

<span data-ttu-id="0f430-144">Установите следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="0f430-144">Install the following:</span></span>

* <span data-ttu-id="0f430-145">[предварительной визуализации ASPNET](https://www.npmjs.com/package/aspnet-prerendering) пакета npm:</span><span class="sxs-lookup"><span data-stu-id="0f430-145">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="0f430-146">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="0f430-146">Configuration</span></span>

<span data-ttu-id="0f430-147">Вспомогательные функции тегов выполняются с помощью функции регистрации пространства имен в проекте *_ViewImports.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="0f430-147">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="0f430-148">Эти вспомогательные функции тегов абстрагироваться от конкретных особенностей взаимодействовать непосредственно с низкоуровневых API за счет использования HTML-синтаксис типа в представлении Razor:</span><span class="sxs-lookup"><span data-stu-id="0f430-148">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="0f430-149">`asp-prerender-module` Вспомогательная функция тега</span><span class="sxs-lookup"><span data-stu-id="0f430-149">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="0f430-150">`asp-prerender-module` Вспомогательной функции тега, используемый в предыдущем примере код выполняет *ClientApp/dist/main-server.js* на сервере с помощью Node.js.</span><span class="sxs-lookup"><span data-stu-id="0f430-150">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="0f430-151">Для ясности *main server.js* файл — это артефакт задачи транспилирования TypeScript для JavaScript в [Webpack](http://webpack.github.io/) процесс сборки.</span><span class="sxs-lookup"><span data-stu-id="0f430-151">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="0f430-152">Webpack определяется псевдоним точки входа `main-server`; и начинается обход графа зависимостей для данного псевдонима *ClientApp/boot-server.ts* файла:</span><span class="sxs-lookup"><span data-stu-id="0f430-152">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="0f430-153">В следующем примере Angular *ClientApp/boot-server.ts* использует файл `createServerRenderer` функции и `RenderResult` тип `aspnet-prerendering` пакета npm для настройки подготовки сервера отчетов с помощью Node.js.</span><span class="sxs-lookup"><span data-stu-id="0f430-153">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="0f430-154">HTML-разметка, предназначенные для отрисовки на стороне сервера, переданного в вызов функции разрешения, который обернут в JavaScript со строгой типизацией `Promise` объекта.</span><span class="sxs-lookup"><span data-stu-id="0f430-154">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="0f430-155">`Promise` Точность объекта —, он асинхронно передает разметку HTML для страницы для внедрения в DOM замещающего элемента.</span><span class="sxs-lookup"><span data-stu-id="0f430-155">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="0f430-156">`asp-prerender-data` Вспомогательная функция тега</span><span class="sxs-lookup"><span data-stu-id="0f430-156">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="0f430-157">При связывании с `asp-prerender-module` вспомогательной функции тега, `asp-prerender-data` вспомогательная функция тега может использоваться для передачи контекстно-зависимые сведения из представления Razor для JavaScript на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="0f430-157">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="0f430-158">Например, следующая разметка передает данные на `main-server` модуля:</span><span class="sxs-lookup"><span data-stu-id="0f430-158">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="0f430-159">Полученных `UserName` аргумент сериализуется с помощью встроенного сериализатора JSON и хранятся в `params.data` объекта.</span><span class="sxs-lookup"><span data-stu-id="0f430-159">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="0f430-160">В следующем примере Angular данных используется для создания индивидуальное приветствие в `h1` элемент:</span><span class="sxs-lookup"><span data-stu-id="0f430-160">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="0f430-161">Примечание: Имена свойств, переданный вспомогательные функции тегов представляются с помощью **PascalCase** нотации.</span><span class="sxs-lookup"><span data-stu-id="0f430-161">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="0f430-162">Сравните это JavaScript, где представлены те же имена свойств **camelCase**.</span><span class="sxs-lookup"><span data-stu-id="0f430-162">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="0f430-163">Конфигурация по умолчанию для сериализации JSON несет ответственность за это различие.</span><span class="sxs-lookup"><span data-stu-id="0f430-163">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="0f430-164">Для обзора в предыдущем примере кода, данные могут передаваться с сервера в представление с hydrating `globals` указано свойство для `resolve` функции:</span><span class="sxs-lookup"><span data-stu-id="0f430-164">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="0f430-165">`postList` Массива, определенные внутри `globals` объект присоединяется к браузера глобального `window` объекта.</span><span class="sxs-lookup"><span data-stu-id="0f430-165">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="0f430-166">Этой переменной подъем глобальную область устраняет двойной работы, особенно в том случае, поскольку это имеет отношение к загрузке те же данные один раз на сервере и снова на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="0f430-166">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![глобальная переменная postList, присоединенный к объекту окна](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="0f430-168">По промежуточного слоя Webpack разработки</span><span class="sxs-lookup"><span data-stu-id="0f430-168">Webpack Dev Middleware</span></span>

<span data-ttu-id="0f430-169">[По промежуточного слоя разработки Webpack](https://webpack.github.io/docs/webpack-dev-middleware.html) предлагает рабочий процесс разработки, при котором Webpack создает ресурсы по требованию.</span><span class="sxs-lookup"><span data-stu-id="0f430-169">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="0f430-170">По промежуточного слоя автоматически компилирует и выполняет ресурсы на стороне клиента при перезагрузке страницы в браузере.</span><span class="sxs-lookup"><span data-stu-id="0f430-170">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="0f430-171">Альтернативный подход заключается в том, необходимо вручную вызвать Webpack через скрипт сборки проекта npm при изменении зависимость стороннего или пользовательского кода.</span><span class="sxs-lookup"><span data-stu-id="0f430-171">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="0f430-172">Сценарий построения npm *package.json* файл отображается в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="0f430-172">An npm build script in the *package.json* file is shown in the following example:</span></span>

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="prerequisites"></a><span data-ttu-id="0f430-173">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="0f430-173">Prerequisites</span></span>

<span data-ttu-id="0f430-174">Установите следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="0f430-174">Install the following:</span></span>

* <span data-ttu-id="0f430-175">[ASPNET webpack](https://www.npmjs.com/package/aspnet-webpack) пакета npm:</span><span class="sxs-lookup"><span data-stu-id="0f430-175">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="0f430-176">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="0f430-176">Configuration</span></span>

<span data-ttu-id="0f430-177">По промежуточного слоя разработки Webpack регистрируется в конвейер запросов HTTP с помощью следующего кода в *Startup.cs* файла `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="0f430-177">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

<span data-ttu-id="0f430-178">`UseWebpackDevMiddleware` Метод расширения должен быть вызван перед [регистрации размещения статического файла](xref:fundamentals/static-files) через `UseStaticFiles` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="0f430-178">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="0f430-179">По соображениям безопасности зарегистрируйте по промежуточного слоя, только в том случае, когда приложение запускается в режиме разработки.</span><span class="sxs-lookup"><span data-stu-id="0f430-179">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="0f430-180">*Webpack.config.js* файла `output.publicPath` свойство сообщает по промежуточного слоя, чтобы просмотреть `dist` изменений в папке:</span><span class="sxs-lookup"><span data-stu-id="0f430-180">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="0f430-181">Модуль горячей замены</span><span class="sxs-lookup"><span data-stu-id="0f430-181">Hot Module Replacement</span></span>

<span data-ttu-id="0f430-182">Можно рассматривать его Webpack [горячей замены модуля](https://webpack.js.org/concepts/hot-module-replacement/) компонента (HMR), как развитием [по промежуточного слоя разработки Webpack](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="0f430-182">Think of Webpack's [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="0f430-183">HMR представляет те же преимущества, но дополнительно упрощает рабочий процесс разработки, автоматически обновляя содержимое страницы после компиляции изменений.</span><span class="sxs-lookup"><span data-stu-id="0f430-183">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="0f430-184">Не следует путать с обновить браузер, который может повлиять на текущее состояние в памяти и сеанс отладки из SPA.</span><span class="sxs-lookup"><span data-stu-id="0f430-184">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="0f430-185">Нет активную связь между службой по промежуточного слоя разработки Webpack и браузера, это означает, что изменения будут передаваться в браузер.</span><span class="sxs-lookup"><span data-stu-id="0f430-185">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="0f430-186">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="0f430-186">Prerequisites</span></span>

<span data-ttu-id="0f430-187">Установите следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="0f430-187">Install the following:</span></span>

* <span data-ttu-id="0f430-188">["Горячий" промежуточное webpack](https://www.npmjs.com/package/webpack-hot-middleware) пакета npm:</span><span class="sxs-lookup"><span data-stu-id="0f430-188">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="0f430-189">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="0f430-189">Configuration</span></span>

<span data-ttu-id="0f430-190">Компонент HMR должны быть зарегистрированы в конвейер запросов HTTP MVC в `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="0f430-190">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="0f430-191">Как было [по промежуточного слоя разработки Webpack](#webpack-dev-middleware), `UseWebpackDevMiddleware` метод расширения должен быть вызван перед `UseStaticFiles` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="0f430-191">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="0f430-192">По соображениям безопасности зарегистрируйте по промежуточного слоя, только в том случае, когда приложение запускается в режиме разработки.</span><span class="sxs-lookup"><span data-stu-id="0f430-192">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="0f430-193">*Webpack.config.js* необходимо определить файл `plugins` массив, даже если оставить эти поля пустыми:</span><span class="sxs-lookup"><span data-stu-id="0f430-193">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="0f430-194">После загрузки приложения в браузере, вкладку средства разработчика консоли предоставляет подтверждение HMR активации:</span><span class="sxs-lookup"><span data-stu-id="0f430-194">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Сообщение о подключении горячей замены модуля](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="0f430-196">Вспомогательные функции маршрутизации</span><span class="sxs-lookup"><span data-stu-id="0f430-196">Routing helpers</span></span>

<span data-ttu-id="0f430-197">В большинстве на базе ASP.NET Core одностраничные приложения имеет смысл клиентские маршрутизации в дополнение к маршрутизации на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="0f430-197">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="0f430-198">Не мешая систем маршрутизации SPA и MVC могут работать независимо.</span><span class="sxs-lookup"><span data-stu-id="0f430-198">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="0f430-199">, Однако проблемы вариантов дают один edge: определение ответы HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="0f430-199">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="0f430-200">Рассмотрим сценарий, в котором без расширений маршрут `/some/page` используется.</span><span class="sxs-lookup"><span data-stu-id="0f430-200">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="0f430-201">Предположим, запрос не фильтрует маршрут на стороне сервера, но его шаблон соответствует маршрут со стороны клиента.</span><span class="sxs-lookup"><span data-stu-id="0f430-201">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="0f430-202">Теперь рассмотрим входящий запрос `/images/user-512.png`, который обычно ожидает найти файл изображения на сервере.</span><span class="sxs-lookup"><span data-stu-id="0f430-202">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="0f430-203">Если этот путь запрошенного ресурса не соответствует любой маршрут на стороне сервера или статических файлов, маловероятно, что клиентское приложение будет обрабатывать его, как правило, который необходимо вернуть код состояния HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="0f430-203">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="0f430-204">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="0f430-204">Prerequisites</span></span>

<span data-ttu-id="0f430-205">Установите следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="0f430-205">Install the following:</span></span>

* <span data-ttu-id="0f430-206">Клиентские маршрутизации пакета npm.</span><span class="sxs-lookup"><span data-stu-id="0f430-206">The client-side routing npm package.</span></span> <span data-ttu-id="0f430-207">Используя Angular в качестве примера:</span><span class="sxs-lookup"><span data-stu-id="0f430-207">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="0f430-208">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="0f430-208">Configuration</span></span>

<span data-ttu-id="0f430-209">Метод расширения с именем `MapSpaFallbackRoute` используется в `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="0f430-209">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

<span data-ttu-id="0f430-210">Совет: Маршруты оцениваются в порядке, в котором они настроены.</span><span class="sxs-lookup"><span data-stu-id="0f430-210">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="0f430-211">Следовательно `default` маршрута в приведенном выше примере кода используется сначала для сопоставления шаблонов.</span><span class="sxs-lookup"><span data-stu-id="0f430-211">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="0f430-212">Создание нового проекта</span><span class="sxs-lookup"><span data-stu-id="0f430-212">Creating a new project</span></span>

<span data-ttu-id="0f430-213">JavaScriptServices предоставляет шаблоны предварительно настроенное приложение.</span><span class="sxs-lookup"><span data-stu-id="0f430-213">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="0f430-214">SpaServices используется в этих шаблонах, в сочетании с различных платформ и библиотек, например Angular, React и Redux.</span><span class="sxs-lookup"><span data-stu-id="0f430-214">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, React, and Redux.</span></span>

<span data-ttu-id="0f430-215">Эти шаблоны можно установить с помощью интерфейса командной строки .NET Core, выполнив следующую команду:</span><span class="sxs-lookup"><span data-stu-id="0f430-215">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="0f430-216">Отображается список доступных шаблонов одностраничных ПРИЛОЖЕНИЙ:</span><span class="sxs-lookup"><span data-stu-id="0f430-216">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="0f430-217">Шаблоны</span><span class="sxs-lookup"><span data-stu-id="0f430-217">Templates</span></span>                                 | <span data-ttu-id="0f430-218">Краткое имя</span><span class="sxs-lookup"><span data-stu-id="0f430-218">Short Name</span></span> | <span data-ttu-id="0f430-219">Язык</span><span class="sxs-lookup"><span data-stu-id="0f430-219">Language</span></span> | <span data-ttu-id="0f430-220">Теги</span><span class="sxs-lookup"><span data-stu-id="0f430-220">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="0f430-221">MVC ASP.NET Core с Angular</span><span class="sxs-lookup"><span data-stu-id="0f430-221">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="0f430-222">angular</span><span class="sxs-lookup"><span data-stu-id="0f430-222">angular</span></span>    | <span data-ttu-id="0f430-223">[C#]</span><span class="sxs-lookup"><span data-stu-id="0f430-223">[C#]</span></span>     | <span data-ttu-id="0f430-224">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="0f430-224">Web/MVC/SPA</span></span> |
| <span data-ttu-id="0f430-225">MVC ASP.NET Core с React.js</span><span class="sxs-lookup"><span data-stu-id="0f430-225">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="0f430-226">react</span><span class="sxs-lookup"><span data-stu-id="0f430-226">react</span></span>      | <span data-ttu-id="0f430-227">[C#]</span><span class="sxs-lookup"><span data-stu-id="0f430-227">[C#]</span></span>     | <span data-ttu-id="0f430-228">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="0f430-228">Web/MVC/SPA</span></span> |
| <span data-ttu-id="0f430-229">MVC ASP.NET Core с React.js и Redux</span><span class="sxs-lookup"><span data-stu-id="0f430-229">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="0f430-230">reactredux</span><span class="sxs-lookup"><span data-stu-id="0f430-230">reactredux</span></span> | <span data-ttu-id="0f430-231">[C#]</span><span class="sxs-lookup"><span data-stu-id="0f430-231">[C#]</span></span>     | <span data-ttu-id="0f430-232">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="0f430-232">Web/MVC/SPA</span></span> |

<span data-ttu-id="0f430-233">Чтобы создать новый проект с помощью одного из шаблонов одностраничных ПРИЛОЖЕНИЙ, включают **короткое имя** шаблона в [команды dotnet new](/dotnet/core/tools/dotnet-new) команды.</span><span class="sxs-lookup"><span data-stu-id="0f430-233">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="0f430-234">Следующая команда создает Angular приложения с ASP.NET Core MVC, который настроен на стороне сервера:</span><span class="sxs-lookup"><span data-stu-id="0f430-234">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="0f430-235">Включить режим конфигурации среды выполнения</span><span class="sxs-lookup"><span data-stu-id="0f430-235">Set the runtime configuration mode</span></span>

<span data-ttu-id="0f430-236">Существует два режима конфигурации основной среды выполнения:</span><span class="sxs-lookup"><span data-stu-id="0f430-236">Two primary runtime configuration modes exist:</span></span>

* <span data-ttu-id="0f430-237">**Разработка**:</span><span class="sxs-lookup"><span data-stu-id="0f430-237">**Development**:</span></span>
  * <span data-ttu-id="0f430-238">Включает в себя исходных сопоставлений в целях отладки.</span><span class="sxs-lookup"><span data-stu-id="0f430-238">Includes source maps to ease debugging.</span></span>
  * <span data-ttu-id="0f430-239">Не предназначен для оптимизации клиентского кода для повышения производительности.</span><span class="sxs-lookup"><span data-stu-id="0f430-239">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="0f430-240">**Производство**:</span><span class="sxs-lookup"><span data-stu-id="0f430-240">**Production**:</span></span>
  * <span data-ttu-id="0f430-241">Исключает исходных сопоставлений.</span><span class="sxs-lookup"><span data-stu-id="0f430-241">Excludes source maps.</span></span>
  * <span data-ttu-id="0f430-242">Оптимизирует клиентского кода с помощью объединения и минификации.</span><span class="sxs-lookup"><span data-stu-id="0f430-242">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="0f430-243">ASP.NET Core использует переменную среды с именем `ASPNETCORE_ENVIRONMENT` для хранения режим конфигурации.</span><span class="sxs-lookup"><span data-stu-id="0f430-243">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="0f430-244">См. в разделе **[среду](xref:fundamentals/environments#set-the-environment)** Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="0f430-244">See **[Set the environment](xref:fundamentals/environments#set-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="0f430-245">Интерфейс CLI .NET Core, выполнив</span><span class="sxs-lookup"><span data-stu-id="0f430-245">Running with .NET Core CLI</span></span>

<span data-ttu-id="0f430-246">Восстановление NuGet требуется и пакеты npm, выполнив следующую команду в корневом каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="0f430-246">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="0f430-247">Построение и запуск приложения.</span><span class="sxs-lookup"><span data-stu-id="0f430-247">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="0f430-248">Запускает приложения на localhost в соответствии с [режим конфигурации среды выполнения](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="0f430-248">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="0f430-249">Переход к `http://localhost:5000` в браузере отображает целевую страницу.</span><span class="sxs-lookup"><span data-stu-id="0f430-249">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="0f430-250">С Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="0f430-250">Running with Visual Studio 2017</span></span>

<span data-ttu-id="0f430-251">Откройте *.csproj* файла, созданного [команды dotnet new](/dotnet/core/tools/dotnet-new) команды.</span><span class="sxs-lookup"><span data-stu-id="0f430-251">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="0f430-252">Необходимые пакеты NuGet и npm, автоматически восстанавливаются при открытии проекта.</span><span class="sxs-lookup"><span data-stu-id="0f430-252">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="0f430-253">Этот процесс восстановления может занять несколько минут, и приложение будет готово для запуска после ее завершения.</span><span class="sxs-lookup"><span data-stu-id="0f430-253">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="0f430-254">Щелкните зеленую кнопку запуска или нажмите клавишу `Ctrl + F5`, и в браузере откроется целевая страница приложения.</span><span class="sxs-lookup"><span data-stu-id="0f430-254">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="0f430-255">Приложение выполняется на локальном компьютере в соответствии с [режим конфигурации среды выполнения](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="0f430-255">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span>

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="0f430-256">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="0f430-256">Testing the app</span></span>

<span data-ttu-id="0f430-257">Шаблоны SpaServices предварительно настроены для запуска тестов на стороне клиента, с помощью [Karma](https://karma-runner.github.io/1.0/index.html) и [Jasmine](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="0f430-257">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="0f430-258">Jasmine — популярный платформа модульного тестирования для JavaScript, а Karma — тестов для этих тестов.</span><span class="sxs-lookup"><span data-stu-id="0f430-258">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="0f430-259">Karma настроен для работы с [по промежуточного слоя разработки Webpack](#webpack-dev-middleware) таким образом, чтобы разработчику не нужно будет остановить и запустить тест, каждый раз при внесении изменений.</span><span class="sxs-lookup"><span data-stu-id="0f430-259">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="0f430-260">Будь то код, выполняемый тестовый случай или сам тестовый случай, тест выполняется автоматически.</span><span class="sxs-lookup"><span data-stu-id="0f430-260">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="0f430-261">С помощью приложения Angular, например, два Жасмин тестовых случаев уже предоставляются для `CounterComponent` в *counter.component.spec.ts* файла:</span><span class="sxs-lookup"><span data-stu-id="0f430-261">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="0f430-262">Откройте командную строку в *ClientApp* каталога.</span><span class="sxs-lookup"><span data-stu-id="0f430-262">Open the command prompt in the *ClientApp* directory.</span></span> <span data-ttu-id="0f430-263">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="0f430-263">Run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="0f430-264">Сценарий запускает средство запуска тестов Karma, который считывает параметры, определенные в *karma.conf.js* файла.</span><span class="sxs-lookup"><span data-stu-id="0f430-264">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="0f430-265">Вместе с другими параметрами *karma.conf.js* определяет файлы теста для выполнения с помощью его `files` массива:</span><span class="sxs-lookup"><span data-stu-id="0f430-265">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="0f430-266">Публикация приложения</span><span class="sxs-lookup"><span data-stu-id="0f430-266">Publishing the application</span></span>

<span data-ttu-id="0f430-267">Объединение созданных ресурсов на стороне клиента и опубликованных артефакты ASP.NET Core в готовые к развертыванию пакет может быть громоздким.</span><span class="sxs-lookup"><span data-stu-id="0f430-267">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="0f430-268">К счастью, SpaServices организует этот процесс всей публикации с пользовательский целевой объект MSBuild с именем `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="0f430-268">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="0f430-269">Целевой объект MSBuild должен выполнить следующие:</span><span class="sxs-lookup"><span data-stu-id="0f430-269">The MSBuild target has the following responsibilities:</span></span>

1. <span data-ttu-id="0f430-270">Восстановление пакетов npm</span><span class="sxs-lookup"><span data-stu-id="0f430-270">Restore the npm packages</span></span>
1. <span data-ttu-id="0f430-271">Создание сборки производственного уровня средств сторонних, на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="0f430-271">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="0f430-272">Создание сборки производственного уровня пользовательских средств на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="0f430-272">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="0f430-273">Копирование ресурсов Webpack создается в папке публикации</span><span class="sxs-lookup"><span data-stu-id="0f430-273">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="0f430-274">Целевой объект MSBuild вызывается при запуске:</span><span class="sxs-lookup"><span data-stu-id="0f430-274">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="0f430-275">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0f430-275">Additional resources</span></span>

* [<span data-ttu-id="0f430-276">Документация по angular</span><span class="sxs-lookup"><span data-stu-id="0f430-276">Angular Docs</span></span>](https://angular.io/docs)
