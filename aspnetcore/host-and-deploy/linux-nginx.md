---
title: "Среда размещения ASP.NET Core в операционной системе Linux с Nginx"
author: rick-anderson
description: "Описание процедуры настройки Nginx в качестве обратного прокси-сервера на 16.04 Ubuntu для пересылки трафика HTTP в веб-приложение ASP.NET Core ОС Kestrel."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 08/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 1044a87a4dcc7636413078b0fc09ade206c97d0a
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/11/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="cc610-103">Среда размещения ASP.NET Core в операционной системе Linux с Nginx</span><span class="sxs-lookup"><span data-stu-id="cc610-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="cc610-104">Автор [Сурабх Ширхатти](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="cc610-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="cc610-105">В этом руководстве описывается настройка готовой к работе среды ASP.NET Core на сервере 16.04 Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="cc610-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

<span data-ttu-id="cc610-106">**Примечание:** для Ubuntu 14.04 *supervisord* рекомендуется в качестве решения для мониторинга процесса Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cc610-106">**Note:** For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="cc610-107">*systemd* не доступен на Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="cc610-107">*systemd* isn't available on Ubuntu 14.04.</span></span> [<span data-ttu-id="cc610-108">См. предыдущую версию этого документа</span><span class="sxs-lookup"><span data-stu-id="cc610-108">See previous version of this document</span></span>](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

<span data-ttu-id="cc610-109">В этом руководстве рассматривается</span><span class="sxs-lookup"><span data-stu-id="cc610-109">This guide:</span></span>

* <span data-ttu-id="cc610-110">Помещает существующего приложения ASP.NET Core за обратного прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="cc610-110">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="cc610-111">Настраивает обратного прокси-сервера для перенаправления запросов на Kestrel веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="cc610-111">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="cc610-112">Гарантирует, что веб-приложение запускается при запуске как управляющая программа.</span><span class="sxs-lookup"><span data-stu-id="cc610-112">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="cc610-113">Настраивает средство управления процессами, чтобы перезапустить веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="cc610-113">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cc610-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="cc610-114">Prerequisites</span></span>

1. <span data-ttu-id="cc610-115">Доступ к серверу Ubuntu 16.04 под учетной записи обычного пользователя с правами sudo</span><span class="sxs-lookup"><span data-stu-id="cc610-115">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
1. <span data-ttu-id="cc610-116">Существующие приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cc610-116">An existing ASP.NET Core app</span></span>

## <a name="copy-over-the-app"></a><span data-ttu-id="cc610-117">Скопировать приложение</span><span class="sxs-lookup"><span data-stu-id="cc610-117">Copy over the app</span></span>

<span data-ttu-id="cc610-118">Выполните из среды разработки команду `dotnet publish`, чтобы упаковать приложение в отдельный каталог, который можно запустить на сервере.</span><span class="sxs-lookup"><span data-stu-id="cc610-118">Run `dotnet publish` from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="cc610-119">Скопируйте приложения ASP.NET Core на сервер с помощью того средства, интегрируется в рабочий процесс в организации (например, точка подключения службы, FTP).</span><span class="sxs-lookup"><span data-stu-id="cc610-119">Copy the ASP.NET Core app to the server using whatever tool integrates into the organization's workflow (for example, SCP, FTP).</span></span> <span data-ttu-id="cc610-120">Протестируйте приложение, например</span><span class="sxs-lookup"><span data-stu-id="cc610-120">Test the app, for example:</span></span>

* <span data-ttu-id="cc610-121">Запустите из командной строки, `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="cc610-121">From the command line, run `dotnet <app_assembly>.dll`.</span></span>
* <span data-ttu-id="cc610-122">В браузере откройте страницу `http://<serveraddress>:<port>`, чтобы убедиться, что приложение работает на платформе Linux.</span><span class="sxs-lookup"><span data-stu-id="cc610-122">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="cc610-123">Настройка обратного прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="cc610-123">Configure a reverse proxy server</span></span>

<span data-ttu-id="cc610-124">Обратный прокси-сервер общих настроен для обслуживания динамических веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="cc610-124">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="cc610-125">Обратный прокси-сервер завершает HTTP-запроса и отправляет его в приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cc610-125">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="cc610-126">Для чего нужен обратный прокси-сервер?</span><span class="sxs-lookup"><span data-stu-id="cc610-126">Why use a reverse proxy server?</span></span>

