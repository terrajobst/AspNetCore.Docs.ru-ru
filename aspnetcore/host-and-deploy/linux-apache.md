---
title: Размещение ASP.NET Core в операционной системе Linux с Apache
description: Процедура настройки Apache в качестве обратного прокси-сервера в CentOS для перенаправления трафика HTTP в веб-приложение ASP.NET Core, выполняемое в Kestrel.
author: spboyer
manager: wpickett
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 473585f1be180645395c14a154c9c017ca50edab
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
ms.locfileid: "33962821"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="9abaf-103">Размещение ASP.NET Core в операционной системе Linux с Apache</span><span class="sxs-lookup"><span data-stu-id="9abaf-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="9abaf-104">Автор: [Шейн Бойер (Shayne Boyer)](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="9abaf-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="9abaf-105">Из этого руководства вы узнаете, как настроить [Apache](https://httpd.apache.org/) в качестве обратного прокси-сервера в [CentOS 7](https://www.centos.org/) для перенаправления трафика HTTP в веб-приложение ASP.NET Core, выполняемое в [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="9abaf-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="9abaf-106">[Расширение mod_proxy](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) и связанные с ним модули создают обратный прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="9abaf-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9abaf-107">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="9abaf-107">Prerequisites</span></span>

1. <span data-ttu-id="9abaf-108">Сервер под управлением CentOS 7 и учетная запись обычного пользователя с правами sudo.</span><span class="sxs-lookup"><span data-stu-id="9abaf-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="9abaf-109">Приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9abaf-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="9abaf-110">Публикация приложения</span><span class="sxs-lookup"><span data-stu-id="9abaf-110">Publish the app</span></span>

<span data-ttu-id="9abaf-111">Опубликуйте приложение в качестве [автономного развертывания](/dotnet/core/deploying/#self-contained-deployments-scd) в конфигурации выпуска для среды выполнения CentOS 7 (`centos.7-x64`).</span><span class="sxs-lookup"><span data-stu-id="9abaf-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="9abaf-112">Скопируйте содержимое папки *bin/Release/netcoreapp2.0/centos.7-x64/publish* на сервер с помощью SCP, FTP или другого метода передачи файлов.</span><span class="sxs-lookup"><span data-stu-id="9abaf-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="9abaf-113">Если развертывание выполняется в рабочей среде, рабочий процесс непрерывной интеграции автоматически опубликует приложение и скопирует его ресурсы на сервер.</span><span class="sxs-lookup"><span data-stu-id="9abaf-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="9abaf-114">Настройка прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="9abaf-114">Configure a proxy server</span></span>

<span data-ttu-id="9abaf-115">Обратный прокси-сервер — это стандартный вариант настройки для обслуживания динамических веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="9abaf-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="9abaf-116">Обратный прокси-сервер завершает HTTP-запрос и перенаправляет его в приложение ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9abaf-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="9abaf-117">Прокси-сервер перенаправляет запросы клиента на другой сервер, а не обрабатывает их самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="9abaf-117">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="9abaf-118">Обратный прокси-сервер перенаправляет запросы в фиксированное назначение обычно от имени клиентов.</span><span class="sxs-lookup"><span data-stu-id="9abaf-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="9abaf-119">При работе с этим руководством мы настроим Apache в качестве обратного прокси-сервера, который работает на том же сервере, где Kestrel предоставляет приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9abaf-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="9abaf-120">Так как запросы перенаправляются обратным прокси-сервером, используйте ПО промежуточного слоя перенаправления заголовков, входящее в пакет [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/).</span><span class="sxs-lookup"><span data-stu-id="9abaf-120">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="9abaf-121">Это ПО обновляет `Request.Scheme`, используя заголовок `X-Forwarded-Proto`, что обеспечивает правильную работу URI перенаправления и других политик безопасности.</span><span class="sxs-lookup"><span data-stu-id="9abaf-121">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="9abaf-122">Вне зависимости от того, какой тип ПО промежуточного слоя используется для проверки подлинности, сначала необходимо запустить ПО промежуточного слоя перенаправления заголовков.</span><span class="sxs-lookup"><span data-stu-id="9abaf-122">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="9abaf-123">Такой порядок гарантирует, что ПО промежуточного слоя для проверки подлинности может использовать значения заголовков и сформирует правильные URI перенаправления.</span><span class="sxs-lookup"><span data-stu-id="9abaf-123">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9abaf-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9abaf-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9abaf-125">Вызывайте метод [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) в `Startup.Configure`, прежде чем вызывать [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) или другое ПО промежуточного слоя для проверки подлинности по аналогичной схеме.</span><span class="sxs-lookup"><span data-stu-id="9abaf-125">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="9abaf-126">В ПО промежуточного слоя настройте перенаправление заголовков `X-Forwarded-For` и `X-Forwarded-Proto`:</span><span class="sxs-lookup"><span data-stu-id="9abaf-126">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9abaf-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9abaf-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9abaf-128">Вызывайте метод [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) в `Startup.Configure`, прежде чем вызывать [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity), [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) или другое ПО промежуточного слоя для проверки подлинности по аналогичной схеме.</span><span class="sxs-lookup"><span data-stu-id="9abaf-128">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="9abaf-129">В ПО промежуточного слоя настройте перенаправление заголовков `X-Forwarded-For` и `X-Forwarded-Proto`:</span><span class="sxs-lookup"><span data-stu-id="9abaf-129">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="9abaf-130">Если параметр [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) не задан для ПО промежуточного слоя, по умолчанию перенаправляются заголовки `None`.</span><span class="sxs-lookup"><span data-stu-id="9abaf-130">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="9abaf-131">Для приложений, размещенных за прокси-серверами и подсистемами балансировки нагрузки, может потребоваться дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="9abaf-131">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="9abaf-132">Дополнительные сведения см. в разделе [Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="9abaf-132">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-apache"></a><span data-ttu-id="9abaf-133">Установка Apache</span><span class="sxs-lookup"><span data-stu-id="9abaf-133">Install Apache</span></span>

<span data-ttu-id="9abaf-134">Обновите пакеты CentOS до последних стабильных версий:</span><span class="sxs-lookup"><span data-stu-id="9abaf-134">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="9abaf-135">Установите веб-сервер Apache на CentOS, выполнив одну команду `yum`:</span><span class="sxs-lookup"><span data-stu-id="9abaf-135">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="9abaf-136">Пример выходных данных этой команды:</span><span class="sxs-lookup"><span data-stu-id="9abaf-136">Sample output after running the command:</span></span>

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
> <span data-ttu-id="9abaf-137">В нашем примере выходные данные содержат строку httpd.86_64, так как используется 64-разрядная версия CentOS 7.</span><span class="sxs-lookup"><span data-stu-id="9abaf-137">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="9abaf-138">Чтобы проверить, где установлен Apache, выполните `whereis httpd` из командной строки.</span><span class="sxs-lookup"><span data-stu-id="9abaf-138">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="9abaf-139">Настройка Apache в качестве обратного прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="9abaf-139">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="9abaf-140">Файлы конфигурации для Apache находятся в каталоге `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="9abaf-140">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="9abaf-141">В алфавитном порядке обрабатываются все файлы с расширением *.conf*, а также файлы конфигурации модуля из папки `/etc/httpd/conf.modules.d/`, где содержатся файлы конфигурации, необходимые для загрузки модулей.</span><span class="sxs-lookup"><span data-stu-id="9abaf-141">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="9abaf-142">Создайте для приложения файл конфигурации с именем *hellomvc.conf*:</span><span class="sxs-lookup"><span data-stu-id="9abaf-142">Create a configuration file, named *hellomvc.conf*, for the app:</span></span>

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

<span data-ttu-id="9abaf-143">Блок `VirtualHost` может встречаться несколько раз в одном или нескольких файлах на сервере.</span><span class="sxs-lookup"><span data-stu-id="9abaf-143">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="9abaf-144">С представленным выше файлом конфигурации Apache принимает трафик от любого источника через порт 80.</span><span class="sxs-lookup"><span data-stu-id="9abaf-144">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="9abaf-145">Он обслуживает домен `www.example.com`, а псевдоним `*.example.com` указывает на тот же веб-сайт.</span><span class="sxs-lookup"><span data-stu-id="9abaf-145">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="9abaf-146">Дополнительные сведения см. в статье [о поддержке виртуальных узлов на основе имен](https://httpd.apache.org/docs/current/vhosts/name-based.html).</span><span class="sxs-lookup"><span data-stu-id="9abaf-146">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="9abaf-147">Запросы к корневому каталогу перенаправляются на порт 5000 того же сервера по адресу 127.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="9abaf-147">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="9abaf-148">Для двусторонней связи требуются `ProxyPass` и `ProxyPassReverse`.</span><span class="sxs-lookup"><span data-stu-id="9abaf-148">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span>

> [!WARNING]
> <span data-ttu-id="9abaf-149">Если не удастся указать правильную [директиву ServerName](https://httpd.apache.org/docs/current/mod/core.html#servername) в блоке **VirtualHost**, приложение будет иметь значительные уязвимости.</span><span class="sxs-lookup"><span data-stu-id="9abaf-149">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="9abaf-150">Привязки с подстановочными знаками на уровне дочерних доменов (например, `*.example.com`) не создают таких угроз безопасности, если вы полностью контролируете родительский домен (в отличие от варианта `*.com`, создающего уязвимость).</span><span class="sxs-lookup"><span data-stu-id="9abaf-150">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="9abaf-151">Дополнительные сведения см. в документе [rfc7230, раздел 5.4](https://tools.ietf.org/html/rfc7230#section-5.4).</span><span class="sxs-lookup"><span data-stu-id="9abaf-151">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="9abaf-152">Ведение журнала можно настроить отдельно для каждого `VirtualHost` с помощью директив `ErrorLog` и `CustomLog`.</span><span class="sxs-lookup"><span data-stu-id="9abaf-152">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="9abaf-153">Журналы ошибок сохраняются в расположение `ErrorLog`, а параметр `CustomLog` задает имя и формат для файла журнала.</span><span class="sxs-lookup"><span data-stu-id="9abaf-153">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="9abaf-154">В нашем примере здесь фиксируются сведения о запросах.</span><span class="sxs-lookup"><span data-stu-id="9abaf-154">In this case, this is where request information is logged.</span></span> <span data-ttu-id="9abaf-155">Для каждого запроса создается одна строка.</span><span class="sxs-lookup"><span data-stu-id="9abaf-155">There's one line for each request.</span></span>

<span data-ttu-id="9abaf-156">Сохраните файл и протестируйте конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="9abaf-156">Save the file and test the configuration.</span></span> <span data-ttu-id="9abaf-157">Если проверка выполнена успешно, ответ должен быть `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="9abaf-157">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="9abaf-158">Перезапустите Apache.</span><span class="sxs-lookup"><span data-stu-id="9abaf-158">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="9abaf-159">Мониторинг приложения</span><span class="sxs-lookup"><span data-stu-id="9abaf-159">Monitoring the app</span></span>

<span data-ttu-id="9abaf-160">Теперь Apache настроен на перенаправление запросов к `http://localhost:80` в приложение ASP.NET Core, выполняемое в Kestrel по адресу `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="9abaf-160">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="9abaf-161">Но Apache не настроен для управления процессом Kestrel.</span><span class="sxs-lookup"><span data-stu-id="9abaf-161">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="9abaf-162">Для запуска и мониторинга базового веб-приложения используйте *systemd* и создайте файл службы.</span><span class="sxs-lookup"><span data-stu-id="9abaf-162">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="9abaf-163">*systemd* — это система инициализации, предоставляющая различные функции для запуска и остановки процессов, а также управления ими.</span><span class="sxs-lookup"><span data-stu-id="9abaf-163">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="9abaf-164">Создание файла службы</span><span class="sxs-lookup"><span data-stu-id="9abaf-164">Create the service file</span></span>

<span data-ttu-id="9abaf-165">Создайте файл определения службы.</span><span class="sxs-lookup"><span data-stu-id="9abaf-165">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="9abaf-166">Пример файла службы для приложения:</span><span class="sxs-lookup"><span data-stu-id="9abaf-166">An example service file for the app:</span></span>

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
> <span data-ttu-id="9abaf-167">**Пользователь.** Если пользователь *apache* в вашей конфигурации еще не используется, его необходимо создать и правильно предоставить ему права владения для соответствующих файлов.</span><span class="sxs-lookup"><span data-stu-id="9abaf-167">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

> [!NOTE]
> <span data-ttu-id="9abaf-168">Некоторые значения (например, строки подключения SQL) необходимо экранировать, чтобы поставщики конфигурации могли считать переменные среды.</span><span class="sxs-lookup"><span data-stu-id="9abaf-168">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="9abaf-169">Используйте следующую команду, чтобы создать правильно экранированное значение для файла конфигурации:</span><span class="sxs-lookup"><span data-stu-id="9abaf-169">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="9abaf-170">Сохраните файл и включите службу.</span><span class="sxs-lookup"><span data-stu-id="9abaf-170">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="9abaf-171">Запустите службу и убедитесь, что она работает.</span><span class="sxs-lookup"><span data-stu-id="9abaf-171">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="9abaf-172">Теперь, когда обратный прокси-сервер настроен и *systemd* управляет процессом Kestrel, веб-приложение можно считать полностью настроенным и вы можете обратиться к нему по адресу `http://localhost` из браузера на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="9abaf-172">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="9abaf-173">Заголовок **Server** в ответе подтверждает, что приложение ASP.NET Core обслуживается Kestrel.</span><span class="sxs-lookup"><span data-stu-id="9abaf-173">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="9abaf-174">Просмотр журналов</span><span class="sxs-lookup"><span data-stu-id="9abaf-174">Viewing logs</span></span>

<span data-ttu-id="9abaf-175">Так как веб-приложением, использующим Kestrel, управляет *systemd*, все события и процессы регистрируются в централизованном журнале.</span><span class="sxs-lookup"><span data-stu-id="9abaf-175">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="9abaf-176">Но этот журнал содержит все записи обо всех службах и процессах, управляемых *systemd*.</span><span class="sxs-lookup"><span data-stu-id="9abaf-176">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="9abaf-177">Чтобы просмотреть элементы, связанные с `kestrel-hellomvc.service`, используйте следующую команду.</span><span class="sxs-lookup"><span data-stu-id="9abaf-177">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="9abaf-178">Чтобы отфильтровать элементы по времени, укажите в команде параметры времени.</span><span class="sxs-lookup"><span data-stu-id="9abaf-178">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="9abaf-179">Например, `--since today` позволяет отфильтровать данные за текущий день, а `--until 1 hour ago` — просмотреть записи за предыдущий час.</span><span class="sxs-lookup"><span data-stu-id="9abaf-179">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="9abaf-180">Дополнительные сведения см. на странице руководства о команде [journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="9abaf-180">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="9abaf-181">Защита приложения</span><span class="sxs-lookup"><span data-stu-id="9abaf-181">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="9abaf-182">Настройка брандмауэра</span><span class="sxs-lookup"><span data-stu-id="9abaf-182">Configure firewall</span></span>

<span data-ttu-id="9abaf-183">*Firewalld* — это динамическая управляющая программа брандмауэра, которая поддерживает зоны сети.</span><span class="sxs-lookup"><span data-stu-id="9abaf-183">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="9abaf-184">Фильтрацию портов и пакетов можно по-прежнему осуществлять через iptables.</span><span class="sxs-lookup"><span data-stu-id="9abaf-184">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="9abaf-185">*Firewalld* обычно устанавливается в системе по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9abaf-185">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="9abaf-186">`yum` позволяет установить этот пакет или убедиться, что он установлен.</span><span class="sxs-lookup"><span data-stu-id="9abaf-186">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="9abaf-187">С помощью `firewalld` вы можете открыть только те порты, которые необходимые для работы приложения.</span><span class="sxs-lookup"><span data-stu-id="9abaf-187">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="9abaf-188">В этом случае используются порты 80 и 443.</span><span class="sxs-lookup"><span data-stu-id="9abaf-188">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="9abaf-189">Следующие команды назначают порты 80 и 443 постоянно открытыми.</span><span class="sxs-lookup"><span data-stu-id="9abaf-189">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="9abaf-190">Обновите параметры брандмауэра.</span><span class="sxs-lookup"><span data-stu-id="9abaf-190">Reload the firewall settings.</span></span> <span data-ttu-id="9abaf-191">Проверьте, что доступные службы и порты находятся в зоне по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9abaf-191">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="9abaf-192">Эти параметры можно просмотреть с помощью `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="9abaf-192">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="9abaf-193">Конфигурация SSL</span><span class="sxs-lookup"><span data-stu-id="9abaf-193">SSL configuration</span></span>

<span data-ttu-id="9abaf-194">Apache для SSL настраивается с помощью модуля *mod_ssl*.</span><span class="sxs-lookup"><span data-stu-id="9abaf-194">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="9abaf-195">При установке модуля *httpd* автоматически добавляется и модуль *mod_ssl*.</span><span class="sxs-lookup"><span data-stu-id="9abaf-195">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="9abaf-196">Если он по каким-то причинам отсутствует, добавьте его в систему с помощью `yum`.</span><span class="sxs-lookup"><span data-stu-id="9abaf-196">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="9abaf-197">Чтобы принудительно использовать SSL, установите модуль `mod_rewrite` для перезаписи URL-адресов.</span><span class="sxs-lookup"><span data-stu-id="9abaf-197">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="9abaf-198">Измените файл *hellomvc.conf*, чтобы разрешить перезапись URL-адресов и безопасный обмен данными через порт 443.</span><span class="sxs-lookup"><span data-stu-id="9abaf-198">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
> <span data-ttu-id="9abaf-199">В этом примере используется локально созданный сертификат.</span><span class="sxs-lookup"><span data-stu-id="9abaf-199">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="9abaf-200">В **SSLCertificateFile** должен быть указан основной файл сертификата для доменного имени.</span><span class="sxs-lookup"><span data-stu-id="9abaf-200">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="9abaf-201">В **SSLCertificateKeyFile** должен быть указан файл ключа, сформированный при создании CSR.</span><span class="sxs-lookup"><span data-stu-id="9abaf-201">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="9abaf-202">В **SSLCertificateChainFile** должен быть указан файл промежуточного сертификата (если он существует), предоставленный центром сертификации.</span><span class="sxs-lookup"><span data-stu-id="9abaf-202">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="9abaf-203">Сохраните файл и протестируйте конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="9abaf-203">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="9abaf-204">Перезапустите Apache.</span><span class="sxs-lookup"><span data-stu-id="9abaf-204">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="9abaf-205">Дополнительные предложения Apache</span><span class="sxs-lookup"><span data-stu-id="9abaf-205">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="9abaf-206">Дополнительные заголовки</span><span class="sxs-lookup"><span data-stu-id="9abaf-206">Additional headers</span></span>

<span data-ttu-id="9abaf-207">Для защиты от вредоносных атак следует изменить или добавить несколько заголовков.</span><span class="sxs-lookup"><span data-stu-id="9abaf-207">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="9abaf-208">Убедитесь, что модуль `mod_headers` установлен.</span><span class="sxs-lookup"><span data-stu-id="9abaf-208">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="9abaf-209">Защита Apache от атак кликджекинга</span><span class="sxs-lookup"><span data-stu-id="9abaf-209">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="9abaf-210">[Кликджекинг](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger) (или *атака с подменой пользовательского интерфейса*) является вредоносной атакой, при которой посетителя сайта обманным путем вынуждают щелкнуть ссылку или нажать кнопку не той страницы, на которой он находится.</span><span class="sxs-lookup"><span data-stu-id="9abaf-210">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="9abaf-211">Используйте `X-FRAME-OPTIONS` для защиты сайта.</span><span class="sxs-lookup"><span data-stu-id="9abaf-211">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="9abaf-212">Измените файл *httpd.conf*.</span><span class="sxs-lookup"><span data-stu-id="9abaf-212">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="9abaf-213">Добавьте строку `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="9abaf-213">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="9abaf-214">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="9abaf-214">Save the file.</span></span> <span data-ttu-id="9abaf-215">Перезапустите Apache.</span><span class="sxs-lookup"><span data-stu-id="9abaf-215">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="9abaf-216">Сканирование типа MIME</span><span class="sxs-lookup"><span data-stu-id="9abaf-216">MIME-type sniffing</span></span>

<span data-ttu-id="9abaf-217">Заголовок `X-Content-Type-Options` защищает Internet Explorer от *сканирования MIME* (определения `Content-Type` для файла по его содержимому).</span><span class="sxs-lookup"><span data-stu-id="9abaf-217">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="9abaf-218">Если сервер задает заголовок `Content-Type` со значением `text/html`, и при этом установлен параметр `nosniff`, Internet Explorer отображает содержимое как `text/html` независимо от содержимого файла.</span><span class="sxs-lookup"><span data-stu-id="9abaf-218">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="9abaf-219">Измените файл *httpd.conf*.</span><span class="sxs-lookup"><span data-stu-id="9abaf-219">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="9abaf-220">Добавьте строку `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="9abaf-220">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="9abaf-221">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="9abaf-221">Save the file.</span></span> <span data-ttu-id="9abaf-222">Перезапустите Apache.</span><span class="sxs-lookup"><span data-stu-id="9abaf-222">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="9abaf-223">Балансировка нагрузки</span><span class="sxs-lookup"><span data-stu-id="9abaf-223">Load Balancing</span></span> 

<span data-ttu-id="9abaf-224">В этом примере показано, как установить и настроить Apache в CentOS 7 и Kestrel на том же компьютере.</span><span class="sxs-lookup"><span data-stu-id="9abaf-224">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="9abaf-225">Чтобы устранить единую точку отказа, воспользуйтесь *mod_proxy_balancer* и измените значение **VirtualHost** для управления несколькими экземплярами веб-приложений за прокси-сервером Apache.</span><span class="sxs-lookup"><span data-stu-id="9abaf-225">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="9abaf-226">В представленном ниже файле конфигурации настроен дополнительный экземпляр приложения `hellomvc`, работающий на порту 5001.</span><span class="sxs-lookup"><span data-stu-id="9abaf-226">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="9abaf-227">В разделе *Proxy* настраивается конфигурация подсистемы балансировки нагрузки с двумя членами для распределения нагрузки методом *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="9abaf-227">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="9abaf-228">Ограничения скорости</span><span class="sxs-lookup"><span data-stu-id="9abaf-228">Rate Limits</span></span>
<span data-ttu-id="9abaf-229">С помощью элемента *mod_ratelimit*, который входит в модуль *httpd*, можно ограничить пропускную способность для клиентов:</span><span class="sxs-lookup"><span data-stu-id="9abaf-229">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="9abaf-230">В примере файла пропускная способность составляет 600 Кбит/с в корневой папке.</span><span class="sxs-lookup"><span data-stu-id="9abaf-230">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
