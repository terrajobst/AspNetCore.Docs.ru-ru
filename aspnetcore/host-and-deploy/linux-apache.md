---
title: Размещение ASP.NET Core в операционной системе Linux с Apache
description: Дополнительные сведения о настроить Apache как обратного прокси-сервера на CentOS для перенаправления трафика HTTP к веб-приложению ASP.NET Core ОС Kestrel.
author: spboyer
manager: wpickett
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 5a8a035ff3f127d01655888d4f83a871645b0bf5
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="82d7e-103">Размещение ASP.NET Core в операционной системе Linux с Apache</span><span class="sxs-lookup"><span data-stu-id="82d7e-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="82d7e-104">Автор: [Шейн Бойер (Shayne Boyer)](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="82d7e-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="82d7e-105">С помощью этого руководства, узнайте, как настроить [Apache](https://httpd.apache.org/) в качестве обратного прокси-сервера на [CentOS 7](https://www.centos.org/) для перенаправления трафика HTTP к веб-приложению ASP.NET Core ОС [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="82d7e-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="82d7e-106">[Mod_proxy расширения](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) и связанных модулей создание обратного прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="82d7e-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="82d7e-107">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="82d7e-107">Prerequisites</span></span>

1. <span data-ttu-id="82d7e-108">Сервер под управлением CentOS 7 с помощью учетной записи обычного пользователя с правами sudo</span><span class="sxs-lookup"><span data-stu-id="82d7e-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="82d7e-109">Приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="82d7e-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="82d7e-110">Публикация приложения</span><span class="sxs-lookup"><span data-stu-id="82d7e-110">Publish the app</span></span>

<span data-ttu-id="82d7e-111">Опубликуйте приложение в качестве [автономное развертывание](/dotnet/core/deploying/#self-contained-deployments-scd) в конфигурации выпуска для среды выполнения CentOS 7 (`centos.7-x64`).</span><span class="sxs-lookup"><span data-stu-id="82d7e-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="82d7e-112">Скопируйте содержимое *bin/Release/netcoreapp2.0/centos.7-x64/publish* папки на сервере с помощью точки подключения службы, FTP или другой метод передачи файлов.</span><span class="sxs-lookup"><span data-stu-id="82d7e-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="82d7e-113">В рабочих сценариях развертывания непрерывной интеграции рабочего процесса выполняет работу, публикации приложения и копировании ресурсов на сервере.</span><span class="sxs-lookup"><span data-stu-id="82d7e-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="82d7e-114">Настройка прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="82d7e-114">Configure a proxy server</span></span>

<span data-ttu-id="82d7e-115">Обратный прокси-сервер общих настроен для обслуживания динамических веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="82d7e-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="82d7e-116">Обратный прокси-сервер завершает HTTP-запроса и отправляет его в приложение ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="82d7e-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="82d7e-117">Прокси-сервер, называется источник, который перенаправляет запросы клиента на другой сервер вместо самого выполнении запроса.</span><span class="sxs-lookup"><span data-stu-id="82d7e-117">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="82d7e-118">Обратный прокси-сервер перенаправляет запросы в фиксированное назначение обычно от имени клиентов.</span><span class="sxs-lookup"><span data-stu-id="82d7e-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="82d7e-119">В этом руководстве Apache настраивается как обратный прокси-сервер запущен на том же сервере, что Kestrel обслуживает приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="82d7e-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="82d7e-120">Поскольку запросы перенаправляются обратного прокси-сервера, с помощью перенаправленных заголовки по промежуточного слоя из [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) пакета.</span><span class="sxs-lookup"><span data-stu-id="82d7e-120">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="82d7e-121">Обновления по промежуточного слоя `Request.Scheme`, с использованием `X-Forwarded-Proto` заголовок, чтобы правильно работать, идентификаторы URI перенаправления и другие политики безопасности.</span><span class="sxs-lookup"><span data-stu-id="82d7e-121">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="82d7e-122">При использовании любого типа проверки подлинности по промежуточного слоя, необходимо запустить сначала по промежуточного слоя перенаправленных заголовки.</span><span class="sxs-lookup"><span data-stu-id="82d7e-122">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="82d7e-123">Этот гарантирует, что по промежуточного слоя проверки подлинности можно использовать значения заголовка и создавать правильный перенаправления идентификаторы URI.</span><span class="sxs-lookup"><span data-stu-id="82d7e-123">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="82d7e-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="82d7e-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="82d7e-125">Вызвать [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) метод в `Startup.Configure` перед вызовом [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) или аналогичные схему проверки подлинности по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="82d7e-125">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="82d7e-126">Настройки по промежуточного слоя для пересылки `X-Forwarded-For` и `X-Forwarded-Proto` заголовки:</span><span class="sxs-lookup"><span data-stu-id="82d7e-126">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="82d7e-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="82d7e-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="82d7e-128">Вызвать [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) метод в `Startup.Configure` перед вызовом [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) и [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) или аналогичные схему проверки подлинности по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="82d7e-128">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="82d7e-129">Настройки по промежуточного слоя для пересылки `X-Forwarded-For` и `X-Forwarded-Proto` заголовки:</span><span class="sxs-lookup"><span data-stu-id="82d7e-129">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="82d7e-130">Если не [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) для указания по промежуточного слоя, заголовки по умолчанию для пересылки `None`.</span><span class="sxs-lookup"><span data-stu-id="82d7e-130">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="82d7e-131">Для приложений, размещенных за прокси-серверы и подсистемы балансировки нагрузки, может потребоваться дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="82d7e-131">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="82d7e-132">Дополнительные сведения см. в разделе [Настройка ASP.NET Core для работы с прокси-серверы и подсистем балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="82d7e-132">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-apache"></a><span data-ttu-id="82d7e-133">Установка Apache</span><span class="sxs-lookup"><span data-stu-id="82d7e-133">Install Apache</span></span>

<span data-ttu-id="82d7e-134">Пакеты обновлений CentOS их последней стабильной версии:</span><span class="sxs-lookup"><span data-stu-id="82d7e-134">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="82d7e-135">Установить веб-сервер Apache на CentOS с одним `yum` команды:</span><span class="sxs-lookup"><span data-stu-id="82d7e-135">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="82d7e-136">Пример выходных данных после выполнения команды.</span><span class="sxs-lookup"><span data-stu-id="82d7e-136">Sample output after running the command:</span></span>

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
> <span data-ttu-id="82d7e-137">В этом примере выходных данных отражает httpd.86_64, так как 64-разрядные версии CentOS 7.</span><span class="sxs-lookup"><span data-stu-id="82d7e-137">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="82d7e-138">Чтобы проверить, где установлен Apache, выполните `whereis httpd` из командной строки.</span><span class="sxs-lookup"><span data-stu-id="82d7e-138">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="82d7e-139">Настройка Apache в качестве обратного прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="82d7e-139">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="82d7e-140">Файлы конфигурации для Apache находятся в каталоге `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="82d7e-140">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="82d7e-141">Все файлы с *.conf* расширения обрабатывается в алфавитном порядке, кроме файлов конфигурации модуля в `/etc/httpd/conf.modules.d/`, содержащий какой-либо настройки файлы для загрузки модулей.</span><span class="sxs-lookup"><span data-stu-id="82d7e-141">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="82d7e-142">Создайте файл конфигурации с именем *hellomvc.conf*, для приложения:</span><span class="sxs-lookup"><span data-stu-id="82d7e-142">Create a configuration file, named *hellomvc.conf*, for the app:</span></span>

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ServerName www.example.com
    ServerAlias *.example.com
    ErrorLog ${APACHE_LOG_DIR}hellomvc-error.log
    CustomLog ${APACHE_LOG_DIR}hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="82d7e-143">`VirtualHost` Блок может встречаться несколько раз в один или несколько файлов на сервере.</span><span class="sxs-lookup"><span data-stu-id="82d7e-143">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="82d7e-144">В файле конфигурации выше Apache принимает общего трафика через порт 80.</span><span class="sxs-lookup"><span data-stu-id="82d7e-144">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="82d7e-145">Домен `www.example.com` предоставляется и `*.example.com` псевдоним разрешается в веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="82d7e-145">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="82d7e-146">В разделе [поддержки на основании имени виртуального узла](https://httpd.apache.org/docs/current/vhosts/name-based.html) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="82d7e-146">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="82d7e-147">Запросы выполняются через прокси, в корне порт 5000 сервера на 127.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="82d7e-147">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="82d7e-148">Для двусторонней связи `ProxyPass` и `ProxyPassReverse` являются обязательными.</span><span class="sxs-lookup"><span data-stu-id="82d7e-148">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span>

> [!WARNING]
> <span data-ttu-id="82d7e-149">Если не задать строгим [директива ServerName](https://httpd.apache.org/docs/current/mod/core.html#servername) в **VirtualHost** блок предоставляет доступ приложения к уязвимостям системы безопасности.</span><span class="sxs-lookup"><span data-stu-id="82d7e-149">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="82d7e-150">Привязки поддомен подстановочный знак (например, `*.example.com`) не вызывает риск безопасности, если вы можете управлять всей родительского домена (в отличие от `*.com`, которой уязвима).</span><span class="sxs-lookup"><span data-stu-id="82d7e-150">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="82d7e-151">Дополнительные сведения см. в документе [rfc7230, раздел 5.4](https://tools.ietf.org/html/rfc7230#section-5.4).</span><span class="sxs-lookup"><span data-stu-id="82d7e-151">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="82d7e-152">Можно настроить ведение журнала для каждого `VirtualHost` с помощью `ErrorLog` и `CustomLog` директивы.</span><span class="sxs-lookup"><span data-stu-id="82d7e-152">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="82d7e-153">`ErrorLog` расположение, где сервер регистрирует ошибки, и `CustomLog` задает имя файла и формат файла журнала.</span><span class="sxs-lookup"><span data-stu-id="82d7e-153">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="82d7e-154">В этом случае это где записываются сведения запроса.</span><span class="sxs-lookup"><span data-stu-id="82d7e-154">In this case, this is where request information is logged.</span></span> <span data-ttu-id="82d7e-155">Имеется одна строка для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="82d7e-155">There's one line for each request.</span></span>

<span data-ttu-id="82d7e-156">Сохраните файл и протестировать конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="82d7e-156">Save the file and test the configuration.</span></span> <span data-ttu-id="82d7e-157">Если проверка выполнена успешно, ответ должен быть `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="82d7e-157">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="82d7e-158">Перезапустите Apache:</span><span class="sxs-lookup"><span data-stu-id="82d7e-158">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="82d7e-159">Мониторинг приложений</span><span class="sxs-lookup"><span data-stu-id="82d7e-159">Monitoring the app</span></span>

<span data-ttu-id="82d7e-160">Apache сейчас программу установки, чтобы перенаправлять запросы, адресованные `http://localhost:80` для ASP.NET Core приложение, работающее на Kestrel на `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="82d7e-160">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="82d7e-161">Однако Apache настроена для управления процессом Kestrel.</span><span class="sxs-lookup"><span data-stu-id="82d7e-161">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="82d7e-162">Используйте *systemd* и создайте файл службы для запуска и наблюдения за базовой веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="82d7e-162">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="82d7e-163">*systemd* — это система инициализации, предоставляющая различные функции для запуска и остановки процессов, а также управления ими.</span><span class="sxs-lookup"><span data-stu-id="82d7e-163">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="82d7e-164">Создание файла службы</span><span class="sxs-lookup"><span data-stu-id="82d7e-164">Create the service file</span></span>

<span data-ttu-id="82d7e-165">Создайте файл определения службы.</span><span class="sxs-lookup"><span data-stu-id="82d7e-165">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="82d7e-166">Пример файла службы для приложения:</span><span class="sxs-lookup"><span data-stu-id="82d7e-166">An example service file for the app:</span></span>

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
> <span data-ttu-id="82d7e-167">**Пользователь** &mdash; Если пользователь *apache* не используется в конфигурации пользователь должен сначала создать и получает соответствующие права владения для файлов.</span><span class="sxs-lookup"><span data-stu-id="82d7e-167">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="82d7e-168">Сохраните файл и включите службы:</span><span class="sxs-lookup"><span data-stu-id="82d7e-168">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="82d7e-169">Запустите службу и проверьте, работает ли:</span><span class="sxs-lookup"><span data-stu-id="82d7e-169">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="82d7e-170">Обратный прокси-сервер настроен и Kestrel, управляемых с помощью *systemd*, веб-приложения, полностью настроена и может осуществляться из браузера на локальном компьютере в `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="82d7e-170">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="82d7e-171">Проверка заголовки ответа **сервера** заголовок указывает, что приложения ASP.NET Core обслуживается Kestrel:</span><span class="sxs-lookup"><span data-stu-id="82d7e-171">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="82d7e-172">Просмотр журналов</span><span class="sxs-lookup"><span data-stu-id="82d7e-172">Viewing logs</span></span>

<span data-ttu-id="82d7e-173">Поскольку веб-приложения с помощью Kestrel управляется с помощью *systemd*, для централизованного журнала регистрируются события и процессов.</span><span class="sxs-lookup"><span data-stu-id="82d7e-173">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="82d7e-174">Тем не менее, этот журнал содержит записи для всех служб и процессов, управляемых *systemd*.</span><span class="sxs-lookup"><span data-stu-id="82d7e-174">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="82d7e-175">Чтобы просмотреть элементы, связанные с `kestrel-hellomvc.service`, используйте следующую команду.</span><span class="sxs-lookup"><span data-stu-id="82d7e-175">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="82d7e-176">Для фильтрации времени, укажите параметры времени с помощью команды.</span><span class="sxs-lookup"><span data-stu-id="82d7e-176">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="82d7e-177">Например, использовать `--since today` для фильтрации в настоящее время или `--until 1 hour ago` для просмотра операций предыдущий час.</span><span class="sxs-lookup"><span data-stu-id="82d7e-177">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="82d7e-178">Дополнительные сведения см. в разделе [для получения journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="82d7e-178">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="82d7e-179">Обеспечение безопасности приложения</span><span class="sxs-lookup"><span data-stu-id="82d7e-179">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="82d7e-180">Настройка брандмауэра</span><span class="sxs-lookup"><span data-stu-id="82d7e-180">Configure firewall</span></span>

<span data-ttu-id="82d7e-181">*Firewalld* — динамический управляющая программа для управления брандмауэром с поддержкой зон сети.</span><span class="sxs-lookup"><span data-stu-id="82d7e-181">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="82d7e-182">Порты и фильтрации пакетов можно по-прежнему осуществлять утилита iptables.</span><span class="sxs-lookup"><span data-stu-id="82d7e-182">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="82d7e-183">*Firewalld* должен быть установлен по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="82d7e-183">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="82d7e-184">`yum` можно использовать для установки пакета или убедитесь, что она установлена.</span><span class="sxs-lookup"><span data-stu-id="82d7e-184">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="82d7e-185">Используйте `firewalld` открыть только порты, необходимые для приложения.</span><span class="sxs-lookup"><span data-stu-id="82d7e-185">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="82d7e-186">В этом случае используются порты 80 и 443.</span><span class="sxs-lookup"><span data-stu-id="82d7e-186">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="82d7e-187">Порты 80 и 443, чтобы открыть окончательно заданы следующие команды:</span><span class="sxs-lookup"><span data-stu-id="82d7e-187">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="82d7e-188">Загрузить параметры брандмауэра.</span><span class="sxs-lookup"><span data-stu-id="82d7e-188">Reload the firewall settings.</span></span> <span data-ttu-id="82d7e-189">Проверьте доступные службы и порты зоны по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="82d7e-189">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="82d7e-190">Проверка кода доступны параметры `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="82d7e-190">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="82d7e-191">Конфигурация SSL</span><span class="sxs-lookup"><span data-stu-id="82d7e-191">SSL configuration</span></span>

<span data-ttu-id="82d7e-192">Чтобы настроить Apache для SSL, *mod_ssl* используется модуль.</span><span class="sxs-lookup"><span data-stu-id="82d7e-192">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="82d7e-193">Когда *httpd* модуль был установлен, *mod_ssl* модуль также был установлен.</span><span class="sxs-lookup"><span data-stu-id="82d7e-193">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="82d7e-194">Если он не был установлен, используйте `yum` Чтобы добавить его в конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="82d7e-194">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="82d7e-195">Чтобы принудительно использовать SSL, установите `mod_rewrite` для включения перезаписи URL-адресов:</span><span class="sxs-lookup"><span data-stu-id="82d7e-195">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="82d7e-196">Изменить *hellomvc.conf* файл, чтобы включить перезаписи URL-адресов и безопасный обмен данными через порт 443:</span><span class="sxs-lookup"><span data-stu-id="82d7e-196">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
> <span data-ttu-id="82d7e-197">В этом примере используется локально создаваемого сертификата.</span><span class="sxs-lookup"><span data-stu-id="82d7e-197">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="82d7e-198">**SSLCertificateFile** должен быть файлом основного сертификата для доменного имени.</span><span class="sxs-lookup"><span data-stu-id="82d7e-198">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="82d7e-199">**SSLCertificateKeyFile** файл ключа генерацию при создании CSR.</span><span class="sxs-lookup"><span data-stu-id="82d7e-199">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="82d7e-200">**SSLCertificateChainFile** должен быть файл промежуточного сертификата (если таковые имеются), предоставленный центром сертификации.</span><span class="sxs-lookup"><span data-stu-id="82d7e-200">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="82d7e-201">Сохраните файл и проверьте правильность конфигурации:</span><span class="sxs-lookup"><span data-stu-id="82d7e-201">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="82d7e-202">Перезапустите Apache:</span><span class="sxs-lookup"><span data-stu-id="82d7e-202">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="82d7e-203">Дополнительные предложения Apache</span><span class="sxs-lookup"><span data-stu-id="82d7e-203">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="82d7e-204">Дополнительные заголовки</span><span class="sxs-lookup"><span data-stu-id="82d7e-204">Additional headers</span></span>

<span data-ttu-id="82d7e-205">Для защиты от вредоносных атак, существует несколько заголовков, которые следует быть изменены или добавлены.</span><span class="sxs-lookup"><span data-stu-id="82d7e-205">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="82d7e-206">Убедитесь, что `mod_headers` модуль установлен:</span><span class="sxs-lookup"><span data-stu-id="82d7e-206">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="82d7e-207">Защита от атак clickjacking Apache</span><span class="sxs-lookup"><span data-stu-id="82d7e-207">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="82d7e-208">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), также известных как *пользовательского интерфейса redress атаки*, является вредоносной атаки, где браузера обманным путем щелчок ссылки или кнопки на другую страницу, не посещается в данный момент.</span><span class="sxs-lookup"><span data-stu-id="82d7e-208">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="82d7e-209">Используйте `X-FRAME-OPTIONS` для защиты узла.</span><span class="sxs-lookup"><span data-stu-id="82d7e-209">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="82d7e-210">Изменить *httpd.conf* файла:</span><span class="sxs-lookup"><span data-stu-id="82d7e-210">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="82d7e-211">Добавьте строку `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="82d7e-211">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="82d7e-212">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="82d7e-212">Save the file.</span></span> <span data-ttu-id="82d7e-213">Перезапустите Apache.</span><span class="sxs-lookup"><span data-stu-id="82d7e-213">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="82d7e-214">Сканирование типа MIME</span><span class="sxs-lookup"><span data-stu-id="82d7e-214">MIME-type sniffing</span></span>

<span data-ttu-id="82d7e-215">`X-Content-Type-Options` Заголовок предотвращает Internet Explorer из *сканирование MIME* (определение файла `Content-Type` из содержимого файла).</span><span class="sxs-lookup"><span data-stu-id="82d7e-215">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="82d7e-216">Если сервер задает `Content-Type` заголовок `text/html` с `nosniff` набор параметров, Internet Explorer отображает содержимое как `text/html` независимо от его содержимого.</span><span class="sxs-lookup"><span data-stu-id="82d7e-216">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="82d7e-217">Изменить *httpd.conf* файла:</span><span class="sxs-lookup"><span data-stu-id="82d7e-217">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="82d7e-218">Добавьте строку `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="82d7e-218">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="82d7e-219">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="82d7e-219">Save the file.</span></span> <span data-ttu-id="82d7e-220">Перезапустите Apache.</span><span class="sxs-lookup"><span data-stu-id="82d7e-220">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="82d7e-221">Балансировка нагрузки</span><span class="sxs-lookup"><span data-stu-id="82d7e-221">Load Balancing</span></span> 

<span data-ttu-id="82d7e-222">В этом примере показано, как установить и настроить Apache в CentOS 7 и Kestrel на том же компьютере.</span><span class="sxs-lookup"><span data-stu-id="82d7e-222">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="82d7e-223">Чтобы не имеют одной точки сбоя; с помощью *mod_proxy_balancer* и изменение **VirtualHost** позволит обеспечить управление несколькими экземплярами веб-приложений за прокси-сервера Apache.</span><span class="sxs-lookup"><span data-stu-id="82d7e-223">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="82d7e-224">В файле конфигурации показано ниже, дополнительный экземпляр `hellomvc` приложения настроен для запуска на порту 5001.</span><span class="sxs-lookup"><span data-stu-id="82d7e-224">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="82d7e-225">*Прокси* раздел, задайте для балансировки нагрузки с конфигурацией подсистемы балансировки с двумя членами *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="82d7e-225">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="82d7e-226">Ограничения скорости</span><span class="sxs-lookup"><span data-stu-id="82d7e-226">Rate Limits</span></span>
<span data-ttu-id="82d7e-227">С помощью *mod_ratelimit*, который входит в *httpd* модуля, пропускную способность для клиентов может быть ограничен:</span><span class="sxs-lookup"><span data-stu-id="82d7e-227">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="82d7e-228">Пример файла ограничивает пропускную способность как 600 КБ в секунду в корневой папке:</span><span class="sxs-lookup"><span data-stu-id="82d7e-228">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