<span data-ttu-id="cc610-127">Kestrel отлично подходит для обслуживания динамического содержимого из ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cc610-127">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="cc610-128">Однако возможности обслуживания web не обладая такими широкими возможностями функции в качестве серверов, таких как IIS, Apache или Nginx.</span><span class="sxs-lookup"><span data-stu-id="cc610-128">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="cc610-129">Обратного прокси-сервера можно перенаправить работу работы, такие как обслуживание статического содержимого, кэширование запросов, сжатие запросов и завершения запросов SSL с HTTP-сервера.</span><span class="sxs-lookup"><span data-stu-id="cc610-129">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="cc610-130">Обратный прокси-сервер можно разместить на отдельном компьютере или развернуть параллельно с HTTP-сервером.</span><span class="sxs-lookup"><span data-stu-id="cc610-130">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="cc610-131">В контексте данного руководства используется отдельный экземпляр Nginx.</span><span class="sxs-lookup"><span data-stu-id="cc610-131">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="cc610-132">Он выполняется на том же сервере, что и HTTP-сервер.</span><span class="sxs-lookup"><span data-stu-id="cc610-132">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="cc610-133">В соответствии с требованиями различных установка может быть выбираться.</span><span class="sxs-lookup"><span data-stu-id="cc610-133">Based on requirements, a different setup may be choosen.</span></span>

<span data-ttu-id="cc610-134">Поскольку запросы перенаправляются обратного прокси-сервера, с помощью перенаправленных заголовки по промежуточного слоя из [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) пакета.</span><span class="sxs-lookup"><span data-stu-id="cc610-134">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="cc610-135">Обновления по промежуточного слоя `Request.Scheme`, с использованием `X-Forwarded-Proto` заголовок, чтобы правильно работать, идентификаторы URI перенаправления и другие политики безопасности.</span><span class="sxs-lookup"><span data-stu-id="cc610-135">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="cc610-136">При использовании любого типа проверки подлинности по промежуточного слоя, необходимо запустить сначала по промежуточного слоя перенаправленных заголовки.</span><span class="sxs-lookup"><span data-stu-id="cc610-136">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="cc610-137">Этот гарантирует, что по промежуточного слоя проверки подлинности можно использовать значения заголовка и создавать правильный перенаправления идентификаторы URI.</span><span class="sxs-lookup"><span data-stu-id="cc610-137">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="cc610-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="cc610-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="cc610-139">Вызвать [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) метод в `Startup.Configure` перед вызовом [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) или аналогичные схему проверки подлинности по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="cc610-139">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="cc610-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="cc610-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="cc610-141">Вызвать [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) метод в `Startup.Configure` перед вызовом [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) и [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) или аналогичные схему проверки подлинности по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="cc610-141">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware:</span></span>

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

<span data-ttu-id="cc610-142">Если не [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) для указания по промежуточного слоя, заголовки по умолчанию для пересылки `None`.</span><span class="sxs-lookup"><span data-stu-id="cc610-142">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

### <a name="install-nginx"></a><span data-ttu-id="cc610-143">Установка Nginx</span><span class="sxs-lookup"><span data-stu-id="cc610-143">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="cc610-144">Если дополнительных модулей Nginx будут установлены, может потребоваться создание Nginx из источника.</span><span class="sxs-lookup"><span data-stu-id="cc610-144">If optional Nginx modules will be installed, building Nginx from source might be required.</span></span>

<span data-ttu-id="cc610-145">Установите Nginx с помощью команды `apt-get`.</span><span class="sxs-lookup"><span data-stu-id="cc610-145">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="cc610-146">Программа установки создает сценарий инициализации System V, который запускает Nginx как управляющую программу Nginx при запуске системы.</span><span class="sxs-lookup"><span data-stu-id="cc610-146">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="cc610-147">Так как Nginx устанавливается впервые, запустите его напрямую, выполнив следующую команду.</span><span class="sxs-lookup"><span data-stu-id="cc610-147">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="cc610-148">В браузере должна открыться стартовая страница Nginx по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cc610-148">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="cc610-149">Настройка Nginx</span><span class="sxs-lookup"><span data-stu-id="cc610-149">Configure Nginx</span></span>

