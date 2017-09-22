---
title: "Связь с браузером в ASP.NET Core"
author: ncarandini
description: "Функцию Visual Studio, которая связывает среду разработки с одного или нескольких веб-браузеров"
keywords: "ASP.NET Core, связь с браузером, синхронизации CSS"
ms.author: riande
manager: wpickett
ms.date: 12/28/2016
ms.topic: article
ms.assetid: 11813d4c-3f8a-445a-b23b-e4a57d001abc
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-browserlink
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 211dd5d03e6b8414e0b2ed3234d8970c92f72452
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-browser-link-in-aspnet-core"></a><span data-ttu-id="c2b54-104">Введение в связи с браузером в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c2b54-104">Introduction to Browser Link in ASP.NET Core</span></span> 

<span data-ttu-id="c2b54-105">По [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), и [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c2b54-105">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="c2b54-106">Связь с браузером — это функция в Visual Studio, который создает канал связи между средой разработки и один или несколько веб-браузеры.</span><span class="sxs-lookup"><span data-stu-id="c2b54-106">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="c2b54-107">Можно использовать связь с браузером для обновления веб-приложения в нескольких браузерах одновременно, это полезно для тестирования в различных браузерах.</span><span class="sxs-lookup"><span data-stu-id="c2b54-107">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="c2b54-108">Установка связи обозревателя</span><span class="sxs-lookup"><span data-stu-id="c2b54-108">Browser Link setup</span></span>

<span data-ttu-id="c2b54-109">ASP.NET Core **веб-приложение** шаблонов в Visual Studio 2015 и более поздних версий, имеют все необходимое для связи с браузером.</span><span class="sxs-lookup"><span data-stu-id="c2b54-109">The ASP.NET Core **Web Application** project templates in Visual Studio 2015 and later include everything needed for Browser Link.</span></span>

<span data-ttu-id="c2b54-110">Чтобы добавить в проект, созданный с помощью ASP.NET Core связь с браузером **пустой** или **веб-API** шаблона, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="c2b54-110">To add Browser Link to a project that you created by using the ASP.NET Core **Empty** or **Web API** template, follow these steps:</span></span>

1. <span data-ttu-id="c2b54-111">Добавить *Microsoft.VisualStudio.Web.BrowserLink.Loader* пакета</span><span class="sxs-lookup"><span data-stu-id="c2b54-111">Add the *Microsoft.VisualStudio.Web.BrowserLink.Loader* package</span></span> 
2. <span data-ttu-id="c2b54-112">Добавьте в код конфигурации в *файла Startup.cs* файл.</span><span class="sxs-lookup"><span data-stu-id="c2b54-112">Add configuration code in the *Startup.cs* file.</span></span>

### <a name="add-the-package"></a><span data-ttu-id="c2b54-113">Добавление пакета</span><span class="sxs-lookup"><span data-stu-id="c2b54-113">Add the package</span></span>

<span data-ttu-id="c2b54-114">Поскольку компоненты Visual Studio, самым простым способом добавления пакета — для открытия **консоль диспетчера пакетов** (**представление > Другие окна > консоль диспетчера пакетов**) и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="c2b54-114">Since this is a Visual Studio feature, the easiest way to add the package is to open the **Package Manager Console** (**View > Other Windows > Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink.Loader
```

<span data-ttu-id="c2b54-115">Кроме того, можно использовать **диспетчера пакетов NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c2b54-115">Alternatively, you can use **NuGet Package Manager**.</span></span>  <span data-ttu-id="c2b54-116">Щелкните правой кнопкой мыши имя проекта в **обозревателе решений**и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c2b54-116">Right-click the project name in **Solution Explorer**, and choose **Manage NuGet Packages**.</span></span> 

![Диспетчер пакетов Open NuGet](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="c2b54-118">Затем найдите и установки пакета.</span><span class="sxs-lookup"><span data-stu-id="c2b54-118">Then find and install the package.</span></span>

![Добавление пакета с помощью диспетчера пакетов NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

### <a name="add-configuration-code"></a><span data-ttu-id="c2b54-120">Добавьте в код конфигурации</span><span class="sxs-lookup"><span data-stu-id="c2b54-120">Add configuration code</span></span>

<span data-ttu-id="c2b54-121">Откройте *файла Startup.cs* файл и в `Configure` метод добавьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="c2b54-121">Open the *Startup.cs* file, and in the `Configure` method add the following code:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="c2b54-122">Обычно этот код находится внутри `if` блока, который обеспечивает связь с браузером только в среде разработки, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="c2b54-122">Usually that code is inside an `if` block that enables Browser Link only in the Development environment, as shown here:</span></span>

[!code-csharp[Main](./using-browserlink/sample/BrowserLinkSample/src/BrowserLinkSample/Startup.cs?highlight=1,4&range=40-44)]

<span data-ttu-id="c2b54-123">Дополнительные сведения см. в статье [Работа с несколькими средами](../fundamentals/environments.md).</span><span class="sxs-lookup"><span data-stu-id="c2b54-123">For more information, see [Working with Multiple Environments](../fundamentals/environments.md).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="c2b54-124">Как использовать связь с браузером</span><span class="sxs-lookup"><span data-stu-id="c2b54-124">How to use Browser Link</span></span>

<span data-ttu-id="c2b54-125">При наличии в открытом проекте ASP.NET Core, Visual Studio отображает панели инструментов обозревателя ссылку рядом с **целевой объект отладки** управления панели инструментов:</span><span class="sxs-lookup"><span data-stu-id="c2b54-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Раскрывающееся меню обозревателя ссылки](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="c2b54-127">На панели инструментов связь с браузером можно:</span><span class="sxs-lookup"><span data-stu-id="c2b54-127">From the Browser Link toolbar control, you can:</span></span>

- <span data-ttu-id="c2b54-128">Обновить веб-приложения в нескольких браузерах за один раз</span><span class="sxs-lookup"><span data-stu-id="c2b54-128">Refresh the web application in several browsers at once</span></span>
- <span data-ttu-id="c2b54-129">Откройте **мониторинга связи с браузером**</span><span class="sxs-lookup"><span data-stu-id="c2b54-129">Open the **Browser Link Dashboard**</span></span>
- <span data-ttu-id="c2b54-130">Включить или отключить **связь с браузером**</span><span class="sxs-lookup"><span data-stu-id="c2b54-130">Enable or disable **Browser Link**</span></span>
- <span data-ttu-id="c2b54-131">Включение и отключение автоматической синхронизации CSS</span><span class="sxs-lookup"><span data-stu-id="c2b54-131">Enable or disable CSS Auto-Sync</span></span>

> [!NOTE]
> <span data-ttu-id="c2b54-132">Некоторые Visual Studio подключаемые модули, прежде всего *2015 пакет расширения Web* и *2017 г. пакет расширения веб*, предоставляют расширенные функциональные возможности для связи с браузером, но некоторые дополнительные функции не работают с ASP. Проекты NET Core.</span><span class="sxs-lookup"><span data-stu-id="c2b54-132">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a><span data-ttu-id="c2b54-133">Обновить веб-приложения в нескольких браузерах за один раз</span><span class="sxs-lookup"><span data-stu-id="c2b54-133">Refresh the web application in several browsers at once</span></span>

<span data-ttu-id="c2b54-134">Чтобы выбрать один веб-браузер для запуска при запуске проекта, используйте раскрывающееся меню в **целевой объект отладки** управления панели инструментов:</span><span class="sxs-lookup"><span data-stu-id="c2b54-134">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![Раскрывающееся меню F5](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="c2b54-136">Чтобы одновременно открыть несколько браузеров, выберите **просмотреть с помощью... ** же раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="c2b54-136">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span>  <span data-ttu-id="c2b54-137">Удерживая нажатой клавишу CTRL при выборе в браузерах, требуется и нажмите кнопку **Обзор**:</span><span class="sxs-lookup"><span data-stu-id="c2b54-137">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Одновременное открытие большинство браузеров](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="c2b54-139">Ниже приведен пример снимка экрана отображение откройте Visual Studio с представление Index и два открытые окна браузера.</span><span class="sxs-lookup"><span data-stu-id="c2b54-139">Here's a sample screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![Синхронизация с пример двух браузеров](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="c2b54-141">Наведите указатель мыши на панели инструментов связь с браузером для просмотра в браузерах, подключенных к проекту:</span><span class="sxs-lookup"><span data-stu-id="c2b54-141">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Подсказки при наведении курсора мыши](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="c2b54-143">Изменить представление индекса и всех подключенных браузерах обновляются при нажатии кнопки "Обновить" в связи с браузером:</span><span class="sxs-lookup"><span data-stu-id="c2b54-143">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![браузеры синхронизации для изменения](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="c2b54-145">Связь с браузером также работает с браузерами, запустите из вне Visual Studio и перейдите к URL-адрес приложения.</span><span class="sxs-lookup"><span data-stu-id="c2b54-145">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="c2b54-146">Панели мониторинга связи с браузером</span><span class="sxs-lookup"><span data-stu-id="c2b54-146">The Browser Link Dashboard</span></span>

<span data-ttu-id="c2b54-147">В раскрывающемся меню для управления подключением с браузерами открыть связь с браузером, откройте мониторинга связи с браузером:</span><span class="sxs-lookup"><span data-stu-id="c2b54-147">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![Откройте browserslink панели мониторинга.](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="c2b54-149">Если браузер не подключен, можно запустить сеанс отладки не щелкнув _Просмотр в браузере_ ссылку:</span><span class="sxs-lookup"><span data-stu-id="c2b54-149">If no browser is connected, you can start a non debugging session clicking the _View in Browser_ link:</span></span>

![browserlink-панели мониторинга нет подключений](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="c2b54-151">В противном случае подключенной браузеры отображаются со путь к странице, отображение каждого браузера:</span><span class="sxs-lookup"><span data-stu-id="c2b54-151">Otherwise, the connected browsers are shown, with the path to the page that each browser is showing:</span></span>

![browserlink панель мониторинга — два подключения](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="c2b54-153">При желании можно щелкнуть имя перечисленных браузера, чтобы обновить этот одного браузера.</span><span class="sxs-lookup"><span data-stu-id="c2b54-153">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="c2b54-154">Включить или отключить связь с браузером</span><span class="sxs-lookup"><span data-stu-id="c2b54-154">Enable or disable Browser Link</span></span>

<span data-ttu-id="c2b54-155">При повторном включении связь с браузером после ее отключения, необходимо обновить в браузерах для повторного подключения их.</span><span class="sxs-lookup"><span data-stu-id="c2b54-155">When you re-enable Browser Link after disabling it, you have to refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="c2b54-156">Включение и отключение автоматической синхронизации CSS</span><span class="sxs-lookup"><span data-stu-id="c2b54-156">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="c2b54-157">При включенной автоматической синхронизации CSS подключенных браузеров будут автоматически обновляться при внесении изменений в CSS-файлах.</span><span class="sxs-lookup"><span data-stu-id="c2b54-157">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-does-it-work"></a><span data-ttu-id="c2b54-158">Как это работает?</span><span class="sxs-lookup"><span data-stu-id="c2b54-158">How does it work?</span></span>

<span data-ttu-id="c2b54-159">Связь с браузером SignalR использует для создания канала связи между Visual Studio и браузером.</span><span class="sxs-lookup"><span data-stu-id="c2b54-159">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="c2b54-160">Если включена связь с браузером, Visual Studio действует как сервер SignalR, подключенные к нескольким клиентам (браузерах).</span><span class="sxs-lookup"><span data-stu-id="c2b54-160">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="c2b54-161">Связь с браузером также регистрирует компонент по промежуточного слоя в конвейере ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c2b54-161">Browser Link also registers a middleware component in the ASP.NET request pipeline.</span></span> <span data-ttu-id="c2b54-162">Этот компонент вводит специальные `<script>` references в каждом запросе страницы с сервера.</span><span class="sxs-lookup"><span data-stu-id="c2b54-162">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="c2b54-163">При выборе можно просмотреть ссылки на скрипты **Просмотр исходного кода** в браузере и прокрутки к концу `<body>` разметки содержимого:</span><span class="sxs-lookup"><span data-stu-id="c2b54-163">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="c2b54-164">Исходные файлы не изменяются.</span><span class="sxs-lookup"><span data-stu-id="c2b54-164">Your source files are not modified.</span></span> <span data-ttu-id="c2b54-165">Компонент по промежуточного слоя динамически вставляет ссылки на скрипты.</span><span class="sxs-lookup"><span data-stu-id="c2b54-165">The middleware component injects the script references dynamically.</span></span> 

<span data-ttu-id="c2b54-166">Поскольку код браузером все JavaScript, он работает во всех браузерах, поддерживаемых SignalR, не требуя подключаемый модуль обозревателя.</span><span class="sxs-lookup"><span data-stu-id="c2b54-166">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports, without requiring any browser plug-in.</span></span>
