---
title: "Размещение ASP.NET Core в операционной системе Linux с Apache"
description: "Дополнительные сведения о настроить Apache как обратного прокси-сервера на CentOS для перенаправления трафика HTTP к веб-приложению ASP.NET Core ОС Kestrel."
author: spboyer
manager: wpickett
ms.author: spboyer
ms.custom: mvc
ms.date: 10/19/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-apache
ms.openlocfilehash: b11bc811b6aefce22b60a28afd72c2a2d0b26955
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="4d942-103">Размещение ASP.NET Core в операционной системе Linux с Apache</span><span class="sxs-lookup"><span data-stu-id="4d942-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="4d942-104">Автор: [Шейн Бойер (Shayne Boyer)](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="4d942-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="4d942-105">С помощью этого руководства, узнайте, как настроить [Apache](https://httpd.apache.org/) в качестве обратного прокси-сервера на [CentOS 7](https://www.centos.org/) для перенаправления трафика HTTP к веб-приложению ASP.NET Core ОС [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="4d942-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="4d942-106">[Mod_proxy расширения](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) и связанных модулей создание обратного прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="4d942-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4d942-107">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="4d942-107">Prerequisites</span></span>

1. <span data-ttu-id="4d942-108">Сервер под управлением CentOS 7 с помощью учетной записи обычного пользователя с правами sudo</span><span class="sxs-lookup"><span data-stu-id="4d942-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="4d942-109">Приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4d942-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="4d942-110">Публикация приложения</span><span class="sxs-lookup"><span data-stu-id="4d942-110">Publish the app</span></span>

<span data-ttu-id="4d942-111">Опубликуйте приложение в качестве [автономное развертывание](/dotnet/core/deploying/#self-contained-deployments-scd) в конфигурации выпуска для среды выполнения CentOS 7 (`centos.7-x64`).</span><span class="sxs-lookup"><span data-stu-id="4d942-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="4d942-112">Скопируйте содержимое *bin/Release/netcoreapp2.0/centos.7-x64/publish* папки на сервере с помощью точки подключения службы, FTP или другой метод передачи файлов.</span><span class="sxs-lookup"><span data-stu-id="4d942-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="4d942-113">В рабочих сценариях развертывания непрерывной интеграции рабочего процесса выполняет работу, публикации приложения и копировании ресурсов на сервере.</span><span class="sxs-lookup"><span data-stu-id="4d942-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="4d942-114">Настройка прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="4d942-114">Configure a proxy server</span></span>

<span data-ttu-id="4d942-115">Обратный прокси-сервер общих настроен для обслуживания динамических веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="4d942-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="4d942-116">Обратный прокси-сервер завершает HTTP-запроса и отправляет его в приложение ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4d942-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="4d942-117">Прокси-сервер, называется источник, который перенаправляет запросы клиента на другой сервер вместо самого выполнении запроса.</span><span class="sxs-lookup"><span data-stu-id="4d942-117">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="4d942-118">Обратный прокси-сервер перенаправляет запросы в фиксированное назначение обычно от имени клиентов.</span><span class="sxs-lookup"><span data-stu-id="4d942-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="4d942-119">В этом руководстве Apache настраивается как обратный прокси-сервер запущен на том же сервере, что Kestrel обслуживает приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4d942-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="4d942-120">Поскольку запросы перенаправляются обратного прокси-сервера, с помощью перенаправленных заголовки по промежуточного слоя из [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) пакета.</span><span class="sxs-lookup"><span data-stu-id="4d942-120">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="4d942-121">Обновления по промежуточного слоя `Request.Scheme`, с использованием `X-Forwarded-Proto` заголовок, чтобы правильно работать, идентификаторы URI перенаправления и другие политики безопасности.</span><span class="sxs-lookup"><span data-stu-id="4d942-121">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="4d942-122">При использовании любого типа проверки подлинности по промежуточного слоя, необходимо запустить сначала по промежуточного слоя перенаправленных заголовки.</span><span class="sxs-lookup"><span data-stu-id="4d942-122">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="4d942-123">Этот гарантирует, что по промежуточного слоя проверки подлинности можно использовать значения заголовка и создавать правильный перенаправления идентификаторы URI.</span><span class="sxs-lookup"><span data-stu-id="4d942-123">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4d942-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4d942-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4d942-125">Вызвать [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) метод в `Startup.Configure` перед вызовом [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) или аналогичные схему проверки подлинности по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="4d942-125">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4d942-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4d942-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4d942-127">Вызвать [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) метод в `Startup.Configure` перед вызовом [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) и [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) или аналогичные схему проверки подлинности по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="4d942-127">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware:</span></span>

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

<span data-ttu-id="4d942-128">Если не [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) для указания по промежуточного слоя, заголовки по умолчанию для пересылки `None`.</span><span class="sxs-lookup"><span data-stu-id="4d942-128">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

### <a name="install-apache"></a><span data-ttu-id="4d942-129">Установка Apache</span><span class="sxs-lookup"><span data-stu-id="4d942-129">Install Apache</span></span>

<span data-ttu-id="4d942-130">Пакеты обновлений CentOS их последней стабильной версии:</span><span class="sxs-lookup"><span data-stu-id="4d942-130">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="4d942-131">Установить веб-сервер Apache на CentOS с одним `yum` команды:</span><span class="sxs-lookup"><span data-stu-id="4d942-131">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="4d942-132">Пример выходных данных после выполнения команды.</span><span class="sxs-lookup"><span data-stu-id="4d942-132">Sample output after running the command:</span></span>

```bash
Downloading packages:
httpd-2.4.6-40.el7.centos.4.x86_64.rpm               | 2.7 MB  00:00:01
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
Installing : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 
Verifying  : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 

Installed:
httpd.x86_64 0:2.4.6-40.el7.centos.4

Complete!
```

> [!NOTE]
> <span data-ttu-id="4d942-133">В этом примере выходных данных отражает httpd.86_64, так как 64-разрядные версии CentOS 7.</span><span class="sxs-lookup"><span data-stu-id="4d942-133">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="4d942-134">Чтобы проверить, где установлен Apache, выполните `whereis httpd` из командной строки.</span><span class="sxs-lookup"><span data-stu-id="4d942-134">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span> 

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="4d942-135">Настройка Apache в качестве обратного прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="4d942-135">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="4d942-136">Файлы конфигурации для Apache находятся в каталоге `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="4d942-136">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="4d942-137">Все файлы с *.conf* расширения обрабатывается в алфавитном порядке, кроме файлов конфигурации модуля в `/etc/httpd/conf.modules.d/`, содержащий какой-либо настройки файлы для загрузки модулей.</span><span class="sxs-lookup"><span data-stu-id="4d942-137">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="4d942-138">Создайте файл конфигурации для приложения с именем `hellomvc.conf`:</span><span class="sxs-lookup"><span data-stu-id="4d942-138">Create a configuration file for the app named `hellomvc.conf`:</span></span>

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="4d942-139">**VirtualHost** узел может отображаться несколько раз в один или несколько файлов на сервере.</span><span class="sxs-lookup"><span data-stu-id="4d942-139">The **VirtualHost** node can appear multiple times in one or more files on a server.</span></span> <span data-ttu-id="4d942-140">**VirtualHost** равно прослушивать все IP-адрес, который использует порт 80.</span><span class="sxs-lookup"><span data-stu-id="4d942-140">**VirtualHost** is set to listen on any IP address using port 80.</span></span> <span data-ttu-id="4d942-141">Следующие две строки, задаются на запросы прокси-сервера в корневом каталоге, чтобы на сервер по адресу 127.0.0.1 на порт 5000.</span><span class="sxs-lookup"><span data-stu-id="4d942-141">The next two lines are set to proxy requests at the root to the server at 127.0.0.1 on port 5000.</span></span> <span data-ttu-id="4d942-142">Для двусторонней связи *ProxyPass* и *ProxyPassReverse* являются обязательными.</span><span class="sxs-lookup"><span data-stu-id="4d942-142">For bi-directional communication, *ProxyPass* and *ProxyPassReverse* are required.</span></span>

<span data-ttu-id="4d942-143">Можно настроить ведение журнала для каждого **VirtualHost** с помощью **ErrorLog** и **CustomLog** директивы.</span><span class="sxs-lookup"><span data-stu-id="4d942-143">Logging can be configured per **VirtualHost** using **ErrorLog** and **CustomLog** directives.</span></span> <span data-ttu-id="4d942-144">**ErrorLog** — это расположение, где сервер регистрирует ошибки, и **CustomLog** задает имя файла и формат файла журнала.</span><span class="sxs-lookup"><span data-stu-id="4d942-144">**ErrorLog** is the location where the server logs errors, and **CustomLog** sets the filename and format of log file.</span></span> <span data-ttu-id="4d942-145">В этом случае это где записываются сведения запроса.</span><span class="sxs-lookup"><span data-stu-id="4d942-145">In this case, this is where request information is logged.</span></span> <span data-ttu-id="4d942-146">Имеется одна строка для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="4d942-146">There's one line for each request.</span></span>

<span data-ttu-id="4d942-147">Сохраните файл и протестировать конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="4d942-147">Save the file and test the configuration.</span></span> <span data-ttu-id="4d942-148">Если проверка выполнена успешно, ответ должен быть `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="4d942-148">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="4d942-149">Перезапустите Apache:</span><span class="sxs-lookup"><span data-stu-id="4d942-149">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="4d942-150">Мониторинг приложений</span><span class="sxs-lookup"><span data-stu-id="4d942-150">Monitoring the app</span></span>

<span data-ttu-id="4d942-151">Apache сейчас программу установки, чтобы перенаправлять запросы, адресованные `http://localhost:80` для ASP.NET Core приложение, работающее на Kestrel на `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="4d942-151">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="4d942-152">Однако Apache настроена для управления процессом Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4d942-152">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="4d942-153">Используйте *systemd* и создайте файл службы для запуска и наблюдения за базовой веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="4d942-153">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="4d942-154">*systemd* — это система инициализации, предоставляющая различные функции для запуска и остановки процессов, а также управления ими.</span><span class="sxs-lookup"><span data-stu-id="4d942-154">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="4d942-155">Создание файла службы</span><span class="sxs-lookup"><span data-stu-id="4d942-155">Create the service file</span></span>

<span data-ttu-id="4d942-156">Создайте файл определения службы.</span><span class="sxs-lookup"><span data-stu-id="4d942-156">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="4d942-157">Пример файла службы для приложения:</span><span class="sxs-lookup"><span data-stu-id="4d942-157">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if dotnet service crashes
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> <span data-ttu-id="4d942-158">**Пользователь** &mdash; Если пользователь *apache* не используется в конфигурации пользователь должен сначала создать и получает соответствующие права владения для файлов.</span><span class="sxs-lookup"><span data-stu-id="4d942-158">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="4d942-159">Сохраните файл и включите службы:</span><span class="sxs-lookup"><span data-stu-id="4d942-159">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="4d942-160">Запустите службу и проверьте, работает ли:</span><span class="sxs-lookup"><span data-stu-id="4d942-160">Start the service and verify that it's running:</span></span>

```bash
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="4d942-161">Обратный прокси-сервер настроен и Kestrel, управляемых с помощью *systemd*, веб-приложения, полностью настроена и может осуществляться из браузера на локальном компьютере в `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="4d942-161">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="4d942-162">Проверка заголовки ответа **сервера** заголовок указывает, что приложения ASP.NET Core обслуживается Kestrel:</span><span class="sxs-lookup"><span data-stu-id="4d942-162">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="4d942-163">Просмотр журналов</span><span class="sxs-lookup"><span data-stu-id="4d942-163">Viewing logs</span></span>

<span data-ttu-id="4d942-164">Поскольку веб-приложения с помощью Kestrel управляется с помощью *systemd*, для централизованного журнала регистрируются события и процессов.</span><span class="sxs-lookup"><span data-stu-id="4d942-164">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="4d942-165">Тем не менее, этот журнал содержит записи для всех служб и процессов, управляемых *systemd*.</span><span class="sxs-lookup"><span data-stu-id="4d942-165">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="4d942-166">Чтобы просмотреть элементы, связанные с `kestrel-hellomvc.service`, используйте следующую команду.</span><span class="sxs-lookup"><span data-stu-id="4d942-166">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="4d942-167">Для фильтрации времени, укажите параметры времени с помощью команды.</span><span class="sxs-lookup"><span data-stu-id="4d942-167">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="4d942-168">Например, использовать `--since today` для фильтрации в настоящее время или `--until 1 hour ago` для просмотра операций предыдущий час.</span><span class="sxs-lookup"><span data-stu-id="4d942-168">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="4d942-169">Дополнительные сведения см. в разделе [для получения journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="4d942-169">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="4d942-170">Обеспечение безопасности приложения</span><span class="sxs-lookup"><span data-stu-id="4d942-170">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="4d942-171">Настройка брандмауэра</span><span class="sxs-lookup"><span data-stu-id="4d942-171">Configure firewall</span></span>

<span data-ttu-id="4d942-172">*Firewalld* — динамический управляющая программа для управления брандмауэром с поддержкой зон сети.</span><span class="sxs-lookup"><span data-stu-id="4d942-172">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="4d942-173">Порты и фильтрации пакетов можно по-прежнему осуществлять утилита iptables.</span><span class="sxs-lookup"><span data-stu-id="4d942-173">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="4d942-174">*Firewalld* должен быть установлен по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="4d942-174">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="4d942-175">`yum` можно использовать для установки пакета или убедитесь, что она установлена.</span><span class="sxs-lookup"><span data-stu-id="4d942-175">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="4d942-176">Используйте `firewalld` открыть только порты, необходимые для приложения.</span><span class="sxs-lookup"><span data-stu-id="4d942-176">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="4d942-177">В этом случае используются порты 80 и 443.</span><span class="sxs-lookup"><span data-stu-id="4d942-177">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="4d942-178">Порты 80 и 443, чтобы открыть окончательно заданы следующие команды:</span><span class="sxs-lookup"><span data-stu-id="4d942-178">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="4d942-179">Загрузить параметры брандмауэра.</span><span class="sxs-lookup"><span data-stu-id="4d942-179">Reload the firewall settings.</span></span> <span data-ttu-id="4d942-180">Проверьте доступные службы и порты зоны по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="4d942-180">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="4d942-181">Проверка кода доступны параметры `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="4d942-181">Options are available by inspecting `firewall-cmd -h`.</span></span>

```bash 
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

```bash
public (default, active)
interfaces: eth0
sources: 
services: dhcpv6-client
ports: 443/tcp 80/tcp
masquerade: no
forward-ports: 
icmp-blocks: 
rich rules: 
```

### <a name="ssl-configuration"></a><span data-ttu-id="4d942-182">Конфигурация SSL</span><span class="sxs-lookup"><span data-stu-id="4d942-182">SSL configuration</span></span>

<span data-ttu-id="4d942-183">Чтобы настроить Apache для SSL, *mod_ssl* используется модуль.</span><span class="sxs-lookup"><span data-stu-id="4d942-183">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="4d942-184">Когда *httpd* модуль был установлен, *mod_ssl* модуль также был установлен.</span><span class="sxs-lookup"><span data-stu-id="4d942-184">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="4d942-185">Если он не был установлен, используйте `yum` Чтобы добавить его в конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="4d942-185">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="4d942-186">Чтобы принудительно использовать SSL, установите `mod_rewrite` для включения перезаписи URL-адресов:</span><span class="sxs-lookup"><span data-stu-id="4d942-186">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="4d942-187">Изменить *hellomvc.conf* файл, чтобы включить перезаписи URL-адресов и безопасный обмен данными через порт 443:</span><span class="sxs-lookup"><span data-stu-id="4d942-187">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

> [!NOTE]
> <span data-ttu-id="4d942-188">В этом примере используется локально создаваемого сертификата.</span><span class="sxs-lookup"><span data-stu-id="4d942-188">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="4d942-189">**SSLCertificateFile** должен быть файлом основного сертификата для доменного имени.</span><span class="sxs-lookup"><span data-stu-id="4d942-189">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="4d942-190">**SSLCertificateKeyFile** файл ключа генерацию при создании CSR.</span><span class="sxs-lookup"><span data-stu-id="4d942-190">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="4d942-191">**SSLCertificateChainFile** должен быть файл промежуточного сертификата (если таковые имеются), предоставленный центром сертификации.</span><span class="sxs-lookup"><span data-stu-id="4d942-191">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="4d942-192">Сохраните файл и проверьте правильность конфигурации:</span><span class="sxs-lookup"><span data-stu-id="4d942-192">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="4d942-193">Перезапустите Apache:</span><span class="sxs-lookup"><span data-stu-id="4d942-193">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="4d942-194">Дополнительные предложения Apache</span><span class="sxs-lookup"><span data-stu-id="4d942-194">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="4d942-195">Дополнительные заголовки</span><span class="sxs-lookup"><span data-stu-id="4d942-195">Additional headers</span></span>

<span data-ttu-id="4d942-196">Для защиты от вредоносных атак, существует несколько заголовков, которые следует быть изменены или добавлены.</span><span class="sxs-lookup"><span data-stu-id="4d942-196">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="4d942-197">Убедитесь, что `mod_headers` модуль установлен:</span><span class="sxs-lookup"><span data-stu-id="4d942-197">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="4d942-198">Защита от атак clickjacking Apache</span><span class="sxs-lookup"><span data-stu-id="4d942-198">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="4d942-199">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), также известных как *пользовательского интерфейса redress атаки*, является вредоносной атаки, где браузера обманным путем щелчок ссылки или кнопки на другую страницу, не посещается в данный момент.</span><span class="sxs-lookup"><span data-stu-id="4d942-199">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="4d942-200">Используйте `X-FRAME-OPTIONS` для защиты узла.</span><span class="sxs-lookup"><span data-stu-id="4d942-200">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="4d942-201">Изменить *httpd.conf* файла:</span><span class="sxs-lookup"><span data-stu-id="4d942-201">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="4d942-202">Добавьте строку `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="4d942-202">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="4d942-203">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="4d942-203">Save the file.</span></span> <span data-ttu-id="4d942-204">Перезапустите Apache.</span><span class="sxs-lookup"><span data-stu-id="4d942-204">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="4d942-205">Сканирование типа MIME</span><span class="sxs-lookup"><span data-stu-id="4d942-205">MIME-type sniffing</span></span>

<span data-ttu-id="4d942-206">`X-Content-Type-Options` Заголовок предотвращает Internet Explorer из *сканирование MIME* (определение файла `Content-Type` из содержимого файла).</span><span class="sxs-lookup"><span data-stu-id="4d942-206">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="4d942-207">Если сервер задает `Content-Type` заголовок `text/html` с `nosniff` набор параметров, Internet Explorer отображает содержимое как `text/html` независимо от его содержимого.</span><span class="sxs-lookup"><span data-stu-id="4d942-207">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="4d942-208">Изменить *httpd.conf* файла:</span><span class="sxs-lookup"><span data-stu-id="4d942-208">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="4d942-209">Добавьте строку `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="4d942-209">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="4d942-210">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="4d942-210">Save the file.</span></span> <span data-ttu-id="4d942-211">Перезапустите Apache.</span><span class="sxs-lookup"><span data-stu-id="4d942-211">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="4d942-212">Балансировка нагрузки</span><span class="sxs-lookup"><span data-stu-id="4d942-212">Load Balancing</span></span> 

<span data-ttu-id="4d942-213">В этом примере показано, как установить и настроить Apache в CentOS 7 и Kestrel на том же компьютере.</span><span class="sxs-lookup"><span data-stu-id="4d942-213">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="4d942-214">Чтобы не имеют одной точки сбоя; с помощью *mod_proxy_balancer* и изменение **VirtualHost** позволит обеспечить управление несколькими экземплярами веб-приложений за прокси-сервера Apache.</span><span class="sxs-lookup"><span data-stu-id="4d942-214">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="4d942-215">В файле конфигурации показано ниже, дополнительный экземпляр `hellomvc` приложения настроен для запуска на порту 5001.</span><span class="sxs-lookup"><span data-stu-id="4d942-215">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="4d942-216">*Прокси* раздел, задайте для балансировки нагрузки с конфигурацией подсистемы балансировки с двумя членами *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="4d942-216">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPass / balancer://mycluster/ 

    ProxyPassReverse / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5001/

    <Proxy balancer://mycluster>
        BalancerMember http://127.0.0.1:5000
        BalancerMember http://127.0.0.1:5001 
        ProxySet lbmethod=byrequests
    </Proxy>

    <Location />
        SetHandler balancer
    </Location>
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a><span data-ttu-id="4d942-217">Ограничения скорости</span><span class="sxs-lookup"><span data-stu-id="4d942-217">Rate Limits</span></span>
<span data-ttu-id="4d942-218">С помощью *mod_ratelimit*, который входит в *httpd* модуля, пропускную способность для клиентов может быть ограничен:</span><span class="sxs-lookup"><span data-stu-id="4d942-218">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="4d942-219">Пример файла ограничивает пропускную способность как 600 КБ в секунду в корневой папке:</span><span class="sxs-lookup"><span data-stu-id="4d942-219">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
