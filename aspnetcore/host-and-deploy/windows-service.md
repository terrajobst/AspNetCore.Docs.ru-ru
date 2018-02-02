---
title: "Узел в службе Windows"
author: tdykstra
description: "Узнайте, как для размещения приложения ASP.NET Core в службе Windows."
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: c14a1f62bce4d06be3b8e6356f45cd5e330a0751
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2018
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="d986d-103">Узел приложения ASP.NET Core в службе Windows</span><span class="sxs-lookup"><span data-stu-id="d986d-103">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="d986d-104">По [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d986d-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="d986d-105">Рекомендуется размещать приложения ASP.NET Core в Windows без с помощью служб IIS для запуска в [службы Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="d986d-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="d986d-106">Когда в качестве службы Windows, приложение может автоматически запуск после перезагрузки и аварийно завершает работу без необходимости вмешательства человека.</span><span class="sxs-lookup"><span data-stu-id="d986d-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="d986d-107">[Просмотреть или загрузить образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([загрузке](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d986d-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="d986d-108">Инструкции по запуску примера приложения, см. пример *README.md* файла.</span><span class="sxs-lookup"><span data-stu-id="d986d-108">For instructions on how to run the sample app, see the sample's *README.md* file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d986d-109">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="d986d-109">Prerequisites</span></span>

* <span data-ttu-id="d986d-110">Приложение должно выполняться в среде выполнения .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d986d-110">The app must run on the .NET Framework runtime.</span></span> <span data-ttu-id="d986d-111">В *.csproj* файла, укажите соответствующие значения для [TargetFramework](/nuget/schema/target-frameworks) и [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="d986d-111">In the *.csproj* file, specify appropriate values for [TargetFramework](/nuget/schema/target-frameworks) and [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="d986d-112">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="d986d-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="d986d-113">При создании проекта в Visual Studio, используйте **основное приложение ASP.NET (.NET Framework)** шаблона.</span><span class="sxs-lookup"><span data-stu-id="d986d-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="d986d-114">Если приложение получает запросы из Интернета (не только из внутренней сети), он должен использовать [HTTP.sys](xref:fundamentals/servers/httpsys) веб-сервера (ранее называвшейся [WebListener](xref:fundamentals/servers/weblistener) для приложений ASP.NET Core 1.x) вместо [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="d986d-114">If the app receives requests from the Internet (not just from an internal network), it must use the [HTTP.sys](xref:fundamentals/servers/httpsys) web server (formerly known as [WebListener](xref:fundamentals/servers/weblistener) for ASP.NET Core 1.x apps) rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="d986d-115">IIS рекомендуется для использования в качестве обратного прокси-сервера с Kestrel для развертываний edge.</span><span class="sxs-lookup"><span data-stu-id="d986d-115">IIS is recommended for use as a reverse proxy server with Kestrel for edge deployments.</span></span> <span data-ttu-id="d986d-116">Дополнительные сведения см. в статье [Использование Kestrel с обратным прокси-сервером](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="d986d-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="d986d-117">Начало работы</span><span class="sxs-lookup"><span data-stu-id="d986d-117">Getting started</span></span>

<span data-ttu-id="d986d-118">В этом разделе описывается минимальный набор изменений, необходимых для настройки существующего проекта ASP.NET Core для запуска в службе.</span><span class="sxs-lookup"><span data-stu-id="d986d-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

1. <span data-ttu-id="d986d-119">Установка пакета NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="d986d-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="d986d-120">Внесите следующие изменения в `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="d986d-120">Make the following changes in `Program.Main`:</span></span>
  
   * <span data-ttu-id="d986d-121">Вызовите `host.RunAsService` вместо `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="d986d-121">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
   * <span data-ttu-id="d986d-122">Если код вызывает `UseContentRoot`, используйте путь в расположение публикации вместо `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="d986d-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`.</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d986d-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d986d-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d986d-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d986d-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

1. <span data-ttu-id="d986d-125">Публикация приложения в папке.</span><span class="sxs-lookup"><span data-stu-id="d986d-125">Publish the app to a folder.</span></span> <span data-ttu-id="d986d-126">Используйте [публикации dotnet](/dotnet/articles/core/tools/dotnet-publish) или [профиль публикации Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) , публикует в папку.</span><span class="sxs-lookup"><span data-stu-id="d986d-126">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

1. <span data-ttu-id="d986d-127">Тестирование путем создания и запуска службы.</span><span class="sxs-lookup"><span data-stu-id="d986d-127">Test by creating and starting the service.</span></span>

   <span data-ttu-id="d986d-128">Откройте командную строку с правами администратора для использования [sc.exe](https://technet.microsoft.com/library/bb490995) средство командной строки для создания и запуска службы.</span><span class="sxs-lookup"><span data-stu-id="d986d-128">Open a command shell with administrative privileges to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span> <span data-ttu-id="d986d-129">Если служба называется MyService, опубликованы `c:\svc`, и с именем AspNetCoreService, команды, являются:</span><span class="sxs-lookup"><span data-stu-id="d986d-129">If the service is named MyService, published to `c:\svc`, and named AspNetCoreService, the commands are:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   <span data-ttu-id="d986d-130">`binPath` Значение — это путь к исполняемому файлу приложения, включая имя исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="d986d-130">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span>

   ![Окно консоли, создание и запуск примера](windows-service/_static/create-start.png)

   <span data-ttu-id="d986d-132">После завершения этих команд, перейдите к тому же пути, что при запуске в качестве консольного приложения (по умолчанию `http://localhost:5000`):</span><span class="sxs-lookup"><span data-stu-id="d986d-132">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`):</span></span>

   ![Выполняемые в службе](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="d986d-134">Укажите способ запуска за пределами службы</span><span class="sxs-lookup"><span data-stu-id="d986d-134">Provide a way to run outside of a service</span></span>

<span data-ttu-id="d986d-135">Проще тестирования и отладки при работе вне службы, поэтому обычно добавьте код, вызывающий `RunAsService` только при определенных условиях.</span><span class="sxs-lookup"><span data-stu-id="d986d-135">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="d986d-136">Например, приложение может выполняться как консольное приложение с `--console` аргумент командной строки или при присоединении отладчика:</span><span class="sxs-lookup"><span data-stu-id="d986d-136">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d986d-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d986d-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d986d-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d986d-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="d986d-139">Остановка и запуск событий</span><span class="sxs-lookup"><span data-stu-id="d986d-139">Handle stopping and starting events</span></span>

<span data-ttu-id="d986d-140">Для обработки `OnStarting`, `OnStarted`, и `OnStopping` события, выполните следующие дополнительные изменения:</span><span class="sxs-lookup"><span data-stu-id="d986d-140">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

1. <span data-ttu-id="d986d-141">Создайте класс, производный от `WebHostService`:</span><span class="sxs-lookup"><span data-stu-id="d986d-141">Create a class that derives from `WebHostService`:</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

1. <span data-ttu-id="d986d-142">Создание метода расширения для `IWebHost` , проходящего пользовательский `WebHostService` для `ServiceBase.Run`:</span><span class="sxs-lookup"><span data-stu-id="d986d-142">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`:</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

1. <span data-ttu-id="d986d-143">В `Program.Main`, вызовите новый метод расширения, `RunAsCustomService`, а не `RunAsService`:</span><span class="sxs-lookup"><span data-stu-id="d986d-143">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of `RunAsService`:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d986d-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d986d-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d986d-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d986d-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

<span data-ttu-id="d986d-146">Если пользовательский `WebHostService` кода требует службы из внедрения зависимости (например, средства ведения журнала), получить его из `Services` свойства `IWebHost`:</span><span class="sxs-lookup"><span data-stu-id="d986d-146">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the `Services` property of `IWebHost`:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="acknowledgments"></a><span data-ttu-id="d986d-147">Подтверждения</span><span class="sxs-lookup"><span data-stu-id="d986d-147">Acknowledgments</span></span>

<span data-ttu-id="d986d-148">В этой статье было написано с помощью опубликованных источников:</span><span class="sxs-lookup"><span data-stu-id="d986d-148">This article was written with the help of published sources:</span></span>

* [<span data-ttu-id="d986d-149">Размещение ASP.NET Core в качестве службы Windows</span><span class="sxs-lookup"><span data-stu-id="d986d-149">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="d986d-150">Как разместить основные ASP.NET в службе Windows</span><span class="sxs-lookup"><span data-stu-id="d986d-150">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
