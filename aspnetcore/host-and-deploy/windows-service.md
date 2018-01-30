---
title: "Узел в службе Windows"
author: tdykstra
description: "Узнайте, как для размещения приложения ASP.NET Core в службе Windows."
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/30/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: beda34dbd613f6ffe0afa207ab57dd6ebbc489ee
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="af247-103">Узел приложения ASP.NET Core в службе Windows</span><span class="sxs-lookup"><span data-stu-id="af247-103">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="af247-104">По [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="af247-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="af247-105">Рекомендуется размещать приложения ASP.NET Core в Windows без с помощью служб IIS для запуска в [службы Windows](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="af247-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="af247-106">Таким образом она сможет автоматически начать после перезагрузки и сбои, не ожидая, пока кто-либо выполнить вход.</span><span class="sxs-lookup"><span data-stu-id="af247-106">That way it can automatically start after reboots and crashes, without waiting for someone to log in.</span></span>

<span data-ttu-id="af247-107">[Просмотреть или загрузить образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([загрузке](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="af247-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="af247-108">В разделе [дальнейшие действия](#next-steps) разделе инструкции о том, как запустить его.</span><span class="sxs-lookup"><span data-stu-id="af247-108">See the [Next Steps](#next-steps) section for instructions on how to run it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="af247-109">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="af247-109">Prerequisites</span></span>

* <span data-ttu-id="af247-110">Приложение должно выполняться в среде выполнения .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="af247-110">The app must run on the .NET Framework runtime.</span></span>  <span data-ttu-id="af247-111">В *.csproj* файла, укажите соответствующие значения для [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) и [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="af247-111">In the *.csproj* file, specify appropriate values for [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) and [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="af247-112">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="af247-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="af247-113">При создании проекта в Visual Studio, используйте **основное приложение ASP.NET (.NET Framework)** шаблона.</span><span class="sxs-lookup"><span data-stu-id="af247-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="af247-114">Если приложение получает запросы из Интернета (не только из внутренней сети), он должен использовать [WebListener](xref:fundamentals/servers/weblistener) веб-сервер вместо [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="af247-114">If the app receives requests from the Internet (not just from an internal network), it must use the [WebListener](xref:fundamentals/servers/weblistener) web server rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span>  <span data-ttu-id="af247-115">Для развертываний edge kestrel необходимо использовать со службами IIS.</span><span class="sxs-lookup"><span data-stu-id="af247-115">Kestrel must be used with IIS for edge deployments.</span></span>  <span data-ttu-id="af247-116">Дополнительные сведения см. в статье [Использование Kestrel с обратным прокси-сервером](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="af247-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="af247-117">Начало работы</span><span class="sxs-lookup"><span data-stu-id="af247-117">Getting started</span></span>

<span data-ttu-id="af247-118">В этом разделе описывается минимальный набор изменений, необходимых для настройки существующего проекта ASP.NET Core для запуска в службе.</span><span class="sxs-lookup"><span data-stu-id="af247-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

* <span data-ttu-id="af247-119">Установка пакета NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="af247-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

* <span data-ttu-id="af247-120">Внесите следующие изменения в `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="af247-120">Make the following changes in `Program.Main`:</span></span>
  
  * <span data-ttu-id="af247-121">Вызовите `host.RunAsService` вместо `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="af247-121">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
  * <span data-ttu-id="af247-122">Если код вызывает `UseContentRoot`, используйте путь в расположение публикации вместо`Directory.GetCurrentDirectory()`</span><span class="sxs-lookup"><span data-stu-id="af247-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`</span></span> 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* <span data-ttu-id="af247-123">Публикация приложения в папке.</span><span class="sxs-lookup"><span data-stu-id="af247-123">Publish the application to a folder.</span></span>

  <span data-ttu-id="af247-124">Используйте [публикации dotnet](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) или [профиль публикации Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) , публикует в папку.</span><span class="sxs-lookup"><span data-stu-id="af247-124">Use [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

* <span data-ttu-id="af247-125">Тестирование путем создания и запуска службы.</span><span class="sxs-lookup"><span data-stu-id="af247-125">Test by creating and starting the service.</span></span>

  <span data-ttu-id="af247-126">Откройте окно командной строки администратора для использования [sc.exe](https://technet.microsoft.com/library/bb490995) средство командной строки для создания и запуска службы.</span><span class="sxs-lookup"><span data-stu-id="af247-126">Open an administrator command prompt window to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span>  
  
  <span data-ttu-id="af247-127">Если служба называется MyService, опубликуйте приложение в `c:\svc`и само приложение называется AspNetCoreService, команды будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="af247-127">If the service is named MyService, publish the app to `c:\svc`, and the app itself is named AspNetCoreService, the commands would look like this:</span></span>

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```

  <span data-ttu-id="af247-128">`binPath` Значение — это путь к исполняемому файлу приложения, включая сам имя исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="af247-128">The `binPath` value is the path to the app's executable, including the executable filename itself.</span></span>

  ![Окно консоли, создание и запуск примера](windows-service/_static/create-start.png)

  <span data-ttu-id="af247-130">После завершения этих команд, перейдите к тому же пути, что при запуске в качестве консольного приложения (по умолчанию `http://localhost:5000`)</span><span class="sxs-lookup"><span data-stu-id="af247-130">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`)</span></span>

  ![Выполняемые в службе](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="af247-132">Укажите способ запуска за пределами службы</span><span class="sxs-lookup"><span data-stu-id="af247-132">Provide a way to run outside of a service</span></span>

<span data-ttu-id="af247-133">Проще тестирования и отладки при работе вне службы, поэтому обычно добавьте код, вызывающий `host.RunAsService` только при определенных условиях.</span><span class="sxs-lookup"><span data-stu-id="af247-133">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `host.RunAsService` only under certain conditions.</span></span>  <span data-ttu-id="af247-134">Например, приложение может выполняться как консольное приложение с `--console` аргумент командной строки или если присоединен отладчик.</span><span class="sxs-lookup"><span data-stu-id="af247-134">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached.</span></span>

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="af247-135">Остановка и запуск событий</span><span class="sxs-lookup"><span data-stu-id="af247-135">Handle stopping and starting events</span></span>

<span data-ttu-id="af247-136">Для обработки `OnStarting`, `OnStarted`, и `OnStopping` события, выполните следующие дополнительные изменения:</span><span class="sxs-lookup"><span data-stu-id="af247-136">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

* <span data-ttu-id="af247-137">Создайте класс, наследующий от класса `WebHostService`.</span><span class="sxs-lookup"><span data-stu-id="af247-137">Create a class that derives from `WebHostService`.</span></span>

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* <span data-ttu-id="af247-138">Создание метода расширения для `IWebHost` , проходящего пользовательский `WebHostService` для `ServiceBase.Run`.</span><span class="sxs-lookup"><span data-stu-id="af247-138">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`.</span></span>

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* <span data-ttu-id="af247-139">В `Program.Main` изменений вызывал новый метод расширения, вместо `host.RunAsService`.</span><span class="sxs-lookup"><span data-stu-id="af247-139">In `Program.Main` change call the new extension method instead of `host.RunAsService`.</span></span>

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

<span data-ttu-id="af247-140">Если пользовательский `WebHostService` коду требуется доступ к службе из внедрения зависимости (например, средства ведения журнала), получить его из `Services` свойство `IWebHost`.</span><span class="sxs-lookup"><span data-stu-id="af247-140">If the custom `WebHostService` code needs to get a service from dependency injection (such as a logger), get it from the `Services` property of `IWebHost`.</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a><span data-ttu-id="af247-141">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="af247-141">Next steps</span></span>

<span data-ttu-id="af247-142">[Образец приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) , сопровождающий это статья является простой веб-приложения MVC, который был изменен, как показано в предыдущих примерах кода.</span><span class="sxs-lookup"><span data-stu-id="af247-142">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) that accompanies this article is a simple MVC web app that has been modified as shown in preceding code examples.</span></span>  <span data-ttu-id="af247-143">Чтобы запустить его в службе, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="af247-143">To run it in a service, do the following steps:</span></span>

* <span data-ttu-id="af247-144">Публикация в *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="af247-144">Publish to *c:\svc*.</span></span>

* <span data-ttu-id="af247-145">Откройте окно администратора.</span><span class="sxs-lookup"><span data-stu-id="af247-145">Open an administrator window.</span></span>

* <span data-ttu-id="af247-146">Введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="af247-146">Enter the following commands:</span></span>

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * <span data-ttu-id="af247-147">В браузере перейдите к http://localhost: 5000, чтобы убедиться, что он работает.</span><span class="sxs-lookup"><span data-stu-id="af247-147">In a browser, go to http://localhost:5000 to verify that it's running.</span></span>

<span data-ttu-id="af247-148">Если приложение не запускается до должным образом при запуске в службе, быстрый способ сделать доступной сообщения об ошибках является для добавления поставщика ведения журнала, такие как [поставщик журнала событий Windows](xref:fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="af247-148">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider such as the [Windows EventLog provider](xref:fundamentals/logging/index#eventlog).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="af247-149">Подтверждения</span><span class="sxs-lookup"><span data-stu-id="af247-149">Acknowledgments</span></span>

<span data-ttu-id="af247-150">В этой статье был записан с помощью источников, которые уже были опубликованы.</span><span class="sxs-lookup"><span data-stu-id="af247-150">This article was written with the help of sources that were already published.</span></span> <span data-ttu-id="af247-151">Минимальное значение и наиболее полезные из них были их:</span><span class="sxs-lookup"><span data-stu-id="af247-151">The earliest and most useful of them were these:</span></span>

* [<span data-ttu-id="af247-152">Размещение ASP.NET Core в качестве службы Windows</span><span class="sxs-lookup"><span data-stu-id="af247-152">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="af247-153">Как разместить основные ASP.NET в службе Windows</span><span class="sxs-lookup"><span data-stu-id="af247-153">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
