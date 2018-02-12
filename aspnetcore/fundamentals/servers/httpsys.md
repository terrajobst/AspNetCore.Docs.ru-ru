---
title: "Реализации веб-сервера HTTP.sys в ASP.NET Core"
author: rick-anderson
description: "Общие сведения о веб-сервере HTTP.sys для ASP.NET Core в Windows. HTTP.sys, основанный на работающем в режиме ядра драйвере Http.Sys, является альтернативой для Kestrel, которую можно использовать для прямого подключения к Интернету без служб IIS."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/httpsys
ms.openlocfilehash: f36a86fc67e715165afd0c38992f9767f90cf3b4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="ddde4-104">Реализации веб-сервера HTTP.sys в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ddde4-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="ddde4-105">Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra) и [Крис Росс](https://github.com/Tratcher) (Chris Ross)</span><span class="sxs-lookup"><span data-stu-id="ddde4-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="ddde4-106">Сведения из этого раздела распространяются только на ASP.NET Core 2.0 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="ddde4-106">This topic applies only to ASP.NET Core 2.0 and later.</span></span> <span data-ttu-id="ddde4-107">В более ранних версиях ASP.NET Core HTTP.sys называется [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="ddde4-107">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="ddde4-108">HTTP.sys — это [веб-сервер для ASP.NET Core](index.md), который запускается только в Windows.</span><span class="sxs-lookup"><span data-stu-id="ddde4-108">HTTP.sys is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="ddde4-109">Он основан на [работающем в режиме ядра драйвере Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="ddde4-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="ddde4-110">HTTP.sys является альтернативой для [Kestrel](kestrel.md) и предлагает некоторые функции, отсутствующие в Kestel.</span><span class="sxs-lookup"><span data-stu-id="ddde4-110">HTTP.sys is an alternative to [Kestrel](kestrel.md) that offers some features that Kestel doesn't.</span></span> <span data-ttu-id="ddde4-111">**HTTP.sys не подходит для использования с IIS или IIS Express, так как он не совместим с [модулем ASP.NET Core](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="ddde4-111">**HTTP.sys can't be used with IIS or IIS Express, as it's incompatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="ddde4-112">HTTP.sys поддерживает следующие функции:</span><span class="sxs-lookup"><span data-stu-id="ddde4-112">HTTP.sys supports the following features:</span></span>

- [<span data-ttu-id="ddde4-113">Проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="ddde4-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="ddde4-114">Совместное использование портов</span><span class="sxs-lookup"><span data-stu-id="ddde4-114">Port sharing</span></span>
- <span data-ttu-id="ddde4-115">HTTPS с SNI</span><span class="sxs-lookup"><span data-stu-id="ddde4-115">HTTPS with SNI</span></span>
- <span data-ttu-id="ddde4-116">HTTP/2 через TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="ddde4-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="ddde4-117">Прямая передача файлов</span><span class="sxs-lookup"><span data-stu-id="ddde4-117">Direct file transmission</span></span>
- <span data-ttu-id="ddde4-118">Кэширование откликов</span><span class="sxs-lookup"><span data-stu-id="ddde4-118">Response caching</span></span>
- <span data-ttu-id="ddde4-119">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="ddde4-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="ddde4-120">Поддерживаемые версии Windows:</span><span class="sxs-lookup"><span data-stu-id="ddde4-120">Supported Windows versions:</span></span>

- <span data-ttu-id="ddde4-121">Windows 7 и Windows Server 2008 R2 и более поздних версий</span><span class="sxs-lookup"><span data-stu-id="ddde4-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="ddde4-122">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ddde4-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="ddde4-123">Условия для применения HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ddde4-123">When to use HTTP.sys</span></span>

<span data-ttu-id="ddde4-124">HTTP.sys удобен для развертываний, где нужно подключить сервер к Интернету напрямую без использования служб IIS.</span><span class="sxs-lookup"><span data-stu-id="ddde4-124">HTTP.sys is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![HTTP.sys взаимодействует с Интернетом напрямую.](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="ddde4-126">Так как HTTP.sys основан на Http.Sys, ему не нужен обратный прокси-сервер для защиты от атак.</span><span class="sxs-lookup"><span data-stu-id="ddde4-126">Because it's built on Http.Sys, HTTP.sys doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="ddde4-127">Http.Sys — это проверенная технология, которая защищает от многих типов атак, а также обеспечивает надежность, безопасность и масштабируемость полнофункционального веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="ddde4-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="ddde4-128">Сами службы IIS выполняются в качестве HTTP-прослушивателя поверх Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="ddde4-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="ddde4-129">HTTP.sys хорошо подходит для внутренних развертываний, когда нужна функция, отсутствующая в Kestrel, например проверка подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="ddde4-129">HTTP.sys is a good choice for internal deployments when you need a feature not available in Kestrel, such as Windows authentication.</span></span>

![HTTP.sys взаимодействует с вашей внутренней сетью напрямую.](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a><span data-ttu-id="ddde4-131">Способы применения HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ddde4-131">How to use HTTP.sys</span></span>

<span data-ttu-id="ddde4-132">Ниже приведен обзор задач установки для ОС узла и приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ddde4-132">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="ddde4-133">Настройка Windows Server</span><span class="sxs-lookup"><span data-stu-id="ddde4-133">Configure Windows Server</span></span>

* <span data-ttu-id="ddde4-134">Установите нужную вашему приложению версию платформы .NET, например [.NET Core](https://www.microsoft.com/net/download/core) или [.NET Framework](https://www.microsoft.com/net/download/framework).</span><span class="sxs-lookup"><span data-stu-id="ddde4-134">Install the version of .NET that your application requires, such as [.NET Core](https://www.microsoft.com/net/download/core) or [.NET Framework](https://www.microsoft.com/net/download/framework).</span></span>

* <span data-ttu-id="ddde4-135">Предварительно зарегистрируйте префиксы URL-адресов для привязки к HTTP.sys и настройте SSL-сертификаты.</span><span class="sxs-lookup"><span data-stu-id="ddde4-135">Preregister URL prefixes to bind to HTTP.sys, and set up SSL certificates</span></span>

   <span data-ttu-id="ddde4-136">Если вы не выполняете предварительную регистрацию префиксов URL-адресов в Windows, приложение потребуется запустить с правами администратора.</span><span class="sxs-lookup"><span data-stu-id="ddde4-136">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="ddde4-137">Единственным исключением является привязка к localhost через HTTP (не HTTPS) с номером порта больше 1024. В этом случае права администратора не требуются.</span><span class="sxs-lookup"><span data-stu-id="ddde4-137">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case, administrator privileges aren't required.</span></span>

   <span data-ttu-id="ddde4-138">Дополнительные сведения см. в разделе [Предварительная регистрация префиксов и настройка SSL](#preregister-url-prefixes-and-configure-ssl) ниже.</span><span class="sxs-lookup"><span data-stu-id="ddde4-138">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="ddde4-139">Откройте порты брандмауэра, чтобы разрешить трафик в HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="ddde4-139">Open firewall ports to allow traffic to reach HTTP.sys.</span></span>

   <span data-ttu-id="ddde4-140">Можно использовать *netsh.exe* или [командлеты PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="ddde4-140">You can use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="ddde4-141">Кроме того, существуют [параметры реестра Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="ddde4-141">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a><span data-ttu-id="ddde4-142">Настройка приложения ASP.NET Core для использования HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ddde4-142">Configure your ASP.NET Core application to use HTTP.sys</span></span>

* <span data-ttu-id="ddde4-143">При использовании метапакета [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) установка никаких пакетов не требуется.</span><span class="sxs-lookup"><span data-stu-id="ddde4-143">No package install is needed if you use the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="ddde4-144">Пакет [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) входит в состав метапакета.</span><span class="sxs-lookup"><span data-stu-id="ddde4-144">The [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) package is included in the metapackage.</span></span>

* <span data-ttu-id="ddde4-145">Вызовите метод расширения `UseHttpSys` для `WebHostBuilder` в вашем методе `Main`, указав все нужные [параметры HTTP.sys](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs), как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="ddde4-145">Call the `UseHttpSys` extension method on `WebHostBuilder` in your `Main` method, specifying any [HTTP.sys options](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a><span data-ttu-id="ddde4-146">Настройка параметров HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ddde4-146">Configure HTTP.sys options</span></span>

<span data-ttu-id="ddde4-147">Ниже приведены некоторые ограничения и параметры HTTP.sys, которые можно настроить.</span><span class="sxs-lookup"><span data-stu-id="ddde4-147">Here are some of the HTTP.sys settings and limits that you can configure.</span></span>

<span data-ttu-id="ddde4-148">**Максимальное число клиентских подключений**</span><span class="sxs-lookup"><span data-stu-id="ddde4-148">**Maximum client connections**</span></span>

<span data-ttu-id="ddde4-149">Максимальное число одновременно открытых подключений TCP для всего приложения можно задать с помощью следующего кода в файле *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="ddde4-149">The maximum number of concurrent open TCP connections can be set for the entire application with the following code in *Program.cs*:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

<span data-ttu-id="ddde4-150">По умолчанию максимальное число подключений не ограничено (null).</span><span class="sxs-lookup"><span data-stu-id="ddde4-150">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="ddde4-151">**Maximum request body size** (Максимальный размер текста запроса)</span><span class="sxs-lookup"><span data-stu-id="ddde4-151">**Maximum request body size**</span></span>

<span data-ttu-id="ddde4-152">По умолчанию максимальный размер текста запроса составляет 30 000 000 байт, что примерно соответствует 28,6 МБ.</span><span class="sxs-lookup"><span data-stu-id="ddde4-152">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span>

<span data-ttu-id="ddde4-153">Чтобы переопределить это ограничение в приложении ASP.NET Core MVC, рекомендуется использовать атрибут [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) в методе действия:</span><span class="sxs-lookup"><span data-stu-id="ddde4-153">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="ddde4-154">Приведенный ниже пример показывает, как настроить ограничение для всего приложения и каждого запроса:</span><span class="sxs-lookup"><span data-stu-id="ddde4-154">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

<span data-ttu-id="ddde4-155">Можно переопределить параметр для конкретного запроса в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ddde4-155">You can override the setting on a specific request in *Startup.cs*:</span></span>

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
<span data-ttu-id="ddde4-156">При попытке настроить ограничение для запроса после того, как приложение начало считывать запрос, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="ddde4-156">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="ddde4-157">Доступно свойство `IsReadOnly`, указывающее, что свойство `MaxRequestBodySize` находится в состоянии только для чтения и настраивать ограничение слишком поздно.</span><span class="sxs-lookup"><span data-stu-id="ddde4-157">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="ddde4-158">Дополнительные сведения о других параметрах HTTP.sys см. в разделе [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="ddde4-158">For information about other HTTP.sys options, see [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> 

### <a name="configure-urls-and-ports-to-listen-on"></a><span data-ttu-id="ddde4-159">Настройка URL-адресов и портов для прослушивания</span><span class="sxs-lookup"><span data-stu-id="ddde4-159">Configure URLs and ports to listen on</span></span> 

<span data-ttu-id="ddde4-160">По умолчанию ASP.NET Core привязан к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="ddde4-160">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="ddde4-161">Чтобы настроить префиксы URL-адресов и порты, можно использовать метод расширения `UseUrls`, аргумент командной строки `urls`, переменную среды ASPNETCORE_URLS или свойство `UrlPrefixes` объекта [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="ddde4-161">To configure URL prefixes and ports, you can use the `UseUrls` extension method, the `urls` command-line argument, the ASPNETCORE_URLS environment variable, or the `UrlPrefixes` property on [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> <span data-ttu-id="ddde4-162">Приведенный ниже пример кода использует `UrlPrefixes`.</span><span class="sxs-lookup"><span data-stu-id="ddde4-162">The following code example uses `UrlPrefixes`.</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

<span data-ttu-id="ddde4-163">Преимуществом `UrlPrefixes` является то, что вы получаете сообщение об ошибке сразу, как только попытаетесь добавить префикс в неправильном формате.</span><span class="sxs-lookup"><span data-stu-id="ddde4-163">An advantage of `UrlPrefixes` is that you get an error message immediately if you try to add a prefix that's formatted wrong.</span></span> <span data-ttu-id="ddde4-164">Преимуществом `UseUrls` (совместно с `urls` и ASPNETCORE_URLS) является упрощенное переключение между Kestrel и HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="ddde4-164">An advantage of `UseUrls` (shared with `urls` and ASPNETCORE_URLS) is that you can more easily switch between Kestrel and HTTP.sys.</span></span>

<span data-ttu-id="ddde4-165">Если вы используете как `UseUrls` (либо `urls` или ASPNETCORE_URLS), так и `UrlPrefixes`, параметры в `UrlPrefixes` переопределяют значения в `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="ddde4-165">If you use both `UseUrls` (or `urls` or ASPNETCORE_URLS) and `UrlPrefixes`, the settings in `UrlPrefixes` override the ones in `UseUrls`.</span></span> <span data-ttu-id="ddde4-166">Дополнительные сведения см. в разделе [Размещение](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="ddde4-166">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="ddde4-167">HTTP.sys использует [форматы строк UrlPrefix API HTTP-сервера](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="ddde4-167">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="ddde4-168">Убедитесь, что указали одинаковые строки префикса в `UseUrls` или `UrlPrefixes`, предварительно регистрируемых на сервере.</span><span class="sxs-lookup"><span data-stu-id="ddde4-168">Make sure that you specify the same prefix strings in `UseUrls` or `UrlPrefixes` that you preregister on the server.</span></span> 

### <a name="dont-use-iis"></a><span data-ttu-id="ddde4-169">Отказ от использования IIS</span><span class="sxs-lookup"><span data-stu-id="ddde4-169">Don't use IIS</span></span>

<span data-ttu-id="ddde4-170">Убедитесь, что приложение не настроено для запуска IIS или IIS Express.</span><span class="sxs-lookup"><span data-stu-id="ddde4-170">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="ddde4-171">В Visual Studio профиль запуска по умолчанию использует IIS Express.</span><span class="sxs-lookup"><span data-stu-id="ddde4-171">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="ddde4-172">Чтобы запустить проект как консольное приложение, вручную измените выбранный профиль, как показано на следующем снимке экрана.</span><span class="sxs-lookup"><span data-stu-id="ddde4-172">To run the project as a console application, manually change the selected profile, as shown in the following screen shot.</span></span>

![Выбор профиля консольного приложения](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="ddde4-174">Предварительная регистрация префиксов URL-адресов и настройка SSL</span><span class="sxs-lookup"><span data-stu-id="ddde4-174">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="ddde4-175">Как IIS, так и HTTP.sys основаны на работающем в режиме ядра драйвере Http.Sys, который осуществляет прослушивание запросов и первоначальную обработку.</span><span class="sxs-lookup"><span data-stu-id="ddde4-175">Both IIS and HTTP.sys rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="ddde4-176">В IIS пользовательский интерфейс управления позволяет сравнительно легко настраивать все необходимое.</span><span class="sxs-lookup"><span data-stu-id="ddde4-176">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="ddde4-177">Однако Http.Sys нужно настроить самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="ddde4-177">However, you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="ddde4-178">Для этого служит встроенное средство *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="ddde4-178">The built-in tool for doing that's *netsh.exe*.</span></span> 

<span data-ttu-id="ddde4-179">С помощью*netsh.exe* можно зарезервировать префиксы URL-адресов и назначить SSL-сертификаты.</span><span class="sxs-lookup"><span data-stu-id="ddde4-179">With *netsh.exe* you can reserve URL prefixes and assign SSL certificates.</span></span> <span data-ttu-id="ddde4-180">Для этого средства нужны права администратора.</span><span class="sxs-lookup"><span data-stu-id="ddde4-180">The tool requires administrative privileges.</span></span>

<span data-ttu-id="ddde4-181">Следующий пример показывает минимальный код, позволяющий зарезервировать префиксы URL-адресов для портов 80 и 443:</span><span class="sxs-lookup"><span data-stu-id="ddde4-181">The following example shows the minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="ddde4-182">Следующий пример показывает, как назначить SSL-сертификат:</span><span class="sxs-lookup"><span data-stu-id="ddde4-182">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

<span data-ttu-id="ddde4-183">Ниже указана справочная документация по *netsh.exe*:</span><span class="sxs-lookup"><span data-stu-id="ddde4-183">Here is the reference documentation for *netsh.exe*:</span></span>

* [<span data-ttu-id="ddde4-184">Команды Netsh для протокола HTTP</span><span class="sxs-lookup"><span data-stu-id="ddde4-184">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="ddde4-185">Строки UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="ddde4-185">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="ddde4-186">Приведенные ниже ресурсы содержат подробные инструкции по нескольким сценариям.</span><span class="sxs-lookup"><span data-stu-id="ddde4-186">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="ddde4-187">Статьи, описывающие HttpListener, в равной степени применимы и к HTTP.sys, так как оба этих компонента основаны на Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="ddde4-187">Articles that refer to HttpListener apply equally to HTTP.sys, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="ddde4-188">Практическое руководство. Настройка порта с использованием SSL-сертификата</span><span class="sxs-lookup"><span data-stu-id="ddde4-188">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="ddde4-189">[Взаимодействие через HTTPS: размещение и сертификация клиента на основе HttpListener](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) — это довольно старый блог сторонних разработчиков, однако в нем все равно есть полезные сведения.</span><span class="sxs-lookup"><span data-stu-id="ddde4-189">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="ddde4-190">[Практическое руководство. Пошаговые инструкции по использованию HttpListener или неуправляемого кода HTTP-сервера (C++) в качестве простого сервера SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) — это еще один старый блог с полезными сведениями.</span><span class="sxs-lookup"><span data-stu-id="ddde4-190">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>

<span data-ttu-id="ddde4-191">Ниже приведены некоторые сторонние средства, которые могут быть удобнее, чем командная строка *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="ddde4-191">Here are some third-party tools that can be easier to use than the *netsh.exe* command line.</span></span> <span data-ttu-id="ddde4-192">Они не предоставляются и не поддерживаются корпорацией Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="ddde4-192">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="ddde4-193">По умолчанию эти средства запускаются от имени администратора, так как права администратора нужны и *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="ddde4-193">The tools run as administrator by default, since *netsh.exe* itself requires administrator privileges.</span></span>

* <span data-ttu-id="ddde4-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) предоставляет пользовательский интерфейс для перечисления и настройки SSL-сертификатов и параметров, резервирований префиксов и списков доверия сертификатов.</span><span class="sxs-lookup"><span data-stu-id="ddde4-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="ddde4-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) позволяет перечислить или настроить SSL-сертификаты и префиксы URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="ddde4-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="ddde4-196">По сравнению с http.sys Manager пользовательский интерфейс более подробный и предоставляет несколько дополнительных параметров конфигурации, а в остальном аналогичен.</span><span class="sxs-lookup"><span data-stu-id="ddde4-196">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="ddde4-197">В нем невозможно создать список доверия сертификатов (CTL), но можно назначить уже существующий.</span><span class="sxs-lookup"><span data-stu-id="ddde4-197">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a><span data-ttu-id="ddde4-198">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="ddde4-198">Next steps</span></span>

<span data-ttu-id="ddde4-199">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="ddde4-199">For more information, see the following resources:</span></span>

* [<span data-ttu-id="ddde4-200">Пример приложения для этой статьи</span><span class="sxs-lookup"><span data-stu-id="ddde4-200">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [<span data-ttu-id="ddde4-201">Исходный код HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ddde4-201">HTTP.sys source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="ddde4-202">Размещение</span><span class="sxs-lookup"><span data-stu-id="ddde4-202">Hosting</span></span>](xref:fundamentals/hosting)
