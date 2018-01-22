---
title: "Реализация WebListener веб-сервера в ASP.NET Core"
author: rick-anderson
description: "Вводит WebListener веб-сервер для ASP.NET Core в Windows. Основанное на драйвер режима ядра Http.Sys, WebListener является альтернативой Kestrel, который можно использовать для прямого подключения к Интернету, без IIS."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/weblistener
ms.openlocfilehash: f1bdbc723e4602c2e53723aff91ec5d254f4bd93
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="fa86c-104">Реализация WebListener веб-сервера в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fa86c-104">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="fa86c-105">По [Tom Dykstra](https://github.com/tdykstra) и [Ross Крис](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="fa86c-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="fa86c-106">Этот раздел относится только к ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="fa86c-106">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="fa86c-107">В ASP.NET Core 2.0, называется WebListener [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="fa86c-107">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="fa86c-108">— WebListener [веб-сервер для ASP.NET Core](index.md) , которое будет выполняться только в Windows.</span><span class="sxs-lookup"><span data-stu-id="fa86c-108">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="fa86c-109">Она была основана на [драйвер режима ядра Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="fa86c-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="fa86c-110">WebListener является альтернативой [Kestrel](kestrel.md) , можно использовать для прямого подключения к Интернету без использования IIS в качестве обратного прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="fa86c-110">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="fa86c-111">На самом деле **WebListener не может использоваться с IIS или IIS Express, как оно не совместимо с [модуль ASP.NET Core](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="fa86c-111">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="fa86c-112">Несмотря на то, что WebListener был разработан для ASP.NET Core, он может использоваться непосредственно в любом приложении .NET Core или .NET Framework, через [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="fa86c-112">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="fa86c-113">WebListener поддерживает следующие функции:</span><span class="sxs-lookup"><span data-stu-id="fa86c-113">WebListener supports the following features:</span></span>

- [<span data-ttu-id="fa86c-114">Проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="fa86c-114">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="fa86c-115">Совместное использование порта</span><span class="sxs-lookup"><span data-stu-id="fa86c-115">Port sharing</span></span>
- <span data-ttu-id="fa86c-116">HTTPS с сетевого Адаптера</span><span class="sxs-lookup"><span data-stu-id="fa86c-116">HTTPS with SNI</span></span>
- <span data-ttu-id="fa86c-117">HTTP/2 через TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="fa86c-117">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="fa86c-118">Файл прямой передачи данных</span><span class="sxs-lookup"><span data-stu-id="fa86c-118">Direct file transmission</span></span>
- <span data-ttu-id="fa86c-119">Кэширование ответов</span><span class="sxs-lookup"><span data-stu-id="fa86c-119">Response caching</span></span>
- <span data-ttu-id="fa86c-120">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="fa86c-120">WebSockets (Windows 8)</span></span>

<span data-ttu-id="fa86c-121">Поддерживаемые версии Windows:</span><span class="sxs-lookup"><span data-stu-id="fa86c-121">Supported Windows versions:</span></span>

- <span data-ttu-id="fa86c-122">Windows 7 и Windows Server 2008 R2 и более поздних версий</span><span class="sxs-lookup"><span data-stu-id="fa86c-122">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="fa86c-123">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fa86c-123">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="fa86c-124">Когда следует использовать WebListener</span><span class="sxs-lookup"><span data-stu-id="fa86c-124">When to use WebListener</span></span>

<span data-ttu-id="fa86c-125">WebListener полезен для развертываний, где нужно предоставить сервера к Интернету напрямую без использования служб IIS.</span><span class="sxs-lookup"><span data-stu-id="fa86c-125">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![Weblistener взаимодействует с Интернетом напрямую.](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="fa86c-127">Так как она была основана на Http.Sys, WebListener не требует обратного прокси-сервера для защиты от атак на систему.</span><span class="sxs-lookup"><span data-stu-id="fa86c-127">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="fa86c-128">Компонент Http.Sys — это надежная технология, которое защищает от атак многих типов, а также обеспечивает надежности, безопасности и масштабируемости полнофункциональное веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="fa86c-128">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="fa86c-129">Сами службы IIS выполняется как HTTP-прослушивателем на базе Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="fa86c-129">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="fa86c-130">WebListener при также хорошо подходит для развертываний внутренней требуется одна из возможностей, которой он обеспечивает, что не удается получить с помощью Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fa86c-130">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![Weblistener взаимодействует с вашей внутренней сетью напрямую.](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="fa86c-132">Как использовать WebListener</span><span class="sxs-lookup"><span data-stu-id="fa86c-132">How to use WebListener</span></span>

<span data-ttu-id="fa86c-133">Ниже приведен обзор задач установки для ОС и приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fa86c-133">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="fa86c-134">Настройка Windows Server</span><span class="sxs-lookup"><span data-stu-id="fa86c-134">Configure Windows Server</span></span>

* <span data-ttu-id="fa86c-135">Установите версию .NET с требованиями приложения, такие как [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) или .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="fa86c-135">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="fa86c-136">Предварительной регистрации префиксы URL-адрес, привязку к WebListener и настроить сертификаты SSL</span><span class="sxs-lookup"><span data-stu-id="fa86c-136">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="fa86c-137">Если не предварительной регистрации URL-префиксы в Windows, необходимо запустить приложение с правами администратора.</span><span class="sxs-lookup"><span data-stu-id="fa86c-137">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="fa86c-138">Единственное исключение — если привязать к локальному компьютеру с помощью HTTP (не HTTPS) с номерами выше 1024; номер порта в этом случае правами администратора, не являются обязательными.</span><span class="sxs-lookup"><span data-stu-id="fa86c-138">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="fa86c-139">Дополнительные сведения см. в разделе [предварительной регистрации префиксов и настройке SSL](#preregister-url-prefixes-and-configure-ssl) далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="fa86c-139">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="fa86c-140">Открыть порты брандмауэра, разрешающее трафик для достижения WebListener.</span><span class="sxs-lookup"><span data-stu-id="fa86c-140">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="fa86c-141">Можно использовать netsh.exe или [командлеты PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="fa86c-141">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="fa86c-142">Существуют также [параметры реестра Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="fa86c-142">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="fa86c-143">Настройка приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fa86c-143">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="fa86c-144">Установка пакета NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span><span class="sxs-lookup"><span data-stu-id="fa86c-144">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="fa86c-145">Также устанавливает [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) как зависимость.</span><span class="sxs-lookup"><span data-stu-id="fa86c-145">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="fa86c-146">Вызовите `UseWebListener` метод расширения в [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) в вашей `Main` метод, указывая любой WebListener [параметры](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) и [параметры](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) нужно , как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="fa86c-146">Call the `UseWebListener` extension method on [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="fa86c-147">Настройка URL-адреса и порты прослушивания</span><span class="sxs-lookup"><span data-stu-id="fa86c-147">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="fa86c-148">По умолчанию ASP.NET Core привязывается к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="fa86c-148">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="fa86c-149">Чтобы настроить префиксы URL-адреса и порты, можно использовать `UseURLs` метод расширения `urls` аргумент командной строки или система конфигурации ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fa86c-149">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="fa86c-150">Дополнительные сведения см. в разделе [Размещение](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="fa86c-150">For more information, see [Hosting](../../fundamentals/hosting.md).</span></span>

  <span data-ttu-id="fa86c-151">Веб-прослушиватель использует [Http.Sys префикс строковые форматы](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="fa86c-151">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="fa86c-152">Требования к формат строки префикса, относящиеся к WebListener отсутствуют.</span><span class="sxs-lookup"><span data-stu-id="fa86c-152">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!NOTE]
  > <span data-ttu-id="fa86c-153">Убедитесь, что указан одинаковые строки префикса в `UseUrls` , вы предварительной регистрации на сервере.</span><span class="sxs-lookup"><span data-stu-id="fa86c-153">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="fa86c-154">Убедитесь, что приложение не настроено для запуска служб IIS или IIS Express.</span><span class="sxs-lookup"><span data-stu-id="fa86c-154">Make sure your application is not configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="fa86c-155">В Visual Studio для IIS Express указан профиль запуска по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fa86c-155">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="fa86c-156">Чтобы запустить проект как консольное приложение необходимо вручную изменить выбранного профиля, как показано на следующем снимке экрана.</span><span class="sxs-lookup"><span data-stu-id="fa86c-156">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Выберите профиль приложения консоли](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="fa86c-158">Как использовать WebListener за пределами ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fa86c-158">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="fa86c-159">Установка [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="fa86c-159">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="fa86c-160">[Префиксы URL-адрес, привязку к WebListener и настроить сертификаты SSL предварительной регистрации](#preregister-url-prefixes-and-configure-ssl) как для использования в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fa86c-160">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="fa86c-161">Существуют также [параметры реестра Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="fa86c-161">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="fa86c-162">Ниже приведен пример кода, демонстрирующий применение WebListener за пределами ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="fa86c-162">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

```csharp
var settings = new WebListenerSettings();
settings.UrlPrefixes.Add("http://localhost:8080");

using (WebListener listener = new WebListener(settings))
{
    listener.Start();

    while (true)
    {
        var context = await listener.AcceptAsync();
        byte[] bytes = Encoding.ASCII.GetBytes("Hello World: " + DateTime.Now);
        context.Response.ContentLength = bytes.Length;
        context.Response.ContentType = "text/plain";

        await context.Response.Body.WriteAsync(bytes, 0, bytes.Length);
        context.Dispose();
    }
}
```

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="fa86c-163">Предварительной регистрации префиксы URL-адреса и настроить протокол SSL</span><span class="sxs-lookup"><span data-stu-id="fa86c-163">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="fa86c-164">Службы IIS и WebListener основаны на базовый драйвер режима ядра Http.Sys для прослушивания запросов и первоначального обработки.</span><span class="sxs-lookup"><span data-stu-id="fa86c-164">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="fa86c-165">В службах IIS пользовательский Интерфейс управления позволяет сравнительно легко настраивать все.</span><span class="sxs-lookup"><span data-stu-id="fa86c-165">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="fa86c-166">Если вы используете WebListener необходимо настроить Http.Sys самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="fa86c-166">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="fa86c-167">Встроенное средство выполнения этих процедур является netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="fa86c-167">The built-in tool for doing that is netsh.exe.</span></span> 

<span data-ttu-id="fa86c-168">Наиболее распространенные задачи, необходимо использовать netsh.exe для резервирования URL-префиксы и назначение SSL-сертификаты.</span><span class="sxs-lookup"><span data-stu-id="fa86c-168">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="fa86c-169">NetSh.exe не удобное средство для начинающих.</span><span class="sxs-lookup"><span data-stu-id="fa86c-169">NetSh.exe is not an easy tool to use for beginners.</span></span> <span data-ttu-id="fa86c-170">Ниже приведен пример минимальный набор необходимых для резервирования URL-префиксы для портов 80 и 443.</span><span class="sxs-lookup"><span data-stu-id="fa86c-170">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="fa86c-171">Приведенный ниже показано, как назначить SSL-сертификата:</span><span class="sxs-lookup"><span data-stu-id="fa86c-171">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="fa86c-172">Ниже приведен в официальной документации.</span><span class="sxs-lookup"><span data-stu-id="fa86c-172">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="fa86c-173">Команды Netsh для гипертекста передачи протокол (HTTP)</span><span class="sxs-lookup"><span data-stu-id="fa86c-173">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="fa86c-174">UrlPrefix строк</span><span class="sxs-lookup"><span data-stu-id="fa86c-174">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="fa86c-175">Подробные инструкции для нескольких сценариев на следующих ресурсах.</span><span class="sxs-lookup"><span data-stu-id="fa86c-175">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="fa86c-176">Статьи, которые ссылаются на `HttpListener` применяются как к `WebListener`, как они строятся на Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="fa86c-176">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="fa86c-177">Практическое руководство. Настройка порта с использованием SSL-сертификата</span><span class="sxs-lookup"><span data-stu-id="fa86c-177">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="fa86c-178">[Подключения по протоколу HTTPS - HttpListener и на основе размещения сертификацию клиента](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) это является блог сторонних разработчиков и довольно старый, но по-прежнему содержит полезные сведения.</span><span class="sxs-lookup"><span data-stu-id="fa86c-178">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="fa86c-179">[Практическое руководство: Пошаговое руководство с помощью HttpListener или HTTP-сервере неуправляемого типов кода (C++) как простой сервера SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) это тоже старые блог полезные сведения.</span><span class="sxs-lookup"><span data-stu-id="fa86c-179">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="fa86c-180">Как настроить WebListener Core .NET с помощью протокола SSL?</span><span class="sxs-lookup"><span data-stu-id="fa86c-180">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="fa86c-181">Ниже приведены некоторые сторонних средств, которые могут быть проще в использовании, чем netsh.exe командной строки.</span><span class="sxs-lookup"><span data-stu-id="fa86c-181">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="fa86c-182">Они не предоставляется или не поддерживается корпорацией Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="fa86c-182">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="fa86c-183">Средства Запуск от имени администратора по умолчанию, поскольку сам netsh.exe требуются права администратора.</span><span class="sxs-lookup"><span data-stu-id="fa86c-183">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="fa86c-184">[HTTP.sys Manager](http://httpsysmanager.codeplex.com/) предоставляет пользовательский Интерфейс для включения в список и Настройка SSL-сертификатов и параметров, префиксов резервирования и списки доверия сертификатов.</span><span class="sxs-lookup"><span data-stu-id="fa86c-184">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="fa86c-185">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) позволяет перечислить или настроить сертификаты SSL и префиксы URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="fa86c-185">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="fa86c-186">Пользовательский Интерфейс будет более http.sys Manager предоставляет несколько дополнительных параметров конфигурации, а в противном случае она предоставляет аналогичные функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="fa86c-186">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="fa86c-187">Не удается создать новый список доверия сертификатов (CTL), но можно назначить уже существующую.</span><span class="sxs-lookup"><span data-stu-id="fa86c-187">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="fa86c-188">Для создания самозаверяющего SSL-сертификаты, корпорация Майкрософт предоставляет средства командной строки: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) и командлет PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="fa86c-188">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="fa86c-189">Существуют также сторонние пользовательского интерфейса средства, облегчающие создавать самозаверяющие сертификаты SSL:</span><span class="sxs-lookup"><span data-stu-id="fa86c-189">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="fa86c-190">SelfCert</span><span class="sxs-lookup"><span data-stu-id="fa86c-190">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="fa86c-191">Makecert пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="fa86c-191">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="fa86c-192">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="fa86c-192">Next steps</span></span>

<span data-ttu-id="fa86c-193">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="fa86c-193">For more information, see the following resources:</span></span>

* [<span data-ttu-id="fa86c-194">Образец приложения для этой статьи</span><span class="sxs-lookup"><span data-stu-id="fa86c-194">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="fa86c-195">WebListener исходного кода</span><span class="sxs-lookup"><span data-stu-id="fa86c-195">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="fa86c-196">Размещение</span><span class="sxs-lookup"><span data-stu-id="fa86c-196">Hosting</span></span>](../hosting.md)
