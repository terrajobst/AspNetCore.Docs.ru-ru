---
title: "Размещение ASP.NET Core в Windows со службами IIS"
author: guardrex
description: "Сведения о размещении приложений ASP.NET Core в службах Windows Server Internet Information Services (IIS)."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/index
ms.openlocfilehash: 18c7448ad79891d04eca1e939a0aeeabe417bde8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="bffdf-103">Размещение ASP.NET Core в Windows со службами IIS</span><span class="sxs-lookup"><span data-stu-id="bffdf-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="bffdf-104">Авторы: [Люк Лэтэм](https://github.com/guardrex) (Luke Latham) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="bffdf-104">By [Luke Latham](https://github.com/guardrex) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="bffdf-105">Поддерживаемые операционные системы</span><span class="sxs-lookup"><span data-stu-id="bffdf-105">Supported operating systems</span></span>

<span data-ttu-id="bffdf-106">Поддерживаются следующие операционные системы:</span><span class="sxs-lookup"><span data-stu-id="bffdf-106">The following operating systems are supported:</span></span>

* <span data-ttu-id="bffdf-107">Windows 7 и более поздних версий;</span><span class="sxs-lookup"><span data-stu-id="bffdf-107">Windows 7 and newer</span></span>
* <span data-ttu-id="bffdf-108">Windows Server 2008 R2 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="bffdf-108">Windows Server 2008 R2 and newer&#8224;</span></span>

<span data-ttu-id="bffdf-109">&#8224;В принципе, конфигурация IIS, описываемая в этом документе, также применима при размещении приложений ASP.NET Core в службах IIS Nano Server, но более точные инструкции см. в статье [ASP.NET Core со службами IIS в Nano Server](xref:tutorials/nano-server).</span><span class="sxs-lookup"><span data-stu-id="bffdf-109">&#8224;Conceptually, the IIS configuration described in this document also applies to hosting ASP.NET Core apps on Nano Server IIS, but refer to [ASP.NET Core with IIS on Nano Server](xref:tutorials/nano-server) for specific instructions.</span></span>

<span data-ttu-id="bffdf-110">[Сервер HTTP.sys](xref:fundamentals/servers/httpsys) (который ранее назывался [WebListener](xref:fundamentals/servers/weblistener)) не будет работать в конфигурации обратного прокси-сервера со службами IIS.</span><span class="sxs-lookup"><span data-stu-id="bffdf-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) won't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="bffdf-111">Используйте [сервер Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="bffdf-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="bffdf-112">Конфигурация IIS</span><span class="sxs-lookup"><span data-stu-id="bffdf-112">IIS configuration</span></span>

<span data-ttu-id="bffdf-113">Включите роль **Веб-сервер (IIS)** и создайте службы роли.</span><span class="sxs-lookup"><span data-stu-id="bffdf-113">Enable the **Web Server (IIS)** role and establish role services.</span></span>

### <a name="windows-desktop-operating-systems"></a><span data-ttu-id="bffdf-114">Операционные системы Windows для настольных компьютеров</span><span class="sxs-lookup"><span data-stu-id="bffdf-114">Windows desktop operating systems</span></span>

<span data-ttu-id="bffdf-115">Последовательно выберите **Панель управления** > **Программы** > **Программы и компоненты** > **Включение или отключение компонентов Windows** (в левой части экрана).</span><span class="sxs-lookup"><span data-stu-id="bffdf-115">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="bffdf-116">Откройте группу **Службы IIS**, а затем — **Средства управления веб-сайтом**.</span><span class="sxs-lookup"><span data-stu-id="bffdf-116">Open the group for **Internet Information Services** and **Web Management Tools**.</span></span> <span data-ttu-id="bffdf-117">Установите флажок **Консоль управления IIS**.</span><span class="sxs-lookup"><span data-stu-id="bffdf-117">Check the box for **IIS Management Console**.</span></span> <span data-ttu-id="bffdf-118">Установите флажок **Службы Интернета**.</span><span class="sxs-lookup"><span data-stu-id="bffdf-118">Check the box for **World Wide Web Services**.</span></span> <span data-ttu-id="bffdf-119">В группе **Службы Интернета** оставьте компоненты по умолчанию или настройте компоненты IIS.</span><span class="sxs-lookup"><span data-stu-id="bffdf-119">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

