---
title: Ссылка на браузер в ASP.NET Core
author: ncarandini
description: Объясняется, как связь с браузером является компонентом Visual Studio, который связывает среду разработки с одним или несколькими веб-браузерами.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/12/2019
no-loc:
- SignalR
uid: client-side/using-browserlink
ms.openlocfilehash: b21b698d49e72b559cd9cd3753c48a38c99db24d
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962784"
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="03bbc-103">Ссылка на браузер в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="03bbc-103">Browser Link in ASP.NET Core</span></span>

<span data-ttu-id="03bbc-104">[Николò карандини](https://github.com/ncarandini), [Mike Уоссон](https://github.com/MikeWasson)и [Tom Dykstra)](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="03bbc-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="03bbc-105">Связь с браузером — это функция Visual Studio, которая создает коммуникационный канал между средой разработки и одним или несколькими веб-браузерами.</span><span class="sxs-lookup"><span data-stu-id="03bbc-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="03bbc-106">Можно использовать ссылку на браузер для обновления веб-приложения одновременно в нескольких браузерах, что полезно для тестирования в разных браузерах.</span><span class="sxs-lookup"><span data-stu-id="03bbc-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="03bbc-107">Настройка связи с браузером</span><span class="sxs-lookup"><span data-stu-id="03bbc-107">Browser Link setup</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="03bbc-108">При преобразовании проекта ASP.NET Core 2,0 в ASP.NET Core 2,1 и переходе к [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app)установите пакет [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) для функций BrowserLink.</span><span class="sxs-lookup"><span data-stu-id="03bbc-108">When converting an ASP.NET Core 2.0 project to ASP.NET Core 2.1 and transitioning to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), install the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package for BrowserLink functionality.</span></span> <span data-ttu-id="03bbc-109">Шаблоны проектов ASP.NET Core 2,1 по умолчанию используют `Microsoft.AspNetCore.App` метапакет.</span><span class="sxs-lookup"><span data-stu-id="03bbc-109">The ASP.NET Core 2.1 project templates use the `Microsoft.AspNetCore.App` metapackage by default.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="03bbc-110">Шаблоны проектов **веб-приложений**ASP.NET Core 2,0, **Empty**и **Web API** используют объект [Microsoft. AspNetCore. ALL метапакет](xref:fundamentals/metapackage), содержащий ссылку на пакет для [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span><span class="sxs-lookup"><span data-stu-id="03bbc-110">The ASP.NET Core 2.0 **Web Application**, **Empty**, and **Web API** project templates use the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="03bbc-111">Поэтому использование `Microsoft.AspNetCore.All` метапакет не требует дальнейших действий, чтобы сделать связь с браузером доступной для использования.</span><span class="sxs-lookup"><span data-stu-id="03bbc-111">Therefore, using the `Microsoft.AspNetCore.All` metapackage requires no further action to make Browser Link available for use.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="03bbc-112">Шаблон проекта **веб-приложения** ASP.NET Core 1. x содержит ссылку на пакет для пакета [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) .</span><span class="sxs-lookup"><span data-stu-id="03bbc-112">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="03bbc-113">Для шаблонов **пустых** **веб-API** или проектов необходимо добавить ссылку на пакет `Microsoft.VisualStudio.Web.BrowserLink`.</span><span class="sxs-lookup"><span data-stu-id="03bbc-113">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="03bbc-114">Поскольку это функция Visual Studio, самым простым способом добавления пакета в **пустой** проект или проекта шаблона **веб-API** является открытие **консоли диспетчера пакетов** (**просмотр** > другая **консоль диспетчера пакетов** **Windows** >) и выполнение следующей команды:</span><span class="sxs-lookup"><span data-stu-id="03bbc-114">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="03bbc-115">Кроме того, можно использовать **Диспетчер пакетов NuGet**.</span><span class="sxs-lookup"><span data-stu-id="03bbc-115">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="03bbc-116">Щелкните правой кнопкой мыши имя проекта в **Обозреватель решений** и выберите пункт **Управление пакетами NuGet**:</span><span class="sxs-lookup"><span data-stu-id="03bbc-116">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![Открыть диспетчер пакетов NuGet](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="03bbc-118">Найдите и установите пакет:</span><span class="sxs-lookup"><span data-stu-id="03bbc-118">Find and install the package:</span></span>

![Добавление пакета с помощью диспетчера пакетов NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="03bbc-120">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="03bbc-120">Configuration</span></span>

<span data-ttu-id="03bbc-121">В методе `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="03bbc-121">In the `Startup.Configure` method:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="03bbc-122">Обычно код находится внутри блока `if`, который включает в среде разработки связь только с браузером, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="03bbc-122">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="03bbc-123">Дополнительные сведения см. в статье [Использование нескольких сред](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="03bbc-123">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="03bbc-124">Как использовать связь с браузером</span><span class="sxs-lookup"><span data-stu-id="03bbc-124">How to use Browser Link</span></span>

<span data-ttu-id="03bbc-125">При открытии проекта ASP.NET Core в Visual Studio отображается элемент управления панели инструментов ссылки на браузер рядом с элементом управления панели инструментов **отладки Target** :</span><span class="sxs-lookup"><span data-stu-id="03bbc-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Раскрывающееся меню связи с браузером](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="03bbc-127">С помощью элемента управления панели инструментов связи в браузере можно:</span><span class="sxs-lookup"><span data-stu-id="03bbc-127">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="03bbc-128">Одновременное обновление веб-приложения в нескольких браузерах.</span><span class="sxs-lookup"><span data-stu-id="03bbc-128">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="03bbc-129">Откройте **панель мониторинга связи с браузером**.</span><span class="sxs-lookup"><span data-stu-id="03bbc-129">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="03bbc-130">Включение или отключение **связи с браузером**.</span><span class="sxs-lookup"><span data-stu-id="03bbc-130">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="03bbc-131">Примечание. ссылка на браузер отключена по умолчанию в Visual Studio 2017 (15,3).</span><span class="sxs-lookup"><span data-stu-id="03bbc-131">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="03bbc-132">Включение или отключение [автоматической синхронизации CSS](#enable-or-disable-css-auto-sync).</span><span class="sxs-lookup"><span data-stu-id="03bbc-132">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="03bbc-133">Некоторые подключаемые модули Visual Studio, в первую очередь, *пакет веб-расширений 2015* и *Пакет расширений Web Extension 2017*, предлагают расширенные функции для связи с браузером, но некоторые дополнительные функции не работают с ASP.NET Core проектами.</span><span class="sxs-lookup"><span data-stu-id="03bbc-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a><span data-ttu-id="03bbc-134">Обновление веб-приложения в нескольких браузерах одновременно</span><span class="sxs-lookup"><span data-stu-id="03bbc-134">Refresh the web app in several browsers at once</span></span>

<span data-ttu-id="03bbc-135">Чтобы выбрать один веб-браузер для запуска при запуске проекта, используйте раскрывающееся меню в элементе управления на панели инструментов **отладки** .</span><span class="sxs-lookup"><span data-stu-id="03bbc-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![Раскрывающееся меню F5](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="03bbc-137">Чтобы открыть сразу несколько браузеров, нажмите кнопку **Обзор с помощью...** в одном раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="03bbc-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="03bbc-138">Удерживая нажатой клавишу CTRL, выберите нужные браузеры и нажмите кнопку **Обзор**:</span><span class="sxs-lookup"><span data-stu-id="03bbc-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Одновременное открытие нескольких браузеров](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="03bbc-140">Ниже приведен снимок экрана, показывающий Visual Studio с открытым представлением индекса и двумя открытыми браузерами:</span><span class="sxs-lookup"><span data-stu-id="03bbc-140">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![Пример синхронизации с двумя браузерами](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="03bbc-142">Наведите указатель мыши на элемент управления ToolBar Link (ссылка на браузер), чтобы просмотреть браузеры, подключенные к проекту:</span><span class="sxs-lookup"><span data-stu-id="03bbc-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Подсказка при наведении](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="03bbc-144">Измените представление индекса, и все подключенные браузеры будут обновлены при нажатии кнопки Обновить связь с браузером:</span><span class="sxs-lookup"><span data-stu-id="03bbc-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![браузеры — синхронизация с изменениями](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="03bbc-146">Связь с браузером также работает с браузерами, запускаемыми извне Visual Studio, и переходить по URL-адресу приложения.</span><span class="sxs-lookup"><span data-stu-id="03bbc-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="03bbc-147">Панель мониторинга связи с браузером</span><span class="sxs-lookup"><span data-stu-id="03bbc-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="03bbc-148">Откройте панель мониторинга связи с браузером в раскрывающемся меню ссылки браузера, чтобы управлять подключением с помощью открытых браузеров:</span><span class="sxs-lookup"><span data-stu-id="03bbc-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![Open-бровсерслинк-Dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="03bbc-150">Если ни один браузер не подключен, можно запустить сеанс без отладки, выбрав ссылку *Просмотреть в браузере* :</span><span class="sxs-lookup"><span data-stu-id="03bbc-150">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink-Dashboard-нет-подключения](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="03bbc-152">В противном случае отображаются подключенные браузеры с указанием пути к странице, отображаемой в каждом браузере:</span><span class="sxs-lookup"><span data-stu-id="03bbc-152">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-Dashboard-два подключения](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="03bbc-154">При желании можно щелкнуть имя указанного браузера, чтобы обновить этот обозреватель.</span><span class="sxs-lookup"><span data-stu-id="03bbc-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="03bbc-155">Включение или отключение связи с браузером</span><span class="sxs-lookup"><span data-stu-id="03bbc-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="03bbc-156">При повторном включении связи с браузером после ее отключения необходимо обновить браузеры, чтобы повторно подключить их.</span><span class="sxs-lookup"><span data-stu-id="03bbc-156">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="03bbc-157">Включение или отключение автоматической синхронизации CSS</span><span class="sxs-lookup"><span data-stu-id="03bbc-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="03bbc-158">При включении автоматической синхронизации CSS подключенные браузеры автоматически обновляются при внесении любых изменений в файлы CSS.</span><span class="sxs-lookup"><span data-stu-id="03bbc-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="03bbc-159">Принцип работы</span><span class="sxs-lookup"><span data-stu-id="03bbc-159">How it works</span></span>

<span data-ttu-id="03bbc-160">Ссылка на браузер использует SignalR для создания коммуникационного канала между Visual Studio и браузером.</span><span class="sxs-lookup"><span data-stu-id="03bbc-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="03bbc-161">Если связь с браузером включена, Visual Studio выступает в качестве SignalR сервера, к которому могут подключаться несколько клиентов (браузеров).</span><span class="sxs-lookup"><span data-stu-id="03bbc-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="03bbc-162">Связь с браузером также регистрирует компонент по промежуточного слоя в конвейере запросов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="03bbc-162">Browser Link also registers a middleware component in the ASP.NET Core request pipeline.</span></span> <span data-ttu-id="03bbc-163">Этот компонент внедряет специальные ссылки `<script>` в каждый запрос страницы с сервера.</span><span class="sxs-lookup"><span data-stu-id="03bbc-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="03bbc-164">Чтобы просмотреть ссылки на скрипты, выберите в браузере пункт **Просмотреть источник** и Прокрутите содержимое тега до конца `<body>`:</span><span class="sxs-lookup"><span data-stu-id="03bbc-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="03bbc-165">Исходные файлы не изменяются.</span><span class="sxs-lookup"><span data-stu-id="03bbc-165">Your source files aren't modified.</span></span> <span data-ttu-id="03bbc-166">Компонент по промежуточного слоя динамически вставляет ссылки на скрипты.</span><span class="sxs-lookup"><span data-stu-id="03bbc-166">The middleware component injects the script references dynamically.</span></span>

<span data-ttu-id="03bbc-167">Так как код на стороне браузера является всего лишь JavaScript, он работает во всех браузерах, которые SignalR поддерживаются без использования подключаемого модуля браузера.</span><span class="sxs-lookup"><span data-stu-id="03bbc-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
