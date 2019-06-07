---
title: Использование служб JavaScript для создания одностраничных приложений ASP.NET Core
author: scottaddie
description: Узнайте о преимуществах использования службы JavaScript для создания одностраничных приложений (SPA) с ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 05/28/2019
uid: client-side/spa-services
ms.openlocfilehash: c7cd35865c5bddf0e5efaa9e616832b6755d9227
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750123"
---
# <a name="use-javascript-services-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="1f420-103">Использование служб JavaScript для создания одностраничных приложений ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1f420-103">Use JavaScript Services to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="1f420-104">По [Scott Addie](https://github.com/scottaddie) и [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="1f420-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="1f420-105">Одностраничное приложение (SPA) — это популярный тип веб-приложения из-за его присущие многофункциональном пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="1f420-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="1f420-106">Интеграция клиентские платформы одностраничных ПРИЛОЖЕНИЙ или библиотек, таких как [Angular](https://angular.io/) или [React](https://facebook.github.io/react/), серверные платформы, такие как ASP.NET Core довольно сложно.</span><span class="sxs-lookup"><span data-stu-id="1f420-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks such as ASP.NET Core can be difficult.</span></span> <span data-ttu-id="1f420-107">Службы JavaScript был разработан для трудностей, в процессе интеграции.</span><span class="sxs-lookup"><span data-stu-id="1f420-107">JavaScript Services was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="1f420-108">Он позволяет работать между различными клиентскими и стеков технологий сервера.</span><span class="sxs-lookup"><span data-stu-id="1f420-108">It enables seamless operation between the different client and server technology stacks.</span></span>

## <a name="what-is-javascript-services"></a><span data-ttu-id="1f420-109">Что такое службы JavaScript</span><span class="sxs-lookup"><span data-stu-id="1f420-109">What is JavaScript Services</span></span>

<span data-ttu-id="1f420-110">Службы JavaScript — это совокупность технологий на стороне клиента для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1f420-110">JavaScript Services is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="1f420-111">Наша цель — для размещения ASP.NET Core разработчиков предпочтительной платформой на стороне сервера для построения одностраничных приложений.</span><span class="sxs-lookup"><span data-stu-id="1f420-111">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="1f420-112">Службы JavaScript состоят из двух различных пакетов NuGet:</span><span class="sxs-lookup"><span data-stu-id="1f420-112">JavaScript Services consists of two distinct NuGet packages:</span></span>

* <span data-ttu-id="1f420-113">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="1f420-113">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="1f420-114">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="1f420-114">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>

<span data-ttu-id="1f420-115">Эти пакеты полезны в следующих сценариях:</span><span class="sxs-lookup"><span data-stu-id="1f420-115">These packages are useful in the following scenarios:</span></span>

* <span data-ttu-id="1f420-116">Выполните на сервере JavaScript</span><span class="sxs-lookup"><span data-stu-id="1f420-116">Run JavaScript on the server</span></span>
* <span data-ttu-id="1f420-117">Использование библиотеки или платформы одностраничных ПРИЛОЖЕНИЙ</span><span class="sxs-lookup"><span data-stu-id="1f420-117">Use a SPA framework or library</span></span>
* <span data-ttu-id="1f420-118">Создание активов на стороне клиента с помощью Webpack</span><span class="sxs-lookup"><span data-stu-id="1f420-118">Build client-side assets with Webpack</span></span>

<span data-ttu-id="1f420-119">В этой статье внимание следует уделять помещается в с помощью пакета SpaServices.</span><span class="sxs-lookup"><span data-stu-id="1f420-119">Much of the focus in this article is placed on using the SpaServices package.</span></span>

## <a name="what-is-spaservices"></a><span data-ttu-id="1f420-120">Что такое SpaServices</span><span class="sxs-lookup"><span data-stu-id="1f420-120">What is SpaServices</span></span>

<span data-ttu-id="1f420-121">SpaServices был создан для размещения ASP.NET Core разработчиков предпочтительной платформой на стороне сервера для построения одностраничных приложений.</span><span class="sxs-lookup"><span data-stu-id="1f420-121">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="1f420-122">SpaServices не обязательно должна разработать одностраничных приложений с помощью ASP.NET Core, и он не блокирует разработчиков в платформу, конкретного клиента.</span><span class="sxs-lookup"><span data-stu-id="1f420-122">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock developers into a particular client framework.</span></span>

