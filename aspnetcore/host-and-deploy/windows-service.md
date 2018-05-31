---
title: Размещение ASP.NET Core в службе Windows
author: rick-anderson
description: Узнайте, как разместить приложение ASP.NET Core в службе Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 29f83ee585c73aeb57a09f70ea8e28650c05ce69
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153533"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="10c96-103">Размещение ASP.NET Core в службе Windows</span><span class="sxs-lookup"><span data-stu-id="10c96-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="10c96-104">Автор: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra)</span><span class="sxs-lookup"><span data-stu-id="10c96-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="10c96-105">Чтобы разместить приложение ASP.NET Core в Windows, не используя IIS, рекомендуем выполнить его в [службе Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="10c96-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="10c96-106">Размещенное в службе Windows приложение может автоматически запускаться после перезагрузок и аварийных завершений работы, не требуя вмешательства оператора.</span><span class="sxs-lookup"><span data-stu-id="10c96-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="10c96-107">[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([описание скачивания](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="10c96-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="10c96-108">Инструкции по запуску примера приложения содержатся в файле *README.md* для этого примера.</span><span class="sxs-lookup"><span data-stu-id="10c96-108">For instructions on how to run the sample app, see the sample's *README.md* file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="10c96-109">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="10c96-109">Prerequisites</span></span>

