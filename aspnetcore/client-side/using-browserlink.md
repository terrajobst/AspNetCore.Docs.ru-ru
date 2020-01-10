---
title: Ссылка на браузер в ASP.NET Core
author: ncarandini
description: Объясняется, как связь с браузером является компонентом Visual Studio, который связывает среду разработки с одним или несколькими веб-браузерами.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 01/09/2020
no-loc:
- SignalR
uid: client-side/using-browserlink
ms.openlocfilehash: 19cc3c2ed91bd9e05df3c036123c78ecbf81fcc0
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828274"
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="7a886-103">Ссылка на браузер в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7a886-103">Browser Link in ASP.NET Core</span></span>

<span data-ttu-id="7a886-104">[Николò карандини](https://github.com/ncarandini), [Mike Уоссон](https://github.com/MikeWasson)и [Tom Dykstra)](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="7a886-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="7a886-105">Ссылка на браузер является компонентом Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7a886-105">Browser Link is a Visual Studio feature.</span></span> <span data-ttu-id="7a886-106">Он создает коммуникационный канал между средой разработки и одним или несколькими веб-браузерами.</span><span class="sxs-lookup"><span data-stu-id="7a886-106">It creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="7a886-107">Можно использовать ссылку на браузер для обновления веб-приложения одновременно в нескольких браузерах, что полезно для тестирования в разных браузерах.</span><span class="sxs-lookup"><span data-stu-id="7a886-107">You can use Browser Link to refresh your web app in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="7a886-108">Настройка связи с браузером</span><span class="sxs-lookup"><span data-stu-id="7a886-108">Browser Link setup</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7a886-109">Добавьте в проект пакет [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) .</span><span class="sxs-lookup"><span data-stu-id="7a886-109">Add the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package to your project.</span></span> <span data-ttu-id="7a886-110">Для ASP.NET Core проектов Razor Pages или MVC также включите компиляцию файлов Razor ( *. cshtml*) во время выполнения, как описано в разделе <xref:mvc/views/view-compilation>.</span><span class="sxs-lookup"><span data-stu-id="7a886-110">For ASP.NET Core Razor Pages or MVC projects, also enable runtime compilation of Razor (*.cshtml*) files as described in <xref:mvc/views/view-compilation>.</span></span> <span data-ttu-id="7a886-111">Синтаксис Razor изменения применяются только при включенной компиляции среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="7a886-111">Razor syntax changes are applied only when runtime compilation has been enabled.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="7a886-112">При преобразовании проекта ASP.NET Core 2,0 в ASP.NET Core 2,1 и переходе к [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app)установите пакет [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) для функции связи с браузером.</span><span class="sxs-lookup"><span data-stu-id="7a886-112">When converting an ASP.NET Core 2.0 project to ASP.NET Core 2.1 and transitioning to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), install the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package for Browser Link functionality.</span></span> <span data-ttu-id="7a886-113">Шаблоны проектов ASP.NET Core 2,1 по умолчанию используют `Microsoft.AspNetCore.App` метапакет.</span><span class="sxs-lookup"><span data-stu-id="7a886-113">The ASP.NET Core 2.1 project templates use the `Microsoft.AspNetCore.App` metapackage by default.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="7a886-114">Шаблоны проектов **веб-приложений**ASP.NET Core 2,0, **Empty**и **Web API** используют объект [Microsoft. AspNetCore. ALL метапакет](xref:fundamentals/metapackage), содержащий ссылку на пакет для [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span><span class="sxs-lookup"><span data-stu-id="7a886-114">The ASP.NET Core 2.0 **Web Application**, **Empty**, and **Web API** project templates use the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="7a886-115">Поэтому использование `Microsoft.AspNetCore.All` метапакет не требует дальнейших действий, чтобы сделать связь с браузером доступной для использования.</span><span class="sxs-lookup"><span data-stu-id="7a886-115">Therefore, using the `Microsoft.AspNetCore.All` metapackage requires no further action to make Browser Link available for use.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="7a886-116">Шаблон проекта **веб-приложения** ASP.NET Core 1. x содержит ссылку на пакет для пакета [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) .</span><span class="sxs-lookup"><span data-stu-id="7a886-116">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="7a886-117">Для других типов проектов требуется добавить ссылку на пакет `Microsoft.VisualStudio.Web.BrowserLink`.</span><span class="sxs-lookup"><span data-stu-id="7a886-117">Other project types require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="7a886-118">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="7a886-118">Configuration</span></span>

<span data-ttu-id="7a886-119">Вызовите `Startup.Configure` в методе `UseBrowserLink`:</span><span class="sxs-lookup"><span data-stu-id="7a886-119">Call `UseBrowserLink` in the `Startup.Configure` method:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="7a886-120">Вызов `UseBrowserLink` обычно размещается в блоке `if`, который разрешает только связь с браузером в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="7a886-120">The `UseBrowserLink` call is typically placed inside an `if` block that only enables Browser Link in the Development environment.</span></span> <span data-ttu-id="7a886-121">Например:</span><span class="sxs-lookup"><span data-stu-id="7a886-121">For example:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="7a886-122">Для получения дополнительной информации см. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="7a886-122">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="7a886-123">Как использовать связь с браузером</span><span class="sxs-lookup"><span data-stu-id="7a886-123">How to use Browser Link</span></span>

<span data-ttu-id="7a886-124">При открытии проекта ASP.NET Core в Visual Studio отображается элемент управления панели инструментов ссылки на браузер рядом с элементом управления панели инструментов **отладки Target** :</span><span class="sxs-lookup"><span data-stu-id="7a886-124">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Раскрывающееся меню связи с браузером](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="7a886-126">С помощью элемента управления панели инструментов связи в браузере можно:</span><span class="sxs-lookup"><span data-stu-id="7a886-126">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="7a886-127">Одновременное обновление веб-приложения в нескольких браузерах.</span><span class="sxs-lookup"><span data-stu-id="7a886-127">Refresh the web app in several browsers at once.</span></span>
* <span data-ttu-id="7a886-128">Откройте **панель мониторинга связи с браузером**.</span><span class="sxs-lookup"><span data-stu-id="7a886-128">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="7a886-129">Включение или отключение **связи с браузером**.</span><span class="sxs-lookup"><span data-stu-id="7a886-129">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="7a886-130">Примечание. ссылка на браузер отключена по умолчанию в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7a886-130">Note: Browser Link is disabled by default in Visual Studio.</span></span>
* <span data-ttu-id="7a886-131">Включение или отключение [автоматической синхронизации CSS](#enable-or-disable-css-auto-sync).</span><span class="sxs-lookup"><span data-stu-id="7a886-131">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a><span data-ttu-id="7a886-132">Обновление веб-приложения в нескольких браузерах одновременно</span><span class="sxs-lookup"><span data-stu-id="7a886-132">Refresh the web app in several browsers at once</span></span>

<span data-ttu-id="7a886-133">Чтобы выбрать один веб-браузер для запуска при запуске проекта, используйте раскрывающееся меню в элементе управления на панели инструментов **отладки** .</span><span class="sxs-lookup"><span data-stu-id="7a886-133">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![Раскрывающееся меню F5](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="7a886-135">Чтобы открыть сразу несколько браузеров, нажмите кнопку **Обзор с помощью...** в одном раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="7a886-135">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="7a886-136">Удерживая нажатой клавишу <kbd>CTRL</kbd> , выберите нужные браузеры и нажмите кнопку **Обзор**:</span><span class="sxs-lookup"><span data-stu-id="7a886-136">Hold down the <kbd>Ctrl</kbd> key to select the browsers you want, and then click **Browse**:</span></span>

![Одновременное открытие нескольких браузеров](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="7a886-138">На следующем снимке экрана показана Visual Studio с открытым представлением индекса и двумя открытыми браузерами:</span><span class="sxs-lookup"><span data-stu-id="7a886-138">The following screenshot shows Visual Studio with the Index view open and two open browsers:</span></span>

![Пример синхронизации с двумя браузерами](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="7a886-140">Наведите указатель мыши на элемент управления ToolBar Link (ссылка на браузер), чтобы просмотреть браузеры, подключенные к проекту:</span><span class="sxs-lookup"><span data-stu-id="7a886-140">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Подсказка при наведении](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="7a886-142">Измените представление индекса, и все подключенные браузеры будут обновлены при нажатии кнопки Обновить связь с браузером:</span><span class="sxs-lookup"><span data-stu-id="7a886-142">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![браузеры — синхронизация с изменениями](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="7a886-144">Связь с браузером также работает с браузерами, запускаемыми извне Visual Studio, и переходить по URL-адресу приложения.</span><span class="sxs-lookup"><span data-stu-id="7a886-144">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the app URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="7a886-145">Панель мониторинга связи с браузером</span><span class="sxs-lookup"><span data-stu-id="7a886-145">The Browser Link Dashboard</span></span>

<span data-ttu-id="7a886-146">Откройте окно **панели мониторинга связи с браузером** в раскрывающемся меню ссылки браузера, чтобы управлять подключением с помощью открытых браузеров:</span><span class="sxs-lookup"><span data-stu-id="7a886-146">Open the **Browser Link Dashboard** window from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![Open-бровсерслинк-Dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="7a886-148">Если ни один браузер не подключен, можно запустить сеанс без отладки, выбрав ссылку **Просмотреть в браузере** :</span><span class="sxs-lookup"><span data-stu-id="7a886-148">If no browser is connected, you can start a non-debugging session by selecting the **View in Browser** link:</span></span>

![browserlink-Dashboard-нет-подключения](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="7a886-150">В противном случае отображаются подключенные браузеры с указанием пути к странице, отображаемой в каждом браузере:</span><span class="sxs-lookup"><span data-stu-id="7a886-150">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-Dashboard-два подключения](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="7a886-152">Можно также щелкнуть имя отдельного браузера, чтобы обновить только этот браузер.</span><span class="sxs-lookup"><span data-stu-id="7a886-152">You can also click on an individual browser name to refresh only that browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="7a886-153">Включение или отключение связи с браузером</span><span class="sxs-lookup"><span data-stu-id="7a886-153">Enable or disable Browser Link</span></span>

<span data-ttu-id="7a886-154">При повторном включении связи с браузером после ее отключения необходимо обновить браузеры, чтобы повторно подключить их.</span><span class="sxs-lookup"><span data-stu-id="7a886-154">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="7a886-155">Включение или отключение автоматической синхронизации CSS</span><span class="sxs-lookup"><span data-stu-id="7a886-155">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="7a886-156">При включении автоматической синхронизации CSS подключенные браузеры автоматически обновляются при внесении любых изменений в файлы CSS.</span><span class="sxs-lookup"><span data-stu-id="7a886-156">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="7a886-157">Как это работает</span><span class="sxs-lookup"><span data-stu-id="7a886-157">How it works</span></span>

<span data-ttu-id="7a886-158">Ссылка на браузер использует [SignalR](xref:signalr/introduction) для создания коммуникационного канала между Visual Studio и браузером.</span><span class="sxs-lookup"><span data-stu-id="7a886-158">Browser Link uses [SignalR](xref:signalr/introduction) to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="7a886-159">Если связь с браузером включена, Visual Studio выступает в качестве SignalR сервера, к которому могут подключаться несколько клиентов (браузеров).</span><span class="sxs-lookup"><span data-stu-id="7a886-159">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="7a886-160">Связь с браузером также регистрирует компонент по промежуточного слоя в конвейере запросов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7a886-160">Browser Link also registers a middleware component in the ASP.NET Core request pipeline.</span></span> <span data-ttu-id="7a886-161">Этот компонент внедряет специальные ссылки `<script>` в каждый запрос страницы с сервера.</span><span class="sxs-lookup"><span data-stu-id="7a886-161">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="7a886-162">Чтобы просмотреть ссылки на скрипты, выберите в браузере пункт **Просмотреть источник** и Прокрутите содержимое тега до конца `<body>`:</span><span class="sxs-lookup"><span data-stu-id="7a886-162">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="7a886-163">Исходные файлы не изменяются.</span><span class="sxs-lookup"><span data-stu-id="7a886-163">Your source files aren't modified.</span></span> <span data-ttu-id="7a886-164">Компонент по промежуточного слоя динамически вставляет ссылки на скрипты.</span><span class="sxs-lookup"><span data-stu-id="7a886-164">The middleware component injects the script references dynamically.</span></span>

<span data-ttu-id="7a886-165">Так как код на стороне браузера является всего лишь JavaScript, он работает во всех браузерах, которые SignalR поддерживаются без использования подключаемого модуля браузера.</span><span class="sxs-lookup"><span data-stu-id="7a886-165">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