![Группы "Консоль управления IIS" и "Службы Интернета" выбраны в окне "Компоненты Windows".](index/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a><span data-ttu-id="bffdf-121">Операционные системы семейства Windows Server</span><span class="sxs-lookup"><span data-stu-id="bffdf-121">Windows Server operating systems</span></span>

<span data-ttu-id="bffdf-122">В серверной операционной системе используйте мастер **добавления ролей и компонентов**, который можно вызвать в меню **Управление** или по ссылке в **диспетчере сервера**.</span><span class="sxs-lookup"><span data-stu-id="bffdf-122">For server operating systems, use the **Add Roles and Features** wizard via the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="bffdf-123">На этапе **Роли сервера** установите флажок **Веб-сервер (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="bffdf-123">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

![Роль "Веб-сервер (IIS)" выбрана на странице "Выбор ролей сервера".](index/_static/server-roles-ws2016.png)

<span data-ttu-id="bffdf-125">На этапе **Службы роли** выберите нужные службы роли IIS или оставьте службы по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="bffdf-125">On the **Role services** step, select the IIS role services desired or accept the default role services provided.</span></span>

![Службы роли по умолчанию, выбранные на странице "Выбор служб ролей".](index/_static/role-services-ws2016.png)

<span data-ttu-id="bffdf-127">Пройдите этап **Подтверждение**, чтобы установить роль и службы веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="bffdf-127">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="bffdf-128">После установки роли "Веб-сервер (IIS)" перезагружать сервер или службы IIS не требуется.</span><span class="sxs-lookup"><span data-stu-id="bffdf-128">A server/IIS restart isn't required after installing the Web Server (IIS) role.</span></span>

## <a name="install-the-net-core-windows-server-hosting-bundle"></a><span data-ttu-id="bffdf-129">Установка пакета размещения .NET Core для Windows Server</span><span class="sxs-lookup"><span data-stu-id="bffdf-129">Install the .NET Core Windows Server Hosting bundle</span></span>

1. <span data-ttu-id="bffdf-130">Установите [пакет размещения .NET Core для Windows Server](https://aka.ms/dotnetcore-2-windowshosting) в размещающей системе.</span><span class="sxs-lookup"><span data-stu-id="bffdf-130">Install the [.NET Core Windows Server Hosting bundle](https://aka.ms/dotnetcore-2-windowshosting) on the hosting system.</span></span> <span data-ttu-id="bffdf-131">В составе пакета устанавливаются среда выполнения .NET Core, библиотека .NET Core и [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="bffdf-131">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="bffdf-132">Модуль создает обратный прокси-сервер между службами IIS и сервером Kestrel.</span><span class="sxs-lookup"><span data-stu-id="bffdf-132">The module creates the reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="bffdf-133">Если система не подключена к Интернету, перед установкой пакета размещения .NET Core для Windows Server получите и установите [Распространяемый компонент Microsoft Visual C++ 2015](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="bffdf-133">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Windows Server Hosting bundle.</span></span>

2. <span data-ttu-id="bffdf-134">Перезапустите систему или выполните в командной строке команду **net stop was /y**, а затем команду **net start w3svc**, чтобы изменение системной переменной PATH вступило в силу.</span><span class="sxs-lookup"><span data-stu-id="bffdf-134">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt to pick up a change to the system PATH.</span></span>

> [!NOTE]
> <span data-ttu-id="bffdf-135">Сведения об общей конфигурации IIS см. в разделе [Модуль ASP.NET Core с общей конфигурацией IIS](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span><span class="sxs-lookup"><span data-stu-id="bffdf-135">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="bffdf-136">Установка веб-развертывания при публикации с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bffdf-136">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="bffdf-137">При развертывании приложений на серверах с помощью [веб-развертывания](/iis/publish/using-web-deploy/introduction-to-web-deploy) установите на сервере последнюю версию веб-развертывания.</span><span class="sxs-lookup"><span data-stu-id="bffdf-137">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="bffdf-138">Чтобы установить веб-развертывание, можно использовать [установщик веб-платформы (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) или получить установщик непосредственно в [Центре загрузки Майкрософт](https://www.microsoft.com/download/details.aspx?id=43717).</span><span class="sxs-lookup"><span data-stu-id="bffdf-138">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="bffdf-139">Предпочтительный метод — использовать WebPI.</span><span class="sxs-lookup"><span data-stu-id="bffdf-139">The preferred method is to use WebPI.</span></span> <span data-ttu-id="bffdf-140">WebPI обеспечивает автономную установку и настройку поставщиков размещения.</span><span class="sxs-lookup"><span data-stu-id="bffdf-140">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="bffdf-141">Настройка приложения</span><span class="sxs-lookup"><span data-stu-id="bffdf-141">Application configuration</span></span>

### <a name="enabling-the-iisintegration-components"></a><span data-ttu-id="bffdf-142">Включение компонентов IISIntegration</span><span class="sxs-lookup"><span data-stu-id="bffdf-142">Enabling the IISIntegration components</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bffdf-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bffdf-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="bffdf-144">Обычно *Program.cs* вызывает [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), чтобы начать настройку узла.</span><span class="sxs-lookup"><span data-stu-id="bffdf-144">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to begin setting up a host.</span></span> <span data-ttu-id="bffdf-145">`CreateDefaultBuilder` настраивает [Kestrel](xref:fundamentals/servers/kestrel) в качестве веб-сервера и активирует интеграцию IIS, задавая базовый путь и порт для [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module):</span><span class="sxs-lookup"><span data-stu-id="bffdf-145">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bffdf-146">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bffdf-146">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="bffdf-147">Включите в зависимости приложения зависимость от пакета [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/).</span><span class="sxs-lookup"><span data-stu-id="bffdf-147">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="bffdf-148">Включите в приложение ПО промежуточного слоя для интеграции IIS, добавив метод расширения *UseIISIntegration* в *WebHostBuilder*.</span><span class="sxs-lookup"><span data-stu-id="bffdf-148">Incorporate IIS Integration middleware into the app by adding the *UseIISIntegration* extension method to *WebHostBuilder*:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="bffdf-149">`UseKestrel` и `UseIISIntegration` являются обязательными элементами.</span><span class="sxs-lookup"><span data-stu-id="bffdf-149">Both `UseKestrel` and `UseIISIntegration` are required.</span></span> <span data-ttu-id="bffdf-150">Код, вызывающий `UseIISIntegration`, не влияет на переносимость кода.</span><span class="sxs-lookup"><span data-stu-id="bffdf-150">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="bffdf-151">Если приложение запускается не в IIS (например, запускается непосредственно в Kestrel), то в `UseIISIntegration` нет операций.</span><span class="sxs-lookup"><span data-stu-id="bffdf-151">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` no-ops.</span></span>

---

<span data-ttu-id="bffdf-152">Дополнительные сведения о размещении см. в разделе [Размещение в ASP.NET Core](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="bffdf-152">For more information on hosting, see [Hosting in ASP.NET Core](xref:fundamentals/hosting).</span></span>

### <a name="iis-options"></a><span data-ttu-id="bffdf-153">Параметры служб IIS</span><span class="sxs-lookup"><span data-stu-id="bffdf-153">IIS options</span></span>

<span data-ttu-id="bffdf-154">Чтобы настроить параметры служб IIS, включите конфигурацию служб для `IISOptions` в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="bffdf-154">To configure IIS options, include a service configuration for `IISOptions` in `ConfigureServices`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| <span data-ttu-id="bffdf-155">Параметр</span><span class="sxs-lookup"><span data-stu-id="bffdf-155">Option</span></span>                         | <span data-ttu-id="bffdf-156">По умолчанию</span><span class="sxs-lookup"><span data-stu-id="bffdf-156">Default</span></span> | <span data-ttu-id="bffdf-157">Параметр</span><span class="sxs-lookup"><span data-stu-id="bffdf-157">Setting</span></span> |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="bffdf-158">Если значение — `true`, ПО промежуточного слоя для проверки подлинности задаст значение `HttpContext.User` и будет отвечать на общие запросы.</span><span class="sxs-lookup"><span data-stu-id="bffdf-158">If `true`, the authentication middleware sets the `HttpContext.User` and responds to generic challenges.</span></span> <span data-ttu-id="bffdf-159">Если значение — `false`, ПО промежуточного слоя для проверки подлинности только предоставит идентификатор (`HttpContext.User`) и будет отвечать только на явные запросы от `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="bffdf-159">If `false`, the authentication middleware only provides an identity (`HttpContext.User`) and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="bffdf-160">Для работы `AutomaticAuthentication` необходимо включить в службах IIS проверку подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="bffdf-160">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="bffdf-161">Задает отображаемое имя для пользователей на страницах входа.</span><span class="sxs-lookup"><span data-stu-id="bffdf-161">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="bffdf-162">Если значение — `true` и если присутствует заголовок запроса `MS-ASPNETCORE-CLIENTCERT`, происходит заполнение `HttpContext.Connection.ClientCertificate`.</span><span class="sxs-lookup"><span data-stu-id="bffdf-162">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="webconfig"></a><span data-ttu-id="bffdf-163">web.config.</span><span class="sxs-lookup"><span data-stu-id="bffdf-163">web.config</span></span>

<span data-ttu-id="bffdf-164">Основным назначением файла *Web.config* является настройка [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="bffdf-164">The *web.config* file's primary purpose is to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="bffdf-165">Он может также предоставлять дополнительные параметры конфигурации IIS.</span><span class="sxs-lookup"><span data-stu-id="bffdf-165">It may optionally provide additional IIS configuration settings.</span></span> <span data-ttu-id="bffdf-166">За создание, преобразования и публикацию *web.config* отвечает веб-пакет SDK для .NET Core (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="bffdf-166">Creating, transforming, and publishing *web.config* is handled by the .NET Core Web SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="bffdf-167">Пакет SDK устанавливается поверх файла проекта (`<Project Sdk="Microsoft.NET.Sdk.Web">`).</span><span class="sxs-lookup"><span data-stu-id="bffdf-167">The SDK is set  at the top of the project file, `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span></span> <span data-ttu-id="bffdf-168">Чтобы пакет SDK не преобразовывал файл *web.config*, добавьте в файл проекта свойство **\<IsTransformWebConfigDisabled>** со значением `true`:</span><span class="sxs-lookup"><span data-stu-id="bffdf-168">To prevent the SDK from transforming the *web.config* file, add the **\<IsTransformWebConfigDisabled>** property to the project file with a setting of `true`:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="bffdf-169">Если в проекте есть файл *web.config*, он преобразуется с использованием соответствующих аргументов *processPath* и *arguments* для настройки [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) и переносится в [опубликованные выходные данные](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="bffdf-169">If a *web.config* file is in the project, it's transformed with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span> <span data-ttu-id="bffdf-170">Преобразование не изменяет параметры конфигурации служб IIS, включенные в файл.</span><span class="sxs-lookup"><span data-stu-id="bffdf-170">The transformation doesn't modify IIS configuration settings in the file.</span></span>

### <a name="webconfig-location"></a><span data-ttu-id="bffdf-171">расположение web.config</span><span class="sxs-lookup"><span data-stu-id="bffdf-171">web.config location</span></span>

<span data-ttu-id="bffdf-172">Приложения .NET Core размещаются с помощью обратного прокси-сервера между службами IIS и сервером Kestrel.</span><span class="sxs-lookup"><span data-stu-id="bffdf-172">.NET Core apps are hosted via a reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="bffdf-173">Для создания обратного прокси-сервера файл *web.config* должен находиться по корневому пути к содержимому развернутого приложения (как правило, это основной путь приложения). Это физический путь к веб-сайту, указанный в службах IIS.</span><span class="sxs-lookup"><span data-stu-id="bffdf-173">In order to create the reverse proxy, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app, which is the website physical path provided to IIS.</span></span> <span data-ttu-id="bffdf-174">Файл *web.config* требуется в корне приложения, чтобы разрешить публикацию нескольких приложений с помощью веб-развертывания.</span><span class="sxs-lookup"><span data-stu-id="bffdf-174">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="bffdf-175">По физическому пути приложения находятся важные файлы, включая такие вложенные папки, как *\<имя_сборки>.runtimeconfig.json*, *\<имя_сборки>.xml* (комментарии к документам XML) и *\<имя_сборки>.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="bffdf-175">Sensitive files exist on the app's physical path, including subfolders, such as *\<assembly_name>.runtimeconfig.json*, *\<assembly_name>.xml* (XML Documentation comments), and *\<assembly_name>.deps.json*.</span></span> <span data-ttu-id="bffdf-176">Когда имеющийся файл *web.config* настраивает сайт, службы IIS препятствует обслуживанию этих конфиденциальных файлов.</span><span class="sxs-lookup"><span data-stu-id="bffdf-176">When the *web.config* file is present and configures the site, IIS prevents these sensitive files from being served.</span></span> <span data-ttu-id="bffdf-177">**Поэтому важно следить за тем, чтобы файл *web.config* не был случайно переименован или удален из развертывания**.</span><span class="sxs-lookup"><span data-stu-id="bffdf-177">**Therefore, it's important that the *web.config* file isn't accidently renamed or removed from the deployment.**</span></span>

## <a name="create-the-iis-website"></a><span data-ttu-id="bffdf-178">Создание веб-сайта IIS</span><span class="sxs-lookup"><span data-stu-id="bffdf-178">Create the IIS Website</span></span>

1. <span data-ttu-id="bffdf-179">В целевой системе IIS создайте папку для опубликованных папок и файлов приложения, которые описываются в статье [Структура каталогов](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="bffdf-179">On the target IIS system, create a folder to contain the app's published folders and files, which are described in [Directory Structure](xref:host-and-deploy/directory-structure).</span></span>

2. <span data-ttu-id="bffdf-180">В папке создайте папку *logs* для хранения журналов StdOut, если включено их ведение.</span><span class="sxs-lookup"><span data-stu-id="bffdf-180">Within the folder, create a *logs* folder to hold stdout logs when stdout logging is enabled.</span></span> <span data-ttu-id="bffdf-181">Если приложение развертывается с папкой *logs* в полезных данных, пропустите этот шаг.</span><span class="sxs-lookup"><span data-stu-id="bffdf-181">If the app is deployed with a *logs* folder in the payload, skip this step.</span></span> <span data-ttu-id="bffdf-182">Инструкции о том, как MSBuild создает папку *logs*, см. в разделе [Структура каталога](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="bffdf-182">For instructions on how to make MSBuild create the *logs* folder, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>

3. <span data-ttu-id="bffdf-183">В **диспетчере IIS** создайте веб-сайт.</span><span class="sxs-lookup"><span data-stu-id="bffdf-183">In **IIS Manager**, create a new website.</span></span> <span data-ttu-id="bffdf-184">Укажите имя в поле **Имя сайта** и задайте значение в поле **Физический путь** для созданной папки развертывания приложения.</span><span class="sxs-lookup"><span data-stu-id="bffdf-184">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="bffdf-185">Укажите конфигурацию **привязки** и создайте веб-сайт.</span><span class="sxs-lookup"><span data-stu-id="bffdf-185">Provide the **Binding** configuration and create the website.</span></span>

4. <span data-ttu-id="bffdf-186">Для пула приложений выберите **Без управляемого кода**.</span><span class="sxs-lookup"><span data-stu-id="bffdf-186">Set the application pool to **No Managed Code**.</span></span> <span data-ttu-id="bffdf-187">ASP.NET Core выполняется в отдельном процессе и управляет средой выполнения.</span><span class="sxs-lookup"><span data-stu-id="bffdf-187">ASP.NET Core runs in a separate process and manages the runtime.</span></span>

5. <span data-ttu-id="bffdf-188">Откройте окно **Добавить веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="bffdf-188">Open the **Add Website** window.</span></span>

   ![В контекстном меню узла "Сайты" выберите пункт "Добавить веб-сайт".](index/_static/add-website-context-menu-ws2016.png)

6. <span data-ttu-id="bffdf-190">Настройте веб-сайт.</span><span class="sxs-lookup"><span data-stu-id="bffdf-190">Configure the website.</span></span>

   ![В окне "Добавить веб-сайт" укажите имя сайта, физический путь и имя узла.](index/_static/add-website-ws2016.png)

7. <span data-ttu-id="bffdf-192">На панели **Пулы приложений** откройте окно **Изменение пула приложений**, щелкнув пул приложений веб-сайта правой кнопкой мыши и выбрав в контекстном меню пункт **Основные параметры...**</span><span class="sxs-lookup"><span data-stu-id="bffdf-192">In the **Application Pools** panel, open the **Edit Application Pool** window by right-clicking on the website's app pool and selecting **Basic Settings...** from the popup menu.</span></span>

   ![В контекстном меню пула приложений выберите пункт "Основные настройки".](index/_static/apppools-basic-settings-ws2016.png)

8. <span data-ttu-id="bffdf-194">Для параметра **Версия среды CLR .NET** выберите значение **Без управляемого кода**.</span><span class="sxs-lookup"><span data-stu-id="bffdf-194">Set the **.NET CLR version** to **No Managed Code**.</span></span>

   ![Выберите "Без управляемого кода" для параметра "Версия среды CLR .NET".](index/_static/edit-apppool-ws2016.png)
     
    <span data-ttu-id="bffdf-196">Примечание. Задавать для параметра **Версия среды CLR .NET** значение **Без управляемого кода** необязательно.</span><span class="sxs-lookup"><span data-stu-id="bffdf-196">Note: Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span> <span data-ttu-id="bffdf-197">Для ASP.NET Core не требуется загрузка классической среды CLR.</span><span class="sxs-lookup"><span data-stu-id="bffdf-197">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span>

9. <span data-ttu-id="bffdf-198">Убедитесь в том, что удостоверение модели процесса имеет соответствующие разрешения.</span><span class="sxs-lookup"><span data-stu-id="bffdf-198">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="bffdf-199">При изменении удостоверения по умолчанию для пула приложений (**Модель процесса** > **Удостоверение**) с **ApplicationPoolIdentity** на другое, убедитесь, что у нового удостоверения есть соответствующие разрешения для доступа к папке приложения, базе данных и другим необходимым ресурсам.</span><span class="sxs-lookup"><span data-stu-id="bffdf-199">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span>
   
## <a name="deploy-the-app"></a><span data-ttu-id="bffdf-200">Развертывание приложения</span><span class="sxs-lookup"><span data-stu-id="bffdf-200">Deploy the app</span></span>

<span data-ttu-id="bffdf-201">Разверните приложение в папке, созданной в целевой системе IIS.</span><span class="sxs-lookup"><span data-stu-id="bffdf-201">Deploy the app to the folder created on the target IIS system.</span></span> <span data-ttu-id="bffdf-202">Рекомендуемый механизм развертывания — [веб-развертывание](/iis/publish/using-web-deploy/introduction-to-web-deploy).</span><span class="sxs-lookup"><span data-stu-id="bffdf-202">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

<span data-ttu-id="bffdf-203">Убедитесь в том, что приложение, публикуемое для развертывания, не запущено.</span><span class="sxs-lookup"><span data-stu-id="bffdf-203">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="bffdf-204">Во время выполнения приложения файлы в папке *publish* блокируются.</span><span class="sxs-lookup"><span data-stu-id="bffdf-204">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="bffdf-205">Заблокированные файлы не могут быть перезаписаны.</span><span class="sxs-lookup"><span data-stu-id="bffdf-205">Locked files can't be overwritten.</span></span> <span data-ttu-id="bffdf-206">Чтобы освободить заблокированные файлы в развертывании, остановите пул приложений.</span><span class="sxs-lookup"><span data-stu-id="bffdf-206">To release locked files in a deployment, stop the app pool:</span></span>

* <span data-ttu-id="bffdf-207">Вручную в диспетчере служб IIS на сервере.</span><span class="sxs-lookup"><span data-stu-id="bffdf-207">Manually in the IIS Manager on the server.</span></span>
* <span data-ttu-id="bffdf-208">С помощью веб-развертывания и ссылки на `Microsoft.NET.Sdk.Web` в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="bffdf-208">Using Web Deploy and referencing `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="bffdf-209">Файл *app_offline.htm* помещается в корень каталога веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="bffdf-209">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="bffdf-210">Если файл присутствует, модуль ASP.NET Core корректно завершает работу приложения и обслуживает файл *app_offline.htm* во время развертывания.</span><span class="sxs-lookup"><span data-stu-id="bffdf-210">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="bffdf-211">Дополнительные сведения см. в разделе [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span><span class="sxs-lookup"><span data-stu-id="bffdf-211">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>
* <span data-ttu-id="bffdf-212">Используйте PowerShell для остановки и перезапуска пула приложений (требуется PowerShell 5 или более поздняя версия).</span><span class="sxs-lookup"><span data-stu-id="bffdf-212">Use PowerShell to stop and restart the app pool (requires PowerShell 5 or later):</span></span>
  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="bffdf-213">Веб-развертывание с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bffdf-213">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="bffdf-214">Сведения о создании профиля публикации для веб-развертывания см. в статье [Профили публикации Visual Studio для развертывания приложений ASP.NET Core](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="bffdf-214">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="bffdf-215">Если поставщик услуг размещения предоставляет профиль публикации или поддерживает его создание, скачайте этот профиль и импортируйте его с помощью диалогового окна **Публикация** в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bffdf-215">If the hosting provider supplies a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![Диалоговое окно "Публикация"](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="bffdf-217">Веб-развертывание вне Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bffdf-217">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="bffdf-218">[Веб-развертывание](/iis/publish/using-web-deploy/introduction-to-web-deploy) можно также использовать вне Visual Studio с помощью командной строки.</span><span class="sxs-lookup"><span data-stu-id="bffdf-218">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="bffdf-219">Дополнительные сведения см. в статье [Средство веб-развертывания](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span><span class="sxs-lookup"><span data-stu-id="bffdf-219">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="bffdf-220">Альтернативы веб-развертыванию</span><span class="sxs-lookup"><span data-stu-id="bffdf-220">Alternatives to Web Deploy</span></span>

<span data-ttu-id="bffdf-221">Используйте любой из нескольких методов для перемещения приложения в систему размещения, такую как Xcopy, Robocopy или PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bffdf-221">Use any of several methods to move the app to the hosting system, such as Xcopy, Robocopy, or PowerShell.</span></span> <span data-ttu-id="bffdf-222">Пользователи Visual Studio могут применять [образцы публикации](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span><span class="sxs-lookup"><span data-stu-id="bffdf-222">Visual Studio users may use the [Publish Samples](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="bffdf-223">Обзор веб-сайта</span><span class="sxs-lookup"><span data-stu-id="bffdf-223">Browse the website</span></span>

![Начальная страница IIS, загруженная в браузере Microsoft Edge.](index/_static/browsewebsite.png)

## <a name="data-protection"></a><span data-ttu-id="bffdf-225">Защита данных</span><span class="sxs-lookup"><span data-stu-id="bffdf-225">Data protection</span></span>

<span data-ttu-id="bffdf-226">Защита данных используется некоторым программным обеспечением промежуточного слоя ASP.NET, в том числе применяемым для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="bffdf-226">Data Protection is used by several ASP.NET middlewares, including those used in authentication.</span></span> <span data-ttu-id="bffdf-227">Даже если API защиты данных не вызываются из пользовательского кода, защиту данных следует настроить с помощью скрипта развертывания или в пользовательском коде для создания постоянного хранилища ключей.</span><span class="sxs-lookup"><span data-stu-id="bffdf-227">Even if Data Protection APIs aren't called from user's code, Data Protection should be configured with a deployment script or in user code to create a persistent key store.</span></span> <span data-ttu-id="bffdf-228">Если защита данных не настроена, ключи хранятся в памяти и удаляются при перезапуске приложения.</span><span class="sxs-lookup"><span data-stu-id="bffdf-228">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="bffdf-229">Если набор ключей хранится в памяти, то при перезапуске приложения происходит следующее.</span><span class="sxs-lookup"><span data-stu-id="bffdf-229">If the keyring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="bffdf-230">Все токены проверки подлинности на основе форм становятся недействительными.</span><span class="sxs-lookup"><span data-stu-id="bffdf-230">All forms authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="bffdf-231">При выполнении следующего запроса пользователю требуется выполнить вход снова.</span><span class="sxs-lookup"><span data-stu-id="bffdf-231">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="bffdf-232">Любые данные, защищенные с помощью набора ключей, больше не могут быть расшифрованы.</span><span class="sxs-lookup"><span data-stu-id="bffdf-232">Any data protected with the keyring can no longer be decrypted.</span></span>

<span data-ttu-id="bffdf-233">Чтобы настроить защиту данных в службах IIS, следует использовать **один** из описанных ниже подходов.</span><span class="sxs-lookup"><span data-stu-id="bffdf-233">To configure Data Protection under IIS, use **one** of the following approaches:</span></span>

### <a name="create-a-data-protection-registry-hive"></a><span data-ttu-id="bffdf-234">Создание куста реестра для защиты данных</span><span class="sxs-lookup"><span data-stu-id="bffdf-234">Create a Data Protection Registry Hive</span></span>

<span data-ttu-id="bffdf-235">Ключи защиты данных, используемые приложениями ASP.NET, хранятся в кустах реестра, являющихся внешними для приложений.</span><span class="sxs-lookup"><span data-stu-id="bffdf-235">Data Protection keys used by ASP.NET apps are stored in registry hives external to the apps.</span></span> <span data-ttu-id="bffdf-236">Чтобы сохранять эти ключи для определенного приложения, нужно создать куст реестра для пула приложений, к которому относится данное приложение.</span><span class="sxs-lookup"><span data-stu-id="bffdf-236">To persist the keys for a given app, create a registry hive for the app pool.</span></span>

<span data-ttu-id="bffdf-237">При автономной установке служб IIS, не поддерживающей веб-ферму, можно выполнить [скрипт PowerShell Provision-AutoGenKeys.ps1 для защиты данных](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) применительно к каждому пулу приложений, в который входит приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bffdf-237">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="bffdf-238">Этот скрипт создает специальный раздел в реестре HKLM, который будет доступен только для учетной записи рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="bffdf-238">This script creates a special registry key in the HKLM registry that's ACLed only to the worker process account.</span></span> <span data-ttu-id="bffdf-239">Неактивные ключи шифруются с помощью API защиты данных с ключом компьютера.</span><span class="sxs-lookup"><span data-stu-id="bffdf-239">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

<span data-ttu-id="bffdf-240">В веб-ферме приложение можно настроить так, чтобы для хранения набора ключей защиты данных использовался UNC-путь.</span><span class="sxs-lookup"><span data-stu-id="bffdf-240">In web farm scenarios, an app can be configured to use a UNC path to store its data protection keyring.</span></span> <span data-ttu-id="bffdf-241">По умолчанию ключи защиты данных не шифруются.</span><span class="sxs-lookup"><span data-stu-id="bffdf-241">By default, the data protection keys are not encrypted.</span></span> <span data-ttu-id="bffdf-242">Разрешения на доступ к файлам в общей папке должны быть предоставлены только учетной записи Windows, от имени которой выполняется приложение.</span><span class="sxs-lookup"><span data-stu-id="bffdf-242">Ensure that the file permissions for such a share are limited to the Windows account the app runs as.</span></span> <span data-ttu-id="bffdf-243">Помимо этого, для защиты неактивных ключей можно использовать сертификат X509.</span><span class="sxs-lookup"><span data-stu-id="bffdf-243">In addition, an X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="bffdf-244">Рассмотрите возможность реализации механизма, позволяющего пользователям отправлять сертификаты: поместить сертификаты в хранилище доверенных сертификатов пользователя и обеспечивать их доступность на всех компьютерах, где выполняется приложение.</span><span class="sxs-lookup"><span data-stu-id="bffdf-244">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="bffdf-245">Подробные сведения см. в разделе [Настройка защиты данных](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="bffdf-245">See [Configuring Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

### <a name="configure-the-iis-application-pool-to-load-the-user-profile"></a><span data-ttu-id="bffdf-246">Настройка пула приложений IIS для загрузки профиля пользователя</span><span class="sxs-lookup"><span data-stu-id="bffdf-246">Configure the IIS Application Pool to load the user profile</span></span>

<span data-ttu-id="bffdf-247">Этот параметр находится на странице **Дополнительные параметры** пула приложений в разделе **Модель процесса**.</span><span class="sxs-lookup"><span data-stu-id="bffdf-247">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="bffdf-248">Задайте для параметра "Загрузить профиль пользователя" значение `True`.</span><span class="sxs-lookup"><span data-stu-id="bffdf-248">Set Load User Profile to `True`.</span></span> <span data-ttu-id="bffdf-249">В результате ключи сохраняются в каталоге профиля пользователя и защищаются посредством API защиты данных с использованием ключа на уровне учетной записи пользователя, применяемой для пула приложений.</span><span class="sxs-lookup"><span data-stu-id="bffdf-249">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used for the app pool.</span></span>

### <a name="use-the-file-system-as-a-key-ring-store"></a><span data-ttu-id="bffdf-250">Использование файловой системы в качестве хранилища набора ключей</span><span class="sxs-lookup"><span data-stu-id="bffdf-250">Use the file system as a key ring store</span></span>

<span data-ttu-id="bffdf-251">Измените код приложения так, чтобы [в качестве хранилища набора ключей использовалась файловая система](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="bffdf-251">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="bffdf-252">Используйте сертификат X509 для защиты набора ключей, причем это должен быть доверенный сертификат.</span><span class="sxs-lookup"><span data-stu-id="bffdf-252">Use an X509 certificate to protect the keyring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="bffdf-253">Если сертификат является самозаверяющим, поместите его в доверенное корневое хранилище.</span><span class="sxs-lookup"><span data-stu-id="bffdf-253">If it's a self signed certificate, place the certificate in the Trusted Root store.</span></span>

<span data-ttu-id="bffdf-254">При использовании служб IIS в веб-ферме:</span><span class="sxs-lookup"><span data-stu-id="bffdf-254">When using IIS in a web farm:</span></span>

* <span data-ttu-id="bffdf-255">используйте общую папку, которая доступна всем компьютерам;</span><span class="sxs-lookup"><span data-stu-id="bffdf-255">Use a file share that all machines can access.</span></span>
* <span data-ttu-id="bffdf-256">разверните сертификат X509 на каждом компьютере;</span><span class="sxs-lookup"><span data-stu-id="bffdf-256">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="bffdf-257">настройте [защиту данных в коде](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="bffdf-257">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

### <a name="set-a-machine-wide-policy-for-data-protection"></a><span data-ttu-id="bffdf-258">Задание политики защиты данных на уровне компьютера</span><span class="sxs-lookup"><span data-stu-id="bffdf-258">Set a machine-wide policy for data protection</span></span>

<span data-ttu-id="bffdf-259">Система защиты данных обеспечивает ограниченную поддержку задания [политики по умолчанию на уровне компьютера](xref:security/data-protection/configuration/machine-wide-policy) для всех приложений, использующих интерфейсы API защиты данных.</span><span class="sxs-lookup"><span data-stu-id="bffdf-259">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="bffdf-260">Дополнительные сведения см. в документации по [защите данных](xref:security/data-protection/index).</span><span class="sxs-lookup"><span data-stu-id="bffdf-260">See the [data protection](xref:security/data-protection/index) documentation for more details.</span></span>

## <a name="configuration-of-sub-applications"></a><span data-ttu-id="bffdf-261">Настройка дочерних приложений</span><span class="sxs-lookup"><span data-stu-id="bffdf-261">Configuration of sub-applications</span></span>

<span data-ttu-id="bffdf-262">Дочерние приложения, добавленные в корневое приложение, не должны включать в себя модуль ASP.NET Core в качестве обработчика.</span><span class="sxs-lookup"><span data-stu-id="bffdf-262">Sub-apps added under the root app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="bffdf-263">Если модуль добавляется в качестве обработчика в файл *web.config* дочернего приложения, то при попытке работы с этим приложением выводится ошибка 500.19 (внутренняя ошибка сервера) с указанием некорректного файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bffdf-263">If the module is added as a handler in a sub-app's *web.config* file, a 500.19 (Internal Server Error) referencing the faulty config file is received when attempting to browse the sub-app.</span></span> <span data-ttu-id="bffdf-264">В приведенном ниже примере показано содержимое опубликованного файла *web.config* для дочернего приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bffdf-264">The following example shows the contents of a published *web.config* file for an ASP.NET Core sub-app:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="bffdf-265">При размещении дочернего приложения не на основе ASP.NET Core в приложении ASP.NET Core, нужно явным образом удалить унаследованный обработчик из файла *web.config* дочернего приложения.</span><span class="sxs-lookup"><span data-stu-id="bffdf-265">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="bffdf-266">Дополнительные сведения о настройке модуля ASP.NET Core см. в статье [Введение в модуль ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) и в [справочнике по настройке модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="bffdf-266">For more information on configuring the ASP.NET Core Module, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="bffdf-267">Настройка служб IIS с помощью файла web.config</span><span class="sxs-lookup"><span data-stu-id="bffdf-267">Configuration of IIS with web.config</span></span>

<span data-ttu-id="bffdf-268">Раздел **\<system.webServer>** файла *web.config* действует для тех компонентов IIS, которые относятся к конфигурации прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="bffdf-268">IIS configuration is influenced by the **\<system.webServer>** section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="bffdf-269">Если в службах IIS на уровне системы настроено динамическое сжатие, для приложения этот параметр можно отключить с помощью элемента **\<urlCompression>** в файле *web.config* приложения.</span><span class="sxs-lookup"><span data-stu-id="bffdf-269">If IIS is configured at the system level to use dynamic compression, that setting can be disabled for an app with the **\<urlCompression>** element in the app's *web.config* file.</span></span> <span data-ttu-id="bffdf-270">Дополнительные сведения см. в [справочнике по настройке \<system.webServer>](https://docs.microsoft.com/iis/configuration/system.webServer/), [справочнике по настройке модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module) и статье [Использование модулей IIS с ASP.NET Core](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="bffdf-270">For more information, see the [configuration reference for \<system.webServer>](https://docs.microsoft.com/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module) and [Using IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="bffdf-271">Если нужно задать переменные среды для отдельных приложений, выполняющихся в изолированных пулах приложений (такая возможность поддерживается в службах IIS 10.0 и более поздних версий), см. подраздел, посвященный *команде AppCmd.exe*, в разделе [Переменные среды \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) справочной документации по службам IIS.</span><span class="sxs-lookup"><span data-stu-id="bffdf-271">If there's a need to set environment variables for individual apps running in isolated app pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="bffdf-272">Разделы конфигурации файла web.config</span><span class="sxs-lookup"><span data-stu-id="bffdf-272">Configuration sections of web.config</span></span>

<span data-ttu-id="bffdf-273">Разделы конфигурации приложений ASP.NET в *web.config* не используются в приложениях ASP.NET Core для конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bffdf-273">Configruation sections of ASP.NET Framework apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* <span data-ttu-id="bffdf-274">**\<system.web>**</span><span class="sxs-lookup"><span data-stu-id="bffdf-274">**\<system.web>**</span></span>
* <span data-ttu-id="bffdf-275">**\<appSettings>**</span><span class="sxs-lookup"><span data-stu-id="bffdf-275">**\<appSettings>**</span></span>
* <span data-ttu-id="bffdf-276">**\<connectionStrings >**</span><span class="sxs-lookup"><span data-stu-id="bffdf-276">**\<connectionStrings>**</span></span>
* <span data-ttu-id="bffdf-277">**\<location>**</span><span class="sxs-lookup"><span data-stu-id="bffdf-277">**\<location>**</span></span>

<span data-ttu-id="bffdf-278">Для настройки приложений ASP.NET Core используются другие поставщики конфигураций.</span><span class="sxs-lookup"><span data-stu-id="bffdf-278">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="bffdf-279">Дополнительные сведения см. в разделе [Конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="bffdf-279">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="bffdf-280">Пулы приложений</span><span class="sxs-lookup"><span data-stu-id="bffdf-280">Application Pools</span></span>

<span data-ttu-id="bffdf-281">Если в одной системе размещается несколько веб-сайтов, приложения следует изолировать друг от друга, запуская каждое из них в отдельном пуле приложений.</span><span class="sxs-lookup"><span data-stu-id="bffdf-281">When hosting multiple websites on a single system, isolate the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="bffdf-282">В диалоговом окне **Добавить веб-сайт** служб IIS такая конфигурация задана по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="bffdf-282">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="bffdf-283">Если указано **имя сайта**, оно автоматически переносится в текстовое поле **Пул приложений**.</span><span class="sxs-lookup"><span data-stu-id="bffdf-283">When **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="bffdf-284">При добавлении веб-сайта создается пул приложений с именем сайта.</span><span class="sxs-lookup"><span data-stu-id="bffdf-284">A new app pool is created using the site name when the website is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="bffdf-285">Удостоверение пула приложений</span><span class="sxs-lookup"><span data-stu-id="bffdf-285">Application Pool Identity</span></span>

<span data-ttu-id="bffdf-286">Учетная запись удостоверения пула приложений позволяет запускать приложение от имени уникальной учетной записи, не создавая домены или локальные учетные записи и не управляя ими.</span><span class="sxs-lookup"><span data-stu-id="bffdf-286">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="bffdf-287">В службах IIS 8.0 и более поздних версий рабочий процесс администратора IIS (WAS) создает виртуальную учетную запись с именем нового пула приложений и по умолчанию выполняет рабочие процессы пула приложений с этой учетной записью.</span><span class="sxs-lookup"><span data-stu-id="bffdf-287">On IIS 8.0+, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="bffdf-288">В консоли управления IIS в окне **Дополнительные параметры** для пула приложений должно быть выбрано **Удостоверение** **ApplicationPoolIdentity**.</span><span class="sxs-lookup"><span data-stu-id="bffdf-288">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![Диалоговое окно дополнительных параметров для пула приложений](index/_static/apppool-identity.png)

<span data-ttu-id="bffdf-290">Процесс управления IIS создает в системе безопасности Windows безопасный идентификатор с именем пула приложений.</span><span class="sxs-lookup"><span data-stu-id="bffdf-290">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="bffdf-291">С помощью этого удостоверения можно защищать ресурсы, но оно не представляет реальную учетную запись пользователя и не отображается в консоли управления Windows пользователя.</span><span class="sxs-lookup"><span data-stu-id="bffdf-291">Resources can be secured by using this identity; however, this identity isn't a real user account and won't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="bffdf-292">Если рабочему процессу IIS требуется предоставить доступ к приложению с повышенными правами, измените список управления доступом (ACL) для каталога, содержащего приложение.</span><span class="sxs-lookup"><span data-stu-id="bffdf-292">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="bffdf-293">Откройте проводник и перейдите к каталогу.</span><span class="sxs-lookup"><span data-stu-id="bffdf-293">Open Windows Explorer and navigate to the directory.</span></span>

2. <span data-ttu-id="bffdf-294">Щелкните каталог правой кнопкой мыши и выберите пункт **Свойства**.</span><span class="sxs-lookup"><span data-stu-id="bffdf-294">Right-click on the directory and select **Properties**.</span></span>

3. <span data-ttu-id="bffdf-295">На вкладке **Безопасность** нажмите кнопку **Изменить**, а затем — кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="bffdf-295">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

4. <span data-ttu-id="bffdf-296">Нажмите кнопку **Размещение** и выберите систему.</span><span class="sxs-lookup"><span data-stu-id="bffdf-296">Select the **Locations** button and make sure the system is selected.</span></span>

5. <span data-ttu-id="bffdf-297">В текстовом поле **Введите имена выбираемых объектов** введите **IIS AppPool\DefaultAppPool**.</span><span class="sxs-lookup"><span data-stu-id="bffdf-297">Enter **IIS AppPool\DefaultAppPool** in **Enter the object names to select** textbox.</span></span>

   ![Диалоговое окно "Выбор пользователей или групп" для папки приложения](index/_static/select-users-or-groups-1.png)

6. <span data-ttu-id="bffdf-299">Нажмите кнопку **Проверить имена**.</span><span class="sxs-lookup"><span data-stu-id="bffdf-299">Select the **Check Names** button.</span></span> <span data-ttu-id="bffdf-300">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="bffdf-300">Select **OK**.</span></span>

   ![Диалоговое окно "Выбор пользователей или групп" для папки приложения](index/_static/select-users-or-groups-2.png)

<span data-ttu-id="bffdf-302">Эту задачу можно также выполнить с помощью командной строки, используя средство **ICACLS**.</span><span class="sxs-lookup"><span data-stu-id="bffdf-302">This can also be accomplished via a command prompt using the **ICACLS** tool:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="additional-resources"></a><span data-ttu-id="bffdf-303">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="bffdf-303">Additional resources</span></span>

* [<span data-ttu-id="bffdf-304">Устранение неполадок ASP.NET Core в службах IIS</span><span class="sxs-lookup"><span data-stu-id="bffdf-304">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="bffdf-305">Справочник по общим ошибкам для службы приложений Azure и служб IIS с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bffdf-305">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="bffdf-306">Введение в модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bffdf-306">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="bffdf-307">Справочник по конфигурации модуля ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bffdf-307">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="bffdf-308">Использование модулей IIS с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bffdf-308">Using IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="bffdf-309">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bffdf-309">Introduction to ASP.NET Core</span></span>](../index.md)
* [<span data-ttu-id="bffdf-310">Официальный веб-сайт Microsoft IIS</span><span class="sxs-lookup"><span data-stu-id="bffdf-310">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="bffdf-311">Библиотека Microsoft TechNet: Windows Server</span><span class="sxs-lookup"><span data-stu-id="bffdf-311">Microsoft TechNet Library: Windows Server</span></span>](https://docs.microsoft.com/windows-server/windows-server-versions)
