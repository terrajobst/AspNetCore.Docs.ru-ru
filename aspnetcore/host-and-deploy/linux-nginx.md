---
title: Среда размещения ASP.NET Core в операционной системе Linux с Nginx
author: rick-anderson
description: В статье описывается процедура настройки Nginx как обратного прокси-сервера на Ubuntu 16.04 для перенаправления трафика HTTP в веб-приложение ASP.NET Core, выполняемое в Kestrel.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: d37aa25c712d715aa4134587a84e5923f9cb5b79
ms.sourcegitcommit: 50d40c83fa641d283c097f986dde5341ebe1b44c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/22/2018
ms.locfileid: "34452559"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="c04dd-103">Среда размещения ASP.NET Core в операционной системе Linux с Nginx</span><span class="sxs-lookup"><span data-stu-id="c04dd-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="c04dd-104">Автор [Сурабх Ширхатти](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="c04dd-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="c04dd-105">В этом руководстве описывается настройка готовой к работе среды ASP.NET Core на сервере 16.04 Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="c04dd-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

> [!NOTE]
> <span data-ttu-id="c04dd-106">Для Ubuntu 14.04 в качестве решения для мониторинга процесса Kestrel рекомендуется *supervisord*.</span><span class="sxs-lookup"><span data-stu-id="c04dd-106">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="c04dd-107">Решение *systemd* в Ubuntu 14.04 недоступно.</span><span class="sxs-lookup"><span data-stu-id="c04dd-107">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="c04dd-108">[См. предыдущую версию этого документа](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="c04dd-108">[See previous version of this document](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="c04dd-109">В этом руководстве рассматривается</span><span class="sxs-lookup"><span data-stu-id="c04dd-109">This guide:</span></span>

* <span data-ttu-id="c04dd-110">размещение существующего приложения ASP.NET Core за обратным прокси-сервером;</span><span class="sxs-lookup"><span data-stu-id="c04dd-110">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="c04dd-111">настройка обратного прокси-сервера для перенаправления запросов на веб-сервер Kestrel;</span><span class="sxs-lookup"><span data-stu-id="c04dd-111">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="c04dd-112">проверка выполнения веб-приложения как управляющей программы при запуске системы;</span><span class="sxs-lookup"><span data-stu-id="c04dd-112">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="c04dd-113">настройка средства управления процессами, с помощью которого можно перезапустить веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="c04dd-113">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c04dd-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="c04dd-114">Prerequisites</span></span>

1. <span data-ttu-id="c04dd-115">Доступ к серверу Ubuntu 16.04 под учетной записи обычного пользователя с правами sudo</span><span class="sxs-lookup"><span data-stu-id="c04dd-115">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
1. <span data-ttu-id="c04dd-116">Существующее приложение ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c04dd-116">An existing ASP.NET Core app</span></span>

## <a name="copy-over-the-app"></a><span data-ttu-id="c04dd-117">Копирование приложения</span><span class="sxs-lookup"><span data-stu-id="c04dd-117">Copy over the app</span></span>

<span data-ttu-id="c04dd-118">Выполните из среды разработки команду [dotnet publish](/dotnet/core/tools/dotnet-publish), чтобы упаковать приложение в отдельный каталог, который можно запустить на сервере.</span><span class="sxs-lookup"><span data-stu-id="c04dd-118">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="c04dd-119">Скопируйте приложение ASP.NET Core на сервер с помощью инструмента, интегрированного в ваш рабочий процесс (например, SCP или FTP).</span><span class="sxs-lookup"><span data-stu-id="c04dd-119">Copy the ASP.NET Core app to the server using whatever tool integrates into the organization's workflow (for example, SCP, FTP).</span></span> <span data-ttu-id="c04dd-120">Протестируйте приложение, например</span><span class="sxs-lookup"><span data-stu-id="c04dd-120">Test the app, for example:</span></span>

* <span data-ttu-id="c04dd-121">Выполните из командной строки команду `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="c04dd-121">From the command line, run `dotnet <app_assembly>.dll`.</span></span>
* <span data-ttu-id="c04dd-122">В браузере откройте страницу `http://<serveraddress>:<port>`, чтобы убедиться, что приложение работает на платформе Linux.</span><span class="sxs-lookup"><span data-stu-id="c04dd-122">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="c04dd-123">Настройка обратного прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="c04dd-123">Configure a reverse proxy server</span></span>

<span data-ttu-id="c04dd-124">Обратный прокси-сервер — это общая настройка для обслуживания динамических веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="c04dd-124">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="c04dd-125">Обратный прокси-сервер завершает HTTP-запрос и перенаправляет его в приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c04dd-125">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="c04dd-126">Для чего нужен обратный прокси-сервер?</span><span class="sxs-lookup"><span data-stu-id="c04dd-126">Why use a reverse proxy server?</span></span>

<span data-ttu-id="c04dd-127">Kestrel является отличным решением для обслуживания динамического содержимого из ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c04dd-127">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="c04dd-128">При этом компоненты для работы с веб-службами не настолько функциональны, как серверы типа IIS, Apache или Nginx.</span><span class="sxs-lookup"><span data-stu-id="c04dd-128">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="c04dd-129">Обратный прокси-сервер может облегчить такую работу, как обслуживание статического содержимого, кэширование запросов, сжатие запросов и завершение SSL с HTTP-сервера.</span><span class="sxs-lookup"><span data-stu-id="c04dd-129">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="c04dd-130">Обратный прокси-сервер можно разместить на отдельном компьютере или развернуть параллельно с HTTP-сервером.</span><span class="sxs-lookup"><span data-stu-id="c04dd-130">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="c04dd-131">В контексте данного руководства используется отдельный экземпляр Nginx.</span><span class="sxs-lookup"><span data-stu-id="c04dd-131">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="c04dd-132">Он выполняется на том же сервере, что и HTTP-сервер.</span><span class="sxs-lookup"><span data-stu-id="c04dd-132">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="c04dd-133">Настройки можно выбирать в зависимости от требований.</span><span class="sxs-lookup"><span data-stu-id="c04dd-133">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="c04dd-134">Так как запросы перенаправляются обратным прокси-сервером, используйте ПО промежуточного слоя перенаправления заголовков, которое входит в пакет [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/).</span><span class="sxs-lookup"><span data-stu-id="c04dd-134">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="c04dd-135">Это ПО обновляет `Request.Scheme`, используя заголовок `X-Forwarded-Proto`, что обеспечивает правильную работу URI перенаправления и других политик безопасности.</span><span class="sxs-lookup"><span data-stu-id="c04dd-135">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="c04dd-136">Вне зависимости от того, какой тип ПО промежуточного слоя используется для проверки подлинности, сначала необходимо запустить ПО промежуточного слоя перенаправления заголовков.</span><span class="sxs-lookup"><span data-stu-id="c04dd-136">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="c04dd-137">Такой порядок гарантирует, что ПО промежуточного слоя для проверки подлинности может использовать значения заголовков и сформирует правильные URI перенаправления.</span><span class="sxs-lookup"><span data-stu-id="c04dd-137">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c04dd-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c04dd-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c04dd-139">Вызовите метод [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) в `Startup.Configure`, прежде чем вызвать [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) или другое ПО промежуточного слоя для проверки подлинности по аналогичной схеме.</span><span class="sxs-lookup"><span data-stu-id="c04dd-139">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="c04dd-140">Настройте ПО промежуточного слоя для перенаправления заголовков `X-Forwarded-For` и `X-Forwarded-Proto`:</span><span class="sxs-lookup"><span data-stu-id="c04dd-140">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c04dd-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c04dd-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c04dd-142">Вызывайте метод [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) в `Startup.Configure`, прежде чем вызвать [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity), [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) или другое ПО промежуточного слоя для проверки подлинности по аналогичной схеме.</span><span class="sxs-lookup"><span data-stu-id="c04dd-142">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="c04dd-143">Настройте ПО промежуточного слоя для перенаправления заголовков `X-Forwarded-For` и `X-Forwarded-Proto`:</span><span class="sxs-lookup"><span data-stu-id="c04dd-143">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="c04dd-144">Если параметр [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) не задан для ПО промежуточного слоя, по умолчанию перенаправляются заголовки `None`.</span><span class="sxs-lookup"><span data-stu-id="c04dd-144">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="c04dd-145">Для приложений, размещенных за прокси-серверами и подсистемами балансировки нагрузки, может потребоваться дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="c04dd-145">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="c04dd-146">Дополнительные сведения см. в разделе [Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="c04dd-146">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-nginx"></a><span data-ttu-id="c04dd-147">Установка Nginx</span><span class="sxs-lookup"><span data-stu-id="c04dd-147">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="c04dd-148">Если будут установлены дополнительные модули Nginx, может потребоваться создание Nginx из источника.</span><span class="sxs-lookup"><span data-stu-id="c04dd-148">If optional Nginx modules will be installed, building Nginx from source might be required.</span></span>

<span data-ttu-id="c04dd-149">Установите Nginx с помощью команды `apt-get`.</span><span class="sxs-lookup"><span data-stu-id="c04dd-149">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="c04dd-150">Программа установки создает сценарий инициализации System V, который запускает Nginx как управляющую программу Nginx при запуске системы.</span><span class="sxs-lookup"><span data-stu-id="c04dd-150">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="c04dd-151">Так как Nginx устанавливается впервые, запустите его напрямую, выполнив следующую команду.</span><span class="sxs-lookup"><span data-stu-id="c04dd-151">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="c04dd-152">В браузере должна открыться стартовая страница Nginx по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="c04dd-152">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="c04dd-153">Настройка Nginx</span><span class="sxs-lookup"><span data-stu-id="c04dd-153">Configure Nginx</span></span>

<span data-ttu-id="c04dd-154">Чтобы настроить Nginx как обратный прокси-сервер для перенаправления запросов в наше приложение ASP.NET Core, измените файл */etc/nginx/sites-available/default*.</span><span class="sxs-lookup"><span data-stu-id="c04dd-154">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="c04dd-155">Откройте этот файл в текстовом редакторе и замените его содержимое на следующий код.</span><span class="sxs-lookup"><span data-stu-id="c04dd-155">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

<span data-ttu-id="c04dd-156">Если отсутствуют совпадения для `server_name`, Nginx использует сервер по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="c04dd-156">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="c04dd-157">Если сервер по умолчанию не определен, первый сервер в файле конфигурации является сервером по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="c04dd-157">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="c04dd-158">Рекомендуется добавить в качестве сервера по умолчанию определенный сервер, который возвращает код состояния 444 в файле конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c04dd-158">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="c04dd-159">Ниже приведен пример конфигурации сервера по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="c04dd-159">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="c04dd-160">С представленным выше файлом конфигурации и сервером по умолчанию Nginx принимает трафик от любого источника через порт 80 с заголовком узла `example.com` или `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="c04dd-160">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="c04dd-161">Запросы, не соответствующие этим узлам, не будут перенаправляться в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c04dd-161">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="c04dd-162">Запросы, которые им соответствуют, Nginx перенаправляет в Kestrel по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c04dd-162">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="c04dd-163">Для получения дополнительной информации см. статью [Как nginx обрабатывает запросы](https://nginx.org/docs/http/request_processing.html).</span><span class="sxs-lookup"><span data-stu-id="c04dd-163">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span>

> [!WARNING]
> <span data-ttu-id="c04dd-164">Если не будет указана правильная [директива server_name](https://nginx.org/docs/http/server_names.html), приложение будет подвержено значительным уязвимостям.</span><span class="sxs-lookup"><span data-stu-id="c04dd-164">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="c04dd-165">Привязки с подстановочными знаками на уровне дочерних доменов (например, `*.example.com`) не создают таких угроз безопасности, если вы полностью контролируете родительский домен (в отличие от варианта `*.com`, создающего уязвимость).</span><span class="sxs-lookup"><span data-stu-id="c04dd-165">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="c04dd-166">Дополнительные сведения см. в документе [rfc7230, раздел 5.4](https://tools.ietf.org/html/rfc7230#section-5.4).</span><span class="sxs-lookup"><span data-stu-id="c04dd-166">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="c04dd-167">Установив конфигурацию Nginx, выполните команду `sudo nginx -t`, чтобы проверить синтаксис файлов конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c04dd-167">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="c04dd-168">Если проверка файла конфигурации прошла успешно, заставьте Nginx принять изменения, выполнив команду `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="c04dd-168">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="c04dd-169">Мониторинг приложения</span><span class="sxs-lookup"><span data-stu-id="c04dd-169">Monitoring the app</span></span>

<span data-ttu-id="c04dd-170">Сервер настроен на перенаправление запросов к `http://<serveraddress>:80` в приложение ASP.NET Core, выполняемое в Kestrel по адресу `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="c04dd-170">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="c04dd-171">При этом Nginx не настроен для управления процессом Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c04dd-171">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="c04dd-172">Для запуска и мониторинга соответствующего веб-приложения можно использовать *systemd* и создать файл службы.</span><span class="sxs-lookup"><span data-stu-id="c04dd-172">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="c04dd-173">*systemd* — это система инициализации, предоставляющая различные функции для запуска и остановки процессов, а также управления ими.</span><span class="sxs-lookup"><span data-stu-id="c04dd-173">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="c04dd-174">Создание файла службы</span><span class="sxs-lookup"><span data-stu-id="c04dd-174">Create the service file</span></span>

<span data-ttu-id="c04dd-175">Создайте файл определения службы.</span><span class="sxs-lookup"><span data-stu-id="c04dd-175">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="c04dd-176">Далее представлен пример файла службы для нашего приложения.</span><span class="sxs-lookup"><span data-stu-id="c04dd-176">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="c04dd-177">Если пользователь *www-data* в конфигурации не используется, необходимо сначала создать определенного в примере пользователя, а затем предоставить ему необходимые права владения для файлов.</span><span class="sxs-lookup"><span data-stu-id="c04dd-177">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="c04dd-178">Linux имеет файловую систему, в которой учитывается регистр символов.</span><span class="sxs-lookup"><span data-stu-id="c04dd-178">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="c04dd-179">Если для параметра ASPNETCORE_ENVIRONMENT установить значение Production, будет выполнен поиск файла конфигурации *appsettings.Production.json*, а не файла *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="c04dd-179">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="c04dd-180">Некоторые значения (например, строки подключения SQL) необходимо экранировать, чтобы поставщики конфигурации могли читать переменные среды.</span><span class="sxs-lookup"><span data-stu-id="c04dd-180">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="c04dd-181">Используйте следующую команду, чтобы создать правильно экранированное значение для файла конфигурации:</span><span class="sxs-lookup"><span data-stu-id="c04dd-181">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="c04dd-182">Сохраните файл и включите службу.</span><span class="sxs-lookup"><span data-stu-id="c04dd-182">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="c04dd-183">Запустите службу и убедитесь, что она работает.</span><span class="sxs-lookup"><span data-stu-id="c04dd-183">Start the service and verify that it's running.</span></span>

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="c04dd-184">Теперь, когда обратный прокси-сервер настроен и systemd управляет процессом Kestrel, веб-приложение можно считать полностью настроенным и вы можете обратиться к нему по адресу `http://localhost` из браузера на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="c04dd-184">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="c04dd-185">Оно также доступно для удаленных компьютеров, несмотря на наличие блокирующих трафик межсетевых экранов.</span><span class="sxs-lookup"><span data-stu-id="c04dd-185">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="c04dd-186">Заголовок `Server` в ответе подтверждает, что приложение ASP.NET Core обслуживается Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c04dd-186">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="c04dd-187">Просмотр журналов</span><span class="sxs-lookup"><span data-stu-id="c04dd-187">Viewing logs</span></span>

<span data-ttu-id="c04dd-188">Так как веб-приложение, использующее Kestrel, управляется через `systemd`, все события и процессы регистрируются в централизованном журнале.</span><span class="sxs-lookup"><span data-stu-id="c04dd-188">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="c04dd-189">При этом журнал содержит все записи обо всех службах и процессах, управляемых `systemd`.</span><span class="sxs-lookup"><span data-stu-id="c04dd-189">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="c04dd-190">Чтобы просмотреть элементы, связанные с `kestrel-hellomvc.service`, используйте следующую команду.</span><span class="sxs-lookup"><span data-stu-id="c04dd-190">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="c04dd-191">Кроме того, количество возвращаемых записей можно уменьшить, указав параметры времени, например `--since today`, `--until 1 hour ago` или их комбинацию.</span><span class="sxs-lookup"><span data-stu-id="c04dd-191">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="c04dd-192">Защита приложения</span><span class="sxs-lookup"><span data-stu-id="c04dd-192">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="c04dd-193">Включение AppArmor</span><span class="sxs-lookup"><span data-stu-id="c04dd-193">Enable AppArmor</span></span>

<span data-ttu-id="c04dd-194">Начиная с версии 2.6 ядро Linux включает платформу модулей безопасности Linux (LSM).</span><span class="sxs-lookup"><span data-stu-id="c04dd-194">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="c04dd-195">LSM поддерживают различные реализации модулей безопасности.</span><span class="sxs-lookup"><span data-stu-id="c04dd-195">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="c04dd-196">[AppArmor](https://wiki.ubuntu.com/AppArmor) — это LSM, который реализует систему обязательного контроля доступа, позволяющую ограничивать программу определенным набором ресурсов.</span><span class="sxs-lookup"><span data-stu-id="c04dd-196">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="c04dd-197">Убедитесь, что AppArmor включен и правильно настроен.</span><span class="sxs-lookup"><span data-stu-id="c04dd-197">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="c04dd-198">Настройка межсетевого экрана</span><span class="sxs-lookup"><span data-stu-id="c04dd-198">Configuring the firewall</span></span>

<span data-ttu-id="c04dd-199">Закройте все внешние порты, которые не используются.</span><span class="sxs-lookup"><span data-stu-id="c04dd-199">Close off all external ports that are not in use.</span></span> <span data-ttu-id="c04dd-200">Незамысловатый межсетевой экран (ufw) позволяет взаимодействовать с `iptables`, предоставляя интерфейс командной строки для настройки межсетевого экрана.</span><span class="sxs-lookup"><span data-stu-id="c04dd-200">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="c04dd-201">Убедитесь, что настройки `ufw` пропускают трафик на все необходимые порты.</span><span class="sxs-lookup"><span data-stu-id="c04dd-201">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="c04dd-202">Защита Nginx</span><span class="sxs-lookup"><span data-stu-id="c04dd-202">Securing Nginx</span></span>

<span data-ttu-id="c04dd-203">В дистрибутиве Nginx по умолчанию SSL не включен.</span><span class="sxs-lookup"><span data-stu-id="c04dd-203">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="c04dd-204">Чтобы включить дополнительные функции безопасности, выполните сборку из исходного файла.</span><span class="sxs-lookup"><span data-stu-id="c04dd-204">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="c04dd-205">Загрузка исходного файла и установка зависимостей сборки</span><span class="sxs-lookup"><span data-stu-id="c04dd-205">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="c04dd-206">Изменение имени ответа Nginx</span><span class="sxs-lookup"><span data-stu-id="c04dd-206">Change the Nginx response name</span></span>

<span data-ttu-id="c04dd-207">Внесите изменения в файл *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="c04dd-207">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="c04dd-208">Настройка параметров и сборки</span><span class="sxs-lookup"><span data-stu-id="c04dd-208">Configure the options and build</span></span>

<span data-ttu-id="c04dd-209">Для регулярных выражений требуется библиотека PCRE.</span><span class="sxs-lookup"><span data-stu-id="c04dd-209">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="c04dd-210">Регулярные выражения используются в директиве местонахождения для модуля ngx_http_rewrite_module.</span><span class="sxs-lookup"><span data-stu-id="c04dd-210">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="c04dd-211">Модуль http_ssl_module добавляет поддержку протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c04dd-211">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="c04dd-212">Для дополнительной защиты приложения можно использовать межсетевой экран для веб-приложений, например *ModSecurity*.</span><span class="sxs-lookup"><span data-stu-id="c04dd-212">Consider using a web app firewall like *ModSecurity* to harden the app.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="c04dd-213">Настройка SSL</span><span class="sxs-lookup"><span data-stu-id="c04dd-213">Configure SSL</span></span>

* <span data-ttu-id="c04dd-214">Настройте сервер для прослушивания трафика HTTPS через порт `443`, указав действительный сертификат, выпущенный доверенным центром сертификации (ЦС).</span><span class="sxs-lookup"><span data-stu-id="c04dd-214">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="c04dd-215">Обеспечьте дополнительную защиту, применив некоторые методы, показанные в представленном ниже файле */etc/nginx/nginx.conf*.</span><span class="sxs-lookup"><span data-stu-id="c04dd-215">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="c04dd-216">Это может быть выбор более строгого шифра и перенаправление всего HTTP-трафика в HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c04dd-216">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="c04dd-217">При добавлении заголовка `HTTP Strict-Transport-Security` (HSTS) все последующие запросы клиента будут проходить только по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c04dd-217">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="c04dd-218">Если в дальнейшем планируется отключить SSL, не добавляйте заголовок Strict-Transport-Security или выберите соответствующий `max-age`.</span><span class="sxs-lookup"><span data-stu-id="c04dd-218">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="c04dd-219">Добавьте файл конфигурации */etc/nginx/proxy.conf*:</span><span class="sxs-lookup"><span data-stu-id="c04dd-219">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="c04dd-220">Измените файл конфигурации */etc/nginx/proxy.conf*.</span><span class="sxs-lookup"><span data-stu-id="c04dd-220">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="c04dd-221">В этом примере показаны разделы `http` и `server` одного и того же файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c04dd-221">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="c04dd-222">Защита Nginx от кликджекинга</span><span class="sxs-lookup"><span data-stu-id="c04dd-222">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="c04dd-223">Кликджекинг — это способ обмана, предназначенный для перехвата кликов пострадавшего пользователя.</span><span class="sxs-lookup"><span data-stu-id="c04dd-223">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="c04dd-224">Кликджекинг обманом заставляет пользователей (посетителей) щелкать зараженные сайты.</span><span class="sxs-lookup"><span data-stu-id="c04dd-224">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="c04dd-225">Для защиты сайта используйте X-FRAME-OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="c04dd-225">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="c04dd-226">Измените файл *nginx.conf*.</span><span class="sxs-lookup"><span data-stu-id="c04dd-226">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="c04dd-227">Добавьте строку `add_header X-Frame-Options "SAMEORIGIN";` и сохраните файл, а затем перезапустите Nginx.</span><span class="sxs-lookup"><span data-stu-id="c04dd-227">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="c04dd-228">Сканирование типа MIME</span><span class="sxs-lookup"><span data-stu-id="c04dd-228">MIME-type sniffing</span></span>

<span data-ttu-id="c04dd-229">Этот заголовок предотвращает MIME-сканирование ответов с указанным типом содержимого в большинстве браузеров, запрещая браузеру переопределять тип содержимого ответа.</span><span class="sxs-lookup"><span data-stu-id="c04dd-229">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="c04dd-230">Параметр `nosniff` означает, что, если сервер определяет содержимое как text/html, браузер будет обрабатывать его как text/html.</span><span class="sxs-lookup"><span data-stu-id="c04dd-230">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="c04dd-231">Измените файл *nginx.conf*.</span><span class="sxs-lookup"><span data-stu-id="c04dd-231">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="c04dd-232">Добавьте строку `add_header X-Content-Type-Options "nosniff";` и сохраните файл, а затем перезапустите Nginx.</span><span class="sxs-lookup"><span data-stu-id="c04dd-232">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
