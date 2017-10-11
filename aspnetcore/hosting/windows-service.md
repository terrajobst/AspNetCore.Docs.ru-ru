---
title: "Узел в службе Windows"
author: tdykstra
description: "Узнайте, как для размещения приложения ASP.NET Core в службе Windows."
keywords: "Размещение ASP.NET Core, службы Windows"
ms.author: tdykstra
manager: wpickett
ms.date: 03/30/2017
ms.topic: article
ms.assetid: d9a65066-d7cb-47df-b046-64629c4d2c6f
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/windows-service
ms.openlocfilehash: 107436d2d49816d18d230b86636a5ee7e39610f2
ms.sourcegitcommit: 58ccf3f7d592b28eaff3534b73a45d9d190ac8c8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/06/2017
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="a25fb-104">Узел приложения ASP.NET Core в службе Windows</span><span class="sxs-lookup"><span data-stu-id="a25fb-104">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="a25fb-105">По [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a25fb-105">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="a25fb-106">Для размещения приложения ASP.NET Core в Windows, если вы не используете IIS рекомендуется выполнять [службы Windows](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="a25fb-106">The recommended way to host an ASP.NET Core app on Windows when you don't use IIS is to run it in a [Windows Service](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="a25fb-107">Таким образом она сможет автоматически начать после перезагрузки и сбои, не ожидая, пока кто-либо выполнить вход.</span><span class="sxs-lookup"><span data-stu-id="a25fb-107">That way it can automatically start after reboots and crashes, without waiting for someone to log in.</span></span>

<span data-ttu-id="a25fb-108">[Просмотреть или загрузить образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) ([загрузке](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="a25fb-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="a25fb-109">В разделе [дальнейшие действия](#next-steps) разделе инструкции о том, как запустить его.</span><span class="sxs-lookup"><span data-stu-id="a25fb-109">See the [Next Steps](#next-steps) section for instructions on how to run it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a25fb-110">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="a25fb-110">Prerequisites</span></span>

* <span data-ttu-id="a25fb-111">Приложение должно выполняться в среде выполнения .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a25fb-111">The app must run on the .NET Framework runtime.</span></span>  <span data-ttu-id="a25fb-112">В *.csproj* файла, укажите соответствующие значения для [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) и [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="a25fb-112">In the *.csproj* file, specify appropriate values for [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) and [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="a25fb-113">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="a25fb-113">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="a25fb-114">При создании проекта в Visual Studio, используйте **основное приложение ASP.NET (.NET Framework)** шаблона.</span><span class="sxs-lookup"><span data-stu-id="a25fb-114">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="a25fb-115">Если приложение будет получать запросы из Интернета (не только из внутренней сети), он должен использовать [WebListener](xref:fundamentals/servers/weblistener) веб-сервер вместо [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="a25fb-115">If the app will get requests from the internet (not just from an internal network), it must use the [WebListener](xref:fundamentals/servers/weblistener) web server rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span>  <span data-ttu-id="a25fb-116">Для развертываний edge kestrel необходимо использовать со службами IIS.</span><span class="sxs-lookup"><span data-stu-id="a25fb-116">Kestrel must be used with IIS for edge deployments.</span></span>  <span data-ttu-id="a25fb-117">Дополнительные сведения см. в статье [Использование Kestrel с обратным прокси-сервером](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="a25fb-117">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="a25fb-118">Начало работы</span><span class="sxs-lookup"><span data-stu-id="a25fb-118">Getting started</span></span>

<span data-ttu-id="a25fb-119">В этом разделе описывается минимальный набор изменений, необходимых для настройки существующего проекта ASP.NET Core для запуска в службе.</span><span class="sxs-lookup"><span data-stu-id="a25fb-119">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

* <span data-ttu-id="a25fb-120">Установка пакета NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="a25fb-120">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

* <span data-ttu-id="a25fb-121">Внесите следующие изменения в `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="a25fb-121">Make the following changes in `Program.Main`:</span></span>
  
  * <span data-ttu-id="a25fb-122">Вызовите `host.RunAsService` вместо `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="a25fb-122">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
  * <span data-ttu-id="a25fb-123">Если код вызывает `UseContentRoot`, используйте путь в расположение публикации вместо`Directory.GetCurrentDirectory()`</span><span class="sxs-lookup"><span data-stu-id="a25fb-123">If your code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`</span></span> 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* <span data-ttu-id="a25fb-124">Публикация приложения в папке.</span><span class="sxs-lookup"><span data-stu-id="a25fb-124">Publish the application to a folder.</span></span>

  <span data-ttu-id="a25fb-125">Используйте [публикации dotnet](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) или [профиль публикации Visual Studio](xref:publishing/web-publishing-vs) , публикует в папку.</span><span class="sxs-lookup"><span data-stu-id="a25fb-125">Use [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:publishing/web-publishing-vs) that publishes to a folder.</span></span>

* <span data-ttu-id="a25fb-126">Тестирование путем создания и запуска службы.</span><span class="sxs-lookup"><span data-stu-id="a25fb-126">Test by creating and starting the service.</span></span>

  <span data-ttu-id="a25fb-127">Откройте окно командной строки администратора для использования [sc.exe](https://technet.microsoft.com/library/bb490995) средство командной строки для создания и запуска службы.</span><span class="sxs-lookup"><span data-stu-id="a25fb-127">Open an administrator command prompt window to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span>  
  
  <span data-ttu-id="a25fb-128">Если имя службы MyService, публикации приложения для `c:\svc`и само приложение называется AspNetCoreService, команды будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a25fb-128">If you name your service MyService, you publish your app to `c:\svc`, and the app itself is named AspNetCoreService, the commands would look like this:</span></span>

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```
  <span data-ttu-id="a25fb-129">`binPath` Значение — это путь к исполняемому файлу приложения, включая сам имя исполняемого файла.</span><span class="sxs-lookup"><span data-stu-id="a25fb-129">The `binPath` value is the path to your app's executable, including the executable filename itself.</span></span>

  ![Окно консоли, создание и запуск примера](windows-service/_static/create-start.png)

  <span data-ttu-id="a25fb-131">После завершения этих команд, можно просмотреть по тому же пути, которое использовалось для выполнения как консольное приложение (по умолчанию `http://localhost:5000`)</span><span class="sxs-lookup"><span data-stu-id="a25fb-131">When these commands finish, you can browse to the same path as when you run as a console app (by default, `http://localhost:5000`)</span></span>

  ![Выполняемые в службе](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="a25fb-133">Укажите способ запуска за пределами службы</span><span class="sxs-lookup"><span data-stu-id="a25fb-133">Provide a way to run outside of a service</span></span>

<span data-ttu-id="a25fb-134">Это упрощает тестирование и отладку при запуске вне службы, поэтому обычно добавьте код, вызывающий `host.RunAsService` только при определенных условиях.</span><span class="sxs-lookup"><span data-stu-id="a25fb-134">It's easier to test and debug when you're running outside of a service, so it's customary to add code that calls `host.RunAsService` only under certain conditions.</span></span>  <span data-ttu-id="a25fb-135">Например, можно выполнить как консольное приложение при получении `--console` аргумент командной строки или если присоединен отладчик.</span><span class="sxs-lookup"><span data-stu-id="a25fb-135">For example, you could run as a console app if you get a `--console` command-line argument or if the debugger is attached.</span></span>

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="a25fb-136">Остановка и запуск событий</span><span class="sxs-lookup"><span data-stu-id="a25fb-136">Handle stopping and starting events</span></span>

<span data-ttu-id="a25fb-137">Если требуется обрабатывать `OnStarting`, `OnStarted`, и `OnStopping` события, выполните следующие дополнительные изменения:</span><span class="sxs-lookup"><span data-stu-id="a25fb-137">If you want to handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

* <span data-ttu-id="a25fb-138">Создайте класс, наследующий от класса `WebHostService`.</span><span class="sxs-lookup"><span data-stu-id="a25fb-138">Create a class that derives from `WebHostService`.</span></span>

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* <span data-ttu-id="a25fb-139">Создание метода расширения для `IWebHost` , передает пользовательскую `WebHostService` для `ServiceBase.Run`.</span><span class="sxs-lookup"><span data-stu-id="a25fb-139">Create an extension method for `IWebHost` that passes your custom `WebHostService` to `ServiceBase.Run`.</span></span>

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* <span data-ttu-id="a25fb-140">В `Program.Main` изменений вызывал новый метод расширения, вместо `host.RunAsService`.</span><span class="sxs-lookup"><span data-stu-id="a25fb-140">In `Program.Main` change call the new extension method instead of `host.RunAsService`.</span></span>

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

<span data-ttu-id="a25fb-141">Если пользовательский `WebHostService` коду требуется доступ к службе из внедрения зависимости (например, средства ведения журнала), можно получить его из `Services` свойство `IWebHost`.</span><span class="sxs-lookup"><span data-stu-id="a25fb-141">If your custom `WebHostService` code needs to get a service from dependency injection (such as a logger), you can get it from the `Services` property of `IWebHost`.</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a><span data-ttu-id="a25fb-142">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="a25fb-142">Next steps</span></span>

<span data-ttu-id="a25fb-143">[Образец приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) , сопровождающий это статья является простой веб-приложения MVC, который был изменен, как показано в предыдущих примерах кода.</span><span class="sxs-lookup"><span data-stu-id="a25fb-143">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) that accompanies this article is a simple MVC web app that has been modified as shown in preceding code examples.</span></span>  <span data-ttu-id="a25fb-144">Чтобы запустить его в службе, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="a25fb-144">To run it in a service, do the following steps:</span></span>

* <span data-ttu-id="a25fb-145">Публикация в *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="a25fb-145">Publish to *c:\svc*.</span></span>

* <span data-ttu-id="a25fb-146">Откройте окно администратора.</span><span class="sxs-lookup"><span data-stu-id="a25fb-146">Open an administrator window.</span></span>

* <span data-ttu-id="a25fb-147">Введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="a25fb-147">Enter the following commands:</span></span>

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * <span data-ttu-id="a25fb-148">В браузере перейдите к http://localhost: 5000, чтобы убедиться, что он работает.</span><span class="sxs-lookup"><span data-stu-id="a25fb-148">In a browser, go to http://localhost:5000 to verify that it's running.</span></span>

<span data-ttu-id="a25fb-149">Если приложение не запускается до должным образом при запуске в службе, быстрый способ сделать доступной сообщения об ошибках является для добавления поставщика ведения журнала, такие как [поставщик журнала событий Windows](xref:fundamentals/logging#eventlog).</span><span class="sxs-lookup"><span data-stu-id="a25fb-149">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider such as the [Windows EventLog provider](xref:fundamentals/logging#eventlog).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="a25fb-150">Подтверждения</span><span class="sxs-lookup"><span data-stu-id="a25fb-150">Acknowledgments</span></span>

<span data-ttu-id="a25fb-151">В этой статье был записан с помощью источников, которые уже были опубликованы.</span><span class="sxs-lookup"><span data-stu-id="a25fb-151">This article was written with the help of sources that were already published.</span></span> <span data-ttu-id="a25fb-152">Минимальное значение и наиболее полезные из них были их:</span><span class="sxs-lookup"><span data-stu-id="a25fb-152">The earliest and most useful of them were these:</span></span>

* [<span data-ttu-id="a25fb-153">Размещение ASP.NET Core в качестве службы Windows</span><span class="sxs-lookup"><span data-stu-id="a25fb-153">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="a25fb-154">Как разместить основные ASP.NET в службе Windows</span><span class="sxs-lookup"><span data-stu-id="a25fb-154">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