<span data-ttu-id="cc610-150">Чтобы настроить в качестве обратного прокси-сервера для пересылки запросов нашего приложения ASP.NET Core Nginx, измените `/etc/nginx/sites-available/default`.</span><span class="sxs-lookup"><span data-stu-id="cc610-150">To configure Nginx as a reverse proxy to forward requests to our ASP.NET Core app, modify `/etc/nginx/sites-available/default`.</span></span> <span data-ttu-id="cc610-151">Откройте этот файл в текстовом редакторе и замените его содержимое на следующий код.</span><span class="sxs-lookup"><span data-stu-id="cc610-151">Open it in a text editor, and replace the contents with the following:</span></span>

```
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

<span data-ttu-id="cc610-152">Этот файл конфигурации Nginx перенаправляет входящий публичный трафик с порта `80` на порт `5000`.</span><span class="sxs-lookup"><span data-stu-id="cc610-152">This Nginx configuration file forwards incoming public traffic from port `80` to port `5000`.</span></span>

<span data-ttu-id="cc610-153">После установления Nginx конфигурации запуска `sudo nginx -t` проверить синтаксис файлов конфигурации.</span><span class="sxs-lookup"><span data-stu-id="cc610-153">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="cc610-154">Если проверка файла конфигурации прошла успешно, принудительно Nginx, чтобы принять изменения, запустив `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="cc610-154">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="cc610-155">Мониторинг приложений</span><span class="sxs-lookup"><span data-stu-id="cc610-155">Monitoring the app</span></span>

<span data-ttu-id="cc610-156">Сервер настроен для пересылки запросов к `http://<serveraddress>:80` на ASP.NET Core приложение, работающее на Kestrel на `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="cc610-156">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="cc610-157">Однако Nginx настроена для управления процессом Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cc610-157">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="cc610-158">*systemd* можно использовать для создания файла службы для запуска и наблюдения за базовой веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="cc610-158">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="cc610-159">*systemd* — это система инициализации, предоставляющая различные функции для запуска и остановки процессов, а также управления ими.</span><span class="sxs-lookup"><span data-stu-id="cc610-159">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="cc610-160">Создание файла службы</span><span class="sxs-lookup"><span data-stu-id="cc610-160">Create the service file</span></span>

<span data-ttu-id="cc610-161">Создайте файл определения службы.</span><span class="sxs-lookup"><span data-stu-id="cc610-161">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="cc610-162">Ниже приведен пример файла службы для приложения.</span><span class="sxs-lookup"><span data-stu-id="cc610-162">The following is an example service file for the app:</span></span>

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

<span data-ttu-id="cc610-163">**Примечание:** Если пользователь *www данных* не используется в конфигурации пользовательские необходимо сначала создать и получает соответствующие права владения для файлов.</span><span class="sxs-lookup"><span data-stu-id="cc610-163">**Note:** If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>
<span data-ttu-id="cc610-164">**Примечание:** Linux содержит с учетом регистра файловую систему.</span><span class="sxs-lookup"><span data-stu-id="cc610-164">**Note:** Linux has a case-sensitive file system.</span></span> <span data-ttu-id="cc610-165">Параметр ASPNETCORE_ENVIRONMENT поиск файла конфигурации приводит к «Production» *appsettings. Production.JSON*, а не *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="cc610-165">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

<span data-ttu-id="cc610-166">Сохраните файл и включите службу.</span><span class="sxs-lookup"><span data-stu-id="cc610-166">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="cc610-167">Запустите службу и убедитесь, что он работает.</span><span class="sxs-lookup"><span data-stu-id="cc610-167">Start the service and verify that it's running.</span></span>

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

<span data-ttu-id="cc610-168">Обратный прокси-сервер настроен и управляемых с помощью systemd Kestrel, веб-приложения, полностью настроена и может осуществляться из браузера на локальном компьютере, на `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="cc610-168">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="cc610-169">Он также доступен с удаленного компьютера, за исключением выполнения любой брандмауэр может блокировать.</span><span class="sxs-lookup"><span data-stu-id="cc610-169">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="cc610-170">Проверка заголовки ответа `Server` заголовок показывает предоставляемый Kestrel приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cc610-170">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="cc610-171">Просмотр журналов</span><span class="sxs-lookup"><span data-stu-id="cc610-171">Viewing logs</span></span>

