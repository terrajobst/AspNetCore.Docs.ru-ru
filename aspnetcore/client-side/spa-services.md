---
title: Использование служб JavaScript для создания одностраничных приложений в ASP.NET Core
author: scottaddie
description: Узнайте о преимуществах использования служб JavaScript для создания одностраничного приложения (SPA), поддерживаемого ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 09/06/2019
uid: client-side/spa-services
ms.openlocfilehash: 7aff46f739239246191763e0590046b2d9995922
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080512"
---
# <a name="use-javascript-services-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="11c7b-103">Использование служб JavaScript для создания одностраничных приложений в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="11c7b-103">Use JavaScript Services to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="11c7b-104">По [Scott Addie](https://github.com/scottaddie) и [Fiyaz Hasan](https://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="11c7b-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](https://fiyazhasan.me/)</span></span>

<span data-ttu-id="11c7b-105">Одностраничное приложение (SPA) — это популярный тип веб-приложения из-за его присущие многофункциональном пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="11c7b-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="11c7b-106">Интеграция клиентских платформ и библиотек SPA, таких как [угловой](https://angular.io/) или [реагирование](https://facebook.github.io/react/), с серверными платформами, такими как ASP.NET Core, может быть трудной задачей.</span><span class="sxs-lookup"><span data-stu-id="11c7b-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks such as ASP.NET Core can be difficult.</span></span> <span data-ttu-id="11c7b-107">Службы JavaScript были разработаны для уменьшения трения в процессе интеграции.</span><span class="sxs-lookup"><span data-stu-id="11c7b-107">JavaScript Services was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="11c7b-108">Он позволяет работать между различными клиентскими и стеков технологий сервера.</span><span class="sxs-lookup"><span data-stu-id="11c7b-108">It enables seamless operation between the different client and server technology stacks.</span></span>

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="11c7b-109">Функции, описанные в этой статье, устарели по ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="11c7b-109">The features described in this article are obsolete as of ASP.NET Core 3.0.</span></span> <span data-ttu-id="11c7b-110">В пакете NuGet [Microsoft. AspNetCore. спасервицес. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices.Extensions) доступен более простой механизм интеграции с алгоритмом Spa.</span><span class="sxs-lookup"><span data-stu-id="11c7b-110">A simpler SPA frameworks integration mechanism is available in the [Microsoft.AspNetCore.SpaServices.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices.Extensions) NuGet package.</span></span> <span data-ttu-id="11c7b-111">Дополнительные сведения см. в разделе [[объявление] Обсолетинг Microsoft. AspNetCore. спасервицес и Microsoft. AspNetCore. нодесервицес](https://github.com/aspnet/AspNetCore/issues/12890).</span><span class="sxs-lookup"><span data-stu-id="11c7b-111">For more information, see [[Announcement] Obsoleting Microsoft.AspNetCore.SpaServices and Microsoft.AspNetCore.NodeServices](https://github.com/aspnet/AspNetCore/issues/12890).</span></span>

::: moniker-end

## <a name="what-is-javascript-services"></a><span data-ttu-id="11c7b-112">Что такое службы JavaScript</span><span class="sxs-lookup"><span data-stu-id="11c7b-112">What is JavaScript Services</span></span>

<span data-ttu-id="11c7b-113">Службы JavaScript — это набор технологий на стороне клиента для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="11c7b-113">JavaScript Services is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="11c7b-114">Наша цель — для размещения ASP.NET Core разработчиков предпочтительной платформой на стороне сервера для построения одностраничных приложений.</span><span class="sxs-lookup"><span data-stu-id="11c7b-114">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="11c7b-115">Службы JavaScript состоят из двух разных пакетов NuGet:</span><span class="sxs-lookup"><span data-stu-id="11c7b-115">JavaScript Services consists of two distinct NuGet packages:</span></span>

* <span data-ttu-id="11c7b-116">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="11c7b-116">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="11c7b-117">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="11c7b-117">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>

<span data-ttu-id="11c7b-118">Эти пакеты полезны в следующих сценариях:</span><span class="sxs-lookup"><span data-stu-id="11c7b-118">These packages are useful in the following scenarios:</span></span>

* <span data-ttu-id="11c7b-119">Выполните на сервере JavaScript</span><span class="sxs-lookup"><span data-stu-id="11c7b-119">Run JavaScript on the server</span></span>
* <span data-ttu-id="11c7b-120">Использование библиотеки или платформы одностраничных ПРИЛОЖЕНИЙ</span><span class="sxs-lookup"><span data-stu-id="11c7b-120">Use a SPA framework or library</span></span>
* <span data-ttu-id="11c7b-121">Создание активов на стороне клиента с помощью Webpack</span><span class="sxs-lookup"><span data-stu-id="11c7b-121">Build client-side assets with Webpack</span></span>

<span data-ttu-id="11c7b-122">В этой статье внимание следует уделять помещается в с помощью пакета SpaServices.</span><span class="sxs-lookup"><span data-stu-id="11c7b-122">Much of the focus in this article is placed on using the SpaServices package.</span></span>

## <a name="what-is-spaservices"></a><span data-ttu-id="11c7b-123">Что такое SpaServices</span><span class="sxs-lookup"><span data-stu-id="11c7b-123">What is SpaServices</span></span>

<span data-ttu-id="11c7b-124">SpaServices был создан для размещения ASP.NET Core разработчиков предпочтительной платформой на стороне сервера для построения одностраничных приложений.</span><span class="sxs-lookup"><span data-stu-id="11c7b-124">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="11c7b-125">Спасервицес не требуется для разработки одностраничные приложения с ASP.NET Core и не блокирует разработчиков в определенной клиентской платформе.</span><span class="sxs-lookup"><span data-stu-id="11c7b-125">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock developers into a particular client framework.</span></span>

<span data-ttu-id="11c7b-126">SpaServices предоставляет полезные инфраструктуры, например:</span><span class="sxs-lookup"><span data-stu-id="11c7b-126">SpaServices provides useful infrastructure such as:</span></span>

* [<span data-ttu-id="11c7b-127">Предварительной визуализации на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="11c7b-127">Server-side prerendering</span></span>](#server-side-prerendering)
* [<span data-ttu-id="11c7b-128">По промежуточного слоя Webpack разработки</span><span class="sxs-lookup"><span data-stu-id="11c7b-128">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="11c7b-129">Модуль горячей замены</span><span class="sxs-lookup"><span data-stu-id="11c7b-129">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="11c7b-130">Вспомогательные функции маршрутизации</span><span class="sxs-lookup"><span data-stu-id="11c7b-130">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="11c7b-131">В совокупности эти компоненты инфраструктуры улучшить рабочий процесс разработки и возможности среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="11c7b-131">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="11c7b-132">Компоненты, может быть использована по отдельности.</span><span class="sxs-lookup"><span data-stu-id="11c7b-132">The components can be adopted individually.</span></span>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="11c7b-133">Предварительные требования для использования SpaServices</span><span class="sxs-lookup"><span data-stu-id="11c7b-133">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="11c7b-134">Для работы с SpaServices, установите следующее:</span><span class="sxs-lookup"><span data-stu-id="11c7b-134">To work with SpaServices, install the following:</span></span>

* <span data-ttu-id="11c7b-135">[Node.js](https://nodejs.org/) (версия 6 или более поздней версии) с помощью npm</span><span class="sxs-lookup"><span data-stu-id="11c7b-135">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>

  * <span data-ttu-id="11c7b-136">Чтобы проверить эти компоненты устанавливаются и может быть найден, выполните следующую команду из командной строки:</span><span class="sxs-lookup"><span data-stu-id="11c7b-136">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

  * <span data-ttu-id="11c7b-137">Если развертывание выполняется на веб-сайте Azure, никаких действий не&mdash;требуется, так как Node. js не установлен и не будет доступен в серверных средах.</span><span class="sxs-lookup"><span data-stu-id="11c7b-137">If deploying to an Azure web site, no action is required&mdash;Node.js is installed and available in the server environments.</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * <span data-ttu-id="11c7b-138">В Windows с помощью Visual Studio 2017 пакет SDK устанавливается путем выбора рабочей нагрузки **кросс-платформенная разработка .NET Core** .</span><span class="sxs-lookup"><span data-stu-id="11c7b-138">On Windows using Visual Studio 2017, the SDK is installed by selecting the **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="11c7b-139">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) пакета NuGet</span><span class="sxs-lookup"><span data-stu-id="11c7b-139">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

## <a name="server-side-prerendering"></a><span data-ttu-id="11c7b-140">Предварительной визуализации на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="11c7b-140">Server-side prerendering</span></span>

<span data-ttu-id="11c7b-141">Универсальное приложение (также известный как isomorphic) — это приложение JavaScript, может запускать как на сервере и на клиенте.</span><span class="sxs-lookup"><span data-stu-id="11c7b-141">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="11c7b-142">Angular, React и других популярных платформ предоставляют универсальной платформы для этого стиля разработки приложения.</span><span class="sxs-lookup"><span data-stu-id="11c7b-142">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="11c7b-143">Идея состоит в том, чтобы сначала отрисовки компоненты платформы на сервере с помощью Node.js, а затем делегирует дальнейшего выполнения клиенту.</span><span class="sxs-lookup"><span data-stu-id="11c7b-143">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="11c7b-144">ASP.NET Core [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) предоставляемые SpaServices упрощают реализацию предварительной визуализации на сервере путем вызова функции JavaScript на сервере.</span><span class="sxs-lookup"><span data-stu-id="11c7b-144">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="server-side-prerendering-prerequisites"></a><span data-ttu-id="11c7b-145">Предварительная подготовка необходимых компонентов на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="11c7b-145">Server-side prerendering prerequisites</span></span>

<span data-ttu-id="11c7b-146">Установите пакет NPM для [визуализации ASPNET](https://www.npmjs.com/package/aspnet-prerendering) :</span><span class="sxs-lookup"><span data-stu-id="11c7b-146">Install the [aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

```console
npm i -S aspnet-prerendering
```

### <a name="server-side-prerendering-configuration"></a><span data-ttu-id="11c7b-147">Конфигурация предварительной подготовки к просмотру на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="11c7b-147">Server-side prerendering configuration</span></span>

<span data-ttu-id="11c7b-148">Вспомогательные функции тегов выполняются с помощью функции регистрации пространства имен в проекте *_ViewImports.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="11c7b-148">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="11c7b-149">Эти вспомогательные функции тегов абстрагироваться от конкретных особенностей взаимодействовать непосредственно с низкоуровневых API за счет использования HTML-синтаксис типа в представлении Razor:</span><span class="sxs-lookup"><span data-stu-id="11c7b-149">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="asp-prerender-module-tag-helper"></a><span data-ttu-id="11c7b-150">Вспомогательная функция тега ASP-PreRender-Module</span><span class="sxs-lookup"><span data-stu-id="11c7b-150">asp-prerender-module Tag Helper</span></span>

<span data-ttu-id="11c7b-151">`asp-prerender-module` Вспомогательной функции тега, используемый в предыдущем примере код выполняет *ClientApp/dist/main-server.js* на сервере с помощью Node.js.</span><span class="sxs-lookup"><span data-stu-id="11c7b-151">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="11c7b-152">Для ясности *main server.js* файл — это артефакт задачи транспилирования TypeScript для JavaScript в [Webpack](https://webpack.github.io/) процесс сборки.</span><span class="sxs-lookup"><span data-stu-id="11c7b-152">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](https://webpack.github.io/) build process.</span></span> <span data-ttu-id="11c7b-153">Webpack определяется псевдоним точки входа `main-server`; и начинается обход графа зависимостей для данного псевдонима *ClientApp/boot-server.ts* файла:</span><span class="sxs-lookup"><span data-stu-id="11c7b-153">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="11c7b-154">В следующем примере Angular *ClientApp/boot-server.ts* использует файл `createServerRenderer` функции и `RenderResult` тип `aspnet-prerendering` пакета npm для настройки подготовки сервера отчетов с помощью Node.js.</span><span class="sxs-lookup"><span data-stu-id="11c7b-154">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="11c7b-155">HTML-разметка, предназначенные для отрисовки на стороне сервера, переданного в вызов функции разрешения, который обернут в JavaScript со строгой типизацией `Promise` объекта.</span><span class="sxs-lookup"><span data-stu-id="11c7b-155">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="11c7b-156">`Promise` Точность объекта —, он асинхронно передает разметку HTML для страницы для внедрения в DOM замещающего элемента.</span><span class="sxs-lookup"><span data-stu-id="11c7b-156">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="asp-prerender-data-tag-helper"></a><span data-ttu-id="11c7b-157">Вспомогательная функция тега ASP-PreRender-Data</span><span class="sxs-lookup"><span data-stu-id="11c7b-157">asp-prerender-data Tag Helper</span></span>

<span data-ttu-id="11c7b-158">При связывании с `asp-prerender-module` вспомогательной функции тега, `asp-prerender-data` вспомогательная функция тега может использоваться для передачи контекстно-зависимые сведения из представления Razor для JavaScript на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="11c7b-158">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="11c7b-159">Например, следующая разметка передает данные на `main-server` модуля:</span><span class="sxs-lookup"><span data-stu-id="11c7b-159">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="11c7b-160">Полученных `UserName` аргумент сериализуется с помощью встроенного сериализатора JSON и хранятся в `params.data` объекта.</span><span class="sxs-lookup"><span data-stu-id="11c7b-160">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="11c7b-161">В следующем примере Angular данных используется для создания индивидуальное приветствие в `h1` элемент:</span><span class="sxs-lookup"><span data-stu-id="11c7b-161">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="11c7b-162">Имена свойств, переданные в вспомогательные функции тегов, представлены с помощью нотации **PascalCase** .</span><span class="sxs-lookup"><span data-stu-id="11c7b-162">Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="11c7b-163">Сравните это JavaScript, где представлены те же имена свойств **camelCase**.</span><span class="sxs-lookup"><span data-stu-id="11c7b-163">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="11c7b-164">Конфигурация по умолчанию для сериализации JSON несет ответственность за это различие.</span><span class="sxs-lookup"><span data-stu-id="11c7b-164">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="11c7b-165">Для обзора в предыдущем примере кода, данные могут передаваться с сервера в представление с hydrating `globals` указано свойство для `resolve` функции:</span><span class="sxs-lookup"><span data-stu-id="11c7b-165">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="11c7b-166">`postList` Массива, определенные внутри `globals` объект присоединяется к браузера глобального `window` объекта.</span><span class="sxs-lookup"><span data-stu-id="11c7b-166">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="11c7b-167">Этой переменной подъем глобальную область устраняет двойной работы, особенно в том случае, поскольку это имеет отношение к загрузке те же данные один раз на сервере и снова на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="11c7b-167">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![глобальная переменная postList, присоединенный к объекту окна](spa-services/_static/global_variable.png)

## <a name="webpack-dev-middleware"></a><span data-ttu-id="11c7b-169">По промежуточного слоя Webpack разработки</span><span class="sxs-lookup"><span data-stu-id="11c7b-169">Webpack Dev Middleware</span></span>

<span data-ttu-id="11c7b-170">[По промежуточного слоя разработки Webpack](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) предлагает рабочий процесс разработки, при котором Webpack создает ресурсы по требованию.</span><span class="sxs-lookup"><span data-stu-id="11c7b-170">[Webpack Dev Middleware](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="11c7b-171">По промежуточного слоя автоматически компилирует и выполняет ресурсы на стороне клиента при перезагрузке страницы в браузере.</span><span class="sxs-lookup"><span data-stu-id="11c7b-171">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="11c7b-172">Альтернативный подход заключается в том, необходимо вручную вызвать Webpack через скрипт сборки проекта npm при изменении зависимость стороннего или пользовательского кода.</span><span class="sxs-lookup"><span data-stu-id="11c7b-172">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="11c7b-173">Сценарий построения npm *package.json* файл отображается в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="11c7b-173">An npm build script in the *package.json* file is shown in the following example:</span></span>

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="webpack-dev-middleware-prerequisites"></a><span data-ttu-id="11c7b-174">Предварительные требования для по промежуточного слоя разработки пакета</span><span class="sxs-lookup"><span data-stu-id="11c7b-174">Webpack Dev Middleware prerequisites</span></span>

<span data-ttu-id="11c7b-175">Установите пакет [ASPNET-](https://www.npmjs.com/package/aspnet-webpack) NPM:</span><span class="sxs-lookup"><span data-stu-id="11c7b-175">Install the [aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

```console
npm i -D aspnet-webpack
```

### <a name="webpack-dev-middleware-configuration"></a><span data-ttu-id="11c7b-176">Конфигурация по промежуточного слоя разработки пакета</span><span class="sxs-lookup"><span data-stu-id="11c7b-176">Webpack Dev Middleware configuration</span></span>

<span data-ttu-id="11c7b-177">По промежуточного слоя разработки Webpack регистрируется в конвейер запросов HTTP с помощью следующего кода в *Startup.cs* файла `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="11c7b-177">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_WebpackMiddlewareRegistration&highlight=4)]

<span data-ttu-id="11c7b-178">`UseWebpackDevMiddleware` Метод расширения должен быть вызван перед [регистрации размещения статического файла](xref:fundamentals/static-files) через `UseStaticFiles` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="11c7b-178">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="11c7b-179">По соображениям безопасности зарегистрируйте по промежуточного слоя, только в том случае, когда приложение запускается в режиме разработки.</span><span class="sxs-lookup"><span data-stu-id="11c7b-179">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="11c7b-180">*Webpack.config.js* файла `output.publicPath` свойство сообщает по промежуточного слоя, чтобы просмотреть `dist` изменений в папке:</span><span class="sxs-lookup"><span data-stu-id="11c7b-180">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

## <a name="hot-module-replacement"></a><span data-ttu-id="11c7b-181">Модуль горячей замены</span><span class="sxs-lookup"><span data-stu-id="11c7b-181">Hot Module Replacement</span></span>

<span data-ttu-id="11c7b-182">Можно рассматривать его Webpack [горячей замены модуля](https://webpack.js.org/concepts/hot-module-replacement/) компонента (HMR), как развитием [по промежуточного слоя разработки Webpack](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="11c7b-182">Think of Webpack's [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="11c7b-183">HMR представляет те же преимущества, но дополнительно упрощает рабочий процесс разработки, автоматически обновляя содержимое страницы после компиляции изменений.</span><span class="sxs-lookup"><span data-stu-id="11c7b-183">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="11c7b-184">Не следует путать с обновить браузер, который может повлиять на текущее состояние в памяти и сеанс отладки из SPA.</span><span class="sxs-lookup"><span data-stu-id="11c7b-184">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="11c7b-185">Нет активную связь между службой по промежуточного слоя разработки Webpack и браузера, это означает, что изменения будут передаваться в браузер.</span><span class="sxs-lookup"><span data-stu-id="11c7b-185">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="hot-module-replacement-prerequisites"></a><span data-ttu-id="11c7b-186">Предварительные требования для замены активных модулей</span><span class="sxs-lookup"><span data-stu-id="11c7b-186">Hot Module Replacement prerequisites</span></span>

<span data-ttu-id="11c7b-187">Установите пакет NPM для " [горячего по промежуточного слоя](https://www.npmjs.com/package/webpack-hot-middleware) ".</span><span class="sxs-lookup"><span data-stu-id="11c7b-187">Install the [webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

```console
npm i -D webpack-hot-middleware
```

### <a name="hot-module-replacement-configuration"></a><span data-ttu-id="11c7b-188">Горячая замена конфигурации модуля</span><span class="sxs-lookup"><span data-stu-id="11c7b-188">Hot Module Replacement configuration</span></span>

<span data-ttu-id="11c7b-189">Компонент HMR должны быть зарегистрированы в конвейер запросов HTTP MVC в `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="11c7b-189">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="11c7b-190">Как было [по промежуточного слоя разработки Webpack](#webpack-dev-middleware), `UseWebpackDevMiddleware` метод расширения должен быть вызван перед `UseStaticFiles` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="11c7b-190">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="11c7b-191">По соображениям безопасности зарегистрируйте по промежуточного слоя, только в том случае, когда приложение запускается в режиме разработки.</span><span class="sxs-lookup"><span data-stu-id="11c7b-191">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="11c7b-192">*Webpack.config.js* необходимо определить файл `plugins` массив, даже если оставить эти поля пустыми:</span><span class="sxs-lookup"><span data-stu-id="11c7b-192">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="11c7b-193">После загрузки приложения в браузере, вкладку средства разработчика консоли предоставляет подтверждение HMR активации:</span><span class="sxs-lookup"><span data-stu-id="11c7b-193">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Сообщение о подключении горячей замены модуля](spa-services/_static/hmr_connected.png)

## <a name="routing-helpers"></a><span data-ttu-id="11c7b-195">Вспомогательные функции маршрутизации</span><span class="sxs-lookup"><span data-stu-id="11c7b-195">Routing helpers</span></span>

<span data-ttu-id="11c7b-196">В большинстве ASP.NET Core случаев одностраничные приложения на стороне клиента часто требуется маршрутизация на клиентские службы в дополнение к маршрутизации на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="11c7b-196">In most ASP.NET Core-based SPAs, client-side routing is often desired in addition to server-side routing.</span></span> <span data-ttu-id="11c7b-197">Не мешая систем маршрутизации SPA и MVC могут работать независимо.</span><span class="sxs-lookup"><span data-stu-id="11c7b-197">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="11c7b-198">, Однако проблемы вариантов дают один edge: определение ответы HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="11c7b-198">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="11c7b-199">Рассмотрим сценарий, в котором без расширений маршрут `/some/page` используется.</span><span class="sxs-lookup"><span data-stu-id="11c7b-199">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="11c7b-200">Предположим, запрос не фильтрует маршрут на стороне сервера, но его шаблон соответствует маршрут со стороны клиента.</span><span class="sxs-lookup"><span data-stu-id="11c7b-200">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="11c7b-201">Теперь рассмотрим входящий запрос `/images/user-512.png`, который обычно ожидает найти файл изображения на сервере.</span><span class="sxs-lookup"><span data-stu-id="11c7b-201">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="11c7b-202">Если запрошенный путь к ресурсу не соответствует ни одному маршруту на стороне сервера или статическому файлу, маловероятно, что клиентское приложение будет его&mdash;обработку, обычно возвращая код состояния HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="11c7b-202">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it&mdash;generally returning a 404 HTTP status code is desired.</span></span>

### <a name="routing-helpers-prerequisites"></a><span data-ttu-id="11c7b-203">Предварительные требования для вспомогательных функций маршрутизации</span><span class="sxs-lookup"><span data-stu-id="11c7b-203">Routing helpers prerequisites</span></span>

<span data-ttu-id="11c7b-204">Установите пакет NPM маршрутизации на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="11c7b-204">Install the client-side routing npm package.</span></span> <span data-ttu-id="11c7b-205">Используя Angular в качестве примера:</span><span class="sxs-lookup"><span data-stu-id="11c7b-205">Using Angular as an example:</span></span>

```console
npm i -S @angular/router
```

### <a name="routing-helpers-configuration"></a><span data-ttu-id="11c7b-206">Настройка вспомогательных функций маршрутизации</span><span class="sxs-lookup"><span data-stu-id="11c7b-206">Routing helpers configuration</span></span>

<span data-ttu-id="11c7b-207">Метод расширения с именем `MapSpaFallbackRoute` используется в `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="11c7b-207">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_MvcRoutingTable&highlight=7-9)]

<span data-ttu-id="11c7b-208">Маршруты оцениваются в том порядке, в котором они настроены.</span><span class="sxs-lookup"><span data-stu-id="11c7b-208">Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="11c7b-209">Следовательно `default` маршрута в приведенном выше примере кода используется сначала для сопоставления шаблонов.</span><span class="sxs-lookup"><span data-stu-id="11c7b-209">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="11c7b-210">Создание нового проекта</span><span class="sxs-lookup"><span data-stu-id="11c7b-210">Create a new project</span></span>

<span data-ttu-id="11c7b-211">Службы JavaScript предоставляют предварительно настроенные шаблоны приложений.</span><span class="sxs-lookup"><span data-stu-id="11c7b-211">JavaScript Services provide pre-configured application templates.</span></span> <span data-ttu-id="11c7b-212">Спасервицес используется в этих шаблонах в сочетании с различными платформами и библиотеками, такими как угловые, реагирующие и Redux.</span><span class="sxs-lookup"><span data-stu-id="11c7b-212">SpaServices is used in these templates in conjunction with different frameworks and libraries such as Angular, React, and Redux.</span></span>

<span data-ttu-id="11c7b-213">Эти шаблоны можно установить с помощью интерфейса командной строки .NET Core, выполнив следующую команду:</span><span class="sxs-lookup"><span data-stu-id="11c7b-213">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```dotnetcli
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="11c7b-214">Отображается список доступных шаблонов одностраничных ПРИЛОЖЕНИЙ:</span><span class="sxs-lookup"><span data-stu-id="11c7b-214">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="11c7b-215">Шаблоны</span><span class="sxs-lookup"><span data-stu-id="11c7b-215">Templates</span></span>                                 | <span data-ttu-id="11c7b-216">Краткое имя</span><span class="sxs-lookup"><span data-stu-id="11c7b-216">Short Name</span></span> | <span data-ttu-id="11c7b-217">Язык</span><span class="sxs-lookup"><span data-stu-id="11c7b-217">Language</span></span> | <span data-ttu-id="11c7b-218">Теги</span><span class="sxs-lookup"><span data-stu-id="11c7b-218">Tags</span></span>        |
| ------------------------------------------| :--------: | :------: | :---------: |
| <span data-ttu-id="11c7b-219">MVC ASP.NET Core с Angular</span><span class="sxs-lookup"><span data-stu-id="11c7b-219">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="11c7b-220">angular</span><span class="sxs-lookup"><span data-stu-id="11c7b-220">angular</span></span>    | <span data-ttu-id="11c7b-221">[C#]</span><span class="sxs-lookup"><span data-stu-id="11c7b-221">[C#]</span></span>     | <span data-ttu-id="11c7b-222">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="11c7b-222">Web/MVC/SPA</span></span> |
| <span data-ttu-id="11c7b-223">MVC ASP.NET Core с React.js</span><span class="sxs-lookup"><span data-stu-id="11c7b-223">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="11c7b-224">react</span><span class="sxs-lookup"><span data-stu-id="11c7b-224">react</span></span>      | <span data-ttu-id="11c7b-225">[C#]</span><span class="sxs-lookup"><span data-stu-id="11c7b-225">[C#]</span></span>     | <span data-ttu-id="11c7b-226">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="11c7b-226">Web/MVC/SPA</span></span> |
| <span data-ttu-id="11c7b-227">MVC ASP.NET Core с React.js и Redux</span><span class="sxs-lookup"><span data-stu-id="11c7b-227">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="11c7b-228">reactredux</span><span class="sxs-lookup"><span data-stu-id="11c7b-228">reactredux</span></span> | <span data-ttu-id="11c7b-229">[C#]</span><span class="sxs-lookup"><span data-stu-id="11c7b-229">[C#]</span></span>     | <span data-ttu-id="11c7b-230">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="11c7b-230">Web/MVC/SPA</span></span> |

<span data-ttu-id="11c7b-231">Чтобы создать новый проект с помощью одного из шаблонов одностраничных ПРИЛОЖЕНИЙ, включают **короткое имя** шаблона в [команды dotnet new](/dotnet/core/tools/dotnet-new) команды.</span><span class="sxs-lookup"><span data-stu-id="11c7b-231">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="11c7b-232">Следующая команда создает Angular приложения с ASP.NET Core MVC, который настроен на стороне сервера:</span><span class="sxs-lookup"><span data-stu-id="11c7b-232">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```dotnetcli
dotnet new angular
```

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="11c7b-233">Включить режим конфигурации среды выполнения</span><span class="sxs-lookup"><span data-stu-id="11c7b-233">Set the runtime configuration mode</span></span>

<span data-ttu-id="11c7b-234">Существует два режима конфигурации основной среды выполнения:</span><span class="sxs-lookup"><span data-stu-id="11c7b-234">Two primary runtime configuration modes exist:</span></span>

* <span data-ttu-id="11c7b-235">**Разработка**:</span><span class="sxs-lookup"><span data-stu-id="11c7b-235">**Development**:</span></span>
  * <span data-ttu-id="11c7b-236">Включает в себя исходных сопоставлений в целях отладки.</span><span class="sxs-lookup"><span data-stu-id="11c7b-236">Includes source maps to ease debugging.</span></span>
  * <span data-ttu-id="11c7b-237">Не предназначен для оптимизации клиентского кода для повышения производительности.</span><span class="sxs-lookup"><span data-stu-id="11c7b-237">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="11c7b-238">**Производство**:</span><span class="sxs-lookup"><span data-stu-id="11c7b-238">**Production**:</span></span>
  * <span data-ttu-id="11c7b-239">Исключает исходных сопоставлений.</span><span class="sxs-lookup"><span data-stu-id="11c7b-239">Excludes source maps.</span></span>
  * <span data-ttu-id="11c7b-240">Оптимизирует код на стороне клиента с помощью объединения и минификации.</span><span class="sxs-lookup"><span data-stu-id="11c7b-240">Optimizes the client-side code via bundling and minification.</span></span>

<span data-ttu-id="11c7b-241">ASP.NET Core использует переменную среды с именем `ASPNETCORE_ENVIRONMENT` для хранения режим конфигурации.</span><span class="sxs-lookup"><span data-stu-id="11c7b-241">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="11c7b-242">Дополнительные сведения см. [в разделе Задание среды](xref:fundamentals/environments#set-the-environment).</span><span class="sxs-lookup"><span data-stu-id="11c7b-242">For more information, see [Set the environment](xref:fundamentals/environments#set-the-environment).</span></span>

### <a name="run-with-net-core-cli"></a><span data-ttu-id="11c7b-243">Запуск с .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="11c7b-243">Run with .NET Core CLI</span></span>

<span data-ttu-id="11c7b-244">Восстановление NuGet требуется и пакеты npm, выполнив следующую команду в корневом каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="11c7b-244">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```dotnetcli
dotnet restore && npm i
```

<span data-ttu-id="11c7b-245">Построение и запуск приложения.</span><span class="sxs-lookup"><span data-stu-id="11c7b-245">Build and run the application:</span></span>

```dotnetcli
dotnet run
```

<span data-ttu-id="11c7b-246">Запускает приложения на localhost в соответствии с [режим конфигурации среды выполнения](#set-the-runtime-configuration-mode).</span><span class="sxs-lookup"><span data-stu-id="11c7b-246">The application starts on localhost according to the [runtime configuration mode](#set-the-runtime-configuration-mode).</span></span> <span data-ttu-id="11c7b-247">Переход к `http://localhost:5000` в браузере отображает целевую страницу.</span><span class="sxs-lookup"><span data-stu-id="11c7b-247">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="run-with-visual-studio-2017"></a><span data-ttu-id="11c7b-248">Запуск с Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="11c7b-248">Run with Visual Studio 2017</span></span>

<span data-ttu-id="11c7b-249">Откройте *.csproj* файла, созданного [команды dotnet new](/dotnet/core/tools/dotnet-new) команды.</span><span class="sxs-lookup"><span data-stu-id="11c7b-249">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="11c7b-250">Необходимые пакеты NuGet и npm, автоматически восстанавливаются при открытии проекта.</span><span class="sxs-lookup"><span data-stu-id="11c7b-250">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="11c7b-251">Этот процесс восстановления может занять несколько минут, и приложение будет готово для запуска после ее завершения.</span><span class="sxs-lookup"><span data-stu-id="11c7b-251">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="11c7b-252">Щелкните зеленую кнопку запуска или нажмите клавишу `Ctrl + F5`, и в браузере откроется целевая страница приложения.</span><span class="sxs-lookup"><span data-stu-id="11c7b-252">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="11c7b-253">Приложение выполняется на локальном компьютере в соответствии с [режим конфигурации среды выполнения](#set-the-runtime-configuration-mode).</span><span class="sxs-lookup"><span data-stu-id="11c7b-253">The application runs on localhost according to the [runtime configuration mode](#set-the-runtime-configuration-mode).</span></span>

## <a name="test-the-app"></a><span data-ttu-id="11c7b-254">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="11c7b-254">Test the app</span></span>

<span data-ttu-id="11c7b-255">Шаблоны SpaServices предварительно настроены для запуска тестов на стороне клиента, с помощью [Karma](https://karma-runner.github.io/1.0/index.html) и [Jasmine](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="11c7b-255">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="11c7b-256">Jasmine — популярный платформа модульного тестирования для JavaScript, а Karma — тестов для этих тестов.</span><span class="sxs-lookup"><span data-stu-id="11c7b-256">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="11c7b-257">Karma настроен для работы с [по промежуточного слоя разработки Webpack](#webpack-dev-middleware) таким образом, чтобы разработчику не нужно будет остановить и запустить тест, каждый раз при внесении изменений.</span><span class="sxs-lookup"><span data-stu-id="11c7b-257">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="11c7b-258">Будь то код, выполняемый тестовый случай или сам тестовый случай, тест выполняется автоматически.</span><span class="sxs-lookup"><span data-stu-id="11c7b-258">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="11c7b-259">С помощью приложения Angular, например, два Жасмин тестовых случаев уже предоставляются для `CounterComponent` в *counter.component.spec.ts* файла:</span><span class="sxs-lookup"><span data-stu-id="11c7b-259">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="11c7b-260">Откройте командную строку в *ClientApp* каталога.</span><span class="sxs-lookup"><span data-stu-id="11c7b-260">Open the command prompt in the *ClientApp* directory.</span></span> <span data-ttu-id="11c7b-261">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="11c7b-261">Run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="11c7b-262">Сценарий запускает средство запуска тестов Karma, который считывает параметры, определенные в *karma.conf.js* файла.</span><span class="sxs-lookup"><span data-stu-id="11c7b-262">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="11c7b-263">Вместе с другими параметрами *karma.conf.js* определяет файлы теста для выполнения с помощью его `files` массива:</span><span class="sxs-lookup"><span data-stu-id="11c7b-263">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

## <a name="publish-the-app"></a><span data-ttu-id="11c7b-264">Публикация приложения</span><span class="sxs-lookup"><span data-stu-id="11c7b-264">Publish the app</span></span>

<span data-ttu-id="11c7b-265">Объединение созданных ресурсов на стороне клиента и опубликованных артефакты ASP.NET Core в готовые к развертыванию пакет может быть громоздким.</span><span class="sxs-lookup"><span data-stu-id="11c7b-265">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="11c7b-266">К счастью, SpaServices организует этот процесс всей публикации с пользовательский целевой объект MSBuild с именем `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="11c7b-266">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="11c7b-267">Целевой объект MSBuild должен выполнить следующие:</span><span class="sxs-lookup"><span data-stu-id="11c7b-267">The MSBuild target has the following responsibilities:</span></span>

1. <span data-ttu-id="11c7b-268">Восстановите пакеты NPM.</span><span class="sxs-lookup"><span data-stu-id="11c7b-268">Restore the npm packages.</span></span>
1. <span data-ttu-id="11c7b-269">Создайте сборку производственного уровня сторонних клиентских ресурсов.</span><span class="sxs-lookup"><span data-stu-id="11c7b-269">Create a production-grade build of the third-party, client-side assets.</span></span>
1. <span data-ttu-id="11c7b-270">Создание сборки настраиваемых клиентских ресурсов на рабочем уровне.</span><span class="sxs-lookup"><span data-stu-id="11c7b-270">Create a production-grade build of the custom client-side assets.</span></span>
1. <span data-ttu-id="11c7b-271">Скопируйте созданные в составе пакета ресурсы в папку публикации.</span><span class="sxs-lookup"><span data-stu-id="11c7b-271">Copy the Webpack-generated assets to the publish folder.</span></span>

<span data-ttu-id="11c7b-272">Целевой объект MSBuild вызывается при запуске:</span><span class="sxs-lookup"><span data-stu-id="11c7b-272">The MSBuild target is invoked when running:</span></span>

```dotnetcli
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="11c7b-273">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="11c7b-273">Additional resources</span></span>

* [<span data-ttu-id="11c7b-274">Документация по angular</span><span class="sxs-lookup"><span data-stu-id="11c7b-274">Angular Docs</span></span>](https://angular.io/docs)
