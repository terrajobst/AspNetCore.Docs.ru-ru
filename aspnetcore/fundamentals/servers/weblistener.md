---
title: Реализации веб-сервера WebListener в ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о WebListener, веб-сервере для ASP.NET Core в Windows, который можно использовать для прямого подключения к Интернету без IIS.
manager: wpickett
ms.author: riande
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/weblistener
ms.openlocfilehash: d40243454632550147a7d42ab26a8f1d2d100db2
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/14/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="53660-103">Реализации веб-сервера WebListener в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="53660-103">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="53660-104">Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra) и [Крис Росс](https://github.com/Tratcher) (Chris Ross)</span><span class="sxs-lookup"><span data-stu-id="53660-104">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="53660-105">Сведения из этого раздела распространяются только на ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="53660-105">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="53660-106">В ASP.NET Core 2.0 WebListener называется [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="53660-106">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="53660-107">WebListener — это [веб-сервер для ASP.NET Core](index.md), который запускается только в Windows.</span><span class="sxs-lookup"><span data-stu-id="53660-107">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="53660-108">Он основан на [работающем в режиме ядра драйвере Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="53660-108">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="53660-109">WebListener является альтернативой для [Kestrel](kestrel.md), которую можно использовать для прямого подключения к Интернету, не используя IIS в качестве обратного прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="53660-109">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="53660-110">Фактически **WebListener не подходит для использования с IIS или IIS Express, так как он не совместим с [модулем ASP.NET Core](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="53660-110">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="53660-111">Хотя WebListener был разработан для ASP.NET Core, его невозможно использовать напрямую в каком-либо приложении .NET Core или .NET Framework через пакет NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).</span><span class="sxs-lookup"><span data-stu-id="53660-111">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="53660-112">WebListener поддерживает следующие функции:</span><span class="sxs-lookup"><span data-stu-id="53660-112">WebListener supports the following features:</span></span>

- [<span data-ttu-id="53660-113">Проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="53660-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="53660-114">Совместное использование портов</span><span class="sxs-lookup"><span data-stu-id="53660-114">Port sharing</span></span>
- <span data-ttu-id="53660-115">HTTPS с SNI</span><span class="sxs-lookup"><span data-stu-id="53660-115">HTTPS with SNI</span></span>
- <span data-ttu-id="53660-116">HTTP/2 через TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="53660-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="53660-117">Прямая передача файлов</span><span class="sxs-lookup"><span data-stu-id="53660-117">Direct file transmission</span></span>
- <span data-ttu-id="53660-118">Кэширование откликов</span><span class="sxs-lookup"><span data-stu-id="53660-118">Response caching</span></span>
- <span data-ttu-id="53660-119">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="53660-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="53660-120">Поддерживаемые версии Windows:</span><span class="sxs-lookup"><span data-stu-id="53660-120">Supported Windows versions:</span></span>

- <span data-ttu-id="53660-121">Windows 7 и Windows Server 2008 R2 и более поздних версий</span><span class="sxs-lookup"><span data-stu-id="53660-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="53660-122">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="53660-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="53660-123">Условия использования WebListener</span><span class="sxs-lookup"><span data-stu-id="53660-123">When to use WebListener</span></span>

<span data-ttu-id="53660-124">WebListener удобен для развертываний, где нужно подключить сервер к Интернету напрямую без использования служб IIS.</span><span class="sxs-lookup"><span data-stu-id="53660-124">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![Weblistener взаимодействует с Интернетом напрямую.](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="53660-126">Так как WebListener основан на Http.Sys, ему не нужен обратный прокси-сервер для защиты от атак.</span><span class="sxs-lookup"><span data-stu-id="53660-126">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="53660-127">Http.Sys — это проверенная технология, которая защищает от многих типов атак, а также обеспечивает надежность, безопасность и масштабируемость полнофункционального веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="53660-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="53660-128">Сами службы IIS выполняются в качестве HTTP-прослушивателя поверх Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="53660-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="53660-129">WebListener также хорошо подходит для внутренних развертываний, когда нужна функция, отсутствующая в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="53660-129">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![Weblistener взаимодействует с вашей внутренней сетью напрямую.](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="53660-131">Способы использования WebListener</span><span class="sxs-lookup"><span data-stu-id="53660-131">How to use WebListener</span></span>

<span data-ttu-id="53660-132">Ниже приведен обзор задач установки для ОС узла и приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="53660-132">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="53660-133">Настройка Windows Server</span><span class="sxs-lookup"><span data-stu-id="53660-133">Configure Windows Server</span></span>

* <span data-ttu-id="53660-134">Установите нужную вашему приложению версию платформы .NET, например [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) или .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="53660-134">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="53660-135">Предварительно зарегистрируйте префиксы URL-адресов для привязки к WebListener и настройте SSL-сертификаты.</span><span class="sxs-lookup"><span data-stu-id="53660-135">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="53660-136">Если вы не выполняете предварительную регистрацию префиксов URL-адресов в Windows, приложение потребуется запустить с правами администратора.</span><span class="sxs-lookup"><span data-stu-id="53660-136">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="53660-137">Единственным исключением является привязка к localhost через HTTP (не HTTPS) с номером порта больше 1024. В этом случае права администратора не требуются.</span><span class="sxs-lookup"><span data-stu-id="53660-137">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="53660-138">Дополнительные сведения см. в разделе [Предварительная регистрация префиксов и настройка SSL](#preregister-url-prefixes-and-configure-ssl) ниже.</span><span class="sxs-lookup"><span data-stu-id="53660-138">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="53660-139">Откройте порты брандмауэра, чтобы разрешить трафик в WebListener.</span><span class="sxs-lookup"><span data-stu-id="53660-139">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="53660-140">Можно использовать netsh.exe или [командлеты PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="53660-140">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="53660-141">Кроме того, существуют [параметры реестра Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="53660-141">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="53660-142">Настройка приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="53660-142">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="53660-143">Установите пакет NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span><span class="sxs-lookup"><span data-stu-id="53660-143">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="53660-144">Он также устанавливает [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) в качестве зависимости.</span><span class="sxs-lookup"><span data-stu-id="53660-144">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="53660-145">Вызовите метод расширения `UseWebListener` для [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) в вашем методе `Main`, указав [параметры](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) WebListener и другие нужные [параметры](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs), как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="53660-145">Call the `UseWebListener` extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="53660-146">Настройка URL-адресов и портов для прослушивания</span><span class="sxs-lookup"><span data-stu-id="53660-146">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="53660-147">По умолчанию ASP.NET Core привязан к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="53660-147">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="53660-148">Чтобы настроить префиксы URL-адресов и порты, можно использовать метод расширения `UseURLs`, аргумент командной строки `urls` или систему конфигурации ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="53660-148">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="53660-149">Дополнительные сведения см. в разделе [Размещение](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="53660-149">For more information, see [Hosting](../../fundamentals/hosting.md).</span></span>

  <span data-ttu-id="53660-150">WebListener использует [форматы строк префикса Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="53660-150">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="53660-151">Для WebListener не предъявляются отдельные требования к формату строк префикса.</span><span class="sxs-lookup"><span data-stu-id="53660-151">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!WARNING]
  > <span data-ttu-id="53660-152">**Не используйте** привязки с подстановочными знаками (`http://*:80/` и `http://+:80`) на верхнем уровне.</span><span class="sxs-lookup"><span data-stu-id="53660-152">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="53660-153">Это может создать уязвимость и поставить ваше приложение под угрозу.</span><span class="sxs-lookup"><span data-stu-id="53660-153">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="53660-154">Сюда относятся и строгие, и нестрогие подстановочные знаки.</span><span class="sxs-lookup"><span data-stu-id="53660-154">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="53660-155">Вместо этого используйте имена узлов в явном виде.</span><span class="sxs-lookup"><span data-stu-id="53660-155">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="53660-156">Привязки с подстановочными знаками на уровне дочерних доменов (например `*.mysub.com`) не создают таких угроз безопасности, если вы полностью контролируете родительский домен (в отличие от варианта `*.com`, создающего уязвимость).</span><span class="sxs-lookup"><span data-stu-id="53660-156">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="53660-157">Дополнительные сведения см. в документе [rfc7230, раздел 5.4](https://tools.ietf.org/html/rfc7230#section-5.4).</span><span class="sxs-lookup"><span data-stu-id="53660-157">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

  > [!NOTE]
  > <span data-ttu-id="53660-158">Убедитесь, что указали одинаковые строки префикса в `UseUrls`, предварительно регистрируемого на сервере.</span><span class="sxs-lookup"><span data-stu-id="53660-158">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="53660-159">Убедитесь, что приложение не настроено для запуска IIS или IIS Express.</span><span class="sxs-lookup"><span data-stu-id="53660-159">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="53660-160">В Visual Studio профиль запуска по умолчанию использует IIS Express.</span><span class="sxs-lookup"><span data-stu-id="53660-160">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="53660-161">Чтобы запустить проект как консольное приложение, вручную измените выбранный профиль, как показано на следующем снимке экрана.</span><span class="sxs-lookup"><span data-stu-id="53660-161">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Выбор профиля консольного приложения](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="53660-163">Использование WebListener за пределами ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="53660-163">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="53660-164">Установите пакет NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).</span><span class="sxs-lookup"><span data-stu-id="53660-164">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="53660-165">[Предварительно зарегистрируйте префиксы URL-адресов для привязки к WebListener и настройте SSL-сертификаты](#preregister-url-prefixes-and-configure-ssl), как и при использовании в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="53660-165">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="53660-166">Кроме того, существуют [параметры реестра Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="53660-166">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="53660-167">Ниже приведен пример кода, демонстрирующий применение WebListener за пределами ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="53660-167">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="53660-168">Предварительная регистрация префиксов URL-адресов и настройка SSL</span><span class="sxs-lookup"><span data-stu-id="53660-168">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="53660-169">Как IIS, так и WebListener основаны на работающем в режиме ядра драйвере Http.Sys, который осуществляет прослушивание запросов и первоначальную обработку.</span><span class="sxs-lookup"><span data-stu-id="53660-169">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="53660-170">В IIS пользовательский интерфейс управления позволяет сравнительно легко настраивать все необходимое.</span><span class="sxs-lookup"><span data-stu-id="53660-170">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="53660-171">Однако при использовании WebListener вам нужно настроить Http.Sys самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="53660-171">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="53660-172">Для этого служит встроенное средство netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="53660-172">The built-in tool for doing that's netsh.exe.</span></span> 

<span data-ttu-id="53660-173">Чаще всего netsh.exe используется для резервирования префиксов URL-адресов и назначения SSL-сертификатов.</span><span class="sxs-lookup"><span data-stu-id="53660-173">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="53660-174">Средство NetSh.exe не слишком удобно для начинающих.</span><span class="sxs-lookup"><span data-stu-id="53660-174">NetSh.exe isn't an easy tool to use for beginners.</span></span> <span data-ttu-id="53660-175">Следующий пример показывает минимальный код, позволяющий зарезервировать префиксы URL-адресов для портов 80 и 443:</span><span class="sxs-lookup"><span data-stu-id="53660-175">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="53660-176">Следующий пример показывает, как назначить SSL-сертификат:</span><span class="sxs-lookup"><span data-stu-id="53660-176">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="53660-177">Ниже указана официальная справочная документация:</span><span class="sxs-lookup"><span data-stu-id="53660-177">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="53660-178">Команды Netsh для протокола HTTP</span><span class="sxs-lookup"><span data-stu-id="53660-178">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="53660-179">Строки UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="53660-179">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="53660-180">Приведенные ниже ресурсы содержат подробные инструкции по нескольким сценариям.</span><span class="sxs-lookup"><span data-stu-id="53660-180">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="53660-181">Статьи, описывающие `HttpListener`, в равной степени применимы и к `WebListener`, так как оба этих компонента основаны на Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="53660-181">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="53660-182">Практическое руководство. Настройка порта с использованием SSL-сертификата</span><span class="sxs-lookup"><span data-stu-id="53660-182">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="53660-183">[Взаимодействие через HTTPS: размещение и сертификация клиента на основе HttpListener](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) — это довольно старый блог сторонних разработчиков, однако в нем все равно есть полезные сведения.</span><span class="sxs-lookup"><span data-stu-id="53660-183">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="53660-184">[Практическое руководство. Пошаговые инструкции по использованию HttpListener или неуправляемого кода HTTP-сервера (C++) в качестве простого сервера SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) — это еще один старый блог с полезными сведениями.</span><span class="sxs-lookup"><span data-stu-id="53660-184">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="53660-185">Как настроить .NET Core WebListener с использованием SSL?</span><span class="sxs-lookup"><span data-stu-id="53660-185">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="53660-186">Ниже приведены некоторые сторонние средства, которые могут быть удобнее, чем командная строка netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="53660-186">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="53660-187">Они не предоставляются и не поддерживаются корпорацией Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="53660-187">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="53660-188">По умолчанию эти средства запускаются от имени администратора, так как права администратора нужны и netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="53660-188">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="53660-189">[http.sys Manager](http://httpsysmanager.codeplex.com/) предоставляет пользовательский интерфейс для перечисления и настройки SSL-сертификатов и параметров, резервирований префиксов и списков доверия сертификатов.</span><span class="sxs-lookup"><span data-stu-id="53660-189">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="53660-190">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) позволяет перечислить или настроить SSL-сертификаты и префиксы URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="53660-190">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="53660-191">По сравнению с http.sys Manager пользовательский интерфейс более подробный и предоставляет несколько дополнительных параметров конфигурации, а в остальном аналогичен.</span><span class="sxs-lookup"><span data-stu-id="53660-191">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="53660-192">В нем невозможно создать список доверия сертификатов (CTL), но можно назначить уже существующий.</span><span class="sxs-lookup"><span data-stu-id="53660-192">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="53660-193">Для создания самозаверяющих SSL-сертификатов корпорация Майкрософт предоставляет средства командной строки: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) и командлет PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="53660-193">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="53660-194">Существуют и сторонние средства пользовательского интерфейса, облегчающие создание самозаверяющих SSL-сертификатов:</span><span class="sxs-lookup"><span data-stu-id="53660-194">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="53660-195">SelfCert</span><span class="sxs-lookup"><span data-stu-id="53660-195">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="53660-196">Makecert UI</span><span class="sxs-lookup"><span data-stu-id="53660-196">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="53660-197">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="53660-197">Next steps</span></span>

<span data-ttu-id="53660-198">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="53660-198">For more information, see the following resources:</span></span>

* [<span data-ttu-id="53660-199">Пример приложения для этой статьи</span><span class="sxs-lookup"><span data-stu-id="53660-199">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="53660-200">Исходный код WebListener</span><span class="sxs-lookup"><span data-stu-id="53660-200">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="53660-201">Размещение</span><span class="sxs-lookup"><span data-stu-id="53660-201">Hosting</span></span>](../hosting.md)