<span data-ttu-id="cc610-172">Поскольку веб-приложения с помощью Kestrel управляется с помощью `systemd`, для централизованного журнала регистрируются все события и процессов.</span><span class="sxs-lookup"><span data-stu-id="cc610-172">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="cc610-173">При этом журнал содержит все записи обо всех службах и процессах, управляемых `systemd`.</span><span class="sxs-lookup"><span data-stu-id="cc610-173">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="cc610-174">Чтобы просмотреть элементы, связанные с `kestrel-hellomvc.service`, используйте следующую команду.</span><span class="sxs-lookup"><span data-stu-id="cc610-174">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="cc610-175">Кроме того, количество возвращаемых записей можно уменьшить, указав параметры времени, например `--since today`, `--until 1 hour ago` или их комбинацию.</span><span class="sxs-lookup"><span data-stu-id="cc610-175">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="cc610-176">Обеспечение безопасности приложения</span><span class="sxs-lookup"><span data-stu-id="cc610-176">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="cc610-177">Включение AppArmor</span><span class="sxs-lookup"><span data-stu-id="cc610-177">Enable AppArmor</span></span>

<span data-ttu-id="cc610-178">Модули безопасности Linux (LSM) — это платформа, который является частью ядро Linux с момента Linux 2.6.</span><span class="sxs-lookup"><span data-stu-id="cc610-178">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="cc610-179">LSM поддерживают различные реализации модулей безопасности.</span><span class="sxs-lookup"><span data-stu-id="cc610-179">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="cc610-180">[AppArmor](https://wiki.ubuntu.com/AppArmor) — это LSM, который реализует систему обязательного контроля доступа, позволяющую ограничивать программу определенным набором ресурсов.</span><span class="sxs-lookup"><span data-stu-id="cc610-180">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="cc610-181">Убедитесь, что AppArmor включен и правильно настроен.</span><span class="sxs-lookup"><span data-stu-id="cc610-181">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="cc610-182">Настройка брандмауэра</span><span class="sxs-lookup"><span data-stu-id="cc610-182">Configuring the firewall</span></span>

<span data-ttu-id="cc610-183">Закройте все внешние порты, которые не используются.</span><span class="sxs-lookup"><span data-stu-id="cc610-183">Close off all external ports that are not in use.</span></span> <span data-ttu-id="cc610-184">Незамысловатый межсетевой экран (ufw) позволяет взаимодействовать с `iptables`, предоставляя интерфейс командной строки для настройки межсетевого экрана.</span><span class="sxs-lookup"><span data-stu-id="cc610-184">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="cc610-185">Убедитесь, что `ufw` настроен для разрешения трафика на все необходимые порты.</span><span class="sxs-lookup"><span data-stu-id="cc610-185">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="cc610-186">Защита Nginx</span><span class="sxs-lookup"><span data-stu-id="cc610-186">Securing Nginx</span></span>

<span data-ttu-id="cc610-187">В дистрибутиве Nginx по умолчанию SSL не включен.</span><span class="sxs-lookup"><span data-stu-id="cc610-187">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="cc610-188">Чтобы включить дополнительные функции безопасности, выполните сборку из исходного файла.</span><span class="sxs-lookup"><span data-stu-id="cc610-188">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="cc610-189">Загрузка исходного файла и установка зависимостей сборки</span><span class="sxs-lookup"><span data-stu-id="cc610-189">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="cc610-190">Изменение имени ответа Nginx</span><span class="sxs-lookup"><span data-stu-id="cc610-190">Change the Nginx response name</span></span>

<span data-ttu-id="cc610-191">Внесите изменения в файл *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="cc610-191">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="cc610-192">Настройка параметров и сборки</span><span class="sxs-lookup"><span data-stu-id="cc610-192">Configure the options and build</span></span>

<span data-ttu-id="cc610-193">Для регулярных выражений требуется библиотека PCRE.</span><span class="sxs-lookup"><span data-stu-id="cc610-193">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="cc610-194">Регулярные выражения используются в директиве местонахождения для модуля ngx_http_rewrite_module.</span><span class="sxs-lookup"><span data-stu-id="cc610-194">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="cc610-195">Модуль http_ssl_module добавляет поддержку протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cc610-195">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="cc610-196">Рассмотрите возможность использования брандмауэра web app как *ModSecurity* для усиления защиты приложения.</span><span class="sxs-lookup"><span data-stu-id="cc610-196">Consider using a web app firewall like *ModSecurity* to harden the app.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="cc610-197">Настройка SSL</span><span class="sxs-lookup"><span data-stu-id="cc610-197">Configure SSL</span></span>

* <span data-ttu-id="cc610-198">Настройка сервера для прослушивания трафика HTTPS на порте `443` , указав действительный сертификат, выпущенный доверенного центра сертификации (ЦС).</span><span class="sxs-lookup"><span data-stu-id="cc610-198">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="cc610-199">Улучшение безопасности при использовании некоторые рекомендации, приведенные в следующей */etc/nginx/nginx.conf* файла.</span><span class="sxs-lookup"><span data-stu-id="cc610-199">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="cc610-200">Это может быть выбор более строгого шифра и перенаправление всего HTTP-трафика в HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cc610-200">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="cc610-201">При добавлении заголовка `HTTP Strict-Transport-Security` (HSTS) все последующие запросы клиента будут проходить только по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cc610-201">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="cc610-202">Не добавляйте в заголовке безопасности для транспорта Strict или с соответствующей `max-age` Если SSL будет отключена в будущем.</span><span class="sxs-lookup"><span data-stu-id="cc610-202">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="cc610-203">Добавьте файл конфигурации */etc/nginx/proxy.conf*:</span><span class="sxs-lookup"><span data-stu-id="cc610-203">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[Main](linux-nginx/proxy.conf)]