* <span data-ttu-id="10c96-110">Приложение должно работать в среде выполнения .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="10c96-110">The app must run on the .NET Framework runtime.</span></span> <span data-ttu-id="10c96-111">В файле *.csproj* укажите соответствующие значения для параметров [TargetFramework](/nuget/schema/target-frameworks) (Целевая среда выполнения) и [RuntimeIdentifier](/dotnet/articles/core/rid-catalog) (Идентификатор среды выполнения).</span><span class="sxs-lookup"><span data-stu-id="10c96-111">In the *.csproj* file, specify appropriate values for [TargetFramework](/nuget/schema/target-frameworks) and [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="10c96-112">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="10c96-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="10c96-113">При создании нового проекта в Visual Studio используйте шаблон **приложения ASP.NET Core (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="10c96-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="10c96-114">Если это приложение получает запросы из Интернета (а не только из внутренней сети), для него потребуется веб-сервер [HTTP.sys](xref:fundamentals/servers/httpsys) (который ранее назывался [WebListener](xref:fundamentals/servers/weblistener) для приложений ASP.NET Core 1.x), а не [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="10c96-114">If the app receives requests from the Internet (not just from an internal network), it must use the [HTTP.sys](xref:fundamentals/servers/httpsys) web server (formerly known as [WebListener](xref:fundamentals/servers/weblistener) for ASP.NET Core 1.x apps) rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="10c96-115">Рекомендуем применять IIS в качестве обратного прокси-сервера для развертываний Kestrel на пограничных серверах.</span><span class="sxs-lookup"><span data-stu-id="10c96-115">IIS is recommended for use as a reverse proxy server with Kestrel for edge deployments.</span></span> <span data-ttu-id="10c96-116">Дополнительные сведения см. в статье [Использование Kestrel с обратным прокси-сервером](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="10c96-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="get-started"></a><span data-ttu-id="10c96-117">Начало работы</span><span class="sxs-lookup"><span data-stu-id="10c96-117">Get started</span></span>

<span data-ttu-id="10c96-118">В этом разделе описан минимальный набор изменений, позволяющий настроить существующий проект ASP.NET Core для запуска в службе.</span><span class="sxs-lookup"><span data-stu-id="10c96-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

1. <span data-ttu-id="10c96-119">Установите пакет NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="10c96-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

2. <span data-ttu-id="10c96-120">Внесите следующие изменения в `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="10c96-120">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="10c96-121">Вызовите метод `host.RunAsService` вместо `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="10c96-121">Call `host.RunAsService` instead of `host.Run`.</span></span>

   * <span data-ttu-id="10c96-122">Если в коде есть вызовы `UseContentRoot`, используйте в них путь к расположению публикации вместо `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="10c96-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`.</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="10c96-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="10c96-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="10c96-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="10c96-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

3. <span data-ttu-id="10c96-125">Опубликуйте приложение в папке.</span><span class="sxs-lookup"><span data-stu-id="10c96-125">Publish the app to a folder.</span></span> <span data-ttu-id="10c96-126">Используйте команду [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) или [профиль публикации Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles), выполняющий публикацию в папку.</span><span class="sxs-lookup"><span data-stu-id="10c96-126">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

4. <span data-ttu-id="10c96-127">Выполните проверку, создав и запустив службу.</span><span class="sxs-lookup"><span data-stu-id="10c96-127">Test by creating and starting the service.</span></span>

   <span data-ttu-id="10c96-128">Откройте оболочку командной строки с правами администратора и запустите в ней программу командной строки [sc.exe](https://technet.microsoft.com/library/bb490995) для создания и запуска службы.</span><span class="sxs-lookup"><span data-stu-id="10c96-128">Open a command shell with administrative privileges to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span> <span data-ttu-id="10c96-129">Например, если служба называется MyService и опубликована в `c:\svc` с именем AspNetCoreService, для ее запуска используются следующие команды:</span><span class="sxs-lookup"><span data-stu-id="10c96-129">If the service is named MyService, published to `c:\svc`, and named AspNetCoreService, the commands are:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   <span data-ttu-id="10c96-130">Значение `binPath` обозначает путь к исполняемому файлу приложения, включая имя самого файла.</span><span class="sxs-lookup"><span data-stu-id="10c96-130">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span>

   ![Пример создания и запуска в окне консоли](windows-service/_static/create-start.png)

   <span data-ttu-id="10c96-132">Когда эти команд будут выполнены, перейдите по тому же пути, что и при запуске консольного приложения (по умолчанию это `http://localhost:5000`):</span><span class="sxs-lookup"><span data-stu-id="10c96-132">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`):</span></span>

   ![Выполнение в качестве службы](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="10c96-134">Обеспечение возможности запуска за пределами службы</span><span class="sxs-lookup"><span data-stu-id="10c96-134">Provide a way to run outside of a service</span></span>

<span data-ttu-id="10c96-135">Тестирование и отладку проще выполнять при работе вне службы. Поэтому разработчики обычно добавляют код, который вызывает `RunAsService` только при соблюдении определенных условий.</span><span class="sxs-lookup"><span data-stu-id="10c96-135">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="10c96-136">Например, приложение может выполняться как консольное приложение с аргументом командной строки `--console` или при присоединении отладчика:</span><span class="sxs-lookup"><span data-stu-id="10c96-136">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="10c96-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="10c96-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="10c96-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="10c96-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="10c96-139">Обработка событий остановки и запуска</span><span class="sxs-lookup"><span data-stu-id="10c96-139">Handle stopping and starting events</span></span>

<span data-ttu-id="10c96-140">Чтобы правильно обрабатывать события `OnStarting`, `OnStarted` и `OnStopping`, внесите дополнительные изменения.</span><span class="sxs-lookup"><span data-stu-id="10c96-140">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

1. <span data-ttu-id="10c96-141">Создайте класс, производный от класса `WebHostService`.</span><span class="sxs-lookup"><span data-stu-id="10c96-141">Create a class that derives from `WebHostService`:</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="10c96-142">Создайте метод расширения для `IWebHost`, который передает пользовательский параметр `WebHostService` в `ServiceBase.Run`:</span><span class="sxs-lookup"><span data-stu-id="10c96-142">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`:</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="10c96-143">В `Program.Main` измените вызов `RunAsService` на вызов нового метода расширения `RunAsCustomService`:</span><span class="sxs-lookup"><span data-stu-id="10c96-143">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of `RunAsService`:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="10c96-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="10c96-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="10c96-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="10c96-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

<span data-ttu-id="10c96-146">Если пользовательский код `WebHostService` обращается к службе путем введения зависимости (например, к средству ведения журнала), ее можно получить из свойства `Services` объекта `IWebHost`:</span><span class="sxs-lookup"><span data-stu-id="10c96-146">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the `Services` property of `IWebHost`:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="10c96-147">Сценарии использования прокси-сервера и подсистемы балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="10c96-147">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="10c96-148">Для служб, которые взаимодействуют с запросами из Интернета или корпоративной сети и размещаются за прокси-сервером или подсистемой балансировки нагрузки, может потребоваться дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="10c96-148">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="10c96-149">Дополнительные сведения см. в разделе [Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="10c96-149">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="10c96-150">Благодарности</span><span class="sxs-lookup"><span data-stu-id="10c96-150">Acknowledgments</span></span>

<span data-ttu-id="10c96-151">Эта статья создана с использованием данных из других опубликованных источников:</span><span class="sxs-lookup"><span data-stu-id="10c96-151">This article was written with the help of published sources:</span></span>

* <span data-ttu-id="10c96-152">[Hosting ASP.NET Core as Windows service](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074) (Размещение ASP.NET Core в качестве службы Windows)</span><span class="sxs-lookup"><span data-stu-id="10c96-152">[Hosting ASP.NET Core as Windows service](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)</span></span>
* <span data-ttu-id="10c96-153">[How to host your ASP.NET Core in a Windows Service](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/) (Как разместить ASP.NET Core в службе Windows)</span><span class="sxs-lookup"><span data-stu-id="10c96-153">[How to host your ASP.NET Core in a Windows Service](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)</span></span>
