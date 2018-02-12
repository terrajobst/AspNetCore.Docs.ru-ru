---
title: "Реализации веб-сервера WebListener в ASP.NET Core"
author: rick-anderson
description: "Общие сведения о веб-сервере WebListener для ASP.NET Core в Windows. WebListener, основанный на работающем в режиме ядра драйвере Http.Sys, является альтернативой для Kestrel, которую можно использовать для прямого подключения к Интернету без служб IIS."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/weblistener
ms.openlocfilehash: fb2e0621645a48f4e603d754d8babbc07a78cae4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="7acdd-104">Реализации веб-сервера WebListener в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7acdd-104">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="7acdd-105">Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra) и [Крис Росс](https://github.com/Tratcher) (Chris Ross)</span><span class="sxs-lookup"><span data-stu-id="7acdd-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="7acdd-106">Сведения из этого раздела распространяются только на ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="7acdd-106">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="7acdd-107">В ASP.NET Core 2.0 WebListener называется [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="7acdd-107">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="7acdd-108">WebListener — это [веб-сервер для ASP.NET Core](index.md), который запускается только в Windows.</span><span class="sxs-lookup"><span data-stu-id="7acdd-108">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="7acdd-109">Он основан на [работающем в режиме ядра драйвере Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="7acdd-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="7acdd-110">WebListener является альтернативой для [Kestrel](kestrel.md), которую можно использовать для прямого подключения к Интернету, не используя IIS в качестве обратного прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="7acdd-110">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="7acdd-111">Фактически **WebListener не подходит для использования с IIS или IIS Express, так как он не совместим с [модулем ASP.NET Core](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="7acdd-111">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="7acdd-112">Хотя WebListener был разработан для ASP.NET Core, его невозможно использовать напрямую в каком-либо приложении .NET Core или .NET Framework через пакет NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).</span><span class="sxs-lookup"><span data-stu-id="7acdd-112">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="7acdd-113">WebListener поддерживает следующие функции:</span><span class="sxs-lookup"><span data-stu-id="7acdd-113">WebListener supports the following features:</span></span>

- [<span data-ttu-id="7acdd-114">Проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="7acdd-114">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="7acdd-115">Совместное использование портов</span><span class="sxs-lookup"><span data-stu-id="7acdd-115">Port sharing</span></span>
- <span data-ttu-id="7acdd-116">HTTPS с SNI</span><span class="sxs-lookup"><span data-stu-id="7acdd-116">HTTPS with SNI</span></span>
- <span data-ttu-id="7acdd-117">HTTP/2 через TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="7acdd-117">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="7acdd-118">Прямая передача файлов</span><span class="sxs-lookup"><span data-stu-id="7acdd-118">Direct file transmission</span></span>
- <span data-ttu-id="7acdd-119">Кэширование откликов</span><span class="sxs-lookup"><span data-stu-id="7acdd-119">Response caching</span></span>
- <span data-ttu-id="7acdd-120">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="7acdd-120">WebSockets (Windows 8)</span></span>

<span data-ttu-id="7acdd-121">Поддерживаемые версии Windows:</span><span class="sxs-lookup"><span data-stu-id="7acdd-121">Supported Windows versions:</span></span>

- <span data-ttu-id="7acdd-122">Windows 7 и Windows Server 2008 R2 и более поздних версий</span><span class="sxs-lookup"><span data-stu-id="7acdd-122">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="7acdd-123">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7acdd-123">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="7acdd-124">Условия использования WebListener</span><span class="sxs-lookup"><span data-stu-id="7acdd-124">When to use WebListener</span></span>

<span data-ttu-id="7acdd-125">WebListener удобен для развертываний, где нужно подключить сервер к Интернету напрямую без использования служб IIS.</span><span class="sxs-lookup"><span data-stu-id="7acdd-125">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![Weblistener взаимодействует с Интернетом напрямую.](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="7acdd-127">Так как WebListener основан на Http.Sys, ему не нужен обратный прокси-сервер для защиты от атак.</span><span class="sxs-lookup"><span data-stu-id="7acdd-127">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="7acdd-128">Http.Sys — это проверенная технология, которая защищает от многих типов атак, а также обеспечивает надежность, безопасность и масштабируемость полнофункционального веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="7acdd-128">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="7acdd-129">Сами службы IIS выполняются в качестве HTTP-прослушивателя поверх Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="7acdd-129">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="7acdd-130">WebListener также хорошо подходит для внутренних развертываний, когда нужна функция, отсутствующая в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="7acdd-130">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![Weblistener взаимодействует с вашей внутренней сетью напрямую.](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="7acdd-132">Способы использования WebListener</span><span class="sxs-lookup"><span data-stu-id="7acdd-132">How to use WebListener</span></span>

<span data-ttu-id="7acdd-133">Ниже приведен обзор задач установки для ОС узла и приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7acdd-133">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="7acdd-134">Настройка Windows Server</span><span class="sxs-lookup"><span data-stu-id="7acdd-134">Configure Windows Server</span></span>

* <span data-ttu-id="7acdd-135">Установите нужную вашему приложению версию платформы .NET, например [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) или .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="7acdd-135">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="7acdd-136">Предварительно зарегистрируйте префиксы URL-адресов для привязки к WebListener и настройте SSL-сертификаты.</span><span class="sxs-lookup"><span data-stu-id="7acdd-136">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="7acdd-137">Если вы не выполняете предварительную регистрацию префиксов URL-адресов в Windows, приложение потребуется запустить с правами администратора.</span><span class="sxs-lookup"><span data-stu-id="7acdd-137">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="7acdd-138">Единственным исключением является привязка к localhost через HTTP (не HTTPS) с номером порта больше 1024. В этом случае права администратора не требуются.</span><span class="sxs-lookup"><span data-stu-id="7acdd-138">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="7acdd-139">Дополнительные сведения см. в разделе [Предварительная регистрация префиксов и настройка SSL](#preregister-url-prefixes-and-configure-ssl) ниже.</span><span class="sxs-lookup"><span data-stu-id="7acdd-139">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="7acdd-140">Откройте порты брандмауэра, чтобы разрешить трафик в WebListener.</span><span class="sxs-lookup"><span data-stu-id="7acdd-140">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="7acdd-141">Можно использовать netsh.exe или [командлеты PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="7acdd-141">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="7acdd-142">Кроме того, существуют [параметры реестра Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="7acdd-142">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="7acdd-143">Настройка приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7acdd-143">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="7acdd-144">Установите пакет NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span><span class="sxs-lookup"><span data-stu-id="7acdd-144">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="7acdd-145">Он также устанавливает [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) в качестве зависимости.</span><span class="sxs-lookup"><span data-stu-id="7acdd-145">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="7acdd-146">Вызовите метод расширения `UseWebListener` для [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) в вашем методе `Main`, указав [параметры](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) WebListener и другие нужные [параметры](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs), как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="7acdd-146">Call the `UseWebListener` extension method on [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="7acdd-147">Настройка URL-адресов и портов для прослушивания</span><span class="sxs-lookup"><span data-stu-id="7acdd-147">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="7acdd-148">По умолчанию ASP.NET Core привязан к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="7acdd-148">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="7acdd-149">Чтобы настроить префиксы URL-адресов и порты, можно использовать метод расширения `UseURLs`, аргумент командной строки `urls` или систему конфигурации ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7acdd-149">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="7acdd-150">Дополнительные сведения см. в разделе [Размещение](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="7acdd-150">For more information, see [Hosting](../../fundamentals/hosting.md).</span></span>

  <span data-ttu-id="7acdd-151">WebListener использует [форматы строк префикса Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="7acdd-151">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="7acdd-152">Для WebListener не предъявляются отдельные требования к формату строк префикса.</span><span class="sxs-lookup"><span data-stu-id="7acdd-152">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!NOTE]
  > <span data-ttu-id="7acdd-153">Убедитесь, что указали одинаковые строки префикса в `UseUrls`, предварительно регистрируемого на сервере.</span><span class="sxs-lookup"><span data-stu-id="7acdd-153">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="7acdd-154">Убедитесь, что приложение не настроено для запуска IIS или IIS Express.</span><span class="sxs-lookup"><span data-stu-id="7acdd-154">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="7acdd-155">В Visual Studio профиль запуска по умолчанию использует IIS Express.</span><span class="sxs-lookup"><span data-stu-id="7acdd-155">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="7acdd-156">Чтобы запустить проект как консольное приложение, вручную измените выбранный профиль, как показано на следующем снимке экрана.</span><span class="sxs-lookup"><span data-stu-id="7acdd-156">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Выбор профиля консольного приложения](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="7acdd-158">Использование WebListener за пределами ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7acdd-158">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="7acdd-159">Установите пакет NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).</span><span class="sxs-lookup"><span data-stu-id="7acdd-159">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="7acdd-160">[Предварительно зарегистрируйте префиксы URL-адресов для привязки к WebListener и настройте SSL-сертификаты](#preregister-url-prefixes-and-configure-ssl), как и при использовании в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7acdd-160">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="7acdd-161">Кроме того, существуют [параметры реестра Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="7acdd-161">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="7acdd-162">Ниже приведен пример кода, демонстрирующий применение WebListener за пределами ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="7acdd-162">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="7acdd-163">Предварительная регистрация префиксов URL-адресов и настройка SSL</span><span class="sxs-lookup"><span data-stu-id="7acdd-163">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="7acdd-164">Как IIS, так и WebListener основаны на работающем в режиме ядра драйвере Http.Sys, который осуществляет прослушивание запросов и первоначальную обработку.</span><span class="sxs-lookup"><span data-stu-id="7acdd-164">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="7acdd-165">В IIS пользовательский интерфейс управления позволяет сравнительно легко настраивать все необходимое.</span><span class="sxs-lookup"><span data-stu-id="7acdd-165">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="7acdd-166">Однако при использовании WebListener вам нужно настроить Http.Sys самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="7acdd-166">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="7acdd-167">Для этого служит встроенное средство netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="7acdd-167">The built-in tool for doing that's netsh.exe.</span></span> 

<span data-ttu-id="7acdd-168">Чаще всего netsh.exe используется для резервирования префиксов URL-адресов и назначения SSL-сертификатов.</span><span class="sxs-lookup"><span data-stu-id="7acdd-168">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="7acdd-169">Средство NetSh.exe не слишком удобно для начинающих.</span><span class="sxs-lookup"><span data-stu-id="7acdd-169">NetSh.exe isn't an easy tool to use for beginners.</span></span> <span data-ttu-id="7acdd-170">Следующий пример показывает минимальный код, позволяющий зарезервировать префиксы URL-адресов для портов 80 и 443:</span><span class="sxs-lookup"><span data-stu-id="7acdd-170">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="7acdd-171">Следующий пример показывает, как назначить SSL-сертификат:</span><span class="sxs-lookup"><span data-stu-id="7acdd-171">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="7acdd-172">Ниже указана официальная справочная документация:</span><span class="sxs-lookup"><span data-stu-id="7acdd-172">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="7acdd-173">Команды Netsh для протокола HTTP</span><span class="sxs-lookup"><span data-stu-id="7acdd-173">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="7acdd-174">Строки UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="7acdd-174">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="7acdd-175">Приведенные ниже ресурсы содержат подробные инструкции по нескольким сценариям.</span><span class="sxs-lookup"><span data-stu-id="7acdd-175">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="7acdd-176">Статьи, описывающие `HttpListener`, в равной степени применимы и к `WebListener`, так как оба этих компонента основаны на Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="7acdd-176">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="7acdd-177">Практическое руководство. Настройка порта с использованием SSL-сертификата</span><span class="sxs-lookup"><span data-stu-id="7acdd-177">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="7acdd-178">[Взаимодействие через HTTPS: размещение и сертификация клиента на основе HttpListener](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) — это довольно старый блог сторонних разработчиков, однако в нем все равно есть полезные сведения.</span><span class="sxs-lookup"><span data-stu-id="7acdd-178">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="7acdd-179">[Практическое руководство. Пошаговые инструкции по использованию HttpListener или неуправляемого кода HTTP-сервера (C++) в качестве простого сервера SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) — это еще один старый блог с полезными сведениями.</span><span class="sxs-lookup"><span data-stu-id="7acdd-179">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="7acdd-180">Как настроить .NET Core WebListener с использованием SSL?</span><span class="sxs-lookup"><span data-stu-id="7acdd-180">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="7acdd-181">Ниже приведены некоторые сторонние средства, которые могут быть удобнее, чем командная строка netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="7acdd-181">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="7acdd-182">Они не предоставляются и не поддерживаются корпорацией Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="7acdd-182">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="7acdd-183">По умолчанию эти средства запускаются от имени администратора, так как права администратора нужны и netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="7acdd-183">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="7acdd-184">[http.sys Manager](http://httpsysmanager.codeplex.com/) предоставляет пользовательский интерфейс для перечисления и настройки SSL-сертификатов и параметров, резервирований префиксов и списков доверия сертификатов.</span><span class="sxs-lookup"><span data-stu-id="7acdd-184">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="7acdd-185">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) позволяет перечислить или настроить SSL-сертификаты и префиксы URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="7acdd-185">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="7acdd-186">По сравнению с http.sys Manager пользовательский интерфейс более подробный и предоставляет несколько дополнительных параметров конфигурации, а в остальном аналогичен.</span><span class="sxs-lookup"><span data-stu-id="7acdd-186">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="7acdd-187">В нем невозможно создать список доверия сертификатов (CTL), но можно назначить уже существующий.</span><span class="sxs-lookup"><span data-stu-id="7acdd-187">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="7acdd-188">Для создания самозаверяющих SSL-сертификатов корпорация Майкрософт предоставляет средства командной строки: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) и командлет PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="7acdd-188">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="7acdd-189">Существуют и сторонние средства пользовательского интерфейса, облегчающие создание самозаверяющих SSL-сертификатов:</span><span class="sxs-lookup"><span data-stu-id="7acdd-189">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="7acdd-190">SelfCert</span><span class="sxs-lookup"><span data-stu-id="7acdd-190">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="7acdd-191">Makecert UI</span><span class="sxs-lookup"><span data-stu-id="7acdd-191">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="7acdd-192">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="7acdd-192">Next steps</span></span>

<span data-ttu-id="7acdd-193">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="7acdd-193">For more information, see the following resources:</span></span>

* [<span data-ttu-id="7acdd-194">Пример приложения для этой статьи</span><span class="sxs-lookup"><span data-stu-id="7acdd-194">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="7acdd-195">Исходный код WebListener</span><span class="sxs-lookup"><span data-stu-id="7acdd-195">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="7acdd-196">Размещение</span><span class="sxs-lookup"><span data-stu-id="7acdd-196">Hosting</span></span>](../hosting.md)