<span data-ttu-id="cc610-204">Измените файл конфигурации */etc/nginx/proxy.conf*.</span><span class="sxs-lookup"><span data-stu-id="cc610-204">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="cc610-205">В этом примере показаны разделы `http` и `server` одного и того же файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="cc610-205">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[Main](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="cc610-206">Защита Nginx от кликджекинга</span><span class="sxs-lookup"><span data-stu-id="cc610-206">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="cc610-207">Кликджекинг — это способ обмана, предназначенный для перехвата кликов пострадавшего пользователя.</span><span class="sxs-lookup"><span data-stu-id="cc610-207">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="cc610-208">Кликджекинг обманом заставляет пользователей (посетителей) щелкать зараженные сайты.</span><span class="sxs-lookup"><span data-stu-id="cc610-208">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="cc610-209">Использование X-FRAME-OPTIONS для защиты узла.</span><span class="sxs-lookup"><span data-stu-id="cc610-209">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="cc610-210">Измените файл *nginx.conf*.</span><span class="sxs-lookup"><span data-stu-id="cc610-210">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="cc610-211">Добавьте строку `add_header X-Frame-Options "SAMEORIGIN";` и сохраните файл, а затем перезапустите Nginx.</span><span class="sxs-lookup"><span data-stu-id="cc610-211">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="cc610-212">Сканирование типа MIME</span><span class="sxs-lookup"><span data-stu-id="cc610-212">MIME-type sniffing</span></span>

<span data-ttu-id="cc610-213">Этот заголовок предотвращает MIME-сканирование ответов с указанным типом содержимого в большинстве браузеров, запрещая браузеру переопределять тип содержимого ответа.</span><span class="sxs-lookup"><span data-stu-id="cc610-213">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="cc610-214">Параметр `nosniff` означает, что, если сервер определяет содержимое как text/html, браузер будет обрабатывать его как text/html.</span><span class="sxs-lookup"><span data-stu-id="cc610-214">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="cc610-215">Измените файл *nginx.conf*.</span><span class="sxs-lookup"><span data-stu-id="cc610-215">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="cc610-216">Добавьте строку `add_header X-Content-Type-Options "nosniff";` и сохраните файл, а затем перезапустите Nginx.</span><span class="sxs-lookup"><span data-stu-id="cc610-216">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
