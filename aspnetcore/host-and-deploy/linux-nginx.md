---
title: Среда размещения ASP.NET Core в операционной системе Linux с Nginx
author: rick-anderson
description: В статье описывается процедура настройки Nginx как обратного прокси-сервера на Ubuntu 16.04 для перенаправления трафика HTTP в веб-приложение ASP.NET Core, выполняемое в Kestrel.
ms.author: riande
ms.custom: mvc
ms.date: 05/22/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 0ccc9e396ffc9f7af93d5601fee0182d9e3471f4
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961494"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="60942-103">Среда размещения ASP.NET Core в операционной системе Linux с Nginx</span><span class="sxs-lookup"><span data-stu-id="60942-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="60942-104">Автор [Сурабх Ширхатти](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="60942-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="60942-105">В этом руководстве описывается настройка готовой к работе среды ASP.NET Core на сервере Ubuntu 16.04.</span><span class="sxs-lookup"><span data-stu-id="60942-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="60942-106">Эти инструкции могут подходить для более поздних версий Ubuntu, но они еще не были протестированы в этих версиях.</span><span class="sxs-lookup"><span data-stu-id="60942-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

> [!NOTE]
> <span data-ttu-id="60942-107">Для Ubuntu 14.04 в качестве решения для мониторинга процесса Kestrel рекомендуется *supervisord*.</span><span class="sxs-lookup"><span data-stu-id="60942-107">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="60942-108">Решение *systemd* в Ubuntu 14.04 недоступно.</span><span class="sxs-lookup"><span data-stu-id="60942-108">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="60942-109">Инструкции для Ubuntu 14.04 см. в [предыдущей версии этого раздела](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="60942-109">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="60942-110">В этом руководстве рассматривается</span><span class="sxs-lookup"><span data-stu-id="60942-110">This guide:</span></span>

* <span data-ttu-id="60942-111">размещение существующего приложения ASP.NET Core за обратным прокси-сервером;</span><span class="sxs-lookup"><span data-stu-id="60942-111">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="60942-112">настройка обратного прокси-сервера для перенаправления запросов на веб-сервер Kestrel;</span><span class="sxs-lookup"><span data-stu-id="60942-112">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="60942-113">проверка выполнения веб-приложения как управляющей программы при запуске системы;</span><span class="sxs-lookup"><span data-stu-id="60942-113">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="60942-114">настройка средства управления процессами, с помощью которого можно перезапустить веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="60942-114">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="60942-115">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="60942-115">Prerequisites</span></span>

1. <span data-ttu-id="60942-116">Доступ к серверу Ubuntu 16.04 под учетной записью обычного пользователя с правами sudo.</span><span class="sxs-lookup"><span data-stu-id="60942-116">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="60942-117">Установите среду выполнения .NET Core на сервере.</span><span class="sxs-lookup"><span data-stu-id="60942-117">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="60942-118">Перейдите на [страницу всех загрузок .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="60942-118">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="60942-119">Выберите последнюю не предварительную версию среды выполнения из списка под заголовком **Среда выполнения**.</span><span class="sxs-lookup"><span data-stu-id="60942-119">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="60942-120">Сделайте выбор и следуйте инструкциям для Ubuntu, версия которой совпадает с версией Ubuntu на сервере.</span><span class="sxs-lookup"><span data-stu-id="60942-120">Select and follow the instructions for Ubuntu that match the Ubuntu version of the server.</span></span>
1. <span data-ttu-id="60942-121">Существующее приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="60942-121">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="60942-122">Публикация и копирование приложения</span><span class="sxs-lookup"><span data-stu-id="60942-122">Publish and copy over the app</span></span>

<span data-ttu-id="60942-123">Настройте приложение, чтобы [его развертывание зависело от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="60942-123">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="60942-124">Запустите [dotnet publish](/dotnet/core/tools/dotnet-publish) в среде разработки, чтобы упаковать приложение в каталог (например, *bin/Release/&lt;target_framework_moniker&gt;/publish*), который может выполняться на сервере:</span><span class="sxs-lookup"><span data-stu-id="60942-124">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="60942-125">Приложение может быть опубликовано как [автономное развертывание](/dotnet/core/deploying/#self-contained-deployments-scd), если вы предпочитаете не сохранять среду выполнения .NET Core на сервере.</span><span class="sxs-lookup"><span data-stu-id="60942-125">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="60942-126">Скопируйте приложение ASP.NET Core на сервер с помощью инструмента, интегрированного в ваш рабочий процесс (например, SCP или SFTP).</span><span class="sxs-lookup"><span data-stu-id="60942-126">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="60942-127">Обычно веб-приложения находятся в каталоге *var* (например, *var/aspnetcore/hellomvc*).</span><span class="sxs-lookup"><span data-stu-id="60942-127">It's common to locate web apps under the *var* directory (for example, *var/aspnetcore/hellomvc*).</span></span>

> [!NOTE]
> <span data-ttu-id="60942-128">Если развертывание выполняется в рабочей среде, рабочий процесс непрерывной интеграции автоматически опубликует приложение и скопирует его ресурсы на сервер.</span><span class="sxs-lookup"><span data-stu-id="60942-128">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="60942-129">Проверьте работу приложения:</span><span class="sxs-lookup"><span data-stu-id="60942-129">Test the app:</span></span>

1. <span data-ttu-id="60942-130">Запустите приложение в командной строке: `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="60942-130">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="60942-131">В браузере откройте страницу `http://<serveraddress>:<port>`, чтобы убедиться, что приложение локально работает на платформе Linux.</span><span class="sxs-lookup"><span data-stu-id="60942-131">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="60942-132">Настройка обратного прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="60942-132">Configure a reverse proxy server</span></span>

<span data-ttu-id="60942-133">Обратный прокси-сервер — это стандартный вариант настройки для обслуживания динамических веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="60942-133">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="60942-134">Обратный прокси-сервер завершает HTTP-запрос и перенаправляет его в приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="60942-134">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="60942-135">Любая из этих конфигураций &mdash; с обратным прокси-сервером и без него &mdash; является допустимой и поддерживаемой для размещения основных компонентов приложений ASP.NET версии 2.0 и выше.</span><span class="sxs-lookup"><span data-stu-id="60942-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="60942-136">Дополнительные сведения см. в статье [Использование Kestrel с обратным прокси-сервером](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="60942-136">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="60942-137">Использование обратного прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="60942-137">Use a reverse proxy server</span></span>

<span data-ttu-id="60942-138">Kestrel является отличным решением для обслуживания динамического содержимого из ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="60942-138">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="60942-139">При этом компоненты для работы с веб-службами не настолько функциональны, как серверы типа IIS, Apache или Nginx.</span><span class="sxs-lookup"><span data-stu-id="60942-139">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="60942-140">Обратный прокси-сервер может облегчить такую работу, как обслуживание статического содержимого, кэширование запросов, сжатие запросов и завершение SSL с HTTP-сервера.</span><span class="sxs-lookup"><span data-stu-id="60942-140">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="60942-141">Обратный прокси-сервер можно разместить на отдельном компьютере или развернуть параллельно с HTTP-сервером.</span><span class="sxs-lookup"><span data-stu-id="60942-141">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="60942-142">В контексте данного руководства используется отдельный экземпляр Nginx.</span><span class="sxs-lookup"><span data-stu-id="60942-142">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="60942-143">Он выполняется на том же сервере, что и HTTP-сервер.</span><span class="sxs-lookup"><span data-stu-id="60942-143">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="60942-144">Настройки можно выбирать в зависимости от требований.</span><span class="sxs-lookup"><span data-stu-id="60942-144">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="60942-145">Так как запросы перенаправляются обратным прокси-сервером, используйте [ПО промежуточного слоя перенаправления заголовков](xref:host-and-deploy/proxy-load-balancer), которое входит в пакет [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/).</span><span class="sxs-lookup"><span data-stu-id="60942-145">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="60942-146">Это ПО обновляет `Request.Scheme`, используя заголовок `X-Forwarded-Proto`, что обеспечивает правильную работу URI перенаправления и других политик безопасности.</span><span class="sxs-lookup"><span data-stu-id="60942-146">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="60942-147">Любой компонент, который зависит от схемы, например проверка подлинности, генерация ссылок, перенаправление и геолокация, должен находиться после вызова ПО промежуточного слоя перенаправления заголовков.</span><span class="sxs-lookup"><span data-stu-id="60942-147">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="60942-148">Как правило, ПО промежуточного слоя перенаправления заголовков должно выполняться до остального ПО промежуточного слоя, за исключением ПО промежуточного слоя для диагностики и обработки ошибок.</span><span class="sxs-lookup"><span data-stu-id="60942-148">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="60942-149">Такой порядок гарантирует, что ПО промежуточного слоя, полагающееся на сведения о перенаправленных заголовках, может использовать значения заголовков для обработки.</span><span class="sxs-lookup"><span data-stu-id="60942-149">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="60942-150">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="60942-150">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="60942-151">Вызовите метод [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) в `Startup.Configure`, прежде чем вызвать [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) или другое ПО промежуточного слоя для проверки подлинности по аналогичной схеме.</span><span class="sxs-lookup"><span data-stu-id="60942-151">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="60942-152">В ПО промежуточного слоя настройте перенаправление заголовков `X-Forwarded-For` и `X-Forwarded-Proto`:</span><span class="sxs-lookup"><span data-stu-id="60942-152">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="60942-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="60942-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="60942-154">Вызывайте метод [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) в `Startup.Configure`, прежде чем вызвать [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity), [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) или другое ПО промежуточного слоя для проверки подлинности по аналогичной схеме.</span><span class="sxs-lookup"><span data-stu-id="60942-154">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="60942-155">В ПО промежуточного слоя настройте перенаправление заголовков `X-Forwarded-For` и `X-Forwarded-Proto`:</span><span class="sxs-lookup"><span data-stu-id="60942-155">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="60942-156">Если параметр [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) не задан для ПО промежуточного слоя, по умолчанию перенаправляются заголовки `None`.</span><span class="sxs-lookup"><span data-stu-id="60942-156">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="60942-157">Для приложений, размещенных за прокси-серверами и подсистемами балансировки нагрузки, может потребоваться дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="60942-157">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="60942-158">Дополнительные сведения см. в разделе [Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="60942-158">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-nginx"></a><span data-ttu-id="60942-159">Установка Nginx</span><span class="sxs-lookup"><span data-stu-id="60942-159">Install Nginx</span></span>

<span data-ttu-id="60942-160">Установите Nginx с помощью команды `apt-get`.</span><span class="sxs-lookup"><span data-stu-id="60942-160">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="60942-161">Программа установки создает сценарий инициализации *systemd*, который запускает Nginx как управляющую программу при запуске системы.</span><span class="sxs-lookup"><span data-stu-id="60942-161">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> 

```bash
sudo -s
nginx=stable # use nginx=development for latest development version
add-apt-repository ppa:nginx/$nginx
apt-get update
apt-get install nginx
```

<span data-ttu-id="60942-162">Ubuntu Personal Package Archive (PPA) обслуживается добровольцами и не распространяется на [nginx.org](https://nginx.org/). Дополнительные сведения см. в разделе [Nginx: двоичные выпуски. Официальные пакеты Debian и Ubuntu](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span><span class="sxs-lookup"><span data-stu-id="60942-162">The Ubuntu Personal Package Archive (PPA) is maintained by volunteers and isn't distributed by [nginx.org](https://nginx.org/). For more information, see [Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="60942-163">Если необходимы дополнительные модули Nginx, может потребоваться создание Nginx из источника.</span><span class="sxs-lookup"><span data-stu-id="60942-163">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="60942-164">Так как Nginx устанавливается впервые, запустите его напрямую, выполнив следующую команду.</span><span class="sxs-lookup"><span data-stu-id="60942-164">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="60942-165">В браузере должна открыться стартовая страница Nginx по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="60942-165">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="60942-166">Целевая страница доступна по адресу `http://<server_IP_address>/index.nginx-debian.html`.</span><span class="sxs-lookup"><span data-stu-id="60942-166">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="60942-167">Настройка Nginx</span><span class="sxs-lookup"><span data-stu-id="60942-167">Configure Nginx</span></span>

<span data-ttu-id="60942-168">Чтобы настроить Nginx как обратный прокси-сервер для перенаправления запросов в наше приложение ASP.NET Core, измените файл */etc/nginx/sites-available/default*.</span><span class="sxs-lookup"><span data-stu-id="60942-168">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="60942-169">Откройте этот файл в текстовом редакторе и замените его содержимое на следующий код.</span><span class="sxs-lookup"><span data-stu-id="60942-169">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

<span data-ttu-id="60942-170">Если отсутствуют совпадения для `server_name`, Nginx использует сервер по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="60942-170">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="60942-171">Если сервер по умолчанию не определен, первый сервер в файле конфигурации является сервером по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="60942-171">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="60942-172">Рекомендуется добавить в качестве сервера по умолчанию определенный сервер, который возвращает код состояния 444 в файле конфигурации.</span><span class="sxs-lookup"><span data-stu-id="60942-172">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="60942-173">Ниже приведен пример конфигурации сервера по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="60942-173">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="60942-174">С представленным выше файлом конфигурации и сервером по умолчанию Nginx принимает трафик от любого источника через порт 80 с заголовком узла `example.com` или `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="60942-174">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="60942-175">Запросы, не соответствующие этим узлам, не будут перенаправляться в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="60942-175">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="60942-176">Запросы, которые им соответствуют, Nginx перенаправляет в Kestrel по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="60942-176">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="60942-177">Для получения дополнительной информации см. статью [Как nginx обрабатывает запросы](https://nginx.org/docs/http/request_processing.html).</span><span class="sxs-lookup"><span data-stu-id="60942-177">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span>

> [!WARNING]
> <span data-ttu-id="60942-178">Если не будет указана правильная [директива server_name](https://nginx.org/docs/http/server_names.html), приложение будет подвержено значительным уязвимостям.</span><span class="sxs-lookup"><span data-stu-id="60942-178">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="60942-179">Привязки с подстановочными знаками на уровне дочерних доменов (например, `*.example.com`) не создают таких угроз безопасности, если вы полностью контролируете родительский домен (в отличие от варианта `*.com`, создающего уязвимость).</span><span class="sxs-lookup"><span data-stu-id="60942-179">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="60942-180">Дополнительные сведения см. в документе [rfc7230, раздел 5.4](https://tools.ietf.org/html/rfc7230#section-5.4).</span><span class="sxs-lookup"><span data-stu-id="60942-180">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="60942-181">Установив конфигурацию Nginx, выполните команду `sudo nginx -t`, чтобы проверить синтаксис файлов конфигурации.</span><span class="sxs-lookup"><span data-stu-id="60942-181">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="60942-182">Если проверка файла конфигурации прошла успешно, заставьте Nginx принять изменения, выполнив команду `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="60942-182">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="60942-183">Для непосредственного запуска приложений на сервере:</span><span class="sxs-lookup"><span data-stu-id="60942-183">To directly run the app on the server:</span></span>

1. <span data-ttu-id="60942-184">Перейдите в каталог приложения.</span><span class="sxs-lookup"><span data-stu-id="60942-184">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="60942-185">Запустите исполняемый файл приложения: `./<app_executable>`.</span><span class="sxs-lookup"><span data-stu-id="60942-185">Run the app's executable: `./<app_executable>`.</span></span>

<span data-ttu-id="60942-186">Если возникает ошибка прав доступа, измените права доступа:</span><span class="sxs-lookup"><span data-stu-id="60942-186">If a permissions error occurs, change the permissions:</span></span>

```console
chmod u+x <app_executable>
```

<span data-ttu-id="60942-187">Если приложение выполняется на сервере, но не отвечает по Интернету, проверьте брандмауэр сервера и убедитесь, что порт 80 открыт.</span><span class="sxs-lookup"><span data-stu-id="60942-187">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="60942-188">При использовании виртуальной машины Ubuntu Azure добавьте правило группы безопасности сети (NSG), которое разрешает входящий трафик через порт 80.</span><span class="sxs-lookup"><span data-stu-id="60942-188">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="60942-189">Не нужно включать правило исходящего трафика на порте 80, так как исходящий трафик предоставляется автоматически при включении правила для входящего трафика.</span><span class="sxs-lookup"><span data-stu-id="60942-189">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="60942-190">Когда закончите тестировать приложение, завершите его работу с помощью `Ctrl+C` в командной строке.</span><span class="sxs-lookup"><span data-stu-id="60942-190">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="60942-191">Мониторинг приложения</span><span class="sxs-lookup"><span data-stu-id="60942-191">Monitoring the app</span></span>

<span data-ttu-id="60942-192">Сервер настроен на перенаправление запросов к `http://<serveraddress>:80` в приложение ASP.NET Core, выполняемое в Kestrel по адресу `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="60942-192">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="60942-193">При этом Nginx не настроен для управления процессом Kestrel.</span><span class="sxs-lookup"><span data-stu-id="60942-193">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="60942-194">Для запуска и мониторинга соответствующего веб-приложения можно использовать *systemd* и создать файл службы.</span><span class="sxs-lookup"><span data-stu-id="60942-194">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="60942-195">*systemd* — это система инициализации, предоставляющая различные функции для запуска и остановки процессов, а также управления ими.</span><span class="sxs-lookup"><span data-stu-id="60942-195">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="60942-196">Создание файла службы</span><span class="sxs-lookup"><span data-stu-id="60942-196">Create the service file</span></span>

<span data-ttu-id="60942-197">Создайте файл определения службы.</span><span class="sxs-lookup"><span data-stu-id="60942-197">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="60942-198">Далее представлен пример файла службы для нашего приложения.</span><span class="sxs-lookup"><span data-stu-id="60942-198">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="60942-199">Если пользователь *www-data* в конфигурации не используется, необходимо сначала создать определенного в примере пользователя, а затем предоставить ему необходимые права владения для файлов.</span><span class="sxs-lookup"><span data-stu-id="60942-199">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="60942-200">Linux имеет файловую систему, в которой учитывается регистр символов.</span><span class="sxs-lookup"><span data-stu-id="60942-200">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="60942-201">Если для параметра ASPNETCORE_ENVIRONMENT установить значение Production, будет выполнен поиск файла конфигурации *appsettings.Production.json*, а не файла *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="60942-201">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="60942-202">Некоторые значения (например, строки подключения SQL) необходимо экранировать, чтобы поставщики конфигурации могли читать переменные среды.</span><span class="sxs-lookup"><span data-stu-id="60942-202">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="60942-203">Используйте следующую команду, чтобы создать правильно экранированное значение для файла конфигурации:</span><span class="sxs-lookup"><span data-stu-id="60942-203">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="60942-204">Сохраните файл и включите службу.</span><span class="sxs-lookup"><span data-stu-id="60942-204">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="60942-205">Запустите службу и убедитесь, что она работает.</span><span class="sxs-lookup"><span data-stu-id="60942-205">Start the service and verify that it's running.</span></span>

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

<span data-ttu-id="60942-206">Теперь, когда обратный прокси-сервер настроен и systemd управляет процессом Kestrel, веб-приложение можно считать полностью настроенным и вы можете обратиться к нему по адресу `http://localhost` из браузера на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="60942-206">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="60942-207">Оно также доступно для удаленных компьютеров, несмотря на наличие блокирующих трафик межсетевых экранов.</span><span class="sxs-lookup"><span data-stu-id="60942-207">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="60942-208">Заголовок `Server` в ответе подтверждает, что приложение ASP.NET Core обслуживается Kestrel.</span><span class="sxs-lookup"><span data-stu-id="60942-208">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="60942-209">Просмотр журналов</span><span class="sxs-lookup"><span data-stu-id="60942-209">Viewing logs</span></span>

<span data-ttu-id="60942-210">Так как веб-приложение, использующее Kestrel, управляется через `systemd`, все события и процессы регистрируются в централизованном журнале.</span><span class="sxs-lookup"><span data-stu-id="60942-210">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="60942-211">При этом журнал содержит все записи обо всех службах и процессах, управляемых `systemd`.</span><span class="sxs-lookup"><span data-stu-id="60942-211">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="60942-212">Чтобы просмотреть элементы, связанные с `kestrel-hellomvc.service`, используйте следующую команду.</span><span class="sxs-lookup"><span data-stu-id="60942-212">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="60942-213">Кроме того, количество возвращаемых записей можно уменьшить, указав параметры времени, например `--since today`, `--until 1 hour ago` или их комбинацию.</span><span class="sxs-lookup"><span data-stu-id="60942-213">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="60942-214">Защита приложения</span><span class="sxs-lookup"><span data-stu-id="60942-214">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="60942-215">Включение AppArmor</span><span class="sxs-lookup"><span data-stu-id="60942-215">Enable AppArmor</span></span>

<span data-ttu-id="60942-216">Начиная с версии 2.6 ядро Linux включает платформу модулей безопасности Linux (LSM).</span><span class="sxs-lookup"><span data-stu-id="60942-216">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="60942-217">LSM поддерживают различные реализации модулей безопасности.</span><span class="sxs-lookup"><span data-stu-id="60942-217">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="60942-218">[AppArmor](https://wiki.ubuntu.com/AppArmor) — это LSM, который реализует систему обязательного контроля доступа, позволяющую ограничивать программу определенным набором ресурсов.</span><span class="sxs-lookup"><span data-stu-id="60942-218">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="60942-219">Убедитесь, что AppArmor включен и правильно настроен.</span><span class="sxs-lookup"><span data-stu-id="60942-219">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="60942-220">Настройка межсетевого экрана</span><span class="sxs-lookup"><span data-stu-id="60942-220">Configuring the firewall</span></span>

<span data-ttu-id="60942-221">Закройте все внешние порты, которые не используются.</span><span class="sxs-lookup"><span data-stu-id="60942-221">Close off all external ports that are not in use.</span></span> <span data-ttu-id="60942-222">Незамысловатый межсетевой экран (ufw) позволяет взаимодействовать с `iptables`, предоставляя интерфейс командной строки для настройки межсетевого экрана.</span><span class="sxs-lookup"><span data-stu-id="60942-222">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="60942-223">Убедитесь, что настройки `ufw` пропускают трафик на все необходимые порты.</span><span class="sxs-lookup"><span data-stu-id="60942-223">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="60942-224">Защита Nginx</span><span class="sxs-lookup"><span data-stu-id="60942-224">Securing Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="60942-225">Изменение имени ответа Nginx</span><span class="sxs-lookup"><span data-stu-id="60942-225">Change the Nginx response name</span></span>

<span data-ttu-id="60942-226">Внесите изменения в файл *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="60942-226">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="60942-227">Настройка параметров</span><span class="sxs-lookup"><span data-stu-id="60942-227">Configure options</span></span>

<span data-ttu-id="60942-228">Настройте дополнительные обязательные модули на сервере.</span><span class="sxs-lookup"><span data-stu-id="60942-228">Configure the server with additional required modules.</span></span> <span data-ttu-id="60942-229">Для дополнительной защиты приложения можно использовать межсетевой экран для веб-приложений, например [ModSecurity](https://www.modsecurity.org/).</span><span class="sxs-lookup"><span data-stu-id="60942-229">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="configure-ssl"></a><span data-ttu-id="60942-230">Настройка SSL</span><span class="sxs-lookup"><span data-stu-id="60942-230">Configure SSL</span></span>

* <span data-ttu-id="60942-231">Настройте сервер для прослушивания трафика HTTPS через порт `443`, указав действительный сертификат, выпущенный доверенным центром сертификации (ЦС).</span><span class="sxs-lookup"><span data-stu-id="60942-231">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="60942-232">Обеспечьте дополнительную защиту, применив некоторые методы, показанные в представленном ниже файле */etc/nginx/nginx.conf*.</span><span class="sxs-lookup"><span data-stu-id="60942-232">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="60942-233">Это может быть выбор более строгого шифра и перенаправление всего HTTP-трафика в HTTPS.</span><span class="sxs-lookup"><span data-stu-id="60942-233">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="60942-234">При добавлении заголовка `HTTP Strict-Transport-Security` (HSTS) все последующие запросы клиента будут проходить только по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="60942-234">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="60942-235">Если в дальнейшем планируется отключить SSL, не добавляйте заголовок Strict-Transport-Security или выберите соответствующий `max-age`.</span><span class="sxs-lookup"><span data-stu-id="60942-235">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="60942-236">Добавьте файл конфигурации */etc/nginx/proxy.conf*:</span><span class="sxs-lookup"><span data-stu-id="60942-236">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="60942-237">Измените файл конфигурации */etc/nginx/proxy.conf*.</span><span class="sxs-lookup"><span data-stu-id="60942-237">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="60942-238">В этом примере показаны разделы `http` и `server` одного и того же файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="60942-238">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="60942-239">Защита Nginx от кликджекинга</span><span class="sxs-lookup"><span data-stu-id="60942-239">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="60942-240">Кликджекинг — это способ обмана, предназначенный для перехвата кликов пострадавшего пользователя.</span><span class="sxs-lookup"><span data-stu-id="60942-240">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="60942-241">Кликджекинг обманом заставляет пользователей (посетителей) щелкать зараженные сайты.</span><span class="sxs-lookup"><span data-stu-id="60942-241">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="60942-242">Для защиты сайта используйте X-FRAME-OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="60942-242">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="60942-243">Измените файл *nginx.conf*.</span><span class="sxs-lookup"><span data-stu-id="60942-243">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="60942-244">Добавьте строку `add_header X-Frame-Options "SAMEORIGIN";` и сохраните файл, а затем перезапустите Nginx.</span><span class="sxs-lookup"><span data-stu-id="60942-244">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="60942-245">Сканирование типа MIME</span><span class="sxs-lookup"><span data-stu-id="60942-245">MIME-type sniffing</span></span>

<span data-ttu-id="60942-246">Этот заголовок предотвращает MIME-сканирование ответов с указанным типом содержимого в большинстве браузеров, запрещая браузеру переопределять тип содержимого ответа.</span><span class="sxs-lookup"><span data-stu-id="60942-246">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="60942-247">Параметр `nosniff` означает, что, если сервер определяет содержимое как text/html, браузер будет обрабатывать его как text/html.</span><span class="sxs-lookup"><span data-stu-id="60942-247">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="60942-248">Измените файл *nginx.conf*.</span><span class="sxs-lookup"><span data-stu-id="60942-248">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="60942-249">Добавьте строку `add_header X-Content-Type-Options "nosniff";` и сохраните файл, а затем перезапустите Nginx.</span><span class="sxs-lookup"><span data-stu-id="60942-249">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="60942-250">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="60942-250">Additional resources</span></span>

* [<span data-ttu-id="60942-251">Nginx: двоичные выпуски. Официальные пакеты Debian и Ubuntu</span><span class="sxs-lookup"><span data-stu-id="60942-251">Nginx: Binary Releases: Official Debian/Ubuntu packages</span></span>](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
* [<span data-ttu-id="60942-252">Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="60942-252">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
* [<span data-ttu-id="60942-253">NGINX. Использование перенаправленного заголовка</span><span class="sxs-lookup"><span data-stu-id="60942-253">NGINX: Using the Forwarded header</span></span>](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
