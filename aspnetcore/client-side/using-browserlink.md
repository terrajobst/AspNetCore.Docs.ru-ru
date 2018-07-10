---
title: Связь с браузером в ASP.NET Core
author: ncarandini
description: Объясняет, как связь с браузером — это компонент Visual Studio, который связывает среде разработки с помощью одного или нескольких веб-браузеров.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
uid: client-side/using-browserlink
ms.openlocfilehash: 5ab15c841c472e6c9d47bad70fcf5e0c6dc3010f
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894183"
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="6b40c-103">Связь с браузером в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6b40c-103">Browser Link in ASP.NET Core</span></span>

<span data-ttu-id="6b40c-104">По [Nicolò Carandini](https://github.com/ncarandini), [Майк Уоссон](https://github.com/MikeWasson), и [том Дайкстра](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="6b40c-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="6b40c-105">Связь с браузером — это функция в Visual Studio, который создает канал взаимодействия между средой разработки и один или несколько веб-браузеров.</span><span class="sxs-lookup"><span data-stu-id="6b40c-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="6b40c-106">Можно использовать связь с браузером для обновления веб-приложения в нескольких браузерах одновременно, что полезно для тестирования обозреватели.</span><span class="sxs-lookup"><span data-stu-id="6b40c-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="6b40c-107">Установка связи обозревателя</span><span class="sxs-lookup"><span data-stu-id="6b40c-107">Browser Link setup</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6b40c-108">При преобразовании в проект ASP.NET Core 2.0 в ASP.NET Core 2.1 и переход к [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), установить [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) пакета для Функциональные возможности BrowserLink.</span><span class="sxs-lookup"><span data-stu-id="6b40c-108">When converting an ASP.NET Core 2.0 project to ASP.NET Core 2.1 and transitioning to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), install the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package for BrowserLink functionality.</span></span> <span data-ttu-id="6b40c-109">Используйте шаблоны проектов ASP.NET Core 2.1 `Microsoft.AspNetCore.App` метапакет по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6b40c-109">The ASP.NET Core 2.1 project templates use the `Microsoft.AspNetCore.App` metapackage by default.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6b40c-110">ASP.NET Core 2.0 **веб-приложение**, **пустой**, и **веб-API** проекта используйте шаблоны [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage) , который содержит ссылку на пакет для [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span><span class="sxs-lookup"><span data-stu-id="6b40c-110">The ASP.NET Core 2.0 **Web Application**, **Empty**, and **Web API** project templates use the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="6b40c-111">Таким образом, использование `Microsoft.AspNetCore.All` метапакет не требуется никаких дополнительных действий, чтобы сделать доступными для использования связь с браузером.</span><span class="sxs-lookup"><span data-stu-id="6b40c-111">Therefore, using the `Microsoft.AspNetCore.All` metapackage requires no further action to make Browser Link available for use.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="6b40c-112">ASP.NET Core 1.x **веб-приложение** шаблон проекта содержит ссылку на пакет для [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) пакета.</span><span class="sxs-lookup"><span data-stu-id="6b40c-112">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="6b40c-113">**Пустой** или **веб-API** шаблона проектов требуется добавить ссылки на пакет `Microsoft.VisualStudio.Web.BrowserLink`.</span><span class="sxs-lookup"><span data-stu-id="6b40c-113">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="6b40c-114">Так как эта функция Visual Studio, самым простым способом для добавления пакета **пустой** или **веб-API** проекта шаблона является открытие **консоль диспетчера пакетов** (**Представление** > **Other Windows** > **консоль диспетчера пакетов**) и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6b40c-114">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="6b40c-115">Кроме того, можно использовать **диспетчер пакетов NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6b40c-115">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="6b40c-116">Щелкните правой кнопкой мыши имя проекта в **обозревателе решений** и выберите **управление пакетами NuGet**:</span><span class="sxs-lookup"><span data-stu-id="6b40c-116">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![Диспетчер пакетов NuGet, откройте](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="6b40c-118">Найдите и установите пакет:</span><span class="sxs-lookup"><span data-stu-id="6b40c-118">Find and install the package:</span></span>

![Добавление пакета с помощью диспетчера пакетов NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="6b40c-120">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="6b40c-120">Configuration</span></span>

<span data-ttu-id="6b40c-121">В методе `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="6b40c-121">In the `Startup.Configure` method:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="6b40c-122">Обычно код находится внутри `if` блок, который обеспечивает только связь с браузером в среде разработки, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="6b40c-122">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="6b40c-123">Дополнительные сведения см. в статье [Использование нескольких сред](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="6b40c-123">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="6b40c-124">Как использовать связь с браузером</span><span class="sxs-lookup"><span data-stu-id="6b40c-124">How to use Browser Link</span></span>

<span data-ttu-id="6b40c-125">При наличии в открытом проекте ASP.NET Core, Visual Studio отображает панели инструментов браузера ссылку рядом с полем **целевой объект отладки** элемент управления панели инструментов:</span><span class="sxs-lookup"><span data-stu-id="6b40c-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Раскрывающееся меню браузера ссылку](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="6b40c-127">На панели инструментов привязывание к браузеру вы можете:</span><span class="sxs-lookup"><span data-stu-id="6b40c-127">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="6b40c-128">Обновите веб-приложения в нескольких браузерах за один раз.</span><span class="sxs-lookup"><span data-stu-id="6b40c-128">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="6b40c-129">Откройте **мониторинга связи с браузером**.</span><span class="sxs-lookup"><span data-stu-id="6b40c-129">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="6b40c-130">Включение или отключение **связь с браузером**.</span><span class="sxs-lookup"><span data-stu-id="6b40c-130">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="6b40c-131">Примечание: Привязывание к браузеру отключен по умолчанию в Visual Studio 2017 (версии 15.3).</span><span class="sxs-lookup"><span data-stu-id="6b40c-131">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="6b40c-132">Включение или отключение [Автосинхронизация CSS](#enable-or-disable-css-auto-sync).</span><span class="sxs-lookup"><span data-stu-id="6b40c-132">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="6b40c-133">Некоторые Visual Studio подключаемые модули, особенно *2015 пакет расширения веб* и *2017 пакет расширения Web*, предлагают расширенные функциональные возможности для связи с браузером, но некоторые дополнительные функции, не работают с ASP. Проекты .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6b40c-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a><span data-ttu-id="6b40c-134">Обновить веб-приложения в нескольких браузерах за один раз</span><span class="sxs-lookup"><span data-stu-id="6b40c-134">Refresh the web app in several browsers at once</span></span>

<span data-ttu-id="6b40c-135">Чтобы выбрать один веб-браузер для запуска при запуске проекта, используйте раскрывающееся меню в **целевой объект отладки** элемент управления панели инструментов:</span><span class="sxs-lookup"><span data-stu-id="6b40c-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![Выберите в раскрывающемся меню F5](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="6b40c-137">Чтобы одновременно открыть несколько браузеров, выберите **просмотр с помощью...**  же раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="6b40c-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="6b40c-138">Удерживая нажатой клавишу CTRL, чтобы выберите браузеры, требуется и нажмите кнопку **Обзор**:</span><span class="sxs-lookup"><span data-stu-id="6b40c-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![За один раз открыть Многие браузеры](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="6b40c-140">Ниже приведен на снимке экрана показан откройте Visual Studio в представлении индекса и две открытые окна браузера.</span><span class="sxs-lookup"><span data-stu-id="6b40c-140">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![Синхронизация с двумя браузеры примера](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="6b40c-142">Наведите указатель мыши на панели инструментов привязывание к браузеру для браузеров, которые подключены к проекту см. в разделе:</span><span class="sxs-lookup"><span data-stu-id="6b40c-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Подсказки при наведении курсора мыши](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="6b40c-144">Изменение представления индекса, и все подключенные браузеры обновляются при нажатии кнопки обновления привязывание к браузеру:</span><span class="sxs-lookup"><span data-stu-id="6b40c-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![браузеры синхронизации для изменения](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="6b40c-146">Связь с браузером также работает с браузерами, запустите из вне Visual Studio и перейдите к URL-адрес приложения.</span><span class="sxs-lookup"><span data-stu-id="6b40c-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="6b40c-147">Мониторинга связи с браузером</span><span class="sxs-lookup"><span data-stu-id="6b40c-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="6b40c-148">Откройте мониторинга связи с браузером в раскрывающемся меню управлять взаимодействием с открытым браузеры привязывание к браузеру:</span><span class="sxs-lookup"><span data-stu-id="6b40c-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![открыть browserslink панели мониторинга.](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="6b40c-150">Если браузер не подключен, можете начать сеанс без отладки, выбрав *просмотреть в браузере* ссылку:</span><span class="sxs-lookup"><span data-stu-id="6b40c-150">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink-панели мониторинга нет подключений](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="6b40c-152">В противном случае подключенные браузеры, показаны на путь к странице, на которой отображаются каждым браузером.</span><span class="sxs-lookup"><span data-stu-id="6b40c-152">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-панель мониторинга — двух подключений](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="6b40c-154">При желании можно щелкнуть имя перечисленных браузера, чтобы обновить этот одного браузера.</span><span class="sxs-lookup"><span data-stu-id="6b40c-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="6b40c-155">Включить или отключить связь с браузером</span><span class="sxs-lookup"><span data-stu-id="6b40c-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="6b40c-156">При повторном включении связь с браузером после ее отключения, браузеры для повторного подключения, их необходимо обновить.</span><span class="sxs-lookup"><span data-stu-id="6b40c-156">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="6b40c-157">Включить или отключить автоматическая синхронизация CSS</span><span class="sxs-lookup"><span data-stu-id="6b40c-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="6b40c-158">Если включена автоматическая синхронизация CSS, подключенные браузеры обновляются автоматически при любом изменении файлов CSS.</span><span class="sxs-lookup"><span data-stu-id="6b40c-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="6b40c-159">Принцип работы</span><span class="sxs-lookup"><span data-stu-id="6b40c-159">How it works</span></span>

<span data-ttu-id="6b40c-160">Связь с браузером использует SignalR для создания коммуникационного канала между Visual Studio и браузером.</span><span class="sxs-lookup"><span data-stu-id="6b40c-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="6b40c-161">При включении связь с браузером, Visual Studio выступает в качестве сервера SignalR, несколько клиентов (обозревателей) можно подключиться.</span><span class="sxs-lookup"><span data-stu-id="6b40c-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="6b40c-162">Связь с браузером также регистрирует компонент по промежуточного слоя в конвейере запросов ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6b40c-162">Browser Link also registers a middleware component in the ASP.NET request pipeline.</span></span> <span data-ttu-id="6b40c-163">Этот компонент вводит специальные `<script>` ссылок в каждом запросе страницы с сервера.</span><span class="sxs-lookup"><span data-stu-id="6b40c-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="6b40c-164">Вы увидите ссылки на скрипты, выбрав **Просмотр исходного кода** в браузере и прокрутка в конец `<body>` отметить содержимое:</span><span class="sxs-lookup"><span data-stu-id="6b40c-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="6b40c-165">Исходные файлы не изменяются.</span><span class="sxs-lookup"><span data-stu-id="6b40c-165">Your source files aren't modified.</span></span> <span data-ttu-id="6b40c-166">Компонент по промежуточного слоя динамически внедряет ссылок на скрипты.</span><span class="sxs-lookup"><span data-stu-id="6b40c-166">The middleware component injects the script references dynamically.</span></span>

<span data-ttu-id="6b40c-167">Так как код на стороне обозревателя все JavaScript, он работает во всех браузерах, поддерживаемых SignalR, не требуя подключаемый модуль обозревателя.</span><span class="sxs-lookup"><span data-stu-id="6b40c-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