<span data-ttu-id="1f420-123">SpaServices предоставляет полезные инфраструктуры, например:</span><span class="sxs-lookup"><span data-stu-id="1f420-123">SpaServices provides useful infrastructure such as:</span></span>

* [<span data-ttu-id="1f420-124">Предварительной визуализации на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="1f420-124">Server-side prerendering</span></span>](#server-side-prerendering)
* [<span data-ttu-id="1f420-125">По промежуточного слоя Webpack разработки</span><span class="sxs-lookup"><span data-stu-id="1f420-125">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="1f420-126">Модуль горячей замены</span><span class="sxs-lookup"><span data-stu-id="1f420-126">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="1f420-127">Вспомогательные функции маршрутизации</span><span class="sxs-lookup"><span data-stu-id="1f420-127">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="1f420-128">В совокупности эти компоненты инфраструктуры улучшить рабочий процесс разработки и возможности среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="1f420-128">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="1f420-129">Компоненты, может быть использована по отдельности.</span><span class="sxs-lookup"><span data-stu-id="1f420-129">The components can be adopted individually.</span></span>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="1f420-130">Предварительные требования для использования SpaServices</span><span class="sxs-lookup"><span data-stu-id="1f420-130">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="1f420-131">Для работы с SpaServices, установите следующее:</span><span class="sxs-lookup"><span data-stu-id="1f420-131">To work with SpaServices, install the following:</span></span>

* <span data-ttu-id="1f420-132">[Node.js](https://nodejs.org/) (версия 6 или более поздней версии) с помощью npm</span><span class="sxs-lookup"><span data-stu-id="1f420-132">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>

  * <span data-ttu-id="1f420-133">Чтобы проверить эти компоненты устанавливаются и может быть найден, выполните следующую команду из командной строки:</span><span class="sxs-lookup"><span data-stu-id="1f420-133">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

  * <span data-ttu-id="1f420-134">Если развертывание веб-сайте Azure, никаких действий не требуется&mdash;Node.js установлена и доступна в серверных средах.</span><span class="sxs-lookup"><span data-stu-id="1f420-134">If deploying to an Azure web site, no action is required&mdash;Node.js is installed and available in the server environments.</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * <span data-ttu-id="1f420-135">В Windows, с помощью Visual Studio 2017, установлен пакет SDK, выбрав **кросс Платформенная разработка .NET Core** рабочей нагрузки.</span><span class="sxs-lookup"><span data-stu-id="1f420-135">On Windows using Visual Studio 2017, the SDK is installed by selecting the **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="1f420-136">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) пакета NuGet</span><span class="sxs-lookup"><span data-stu-id="1f420-136">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

## <a name="server-side-prerendering"></a><span data-ttu-id="1f420-137">Предварительной визуализации на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="1f420-137">Server-side prerendering</span></span>

<span data-ttu-id="1f420-138">Универсальное приложение (также известный как isomorphic) — это приложение JavaScript, может запускать как на сервере и на клиенте.</span><span class="sxs-lookup"><span data-stu-id="1f420-138">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="1f420-139">Angular, React и других популярных платформ предоставляют универсальной платформы для этого стиля разработки приложения.</span><span class="sxs-lookup"><span data-stu-id="1f420-139">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="1f420-140">Идея состоит в том, чтобы сначала отрисовки компоненты платформы на сервере с помощью Node.js, а затем делегирует дальнейшего выполнения клиенту.</span><span class="sxs-lookup"><span data-stu-id="1f420-140">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="1f420-141">ASP.NET Core [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) предоставляемые SpaServices упрощают реализацию предварительной визуализации на сервере путем вызова функции JavaScript на сервере.</span><span class="sxs-lookup"><span data-stu-id="1f420-141">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="server-side-prerendering-prerequisites"></a><span data-ttu-id="1f420-142">Необходимые компоненты предварительной отрисовки на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="1f420-142">Server-side prerendering prerequisites</span></span>

<span data-ttu-id="1f420-143">Установка [предварительной визуализации aspnet](https://www.npmjs.com/package/aspnet-prerendering) пакета npm:</span><span class="sxs-lookup"><span data-stu-id="1f420-143">Install the [aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

```console
npm i -S aspnet-prerendering
```

### <a name="server-side-prerendering-configuration"></a><span data-ttu-id="1f420-144">Настройка предварительной отрисовки на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="1f420-144">Server-side prerendering configuration</span></span>

<span data-ttu-id="1f420-145">Вспомогательные функции тегов выполняются с помощью функции регистрации пространства имен в проекте *_ViewImports.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="1f420-145">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="1f420-146">Эти вспомогательные функции тегов абстрагироваться от конкретных особенностей взаимодействовать непосредственно с низкоуровневых API за счет использования HTML-синтаксис типа в представлении Razor:</span><span class="sxs-lookup"><span data-stu-id="1f420-146">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="asp-prerender-module-tag-helper"></a><span data-ttu-id="1f420-147">Вспомогательная функция тега ASP-prerender-module</span><span class="sxs-lookup"><span data-stu-id="1f420-147">asp-prerender-module Tag Helper</span></span>

<span data-ttu-id="1f420-148">`asp-prerender-module` Вспомогательной функции тега, используемый в предыдущем примере код выполняет *ClientApp/dist/main-server.js* на сервере с помощью Node.js.</span><span class="sxs-lookup"><span data-stu-id="1f420-148">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="1f420-149">Для ясности *main server.js* файл — это артефакт задачи транспилирования TypeScript для JavaScript в [Webpack](http://webpack.github.io/) процесс сборки.</span><span class="sxs-lookup"><span data-stu-id="1f420-149">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="1f420-150">Webpack определяется псевдоним точки входа `main-server`; и начинается обход графа зависимостей для данного псевдонима *ClientApp/boot-server.ts* файла:</span><span class="sxs-lookup"><span data-stu-id="1f420-150">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="1f420-151">В следующем примере Angular *ClientApp/boot-server.ts* использует файл `createServerRenderer` функции и `RenderResult` тип `aspnet-prerendering` пакета npm для настройки подготовки сервера отчетов с помощью Node.js.</span><span class="sxs-lookup"><span data-stu-id="1f420-151">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="1f420-152">HTML-разметка, предназначенные для отрисовки на стороне сервера, переданного в вызов функции разрешения, который обернут в JavaScript со строгой типизацией `Promise` объекта.</span><span class="sxs-lookup"><span data-stu-id="1f420-152">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="1f420-153">`Promise` Точность объекта —, он асинхронно передает разметку HTML для страницы для внедрения в DOM замещающего элемента.</span><span class="sxs-lookup"><span data-stu-id="1f420-153">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="asp-prerender-data-tag-helper"></a><span data-ttu-id="1f420-154">Вспомогательная функция тега ASP-prerender-data</span><span class="sxs-lookup"><span data-stu-id="1f420-154">asp-prerender-data Tag Helper</span></span>

<span data-ttu-id="1f420-155">При связывании с `asp-prerender-module` вспомогательной функции тега, `asp-prerender-data` вспомогательная функция тега может использоваться для передачи контекстно-зависимые сведения из представления Razor для JavaScript на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="1f420-155">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="1f420-156">Например, следующая разметка передает данные на `main-server` модуля:</span><span class="sxs-lookup"><span data-stu-id="1f420-156">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="1f420-157">Полученных `UserName` аргумент сериализуется с помощью встроенного сериализатора JSON и хранятся в `params.data` объекта.</span><span class="sxs-lookup"><span data-stu-id="1f420-157">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="1f420-158">В следующем примере Angular данных используется для создания индивидуальное приветствие в `h1` элемент:</span><span class="sxs-lookup"><span data-stu-id="1f420-158">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="1f420-159">Имена свойств, переданный вспомогательные функции тегов представляются с помощью **PascalCase** нотации.</span><span class="sxs-lookup"><span data-stu-id="1f420-159">Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="1f420-160">Сравните это JavaScript, где представлены те же имена свойств **camelCase**.</span><span class="sxs-lookup"><span data-stu-id="1f420-160">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="1f420-161">Конфигурация по умолчанию для сериализации JSON несет ответственность за это различие.</span><span class="sxs-lookup"><span data-stu-id="1f420-161">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="1f420-162">Для обзора в предыдущем примере кода, данные могут передаваться с сервера в представление с hydrating `globals` указано свойство для `resolve` функции:</span><span class="sxs-lookup"><span data-stu-id="1f420-162">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="1f420-163">`postList` Массива, определенные внутри `globals` объект присоединяется к браузера глобального `window` объекта.</span><span class="sxs-lookup"><span data-stu-id="1f420-163">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="1f420-164">Этой переменной подъем глобальную область устраняет двойной работы, особенно в том случае, поскольку это имеет отношение к загрузке те же данные один раз на сервере и снова на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="1f420-164">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![глобальная переменная postList, присоединенный к объекту окна](spa-services/_static/global_variable.png)

## <a name="webpack-dev-middleware"></a><span data-ttu-id="1f420-166">По промежуточного слоя Webpack разработки</span><span class="sxs-lookup"><span data-stu-id="1f420-166">Webpack Dev Middleware</span></span>

<span data-ttu-id="1f420-167">[По промежуточного слоя разработки Webpack](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) предлагает рабочий процесс разработки, при котором Webpack создает ресурсы по требованию.</span><span class="sxs-lookup"><span data-stu-id="1f420-167">[Webpack Dev Middleware](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="1f420-168">По промежуточного слоя автоматически компилирует и выполняет ресурсы на стороне клиента при перезагрузке страницы в браузере.</span><span class="sxs-lookup"><span data-stu-id="1f420-168">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="1f420-169">Альтернативный подход заключается в том, необходимо вручную вызвать Webpack через скрипт сборки проекта npm при изменении зависимость стороннего или пользовательского кода.</span><span class="sxs-lookup"><span data-stu-id="1f420-169">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="1f420-170">Сценарий построения npm *package.json* файл отображается в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="1f420-170">An npm build script in the *package.json* file is shown in the following example:</span></span>

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="webpack-dev-middleware-prerequisites"></a><span data-ttu-id="1f420-171">Предварительные требования по промежуточного слоя Webpack разработки</span><span class="sxs-lookup"><span data-stu-id="1f420-171">Webpack Dev Middleware prerequisites</span></span>

<span data-ttu-id="1f420-172">Установка [aspnet webpack](https://www.npmjs.com/package/aspnet-webpack) пакета npm:</span><span class="sxs-lookup"><span data-stu-id="1f420-172">Install the [aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

```console
npm i -D aspnet-webpack
```

### <a name="webpack-dev-middleware-configuration"></a><span data-ttu-id="1f420-173">Конфигурации по промежуточного слоя Webpack разработки</span><span class="sxs-lookup"><span data-stu-id="1f420-173">Webpack Dev Middleware configuration</span></span>

<span data-ttu-id="1f420-174">По промежуточного слоя разработки Webpack регистрируется в конвейер запросов HTTP с помощью следующего кода в *Startup.cs* файла `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="1f420-174">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_WebpackMiddlewareRegistration&highlight=4)]

<span data-ttu-id="1f420-175">`UseWebpackDevMiddleware` Метод расширения должен быть вызван перед [регистрации размещения статического файла](xref:fundamentals/static-files) через `UseStaticFiles` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="1f420-175">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="1f420-176">По соображениям безопасности зарегистрируйте по промежуточного слоя, только в том случае, когда приложение запускается в режиме разработки.</span><span class="sxs-lookup"><span data-stu-id="1f420-176">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="1f420-177">*Webpack.config.js* файла `output.publicPath` свойство сообщает по промежуточного слоя, чтобы просмотреть `dist` изменений в папке:</span><span class="sxs-lookup"><span data-stu-id="1f420-177">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

## <a name="hot-module-replacement"></a><span data-ttu-id="1f420-178">Модуль горячей замены</span><span class="sxs-lookup"><span data-stu-id="1f420-178">Hot Module Replacement</span></span>

<span data-ttu-id="1f420-179">Можно рассматривать его Webpack [горячей замены модуля](https://webpack.js.org/concepts/hot-module-replacement/) компонента (HMR), как развитием [по промежуточного слоя разработки Webpack](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="1f420-179">Think of Webpack's [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="1f420-180">HMR представляет те же преимущества, но дополнительно упрощает рабочий процесс разработки, автоматически обновляя содержимое страницы после компиляции изменений.</span><span class="sxs-lookup"><span data-stu-id="1f420-180">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="1f420-181">Не следует путать с обновить браузер, который может повлиять на текущее состояние в памяти и сеанс отладки из SPA.</span><span class="sxs-lookup"><span data-stu-id="1f420-181">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="1f420-182">Нет активную связь между службой по промежуточного слоя разработки Webpack и браузера, это означает, что изменения будут передаваться в браузер.</span><span class="sxs-lookup"><span data-stu-id="1f420-182">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="hot-module-replacement-prerequisites"></a><span data-ttu-id="1f420-183">"Горячий" необходимые условия для замены модуля</span><span class="sxs-lookup"><span data-stu-id="1f420-183">Hot Module Replacement prerequisites</span></span>

<span data-ttu-id="1f420-184">Установка [webpack-"Горячий"-по промежуточного слоя](https://www.npmjs.com/package/webpack-hot-middleware) пакета npm:</span><span class="sxs-lookup"><span data-stu-id="1f420-184">Install the [webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

```console
npm i -D webpack-hot-middleware
```

### <a name="hot-module-replacement-configuration"></a><span data-ttu-id="1f420-185">Горячей замены модуля конфигурации</span><span class="sxs-lookup"><span data-stu-id="1f420-185">Hot Module Replacement configuration</span></span>

<span data-ttu-id="1f420-186">Компонент HMR должны быть зарегистрированы в конвейер запросов HTTP MVC в `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="1f420-186">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="1f420-187">Как было [по промежуточного слоя разработки Webpack](#webpack-dev-middleware), `UseWebpackDevMiddleware` метод расширения должен быть вызван перед `UseStaticFiles` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="1f420-187">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="1f420-188">По соображениям безопасности зарегистрируйте по промежуточного слоя, только в том случае, когда приложение запускается в режиме разработки.</span><span class="sxs-lookup"><span data-stu-id="1f420-188">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="1f420-189">*Webpack.config.js* необходимо определить файл `plugins` массив, даже если оставить эти поля пустыми:</span><span class="sxs-lookup"><span data-stu-id="1f420-189">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="1f420-190">После загрузки приложения в браузере, вкладку средства разработчика консоли предоставляет подтверждение HMR активации:</span><span class="sxs-lookup"><span data-stu-id="1f420-190">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Сообщение о подключении горячей замены модуля](spa-services/_static/hmr_connected.png)

## <a name="routing-helpers"></a><span data-ttu-id="1f420-192">Вспомогательные функции маршрутизации</span><span class="sxs-lookup"><span data-stu-id="1f420-192">Routing helpers</span></span>

<span data-ttu-id="1f420-193">В большинстве на базе ASP.NET Core одностраничные приложения маршрутизацию на стороне клиента часто желательно Помимо маршрутизации на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="1f420-193">In most ASP.NET Core-based SPAs, client-side routing is often desired in addition to server-side routing.</span></span> <span data-ttu-id="1f420-194">Не мешая систем маршрутизации SPA и MVC могут работать независимо.</span><span class="sxs-lookup"><span data-stu-id="1f420-194">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="1f420-195">, Однако проблемы вариантов дают один edge: определение ответы HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="1f420-195">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="1f420-196">Рассмотрим сценарий, в котором без расширений маршрут `/some/page` используется.</span><span class="sxs-lookup"><span data-stu-id="1f420-196">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="1f420-197">Предположим, запрос не фильтрует маршрут на стороне сервера, но его шаблон соответствует маршрут со стороны клиента.</span><span class="sxs-lookup"><span data-stu-id="1f420-197">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="1f420-198">Теперь рассмотрим входящий запрос `/images/user-512.png`, который обычно ожидает найти файл изображения на сервере.</span><span class="sxs-lookup"><span data-stu-id="1f420-198">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="1f420-199">Если этот путь запрошенного ресурса не соответствует любой маршрут на стороне сервера или статических файлов, маловероятно, что клиентское приложение будет обрабатывать его&mdash;требуется обычно возвращает код состояния HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="1f420-199">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it&mdash;generally returning a 404 HTTP status code is desired.</span></span>

### <a name="routing-helpers-prerequisites"></a><span data-ttu-id="1f420-200">Маршрутизации необходимые вспомогательные функции</span><span class="sxs-lookup"><span data-stu-id="1f420-200">Routing helpers prerequisites</span></span>

<span data-ttu-id="1f420-201">Установите пакет npm маршрутизации на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="1f420-201">Install the client-side routing npm package.</span></span> <span data-ttu-id="1f420-202">Используя Angular в качестве примера:</span><span class="sxs-lookup"><span data-stu-id="1f420-202">Using Angular as an example:</span></span>

```console
npm i -S @angular/router
```

### <a name="routing-helpers-configuration"></a><span data-ttu-id="1f420-203">Конфигурация маршрутизации вспомогательные функции</span><span class="sxs-lookup"><span data-stu-id="1f420-203">Routing helpers configuration</span></span>

<span data-ttu-id="1f420-204">Метод расширения с именем `MapSpaFallbackRoute` используется в `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="1f420-204">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_MvcRoutingTable&highlight=7-9)]

<span data-ttu-id="1f420-205">Маршруты вычисляются в порядке, в котором они настроены.</span><span class="sxs-lookup"><span data-stu-id="1f420-205">Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="1f420-206">Следовательно `default` маршрута в приведенном выше примере кода используется сначала для сопоставления шаблонов.</span><span class="sxs-lookup"><span data-stu-id="1f420-206">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="1f420-207">Создание нового проекта</span><span class="sxs-lookup"><span data-stu-id="1f420-207">Create a new project</span></span>

<span data-ttu-id="1f420-208">JavaScript службы предоставляют шаблонов предварительно настроенных приложений.</span><span class="sxs-lookup"><span data-stu-id="1f420-208">JavaScript Services provide pre-configured application templates.</span></span> <span data-ttu-id="1f420-209">SpaServices используется в этих шаблонах в сочетании с различных платформ и библиотек, например Angular, React и Redux.</span><span class="sxs-lookup"><span data-stu-id="1f420-209">SpaServices is used in these templates in conjunction with different frameworks and libraries such as Angular, React, and Redux.</span></span>

<span data-ttu-id="1f420-210">Эти шаблоны можно установить с помощью интерфейса командной строки .NET Core, выполнив следующую команду:</span><span class="sxs-lookup"><span data-stu-id="1f420-210">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="1f420-211">Отображается список доступных шаблонов одностраничных ПРИЛОЖЕНИЙ:</span><span class="sxs-lookup"><span data-stu-id="1f420-211">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="1f420-212">Шаблоны</span><span class="sxs-lookup"><span data-stu-id="1f420-212">Templates</span></span>                                 | <span data-ttu-id="1f420-213">Краткое имя</span><span class="sxs-lookup"><span data-stu-id="1f420-213">Short Name</span></span> | <span data-ttu-id="1f420-214">Язык</span><span class="sxs-lookup"><span data-stu-id="1f420-214">Language</span></span> | <span data-ttu-id="1f420-215">Теги</span><span class="sxs-lookup"><span data-stu-id="1f420-215">Tags</span></span>        |
| ------------------------------------------| :--------: | :------: | :---------: |
| <span data-ttu-id="1f420-216">MVC ASP.NET Core с Angular</span><span class="sxs-lookup"><span data-stu-id="1f420-216">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="1f420-217">angular</span><span class="sxs-lookup"><span data-stu-id="1f420-217">angular</span></span>    | <span data-ttu-id="1f420-218">[C#]</span><span class="sxs-lookup"><span data-stu-id="1f420-218">[C#]</span></span>     | <span data-ttu-id="1f420-219">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="1f420-219">Web/MVC/SPA</span></span> |
| <span data-ttu-id="1f420-220">MVC ASP.NET Core с React.js</span><span class="sxs-lookup"><span data-stu-id="1f420-220">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="1f420-221">react</span><span class="sxs-lookup"><span data-stu-id="1f420-221">react</span></span>      | <span data-ttu-id="1f420-222">[C#]</span><span class="sxs-lookup"><span data-stu-id="1f420-222">[C#]</span></span>     | <span data-ttu-id="1f420-223">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="1f420-223">Web/MVC/SPA</span></span> |
| <span data-ttu-id="1f420-224">MVC ASP.NET Core с React.js и Redux</span><span class="sxs-lookup"><span data-stu-id="1f420-224">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="1f420-225">reactredux</span><span class="sxs-lookup"><span data-stu-id="1f420-225">reactredux</span></span> | <span data-ttu-id="1f420-226">[C#]</span><span class="sxs-lookup"><span data-stu-id="1f420-226">[C#]</span></span>     | <span data-ttu-id="1f420-227">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="1f420-227">Web/MVC/SPA</span></span> |

<span data-ttu-id="1f420-228">Чтобы создать новый проект с помощью одного из шаблонов одностраничных ПРИЛОЖЕНИЙ, включают **короткое имя** шаблона в [команды dotnet new](/dotnet/core/tools/dotnet-new) команды.</span><span class="sxs-lookup"><span data-stu-id="1f420-228">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="1f420-229">Следующая команда создает Angular приложения с ASP.NET Core MVC, который настроен на стороне сервера:</span><span class="sxs-lookup"><span data-stu-id="1f420-229">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="1f420-230">Включить режим конфигурации среды выполнения</span><span class="sxs-lookup"><span data-stu-id="1f420-230">Set the runtime configuration mode</span></span>

<span data-ttu-id="1f420-231">Существует два режима конфигурации основной среды выполнения:</span><span class="sxs-lookup"><span data-stu-id="1f420-231">Two primary runtime configuration modes exist:</span></span>

* <span data-ttu-id="1f420-232">**Разработка**:</span><span class="sxs-lookup"><span data-stu-id="1f420-232">**Development**:</span></span>
  * <span data-ttu-id="1f420-233">Включает в себя исходных сопоставлений в целях отладки.</span><span class="sxs-lookup"><span data-stu-id="1f420-233">Includes source maps to ease debugging.</span></span>
  * <span data-ttu-id="1f420-234">Не предназначен для оптимизации клиентского кода для повышения производительности.</span><span class="sxs-lookup"><span data-stu-id="1f420-234">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="1f420-235">**Производство**:</span><span class="sxs-lookup"><span data-stu-id="1f420-235">**Production**:</span></span>
  * <span data-ttu-id="1f420-236">Исключает исходных сопоставлений.</span><span class="sxs-lookup"><span data-stu-id="1f420-236">Excludes source maps.</span></span>
  * <span data-ttu-id="1f420-237">Оптимизирует клиентского кода с помощью объединения и минификации.</span><span class="sxs-lookup"><span data-stu-id="1f420-237">Optimizes the client-side code via bundling and minification.</span></span>

<span data-ttu-id="1f420-238">ASP.NET Core использует переменную среды с именем `ASPNETCORE_ENVIRONMENT` для хранения режим конфигурации.</span><span class="sxs-lookup"><span data-stu-id="1f420-238">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="1f420-239">Дополнительные сведения см. в разделе [среду](xref:fundamentals/environments#set-the-environment).</span><span class="sxs-lookup"><span data-stu-id="1f420-239">For more information, see [Set the environment](xref:fundamentals/environments#set-the-environment).</span></span>

### <a name="run-with-net-core-cli"></a><span data-ttu-id="1f420-240">Запустить с помощью .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1f420-240">Run with .NET Core CLI</span></span>

<span data-ttu-id="1f420-241">Восстановление NuGet требуется и пакеты npm, выполнив следующую команду в корневом каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="1f420-241">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="1f420-242">Построение и запуск приложения.</span><span class="sxs-lookup"><span data-stu-id="1f420-242">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="1f420-243">Запускает приложения на localhost в соответствии с [режим конфигурации среды выполнения](#set-the-runtime-configuration-mode).</span><span class="sxs-lookup"><span data-stu-id="1f420-243">The application starts on localhost according to the [runtime configuration mode](#set-the-runtime-configuration-mode).</span></span> <span data-ttu-id="1f420-244">Переход к `http://localhost:5000` в браузере отображает целевую страницу.</span><span class="sxs-lookup"><span data-stu-id="1f420-244">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="run-with-visual-studio-2017"></a><span data-ttu-id="1f420-245">Запустить с помощью Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="1f420-245">Run with Visual Studio 2017</span></span>

<span data-ttu-id="1f420-246">Откройте *.csproj* файла, созданного [команды dotnet new](/dotnet/core/tools/dotnet-new) команды.</span><span class="sxs-lookup"><span data-stu-id="1f420-246">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="1f420-247">Необходимые пакеты NuGet и npm, автоматически восстанавливаются при открытии проекта.</span><span class="sxs-lookup"><span data-stu-id="1f420-247">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="1f420-248">Этот процесс восстановления может занять несколько минут, и приложение будет готово для запуска после ее завершения.</span><span class="sxs-lookup"><span data-stu-id="1f420-248">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="1f420-249">Щелкните зеленую кнопку запуска или нажмите клавишу `Ctrl + F5`, и в браузере откроется целевая страница приложения.</span><span class="sxs-lookup"><span data-stu-id="1f420-249">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="1f420-250">Приложение выполняется на локальном компьютере в соответствии с [режим конфигурации среды выполнения](#set-the-runtime-configuration-mode).</span><span class="sxs-lookup"><span data-stu-id="1f420-250">The application runs on localhost according to the [runtime configuration mode](#set-the-runtime-configuration-mode).</span></span>

## <a name="test-the-app"></a><span data-ttu-id="1f420-251">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="1f420-251">Test the app</span></span>

<span data-ttu-id="1f420-252">Шаблоны SpaServices предварительно настроены для запуска тестов на стороне клиента, с помощью [Karma](https://karma-runner.github.io/1.0/index.html) и [Jasmine](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="1f420-252">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="1f420-253">Jasmine — популярный платформа модульного тестирования для JavaScript, а Karma — тестов для этих тестов.</span><span class="sxs-lookup"><span data-stu-id="1f420-253">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="1f420-254">Karma настроен для работы с [по промежуточного слоя разработки Webpack](#webpack-dev-middleware) таким образом, чтобы разработчику не нужно будет остановить и запустить тест, каждый раз при внесении изменений.</span><span class="sxs-lookup"><span data-stu-id="1f420-254">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="1f420-255">Будь то код, выполняемый тестовый случай или сам тестовый случай, тест выполняется автоматически.</span><span class="sxs-lookup"><span data-stu-id="1f420-255">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="1f420-256">С помощью приложения Angular, например, два Жасмин тестовых случаев уже предоставляются для `CounterComponent` в *counter.component.spec.ts* файла:</span><span class="sxs-lookup"><span data-stu-id="1f420-256">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="1f420-257">Откройте командную строку в *ClientApp* каталога.</span><span class="sxs-lookup"><span data-stu-id="1f420-257">Open the command prompt in the *ClientApp* directory.</span></span> <span data-ttu-id="1f420-258">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="1f420-258">Run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="1f420-259">Сценарий запускает средство запуска тестов Karma, который считывает параметры, определенные в *karma.conf.js* файла.</span><span class="sxs-lookup"><span data-stu-id="1f420-259">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="1f420-260">Вместе с другими параметрами *karma.conf.js* определяет файлы теста для выполнения с помощью его `files` массива:</span><span class="sxs-lookup"><span data-stu-id="1f420-260">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

## <a name="publish-the-app"></a><span data-ttu-id="1f420-261">Публикация приложения</span><span class="sxs-lookup"><span data-stu-id="1f420-261">Publish the app</span></span>

<span data-ttu-id="1f420-262">Объединение созданных ресурсов на стороне клиента и опубликованных артефакты ASP.NET Core в готовые к развертыванию пакет может быть громоздким.</span><span class="sxs-lookup"><span data-stu-id="1f420-262">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="1f420-263">К счастью, SpaServices организует этот процесс всей публикации с пользовательский целевой объект MSBuild с именем `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="1f420-263">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="1f420-264">Целевой объект MSBuild должен выполнить следующие:</span><span class="sxs-lookup"><span data-stu-id="1f420-264">The MSBuild target has the following responsibilities:</span></span>

1. <span data-ttu-id="1f420-265">Восстановление пакетов npm.</span><span class="sxs-lookup"><span data-stu-id="1f420-265">Restore the npm packages.</span></span>
1. <span data-ttu-id="1f420-266">Создание средств сторонними разработчиками, клиентские сборки производственного уровня.</span><span class="sxs-lookup"><span data-stu-id="1f420-266">Create a production-grade build of the third-party, client-side assets.</span></span>
1. <span data-ttu-id="1f420-267">Создание пользовательских средств клиентские сборки производственного уровня.</span><span class="sxs-lookup"><span data-stu-id="1f420-267">Create a production-grade build of the custom client-side assets.</span></span>
1. <span data-ttu-id="1f420-268">Скопируйте созданный Webpack ресурсы в папку публикации.</span><span class="sxs-lookup"><span data-stu-id="1f420-268">Copy the Webpack-generated assets to the publish folder.</span></span>

<span data-ttu-id="1f420-269">Целевой объект MSBuild вызывается при запуске:</span><span class="sxs-lookup"><span data-stu-id="1f420-269">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="1f420-270">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="1f420-270">Additional resources</span></span>

* [<span data-ttu-id="1f420-271">Документация по angular</span><span class="sxs-lookup"><span data-stu-id="1f420-271">Angular Docs</span></span>](https://angular.io/docs)
