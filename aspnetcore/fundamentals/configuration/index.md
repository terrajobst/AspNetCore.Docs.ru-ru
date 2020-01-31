---
title: Конфигурация в .NET Core
author: guardrex
description: Узнайте, как использовать API конфигурации для настройки приложения ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: df49286c3f050b8e90cb5427cf03e2b896a39294
ms.sourcegitcommit: c81ef12a1b6e6ac838e5e07042717cf492e6635b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2020
ms.locfileid: "76885562"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="68d1b-103">Конфигурация в .NET Core</span><span class="sxs-lookup"><span data-stu-id="68d1b-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="68d1b-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="68d1b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="68d1b-105">Конфигурация приложения в ASP.NET Core основана на парах "ключ — значение", установленных *поставщиками конфигурации*.</span><span class="sxs-lookup"><span data-stu-id="68d1b-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="68d1b-106">Поставщики конфигурации получают данные конфигурации в парах "ключ — значение" из различных источников:</span><span class="sxs-lookup"><span data-stu-id="68d1b-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="68d1b-107">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="68d1b-107">Azure Key Vault</span></span>
* <span data-ttu-id="68d1b-108">конфигурация приложения Azure;</span><span class="sxs-lookup"><span data-stu-id="68d1b-108">Azure App Configuration</span></span>
* <span data-ttu-id="68d1b-109">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="68d1b-109">Command-line arguments</span></span>
* <span data-ttu-id="68d1b-110">а также пользовательские поставщики (устанавливаемые или создаваемые).</span><span class="sxs-lookup"><span data-stu-id="68d1b-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="68d1b-111">справочных файлов;</span><span class="sxs-lookup"><span data-stu-id="68d1b-111">Directory files</span></span>
* <span data-ttu-id="68d1b-112">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="68d1b-112">Environment variables</span></span>
* <span data-ttu-id="68d1b-113">объектов .NET в памяти;</span><span class="sxs-lookup"><span data-stu-id="68d1b-113">In-memory .NET objects</span></span>
* <span data-ttu-id="68d1b-114">файлов параметров;</span><span class="sxs-lookup"><span data-stu-id="68d1b-114">Settings files</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="68d1b-115">Пакеты конфигурации для распространенных вариантов провайдеров конфигурации ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) включаются платформой неявным образом.</span><span class="sxs-lookup"><span data-stu-id="68d1b-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="68d1b-116">Пакеты конфигурации для распространенных вариантов провайдеров конфигурации ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) включаются в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="68d1b-116">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="68d1b-117">В приведенных ниже и представленных в образце приложения примерах кода используется пространство имен <xref:Microsoft.Extensions.Configuration>:</span><span class="sxs-lookup"><span data-stu-id="68d1b-117">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="68d1b-118">*Шаблон параметров* является расширением конфигурации основных понятий, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="68d1b-118">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="68d1b-119">Параметры используют классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="68d1b-119">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="68d1b-120">Для получения дополнительной информации см. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="68d1b-120">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="68d1b-121">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="68d1b-121">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="68d1b-122">Конфигурация узла и приложения</span><span class="sxs-lookup"><span data-stu-id="68d1b-122">Host versus app configuration</span></span>

<span data-ttu-id="68d1b-123">Перед настройкой и запуском приложения настройте и запустите *узел*.</span><span class="sxs-lookup"><span data-stu-id="68d1b-123">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="68d1b-124">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="68d1b-124">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="68d1b-125">Как приложение, так и узел настраиваются с использованием поставщиков конфигурации, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="68d1b-125">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="68d1b-126">Пары "ключ — значение" конфигурации узлов также включаются в конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="68d1b-126">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="68d1b-127">Дополнительные сведения о том, как используются поставщики конфигурации при создании узла и как влияют источники конфигурации на узел, см. в разделе <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="68d1b-127">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="68d1b-128">Другая конфигурация</span><span class="sxs-lookup"><span data-stu-id="68d1b-128">Other configuration</span></span>

<span data-ttu-id="68d1b-129">Этот раздел относится только к *конфигурации приложений*.</span><span class="sxs-lookup"><span data-stu-id="68d1b-129">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="68d1b-130">Другие аспекты запуска и размещения приложений ASP.NET Core настраиваются с помощью файлов конфигурации, которые не рассматриваются в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="68d1b-130">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="68d1b-131">*launch.json*/*launchSettings.json* — это файлы конфигурации инструментов для среды разработки, описанные в следующих источниках:</span><span class="sxs-lookup"><span data-stu-id="68d1b-131">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="68d1b-132">В <xref:fundamentals/environments#development>.</span><span class="sxs-lookup"><span data-stu-id="68d1b-132">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="68d1b-133">В документации, где показано, как файлы используются для настройки приложений ASP.NET Core для сценариев разработки.</span><span class="sxs-lookup"><span data-stu-id="68d1b-133">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="68d1b-134">*web.config* — это файл конфигурации сервера, описанный в следующих источниках:</span><span class="sxs-lookup"><span data-stu-id="68d1b-134">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="68d1b-135">См. сведения о переносе конфигурации приложения из более ранних версий ASP.NET: <xref:migration/proper-to-2x/index#store-configurations>.</span><span class="sxs-lookup"><span data-stu-id="68d1b-135">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="68d1b-136">Конфигурация по умолчанию</span><span class="sxs-lookup"><span data-stu-id="68d1b-136">Default configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="68d1b-137">Веб-приложения на основе шаблонов [dotnet new](/dotnet/core/tools/dotnet-new) ASP.NET Core вызывают <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> при создании узла.</span><span class="sxs-lookup"><span data-stu-id="68d1b-137">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="68d1b-138">`CreateDefaultBuilder` предоставляет конфигурацию по умолчанию для приложения в следующем порядке:</span><span class="sxs-lookup"><span data-stu-id="68d1b-138">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="68d1b-139">Приведенные ниже сведения относятся к приложениям, использующим [универсальный узел](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="68d1b-139">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="68d1b-140">Подробные сведения о конфигурации по умолчанию при использовании [веб-узла](xref:fundamentals/host/web-host) см. в [разделе о версии ASP.NET Core 2.2 в этой статье](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="68d1b-140">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="68d1b-141">Существуют следующие способы предоставления конфигурации узла.</span><span class="sxs-lookup"><span data-stu-id="68d1b-141">Host configuration is provided from:</span></span>
  * <span data-ttu-id="68d1b-142">Переменные среды с префиксом `DOTNET_` (например, `DOTNET_ENVIRONMENT`), использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="68d1b-142">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="68d1b-143">Префикс (`DOTNET_`) исключается при загрузке пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-143">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="68d1b-144">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="68d1b-144">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="68d1b-145">Устанавливается конфигурация веб-узла по умолчанию (`ConfigureWebHostDefaults`):</span><span class="sxs-lookup"><span data-stu-id="68d1b-145">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="68d1b-146">Kestrel используется в качестве веб-сервера и настраивается с помощью поставщиков конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="68d1b-146">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="68d1b-147">Добавьте ПО промежуточного слоя фильтрации узлов.</span><span class="sxs-lookup"><span data-stu-id="68d1b-147">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="68d1b-148">Если переменной среды `ASPNETCORE_FORWARDEDHEADERS_ENABLED` присвоено значение `true`, добавьте ПО промежуточного слоя перенаправления заголовков.</span><span class="sxs-lookup"><span data-stu-id="68d1b-148">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="68d1b-149">Включите интеграцию служб IIS.</span><span class="sxs-lookup"><span data-stu-id="68d1b-149">Enable IIS integration.</span></span>
* <span data-ttu-id="68d1b-150">Конфигурация приложения предоставляется из следующих ресурсов:</span><span class="sxs-lookup"><span data-stu-id="68d1b-150">App configuration is provided from:</span></span>
  * <span data-ttu-id="68d1b-151">Файл *appsettings.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="68d1b-151">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="68d1b-152">Файл *appsettings.{Environment}.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="68d1b-152">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="68d1b-153">[Менеджера секретов](xref:security/app-secrets), когда приложение выполняется в среде `Development` с использованием начальных сборок.</span><span class="sxs-lookup"><span data-stu-id="68d1b-153">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="68d1b-154">Переменные среды, использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="68d1b-154">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="68d1b-155">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="68d1b-155">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="68d1b-156">Веб-приложения на основе шаблонов [dotnet new](/dotnet/core/tools/dotnet-new) ASP.NET Core вызывают <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> при создании узла.</span><span class="sxs-lookup"><span data-stu-id="68d1b-156">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="68d1b-157">`CreateDefaultBuilder` предоставляет конфигурацию по умолчанию для приложения в следующем порядке:</span><span class="sxs-lookup"><span data-stu-id="68d1b-157">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="68d1b-158">Приведенные ниже сведения относятся к приложениям, использующим [веб-узел](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="68d1b-158">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="68d1b-159">Подробные сведения о конфигурации по умолчанию при использовании [универсального узла](xref:fundamentals/host/generic-host) см. в [последней версии этой статьи](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="68d1b-159">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="68d1b-160">Существуют следующие способы предоставления конфигурации узла.</span><span class="sxs-lookup"><span data-stu-id="68d1b-160">Host configuration is provided from:</span></span>
  * <span data-ttu-id="68d1b-161">Переменные среды с префиксом `ASPNETCORE_` (например, `ASPNETCORE_ENVIRONMENT`), использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="68d1b-161">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="68d1b-162">Префикс (`ASPNETCORE_`) исключается при загрузке пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-162">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="68d1b-163">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="68d1b-163">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="68d1b-164">Конфигурация приложения предоставляется из следующих ресурсов:</span><span class="sxs-lookup"><span data-stu-id="68d1b-164">App configuration is provided from:</span></span>
  * <span data-ttu-id="68d1b-165">Файл *appsettings.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="68d1b-165">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="68d1b-166">Файл *appsettings.{Environment}.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="68d1b-166">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="68d1b-167">[Менеджера секретов](xref:security/app-secrets), когда приложение выполняется в среде `Development` с использованием начальных сборок.</span><span class="sxs-lookup"><span data-stu-id="68d1b-167">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="68d1b-168">Переменные среды, использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="68d1b-168">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="68d1b-169">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="68d1b-169">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

## <a name="security"></a><span data-ttu-id="68d1b-170">Безопасность</span><span class="sxs-lookup"><span data-stu-id="68d1b-170">Security</span></span>

<span data-ttu-id="68d1b-171">Применяйте описанные ниже методики для защиты конфиденциальных данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-171">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="68d1b-172">Никогда не храните пароли или другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации обычного текста.</span><span class="sxs-lookup"><span data-stu-id="68d1b-172">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="68d1b-173">Не используйте секреты рабочей среды в средах разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="68d1b-173">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="68d1b-174">Указывайте секреты вне проекта, чтобы их нельзя было случайно зафиксировать в репозитории с исходным кодом.</span><span class="sxs-lookup"><span data-stu-id="68d1b-174">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="68d1b-175">Дополнительные сведения см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="68d1b-175">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="68d1b-176"><xref:security/app-secrets> &ndash; содержит рекомендации по использованию переменных среды для хранения конфиденциальных данных.</span><span class="sxs-lookup"><span data-stu-id="68d1b-176"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="68d1b-177">Менеджер секретов использует поставщик конфигурации файла для хранения конфиденциальных данных пользователя в файле JSON в локальной системе.</span><span class="sxs-lookup"><span data-stu-id="68d1b-177">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="68d1b-178">Поставщик конфигурации файлов описан ниже в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="68d1b-178">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="68d1b-179">В [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) безопасно хранятся секреты приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="68d1b-179">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="68d1b-180">Для получения дополнительной информации см. <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="68d1b-180">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="68d1b-181">Иерархическая модель конфигурации</span><span class="sxs-lookup"><span data-stu-id="68d1b-181">Hierarchical configuration data</span></span>

<span data-ttu-id="68d1b-182">API конфигурации способен поддерживать иерархические данные конфигурации, выполняя преобразование в плоскую структуру иерархических данных с использованием разделителя в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-182">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="68d1b-183">В следующем файле JSON существуют четыре ключа в структурированной иерархии двух разделов.</span><span class="sxs-lookup"><span data-stu-id="68d1b-183">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

<span data-ttu-id="68d1b-184">При считывании файла в конфигурацию для сохранения исходной иерархической структуры данных источника конфигурации создаются уникальные ключи.</span><span class="sxs-lookup"><span data-stu-id="68d1b-184">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="68d1b-185">Разделы и ключи преобразовываются в плоскую структуру с использованием двоеточия (`:`) для сохранения исходной структуры.</span><span class="sxs-lookup"><span data-stu-id="68d1b-185">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="68d1b-186">section0:key0</span><span class="sxs-lookup"><span data-stu-id="68d1b-186">section0:key0</span></span>
* <span data-ttu-id="68d1b-187">section0:key1</span><span class="sxs-lookup"><span data-stu-id="68d1b-187">section0:key1</span></span>
* <span data-ttu-id="68d1b-188">section1:key0</span><span class="sxs-lookup"><span data-stu-id="68d1b-188">section1:key0</span></span>
* <span data-ttu-id="68d1b-189">section1:key1</span><span class="sxs-lookup"><span data-stu-id="68d1b-189">section1:key1</span></span>

<span data-ttu-id="68d1b-190">Методы <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> и <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> доступны для изолирования разделов и дочерних элементов раздела в данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-190"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="68d1b-191">Эти методы описаны далее в разделе [GetSection, GetChildren и Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="68d1b-191">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="68d1b-192">Соглашения</span><span class="sxs-lookup"><span data-stu-id="68d1b-192">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="68d1b-193">Источники и поставщики</span><span class="sxs-lookup"><span data-stu-id="68d1b-193">Sources and providers</span></span>

<span data-ttu-id="68d1b-194">При запуске приложения источники конфигурации считываются в порядке, в котором были указаны их поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-194">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="68d1b-195">Поставщики конфигурации, которые реализуют обнаружение изменений, имеют возможность перезагрузить конфигурацию при изменении базовых параметров.</span><span class="sxs-lookup"><span data-stu-id="68d1b-195">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="68d1b-196">Так, поставщик файла конфигурации (подробнее о нем далее в этой статье) и [поставщик конфигурации Azure Key Vault](xref:security/key-vault-configuration) реализуют обнаружение изменений.</span><span class="sxs-lookup"><span data-stu-id="68d1b-196">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="68d1b-197">Объект <xref:Microsoft.Extensions.Configuration.IConfiguration> доступен в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) приложения.</span><span class="sxs-lookup"><span data-stu-id="68d1b-197"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="68d1b-198"><xref:Microsoft.Extensions.Configuration.IConfiguration> можно внедрить в <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> Razor Pages или <xref:Microsoft.AspNetCore.Mvc.Controller> MVC, чтобы получить конфигурацию для класса.</span><span class="sxs-lookup"><span data-stu-id="68d1b-198"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="68d1b-199">В следующих примерах поле `_config` используется для доступа к значениям конфигурации:</span><span class="sxs-lookup"><span data-stu-id="68d1b-199">In the following examples, the `_config` field is used to access configuration values:</span></span>

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }
}
```

```csharp
public class HomeController : Controller
{
    private readonly IConfiguration _config;

    public HomeController(IConfiguration config)
    {
        _config = config;
    }
}
```

<span data-ttu-id="68d1b-200">Поставщики конфигурации не могут использовать контейнер DI, так как он недоступен при настройке узла.</span><span class="sxs-lookup"><span data-stu-id="68d1b-200">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="68d1b-201">Клавиши</span><span class="sxs-lookup"><span data-stu-id="68d1b-201">Keys</span></span>

<span data-ttu-id="68d1b-202">В ключах конфигурации приняты следующие соглашения.</span><span class="sxs-lookup"><span data-stu-id="68d1b-202">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="68d1b-203">В ключах не учитывается регистр символов.</span><span class="sxs-lookup"><span data-stu-id="68d1b-203">Keys are case-insensitive.</span></span> <span data-ttu-id="68d1b-204">Например `ConnectionString` и `connectionstring` обрабатываются как эквивалентные ключи.</span><span class="sxs-lookup"><span data-stu-id="68d1b-204">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="68d1b-205">Если значение для одного и того же ключа установлено одним и тем же или разными поставщиками конфигурации, последним значением, установленным на ключе, является используемое значение.</span><span class="sxs-lookup"><span data-stu-id="68d1b-205">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="68d1b-206">Иерархические ключи</span><span class="sxs-lookup"><span data-stu-id="68d1b-206">Hierarchical keys</span></span>
  * <span data-ttu-id="68d1b-207">При взаимодействии с API конфигурации разделитель-двоеточие (`:`) поддерживается на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="68d1b-207">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="68d1b-208">В переменных среды разделитель-двоеточие может не работать на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="68d1b-208">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="68d1b-209">Двойной знак подчеркивания (`__`) поддерживается на всех платформах и автоматически преобразовывается в двоеточие.</span><span class="sxs-lookup"><span data-stu-id="68d1b-209">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="68d1b-210">В хранилище ключей Azure иерархические ключи используют `--` (два дефиса) в качестве разделителя.</span><span class="sxs-lookup"><span data-stu-id="68d1b-210">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="68d1b-211">Чтобы заменить дефисы двоеточием, при загрузке секретов в конфигурацию приложения напишите код.</span><span class="sxs-lookup"><span data-stu-id="68d1b-211">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="68d1b-212"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-212">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="68d1b-213">Привязка массива описана в разделе [Привязка массива к классу](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="68d1b-213">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="68d1b-214">Значения</span><span class="sxs-lookup"><span data-stu-id="68d1b-214">Values</span></span>

<span data-ttu-id="68d1b-215">В значениях конфигурации учитываются следующие соглашения.</span><span class="sxs-lookup"><span data-stu-id="68d1b-215">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="68d1b-216">Значения являются строками.</span><span class="sxs-lookup"><span data-stu-id="68d1b-216">Values are strings.</span></span>
* <span data-ttu-id="68d1b-217">Значение NULL не может храниться в конфигурации или быть привязанным к объектам.</span><span class="sxs-lookup"><span data-stu-id="68d1b-217">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="68d1b-218">Поставщики</span><span class="sxs-lookup"><span data-stu-id="68d1b-218">Providers</span></span>

<span data-ttu-id="68d1b-219">В следующей таблице показаны поставщики конфигурации, доступные для приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="68d1b-219">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="68d1b-220">Поставщик</span><span class="sxs-lookup"><span data-stu-id="68d1b-220">Provider</span></span> | <span data-ttu-id="68d1b-221">Предоставляет конфигурацию из &hellip;</span><span class="sxs-lookup"><span data-stu-id="68d1b-221">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="68d1b-222">[Поставщик конфигурации хранилища ключей Azure](xref:security/key-vault-configuration) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="68d1b-222">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="68d1b-223">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="68d1b-223">Azure Key Vault</span></span> |
| <span data-ttu-id="68d1b-224">[Поставщик конфигурации приложений Azure](/azure/azure-app-configuration/quickstart-aspnet-core-app) (документация Azure)</span><span class="sxs-lookup"><span data-stu-id="68d1b-224">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="68d1b-225">конфигурация приложения Azure;</span><span class="sxs-lookup"><span data-stu-id="68d1b-225">Azure App Configuration</span></span> |
| [<span data-ttu-id="68d1b-226">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="68d1b-226">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="68d1b-227">Параметры командной строки</span><span class="sxs-lookup"><span data-stu-id="68d1b-227">Command-line parameters</span></span> |
| [<span data-ttu-id="68d1b-228">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="68d1b-228">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="68d1b-229">Источник пользователя</span><span class="sxs-lookup"><span data-stu-id="68d1b-229">Custom source</span></span> |
| [<span data-ttu-id="68d1b-230">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="68d1b-230">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="68d1b-231">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="68d1b-231">Environment variables</span></span> |
| [<span data-ttu-id="68d1b-232">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="68d1b-232">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="68d1b-233">Файлы (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="68d1b-233">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="68d1b-234">Поставщик конфигурации ключа для каждого файла</span><span class="sxs-lookup"><span data-stu-id="68d1b-234">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="68d1b-235">Справочные файлы</span><span class="sxs-lookup"><span data-stu-id="68d1b-235">Directory files</span></span> |
| [<span data-ttu-id="68d1b-236">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="68d1b-236">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="68d1b-237">Коллекции оперативной памяти</span><span class="sxs-lookup"><span data-stu-id="68d1b-237">In-memory collections</span></span> |
| <span data-ttu-id="68d1b-238">[Секреты пользователей (Менеджер секретов)](xref:security/app-secrets) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="68d1b-238">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="68d1b-239">Файл в каталоге профиля пользователя</span><span class="sxs-lookup"><span data-stu-id="68d1b-239">File in the user profile directory</span></span> |

<span data-ttu-id="68d1b-240">Источники конфигурации считываются в том порядке, в котором при запуске указываются их поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-240">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="68d1b-241">Поставщики конфигурации в этом разделе описаны в алфавитном порядке, а не в необходимом вам порядке в коде.</span><span class="sxs-lookup"><span data-stu-id="68d1b-241">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="68d1b-242">Порядок поставщиков конфигурации в коде соответствует приоритетам ваших основных источников конфигурации, требуемых приложением.</span><span class="sxs-lookup"><span data-stu-id="68d1b-242">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="68d1b-243">Типичная последовательность поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-243">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="68d1b-244">Файлы (*appsettings.json*, *appsettings.{Environment}.json*, где `{Environment}` — это текущая среда размещения приложения)</span><span class="sxs-lookup"><span data-stu-id="68d1b-244">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. <span data-ttu-id="68d1b-245">[Azure Key Vault](xref:security/key-vault-configuration);</span><span class="sxs-lookup"><span data-stu-id="68d1b-245">[Azure Key Vault](xref:security/key-vault-configuration)</span></span>
1. <span data-ttu-id="68d1b-246">[Секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки)</span><span class="sxs-lookup"><span data-stu-id="68d1b-246">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="68d1b-247">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="68d1b-247">Environment variables</span></span>
1. <span data-ttu-id="68d1b-248">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="68d1b-248">Command-line arguments</span></span>

<span data-ttu-id="68d1b-249">Общепринятой практикой является размещение поставщика конфигурации командной строки последним в ряду поставщиков, чтобы аргументы командной строки могли переопределять конфигурацию, установленную другими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="68d1b-249">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="68d1b-250">Предыдущая последовательность поставщиков используется при инициализации нового построителя узла с помощью `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-250">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="68d1b-251">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="68d1b-251">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="68d1b-252">Настройка построителя узла с помощью ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="68d1b-252">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="68d1b-253">Чтобы настроить построитель узла, вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> и укажите конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="68d1b-253">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="68d1b-254">`ConfigureHostConfiguration` используется для инициализации <xref:Microsoft.Extensions.Hosting.IHostEnvironment> для дальнейшего использования в процессе сборки.</span><span class="sxs-lookup"><span data-stu-id="68d1b-254">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="68d1b-255">Метод `ConfigureHostConfiguration` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="68d1b-255">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureHostConfiguration(config =>
        {
            var dict = new Dictionary<string, string>
            {
                {"MemoryCollectionKey1", "value1"},
                {"MemoryCollectionKey2", "value2"}
            };

            config.AddInMemoryCollection(dict);
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="68d1b-256">Настройка построителя узла с помощью UseConfiguration</span><span class="sxs-lookup"><span data-stu-id="68d1b-256">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="68d1b-257">Чтобы настроить построитель узла, вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> в построителе узла с конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="68d1b-257">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

    var config = new ConfigurationBuilder()
        .AddInMemoryCollection(dict)
        .Build();

    return WebHost.CreateDefaultBuilder(args)
        .UseConfiguration(config)
        .UseStartup<Startup>();
}
```

::: moniker-end

## <a name="configureappconfiguration"></a><span data-ttu-id="68d1b-258">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="68d1b-258">ConfigureAppConfiguration</span></span>

<span data-ttu-id="68d1b-259">Вызовите `ConfigureAppConfiguration` при сборке узла, чтобы указать поставщики конфигурации приложения в дополнение к тем, которые автоматически добавлены `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-259">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="68d1b-260">Переопределение предыдущей конфигурации с помощью аргументов командной строки</span><span class="sxs-lookup"><span data-stu-id="68d1b-260">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="68d1b-261">Чтобы предоставить конфигурацию приложения, которую можно переопределить с помощью аргументов командной строки, вызовите метод `AddCommandLine` последним:</span><span class="sxs-lookup"><span data-stu-id="68d1b-261">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="68d1b-262">Удаление поставщиков, добавленных CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="68d1b-262">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="68d1b-263">Чтобы удалить поставщиков, добавленных `CreateDefaultBuilder`, сначала вызовите [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) в [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources):</span><span class="sxs-lookup"><span data-stu-id="68d1b-263">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="68d1b-264">Использование конфигурации во время запуска приложения</span><span class="sxs-lookup"><span data-stu-id="68d1b-264">Consume configuration during app startup</span></span>

<span data-ttu-id="68d1b-265">Конфигурация, предоставленная приложению в `ConfigureAppConfiguration`, доступна во время запуска приложения, включая `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-265">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="68d1b-266">Дополнительные сведения см. в разделе [Доступ к конфигурации во время запуска](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="68d1b-266">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="68d1b-267">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="68d1b-267">Command-line Configuration Provider</span></span>

<span data-ttu-id="68d1b-268"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> загружает конфигурацию из пары "ключ — значение" аргумента командной строки в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="68d1b-268">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="68d1b-269">Чтобы активировать конфигурацию командной строки, вызывается метод расширения <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> для экземпляра <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="68d1b-269">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="68d1b-270">`AddCommandLine` вызывается автоматически при вызове `CreateDefaultBuilder(string [])`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-270">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="68d1b-271">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="68d1b-271">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="68d1b-272">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="68d1b-272">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="68d1b-273">дополнительную конфигурацию из файлов *appsettings.json* и *appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="68d1b-273">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="68d1b-274">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="68d1b-274">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="68d1b-275">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="68d1b-275">Environment variables.</span></span>

<span data-ttu-id="68d1b-276">`CreateDefaultBuilder` добавляет последним поставщика конфигурации командной строки.</span><span class="sxs-lookup"><span data-stu-id="68d1b-276">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="68d1b-277">Аргументы командной строки передаются в набор конфигурации переопределения среды выполнения, установленной другими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="68d1b-277">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="68d1b-278">`CreateDefaultBuilder` действует, когда создается узел.</span><span class="sxs-lookup"><span data-stu-id="68d1b-278">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="68d1b-279">Поэтому конфигурация командной строки, активированная с помощью `CreateDefaultBuilder`, может повлиять на настройку узла.</span><span class="sxs-lookup"><span data-stu-id="68d1b-279">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="68d1b-280">Для приложений на основе шаблонов ASP.NET Core метод `AddCommandLine` уже был вызван методом `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-280">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="68d1b-281">Чтобы добавить дополнительные поставщики конфигурации и обеспечить возможность переопределения конфигурации от этих поставщиков с помощью аргументов командной строки, вызовите дополнительные поставщики приложения в `ConfigureAppConfiguration` и вызовите `AddCommandLine` в последнюю очередь.</span><span class="sxs-lookup"><span data-stu-id="68d1b-281">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="68d1b-282">**Пример**</span><span class="sxs-lookup"><span data-stu-id="68d1b-282">**Example**</span></span>

<span data-ttu-id="68d1b-283">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="68d1b-283">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="68d1b-284">Откройте командную строку в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="68d1b-284">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="68d1b-285">Поставьте аргумент командной строки в команду `dotnet run`: `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-285">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="68d1b-286">После запуска приложения откройте браузер в приложении по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-286">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="68d1b-287">Обратите внимание, что вывод содержит пару "ключ — значение" для аргумента командной строки конфигурации, предоставленного для `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-287">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="68d1b-288">Аргументы</span><span class="sxs-lookup"><span data-stu-id="68d1b-288">Arguments</span></span>

<span data-ttu-id="68d1b-289">Значение должно соответствовать знаку равенства (`=`), или ключ должен иметь префикс (`--` или `/`), когда значение следует за пробелом.</span><span class="sxs-lookup"><span data-stu-id="68d1b-289">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="68d1b-290">Значение не требуется, если используется знак равенства (например, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="68d1b-290">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="68d1b-291">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="68d1b-291">Key prefix</span></span>               | <span data-ttu-id="68d1b-292">Пример</span><span class="sxs-lookup"><span data-stu-id="68d1b-292">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="68d1b-293">Без префикса</span><span class="sxs-lookup"><span data-stu-id="68d1b-293">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="68d1b-294">Два дефиса (`--`)</span><span class="sxs-lookup"><span data-stu-id="68d1b-294">Two dashes (`--`)</span></span>        | <span data-ttu-id="68d1b-295">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="68d1b-295">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="68d1b-296">Прямая косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="68d1b-296">Forward slash (`/`)</span></span>      | <span data-ttu-id="68d1b-297">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="68d1b-297">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="68d1b-298">В рамках одной и той же команды не смешивайте пары "ключ — значение" аргумента командной строки, которые используют знак равенства, с парами "ключ — значение", которые используют пробел.</span><span class="sxs-lookup"><span data-stu-id="68d1b-298">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="68d1b-299">Примеры команд.</span><span class="sxs-lookup"><span data-stu-id="68d1b-299">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="68d1b-300">Сопоставления переключений</span><span class="sxs-lookup"><span data-stu-id="68d1b-300">Switch mappings</span></span>

<span data-ttu-id="68d1b-301">Сопоставление параметров позволяет указать логику замены имен ключей.</span><span class="sxs-lookup"><span data-stu-id="68d1b-301">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="68d1b-302">Когда вручную создается конфигурация с помощью <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, методу <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> можно предоставить словарь сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="68d1b-302">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="68d1b-303">В словаре сопоставлений переключений выполняется поиск ключа, который совпадает с ключом, предоставляемым аргументом командной строки.</span><span class="sxs-lookup"><span data-stu-id="68d1b-303">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="68d1b-304">Если ключ в командной строке находится в словаре, значение словаря (замена ключа) передается обратно, чтобы установить пару "ключ — значение" в конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="68d1b-304">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="68d1b-305">Сопоставление переключений необходимо для любого ключа командной строки с префиксом из одного дефиса (`-`).</span><span class="sxs-lookup"><span data-stu-id="68d1b-305">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="68d1b-306">Правила ключей из словаря сопоставления переключений:</span><span class="sxs-lookup"><span data-stu-id="68d1b-306">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="68d1b-307">Переключения должны начинаться с дефиса (`-`) или двойного дефиса (`--`).</span><span class="sxs-lookup"><span data-stu-id="68d1b-307">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="68d1b-308">Словарь сопоставлений переключений не должен содержать повторяющиеся ключи.</span><span class="sxs-lookup"><span data-stu-id="68d1b-308">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="68d1b-309">Создайте словарь сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="68d1b-309">Create a switch mappings dictionary.</span></span> <span data-ttu-id="68d1b-310">В следующем примере создаются два сопоставления переключений:</span><span class="sxs-lookup"><span data-stu-id="68d1b-310">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="68d1b-311">При сборке узла вызовите `AddCommandLine` со словарем сопоставлений переключений:</span><span class="sxs-lookup"><span data-stu-id="68d1b-311">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="68d1b-312">Для приложений, использующих сопоставления переключений, в вызове `CreateDefaultBuilder` аргументы передаваться не должны.</span><span class="sxs-lookup"><span data-stu-id="68d1b-312">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="68d1b-313">Вызов команды `AddCommandLine` метода `CreateDefaultBuilder` не включает сопоставленные переключения, и нет возможности передать словарь сопоставления переключений в `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-313">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="68d1b-314">Чтобы решить эту проблему, нужно не передавать аргументы команде `CreateDefaultBuilder`, а позволить методу `AddCommandLine` метода `ConfigurationBuilder` обрабатывать как аргументы, так и словарь сопоставления параметров.</span><span class="sxs-lookup"><span data-stu-id="68d1b-314">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="68d1b-315">Созданный словарь сопоставлений переключений содержит данные, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="68d1b-315">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="68d1b-316">Ключ</span><span class="sxs-lookup"><span data-stu-id="68d1b-316">Key</span></span>       | <span data-ttu-id="68d1b-317">Значение</span><span class="sxs-lookup"><span data-stu-id="68d1b-317">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="68d1b-318">Если ключи сопоставления переключений используются при запуске приложения, конфигурация принимает значение конфигурации в ключе, предоставленном в словаре.</span><span class="sxs-lookup"><span data-stu-id="68d1b-318">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="68d1b-319">После выполнения предыдущей команды конфигурация содержит значения, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="68d1b-319">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="68d1b-320">Ключ</span><span class="sxs-lookup"><span data-stu-id="68d1b-320">Key</span></span>               | <span data-ttu-id="68d1b-321">Значение</span><span class="sxs-lookup"><span data-stu-id="68d1b-321">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="68d1b-322">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="68d1b-322">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="68d1b-323"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> загружает конфигурацию из пары "ключ — значение" переменной среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="68d1b-323">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="68d1b-324">Чтобы активировать конфигурацию переменных среды, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="68d1b-324">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="68d1b-325">[Служба приложений Azure](https://azure.microsoft.com/services/app-service/) позволяет задать переменные среды на портале Azure, который может переопределить конфигурацию приложения, используя поставщик конфигурации переменных среды.</span><span class="sxs-lookup"><span data-stu-id="68d1b-325">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="68d1b-326">Дополнительные сведения см. в руководстве по [переопределению конфигурации приложения Azure с помощью портала Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="68d1b-326">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="68d1b-327">`AddEnvironmentVariables` используется для загрузки переменных среды, имеющих префикс `DOTNET_`, для [конфигурации узла](#host-versus-app-configuration) при инициализации нового построителя узла с [универсальным узлом](xref:fundamentals/host/generic-host) и вызове `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-327">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="68d1b-328">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="68d1b-328">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="68d1b-329">`AddEnvironmentVariables` используется для загрузки переменных среды, имеющих префикс `ASPNETCORE_`, для [конфигурации узла](#host-versus-app-configuration) при инициализации нового построителя узла с [веб-узлом](xref:fundamentals/host/web-host) и вызове `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-329">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="68d1b-330">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="68d1b-330">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

<span data-ttu-id="68d1b-331">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="68d1b-331">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="68d1b-332">конфигурация приложения на основе переменных среды без префикса путем вызова `AddEnvironmentVariables` без префикса;</span><span class="sxs-lookup"><span data-stu-id="68d1b-332">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="68d1b-333">дополнительную конфигурацию из файлов *appsettings.json* и *appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="68d1b-333">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="68d1b-334">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="68d1b-334">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="68d1b-335">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="68d1b-335">Command-line arguments.</span></span>

<span data-ttu-id="68d1b-336">Поставщик конфигурации переменных среды вызывается после выполнения настройки с помощью секретов пользователя и файлов *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="68d1b-336">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="68d1b-337">Вызов поставщика в этой позиции разрешает чтение переменных среды выполнения, чтобы переопределить конфигурацию, заданную секретом пользователя и файлом *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="68d1b-337">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="68d1b-338">Чтобы добавить конфигурацию приложения из дополнительных переменных среды, вызовите дополнительные поставщики приложения в `ConfigureAppConfiguration`, а затем вызовите `AddEnvironmentVariables` с префиксом:</span><span class="sxs-lookup"><span data-stu-id="68d1b-338">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="68d1b-339">Вызовите `AddEnvironmentVariables` последним, чтобы разрешить переменным среды с заданным префиксом переопределять значения от других поставщиков.</span><span class="sxs-lookup"><span data-stu-id="68d1b-339">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="68d1b-340">**Пример**</span><span class="sxs-lookup"><span data-stu-id="68d1b-340">**Example**</span></span>

<span data-ttu-id="68d1b-341">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-341">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="68d1b-342">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="68d1b-342">Run the sample app.</span></span> <span data-ttu-id="68d1b-343">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-343">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="68d1b-344">Обратите внимание, что выходные данные содержат пару "ключ — значение" для переменной среды `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-344">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="68d1b-345">Значение отражает среду, в которой выполняется приложение, обычно при локальном запуске это `Development`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-345">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="68d1b-346">Чтобы список переменных среды, отображаемый приложением, был коротким, приложение фильтрует переменные среды.</span><span class="sxs-lookup"><span data-stu-id="68d1b-346">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="68d1b-347">См. пример файла *Pages/Index.cshtml.cs* приложения.</span><span class="sxs-lookup"><span data-stu-id="68d1b-347">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="68d1b-348">Чтобы просмотреть все переменные среды, доступные для приложения, измените значение `FilteredConfiguration` в *Pages/Index.cshtml.cs* на следующее:</span><span class="sxs-lookup"><span data-stu-id="68d1b-348">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="68d1b-349">Префиксы</span><span class="sxs-lookup"><span data-stu-id="68d1b-349">Prefixes</span></span>

<span data-ttu-id="68d1b-350">Переменные среды, которые загружаются в конфигурацию приложения, фильтруются при добавлении префикса к методу `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-350">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="68d1b-351">Например, чтобы отфильтровать переменные среды по префиксу `CUSTOM_`, введите префикс поставщику конфигурации:</span><span class="sxs-lookup"><span data-stu-id="68d1b-351">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="68d1b-352">Префикс отделяется при создании пары конфигурации "ключ — значение".</span><span class="sxs-lookup"><span data-stu-id="68d1b-352">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="68d1b-353">При создании построителя узла конфигурация узла предоставляется переменными среды.</span><span class="sxs-lookup"><span data-stu-id="68d1b-353">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="68d1b-354">Дополнительные сведения о префиксе, используемом для этих переменных среды, см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="68d1b-354">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="68d1b-355">**Префиксы строк подключения**</span><span class="sxs-lookup"><span data-stu-id="68d1b-355">**Connection string prefixes**</span></span>

<span data-ttu-id="68d1b-356">API конфигурации имеет специальные правила обработки для четырех строк подключения переменных среды, связанных с настройкой строк подключения Azure для среды приложения.</span><span class="sxs-lookup"><span data-stu-id="68d1b-356">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="68d1b-357">Если префикс не указан в `AddEnvironmentVariables`, переменные среды с префиксами, указанными в таблице, загружаются в приложение.</span><span class="sxs-lookup"><span data-stu-id="68d1b-357">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="68d1b-358">Префикс строки подключения</span><span class="sxs-lookup"><span data-stu-id="68d1b-358">Connection string prefix</span></span> | <span data-ttu-id="68d1b-359">Поставщик</span><span class="sxs-lookup"><span data-stu-id="68d1b-359">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="68d1b-360">Поставщик пользователя</span><span class="sxs-lookup"><span data-stu-id="68d1b-360">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="68d1b-361">MySQL</span><span class="sxs-lookup"><span data-stu-id="68d1b-361">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="68d1b-362">База данных SQL Azure</span><span class="sxs-lookup"><span data-stu-id="68d1b-362">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="68d1b-363">SQL Server</span><span class="sxs-lookup"><span data-stu-id="68d1b-363">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="68d1b-364">Когда переменная среды обнаруживается и загружается в конфигурацию с одним из четырех префиксов, приведенных в таблице, происходит следующее.</span><span class="sxs-lookup"><span data-stu-id="68d1b-364">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="68d1b-365">Ключ конфигурации создается путем удаления префикса переменных среды и добавления ключа раздела конфигурации (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="68d1b-365">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="68d1b-366">Создается новая пара "ключ — значение" конфигурации, которая представляет поставщика подключения базы данных (за исключением `CUSTOMCONNSTR_`, который не имеет указанного поставщика).</span><span class="sxs-lookup"><span data-stu-id="68d1b-366">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="68d1b-367">Ключ переменной среды</span><span class="sxs-lookup"><span data-stu-id="68d1b-367">Environment variable key</span></span> | <span data-ttu-id="68d1b-368">Преобразованный ключ конфигурации</span><span class="sxs-lookup"><span data-stu-id="68d1b-368">Converted configuration key</span></span> | <span data-ttu-id="68d1b-369">Запись конфигурации поставщика</span><span class="sxs-lookup"><span data-stu-id="68d1b-369">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="68d1b-370">Запись конфигурации не создана.</span><span class="sxs-lookup"><span data-stu-id="68d1b-370">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="68d1b-371">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="68d1b-371">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="68d1b-372">Значение: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="68d1b-372">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="68d1b-373">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="68d1b-373">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="68d1b-374">Значение: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="68d1b-374">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="68d1b-375">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="68d1b-375">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="68d1b-376">Значение: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="68d1b-376">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="68d1b-377">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="68d1b-377">File Configuration Provider</span></span>

<span data-ttu-id="68d1b-378"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> является базовым классом для загрузки конфигурации из файловой системы.</span><span class="sxs-lookup"><span data-stu-id="68d1b-378"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="68d1b-379">Следующие поставщики конфигурации предназначены для определенных типов файлов:</span><span class="sxs-lookup"><span data-stu-id="68d1b-379">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="68d1b-380">Поставщик конфигурации INI</span><span class="sxs-lookup"><span data-stu-id="68d1b-380">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="68d1b-381">Поставщик конфигурации JSON</span><span class="sxs-lookup"><span data-stu-id="68d1b-381">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="68d1b-382">Поставщик конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="68d1b-382">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="68d1b-383">Поставщик конфигурации INI</span><span class="sxs-lookup"><span data-stu-id="68d1b-383">INI Configuration Provider</span></span>

<span data-ttu-id="68d1b-384"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> загружает конфигурацию из пары "ключ — значение" INI-файла во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="68d1b-384">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="68d1b-385">Чтобы активировать конфигурацию INI-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="68d1b-385">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="68d1b-386">Двоеточие можно использовать как разделитель раздела в конфигурации файла INI.</span><span class="sxs-lookup"><span data-stu-id="68d1b-386">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="68d1b-387">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="68d1b-387">Overloads permit specifying:</span></span>

* <span data-ttu-id="68d1b-388">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="68d1b-388">Whether the file is optional.</span></span>
* <span data-ttu-id="68d1b-389">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="68d1b-389">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="68d1b-390"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="68d1b-390">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="68d1b-391">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-391">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="68d1b-392">Общий пример конфигурации INI-файла.</span><span class="sxs-lookup"><span data-stu-id="68d1b-392">A generic example of an INI configuration file:</span></span>

```ini
[section0]
key0=value
key1=value

[section1]
subsection:key=value

[section2:subsection0]
key=value

[section2:subsection1]
key=value
```

<span data-ttu-id="68d1b-393">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-393">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="68d1b-394">section0:key0</span><span class="sxs-lookup"><span data-stu-id="68d1b-394">section0:key0</span></span>
* <span data-ttu-id="68d1b-395">section0:key1</span><span class="sxs-lookup"><span data-stu-id="68d1b-395">section0:key1</span></span>
* <span data-ttu-id="68d1b-396">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="68d1b-396">section1:subsection:key</span></span>
* <span data-ttu-id="68d1b-397">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="68d1b-397">section2:subsection0:key</span></span>
* <span data-ttu-id="68d1b-398">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="68d1b-398">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="68d1b-399">Поставщик конфигурации JSON</span><span class="sxs-lookup"><span data-stu-id="68d1b-399">JSON Configuration Provider</span></span>

<span data-ttu-id="68d1b-400"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> загружает конфигурацию из пары "ключ — значение" JSON-файла в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="68d1b-400">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="68d1b-401">Чтобы активировать конфигурацию JSON-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="68d1b-401">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="68d1b-402">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="68d1b-402">Overloads permit specifying:</span></span>

* <span data-ttu-id="68d1b-403">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="68d1b-403">Whether the file is optional.</span></span>
* <span data-ttu-id="68d1b-404">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="68d1b-404">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="68d1b-405"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="68d1b-405">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="68d1b-406">`AddJsonFile` автоматически вызывается дважды при инициализации нового построителя узла с `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-406">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="68d1b-407">Метод вызывается для загрузки конфигурации из:</span><span class="sxs-lookup"><span data-stu-id="68d1b-407">The method is called to load configuration from:</span></span>

* <span data-ttu-id="68d1b-408">*appsettings.json* &ndash; первым читается этот файл.</span><span class="sxs-lookup"><span data-stu-id="68d1b-408">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="68d1b-409">Версия файла среды может переопределить значения, предоставленные *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="68d1b-409">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="68d1b-410">*appsettings.{Environment}.json* &ndash; версия среды файла загружается на основе [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="68d1b-410">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="68d1b-411">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="68d1b-411">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="68d1b-412">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="68d1b-412">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="68d1b-413">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="68d1b-413">Environment variables.</span></span>
* <span data-ttu-id="68d1b-414">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="68d1b-414">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="68d1b-415">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="68d1b-415">Command-line arguments.</span></span>

<span data-ttu-id="68d1b-416">Сначала устанавливается поставщик конфигурации JSON-файлов.</span><span class="sxs-lookup"><span data-stu-id="68d1b-416">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="68d1b-417">Таким образом, секреты пользователя, переменные среды и аргументы командной строки переопределят конфигурацию, заданную файлами *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="68d1b-417">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="68d1b-418">Вызовите `ConfigureAppConfiguration` при сборке узла, чтобы указать конфигурацию приложения для файлов, отличных от *appsettings.json* и *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="68d1b-418">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="68d1b-419">**Пример**</span><span class="sxs-lookup"><span data-stu-id="68d1b-419">**Example**</span></span>

<span data-ttu-id="68d1b-420">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает два вызова `AddJsonFile`:</span><span class="sxs-lookup"><span data-stu-id="68d1b-420">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="68d1b-421">Первый вызов `AddJsonFile` загружает конфигурацию из *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="68d1b-421">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="68d1b-422">Второй вызов `AddJsonFile` загружает конфигурацию из *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="68d1b-422">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="68d1b-423">Для *appsettings.Development.json* в примере приложения загружается следующий файл:</span><span class="sxs-lookup"><span data-stu-id="68d1b-423">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="68d1b-424">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="68d1b-424">Run the sample app.</span></span> <span data-ttu-id="68d1b-425">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-425">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="68d1b-426">Выходные данные содержат пары "ключ-значение" для конфигурации в зависимости от среды приложения.</span><span class="sxs-lookup"><span data-stu-id="68d1b-426">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="68d1b-427">Уровень ведения журнала для ключа `Logging:LogLevel:Default` имеет значение `Debug` при запуске приложения в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="68d1b-427">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="68d1b-428">Запустите пример приложения еще раз в рабочей среде:</span><span class="sxs-lookup"><span data-stu-id="68d1b-428">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="68d1b-429">Откройте файл *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="68d1b-429">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="68d1b-430">В профиле `ConfigurationSample` измените значение переменной среды `ASPNETCORE_ENVIRONMENT` на `Production`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-430">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="68d1b-431">Сохраните файл и запустите приложение с помощью `dotnet run` в командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="68d1b-431">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="68d1b-432">Параметры в *appsettings.Development.json* больше не переопределяют параметры в *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="68d1b-432">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="68d1b-433">Уровень ведения журнала для ключа `Logging:LogLevel:Default` имеет значение `Information`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-433">The log level for the key `Logging:LogLevel:Default` is `Information`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="68d1b-434">Первый вызов `AddJsonFile` загружает конфигурацию из *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="68d1b-434">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="68d1b-435">Второй вызов `AddJsonFile` загружает конфигурацию из *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="68d1b-435">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="68d1b-436">Для *appsettings.Development.json* в примере приложения загружается следующий файл:</span><span class="sxs-lookup"><span data-stu-id="68d1b-436">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="68d1b-437">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="68d1b-437">Run the sample app.</span></span> <span data-ttu-id="68d1b-438">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-438">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="68d1b-439">Выходные данные содержат пары "ключ-значение" для конфигурации в зависимости от среды приложения.</span><span class="sxs-lookup"><span data-stu-id="68d1b-439">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="68d1b-440">Уровень ведения журнала для ключа `Logging:LogLevel:Default` имеет значение `Debug` при запуске приложения в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="68d1b-440">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="68d1b-441">Запустите пример приложения еще раз в рабочей среде:</span><span class="sxs-lookup"><span data-stu-id="68d1b-441">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="68d1b-442">Откройте файл *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="68d1b-442">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="68d1b-443">В профиле `ConfigurationSample` измените значение переменной среды `ASPNETCORE_ENVIRONMENT` на `Production`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-443">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="68d1b-444">Сохраните файл и запустите приложение с помощью `dotnet run` в командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="68d1b-444">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="68d1b-445">Параметры в *appsettings.Development.json* больше не переопределяют параметры в *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="68d1b-445">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="68d1b-446">Уровень ведения журнала для ключа `Logging:LogLevel:Default` имеет значение `Warning`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-446">The log level for the key `Logging:LogLevel:Default` is `Warning`.</span></span>

::: moniker-end

### <a name="xml-configuration-provider"></a><span data-ttu-id="68d1b-447">Поставщик конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="68d1b-447">XML Configuration Provider</span></span>

<span data-ttu-id="68d1b-448"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> загружает конфигурацию из пары "ключ — значение" XML-файла в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="68d1b-448">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="68d1b-449">Чтобы активировать конфигурацию XML-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="68d1b-449">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="68d1b-450">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="68d1b-450">Overloads permit specifying:</span></span>

* <span data-ttu-id="68d1b-451">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="68d1b-451">Whether the file is optional.</span></span>
* <span data-ttu-id="68d1b-452">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="68d1b-452">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="68d1b-453"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="68d1b-453">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="68d1b-454">Корневой узел файла конфигурации не учитывается при создании пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-454">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="68d1b-455">Не указывайте в файле Document Type Definition (определение типа документа, DTD) или пространство имен.</span><span class="sxs-lookup"><span data-stu-id="68d1b-455">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="68d1b-456">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-456">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="68d1b-457">XML-файлы конфигурации могут использовать имена отдельных элементов для повторяющихся разделов.</span><span class="sxs-lookup"><span data-stu-id="68d1b-457">XML configuration files can use distinct element names for repeating sections:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section0>
    <key0>value</key0>
    <key1>value</key1>
  </section0>
  <section1>
    <key0>value</key0>
    <key1>value</key1>
  </section1>
</configuration>
```

<span data-ttu-id="68d1b-458">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-458">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="68d1b-459">section0:key0</span><span class="sxs-lookup"><span data-stu-id="68d1b-459">section0:key0</span></span>
* <span data-ttu-id="68d1b-460">section0:key1</span><span class="sxs-lookup"><span data-stu-id="68d1b-460">section0:key1</span></span>
* <span data-ttu-id="68d1b-461">section1:key0</span><span class="sxs-lookup"><span data-stu-id="68d1b-461">section1:key0</span></span>
* <span data-ttu-id="68d1b-462">section1:key1</span><span class="sxs-lookup"><span data-stu-id="68d1b-462">section1:key1</span></span>

<span data-ttu-id="68d1b-463">Повторяющиеся элементы, использующие то же имя элемента, работают, если атрибут `name` используется для различения элементов.</span><span class="sxs-lookup"><span data-stu-id="68d1b-463">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section name="section0">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
  <section name="section1">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
</configuration>
```

<span data-ttu-id="68d1b-464">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-464">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="68d1b-465">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="68d1b-465">section:section0:key:key0</span></span>
* <span data-ttu-id="68d1b-466">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="68d1b-466">section:section0:key:key1</span></span>
* <span data-ttu-id="68d1b-467">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="68d1b-467">section:section1:key:key0</span></span>
* <span data-ttu-id="68d1b-468">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="68d1b-468">section:section1:key:key1</span></span>

<span data-ttu-id="68d1b-469">Атрибуты можно использовать для предоставления значений.</span><span class="sxs-lookup"><span data-stu-id="68d1b-469">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="68d1b-470">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-470">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="68d1b-471">key:attribute</span><span class="sxs-lookup"><span data-stu-id="68d1b-471">key:attribute</span></span>
* <span data-ttu-id="68d1b-472">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="68d1b-472">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="68d1b-473">Поставщик конфигурации ключа для каждого файла</span><span class="sxs-lookup"><span data-stu-id="68d1b-473">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="68d1b-474"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> использует файлы каталога как пары "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-474">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="68d1b-475">Ключ является именем файла.</span><span class="sxs-lookup"><span data-stu-id="68d1b-475">The key is the file name.</span></span> <span data-ttu-id="68d1b-476">Значение содержит содержимое файла.</span><span class="sxs-lookup"><span data-stu-id="68d1b-476">The value contains the file's contents.</span></span> <span data-ttu-id="68d1b-477">Поставщик конфигурации ключа для каждого файла используется в сценариях размещения Docker.</span><span class="sxs-lookup"><span data-stu-id="68d1b-477">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="68d1b-478">Чтобы активировать конфигурацию ключа для каждого файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="68d1b-478">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="68d1b-479">Значение параметра `directoryPath` должно быть абсолютным путем к файлам.</span><span class="sxs-lookup"><span data-stu-id="68d1b-479">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="68d1b-480">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="68d1b-480">Overloads permit specifying:</span></span>

* <span data-ttu-id="68d1b-481">`Action<KeyPerFileConfigurationSource>` — делегат, который настраивает источник.</span><span class="sxs-lookup"><span data-stu-id="68d1b-481">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="68d1b-482">Обязательно ли указывать каталог и путь к каталогу.</span><span class="sxs-lookup"><span data-stu-id="68d1b-482">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="68d1b-483">Двойное подчеркивание (`__`) используется в качестве разделителя ключа конфигурации в именах файлов.</span><span class="sxs-lookup"><span data-stu-id="68d1b-483">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="68d1b-484">Например, в имени файла `Logging__LogLevel__System` создается ключ конфигурации `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-484">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="68d1b-485">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-485">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="68d1b-486">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="68d1b-486">Memory Configuration Provider</span></span>

<span data-ttu-id="68d1b-487"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> использует коллекцию памяти в качестве пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-487">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="68d1b-488">Чтобы активировать конфигурацию коллекции в памяти, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="68d1b-488">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="68d1b-489">Поставщик конфигурации может инициализироваться значением `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-489">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="68d1b-490">Чтобы указать конфигурацию приложения, при сборке узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-490">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="68d1b-491">В следующем примере создается словарь конфигурации:</span><span class="sxs-lookup"><span data-stu-id="68d1b-491">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="68d1b-492">Словарь используется с вызовом `AddInMemoryCollection` для предоставления конфигурации:</span><span class="sxs-lookup"><span data-stu-id="68d1b-492">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="68d1b-493">GetValue</span><span class="sxs-lookup"><span data-stu-id="68d1b-493">GetValue</span></span>

<span data-ttu-id="68d1b-494">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) извлекает одно значение из конфигурации с указанным ключом и преобразует его в указанный тип, не являющийся коллекцией.</span><span class="sxs-lookup"><span data-stu-id="68d1b-494">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="68d1b-495">Перегрузка принимает значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="68d1b-495">An overload accepts a default value.</span></span>

<span data-ttu-id="68d1b-496">В следующем примере происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="68d1b-496">The following example:</span></span>

* <span data-ttu-id="68d1b-497">Из конфигурации извлекается строковое значение с ключом `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-497">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="68d1b-498">Если `NumberKey` отсутствует в ключах конфигурации, используется значение по умолчанию `99`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-498">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="68d1b-499">Значение получает тип `int`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-499">Types the value as an `int`.</span></span>
* <span data-ttu-id="68d1b-500">Значение сохраняется в свойстве `NumberConfig` для использования на странице.</span><span class="sxs-lookup"><span data-stu-id="68d1b-500">Stores the value in the `NumberConfig` property for use by the page.</span></span>

```csharp
public class IndexModel : PageModel
{
    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    public int NumberConfig { get; private set; }

    public void OnGet()
    {
        NumberConfig = _config.GetValue<int>("NumberKey", 99);
    }
}
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="68d1b-501">GetSection, GetChildren и Exists</span><span class="sxs-lookup"><span data-stu-id="68d1b-501">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="68d1b-502">В следующих примерах рассмотрим файл JSON.</span><span class="sxs-lookup"><span data-stu-id="68d1b-502">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="68d1b-503">Четыре ключа находятся в двух разделах, один из которых содержит пару из подразделов.</span><span class="sxs-lookup"><span data-stu-id="68d1b-503">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  },
  "section2": {
    "subsection0" : {
      "key0": "value",
      "key1": "value"
    },
    "subsection1" : {
      "key0": "value",
      "key1": "value"
    }
  }
}
```

<span data-ttu-id="68d1b-504">Когда файл считывается в конфигурацию, для сохранения значений конфигурации создаются следующие уникальные иерархические ключи.</span><span class="sxs-lookup"><span data-stu-id="68d1b-504">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="68d1b-505">section0:key0</span><span class="sxs-lookup"><span data-stu-id="68d1b-505">section0:key0</span></span>
* <span data-ttu-id="68d1b-506">section0:key1</span><span class="sxs-lookup"><span data-stu-id="68d1b-506">section0:key1</span></span>
* <span data-ttu-id="68d1b-507">section1:key0</span><span class="sxs-lookup"><span data-stu-id="68d1b-507">section1:key0</span></span>
* <span data-ttu-id="68d1b-508">section1:key1</span><span class="sxs-lookup"><span data-stu-id="68d1b-508">section1:key1</span></span>
* <span data-ttu-id="68d1b-509">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="68d1b-509">section2:subsection0:key0</span></span>
* <span data-ttu-id="68d1b-510">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="68d1b-510">section2:subsection0:key1</span></span>
* <span data-ttu-id="68d1b-511">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="68d1b-511">section2:subsection1:key0</span></span>
* <span data-ttu-id="68d1b-512">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="68d1b-512">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="68d1b-513">GetSection</span><span class="sxs-lookup"><span data-stu-id="68d1b-513">GetSection</span></span>

<span data-ttu-id="68d1b-514">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) извлекает подраздел конфигурации с указанным ключом подраздела.</span><span class="sxs-lookup"><span data-stu-id="68d1b-514">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="68d1b-515">Чтобы вернуть <xref:Microsoft.Extensions.Configuration.IConfigurationSection>, содержащий только пары "ключ — значение" в `section1`, вызовите `GetSection` и укажите имя раздела.</span><span class="sxs-lookup"><span data-stu-id="68d1b-515">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="68d1b-516">`configSection` не содержит значение, только ключ и путь.</span><span class="sxs-lookup"><span data-stu-id="68d1b-516">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="68d1b-517">Аналогично, чтобы получить значения для ключей в `section2:subsection0`, вызовите `GetSection` и укажите путь к разделу.</span><span class="sxs-lookup"><span data-stu-id="68d1b-517">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="68d1b-518">Значение `GetSection` никогда не возвращает значение `null`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-518">`GetSection` never returns `null`.</span></span> <span data-ttu-id="68d1b-519">Если соответствующий раздел не найден, возвращается пустой параметр `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-519">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="68d1b-520">Когда `GetSection` возвращает соответствующий раздел, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> не заполняется.</span><span class="sxs-lookup"><span data-stu-id="68d1b-520">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="68d1b-521"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> и <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> возвращаются, если раздел существует.</span><span class="sxs-lookup"><span data-stu-id="68d1b-521">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="68d1b-522">GetChildren</span><span class="sxs-lookup"><span data-stu-id="68d1b-522">GetChildren</span></span>

<span data-ttu-id="68d1b-523">Вызов [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) в `section2` получает значение `IEnumerable<IConfigurationSection>`, которое включает:</span><span class="sxs-lookup"><span data-stu-id="68d1b-523">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="68d1b-524">Exists</span><span class="sxs-lookup"><span data-stu-id="68d1b-524">Exists</span></span>

<span data-ttu-id="68d1b-525">Используйте [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*), чтобы определить, существует ли раздел конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-525">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="68d1b-526">Учитывая данные примера, `sectionExists` является `false`, потому что в данных конфигурации нет `section2:subsection2`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-526">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="68d1b-527">Привязка к классу</span><span class="sxs-lookup"><span data-stu-id="68d1b-527">Bind to a class</span></span>

<span data-ttu-id="68d1b-528">Конфигурация может быть привязана к классам, которые представляют группы связанных параметров, используя шаблон *вариантов*.</span><span class="sxs-lookup"><span data-stu-id="68d1b-528">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="68d1b-529">Для получения дополнительной информации см. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="68d1b-529">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="68d1b-530">Значения конфигурации возвращаются как строки, но вызов <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> позволяет построить объекты [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="68d1b-530">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="68d1b-531">Модуль привязки привязывает значения ко всем открытым свойствам чтения и записи предоставленного типа.</span><span class="sxs-lookup"><span data-stu-id="68d1b-531">The binder binds values to all of the public read/write properties of the type provided.</span></span> <span data-ttu-id="68d1b-532">Поля **не** привязаны.</span><span class="sxs-lookup"><span data-stu-id="68d1b-532">Fields are **not** bound.</span></span>

<span data-ttu-id="68d1b-533">Пример приложения содержит модель `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="68d1b-533">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="68d1b-534">Раздел `starship` файла *starship.json* создает конфигурацию, когда образец приложения использует поставщик конфигурации JSON для загрузки конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-534">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="68d1b-535">Создаются следующие пары "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-535">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="68d1b-536">Ключ</span><span class="sxs-lookup"><span data-stu-id="68d1b-536">Key</span></span>                   | <span data-ttu-id="68d1b-537">Значение</span><span class="sxs-lookup"><span data-stu-id="68d1b-537">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="68d1b-538">starship:name</span><span class="sxs-lookup"><span data-stu-id="68d1b-538">starship:name</span></span>         | <span data-ttu-id="68d1b-539">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="68d1b-539">USS Enterprise</span></span>                                    |
| <span data-ttu-id="68d1b-540">starship:registry</span><span class="sxs-lookup"><span data-stu-id="68d1b-540">starship:registry</span></span>     | <span data-ttu-id="68d1b-541">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="68d1b-541">NCC-1701</span></span>                                          |
| <span data-ttu-id="68d1b-542">starship:class</span><span class="sxs-lookup"><span data-stu-id="68d1b-542">starship:class</span></span>        | <span data-ttu-id="68d1b-543">Constitution</span><span class="sxs-lookup"><span data-stu-id="68d1b-543">Constitution</span></span>                                      |
| <span data-ttu-id="68d1b-544">starship:length</span><span class="sxs-lookup"><span data-stu-id="68d1b-544">starship:length</span></span>       | <span data-ttu-id="68d1b-545">304.8</span><span class="sxs-lookup"><span data-stu-id="68d1b-545">304.8</span></span>                                             |
| <span data-ttu-id="68d1b-546">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="68d1b-546">starship:commissioned</span></span> | <span data-ttu-id="68d1b-547">False</span><span class="sxs-lookup"><span data-stu-id="68d1b-547">False</span></span>                                             |
| <span data-ttu-id="68d1b-548">trademark</span><span class="sxs-lookup"><span data-stu-id="68d1b-548">trademark</span></span>             | <span data-ttu-id="68d1b-549">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="68d1b-549">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="68d1b-550">Пример приложения вызывает `GetSection` с помощью ключа `starship`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-550">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="68d1b-551">Пары "ключ — значение" `starship` изолированы.</span><span class="sxs-lookup"><span data-stu-id="68d1b-551">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="68d1b-552">Метод `Bind` вызывается в подразделе при прохождении в экземпляр класса `Starship`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-552">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="68d1b-553">После привязки значения экземпляра экземпляру присваивается свойство для преобразования для просмотра.</span><span class="sxs-lookup"><span data-stu-id="68d1b-553">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="68d1b-554">Привязка к графу объектов</span><span class="sxs-lookup"><span data-stu-id="68d1b-554">Bind to an object graph</span></span>

<span data-ttu-id="68d1b-555"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> способна связывать весь граф объекта POCO.</span><span class="sxs-lookup"><span data-stu-id="68d1b-555"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="68d1b-556">Как и при привязке простого объекта, привязываются только открытые свойства чтения и записи.</span><span class="sxs-lookup"><span data-stu-id="68d1b-556">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="68d1b-557">Образец содержит модель `TvShow`, в графе объектов которого находятся классы `Metadata` и `Actors` (*Модели/TvShow.cs*).</span><span class="sxs-lookup"><span data-stu-id="68d1b-557">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="68d1b-558">В примере приложения есть файл *tvshow.xml*, содержащий данные конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-558">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="68d1b-559">Конфигурация привязана ко всему ​​графу объектов `TvShow` с помощью метода `Bind`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-559">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="68d1b-560">Привязанный экземпляр присваивается свойству для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="68d1b-560">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="68d1b-561">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) привязывает и возвращает указанный тип.</span><span class="sxs-lookup"><span data-stu-id="68d1b-561">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="68d1b-562">Метод `Get<T>` может быть более удобным, чем использование `Bind`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-562">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="68d1b-563">В следующем коде показано, как использовать `Get<T>` с предыдущим примером, который позволяет привязать связанный экземпляр непосредственно к свойству, используемому для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="68d1b-563">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="68d1b-564">Привязка массива к классу</span><span class="sxs-lookup"><span data-stu-id="68d1b-564">Bind an array to a class</span></span>

<span data-ttu-id="68d1b-565">*Пример приложения демонстрирует концепции, описанные в этом разделе.*</span><span class="sxs-lookup"><span data-stu-id="68d1b-565">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="68d1b-566"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-566">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="68d1b-567">Любой формат массива, который предоставляет сегмент числового ключа (`:0:`, `:1:`, &hellip; `:{n}:`), способен привязать массив к массиву класса POCO.</span><span class="sxs-lookup"><span data-stu-id="68d1b-567">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="68d1b-568">Привязка предоставляется соглашением.</span><span class="sxs-lookup"><span data-stu-id="68d1b-568">Binding is provided by convention.</span></span> <span data-ttu-id="68d1b-569">Пользовательские поставщики конфигурации не обязаны реализовывать привязку массива.</span><span class="sxs-lookup"><span data-stu-id="68d1b-569">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="68d1b-570">**Обработка массива в оперативной памяти**</span><span class="sxs-lookup"><span data-stu-id="68d1b-570">**In-memory array processing**</span></span>

<span data-ttu-id="68d1b-571">Рассмотрите ключи и значения конфигурации, приведенные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="68d1b-571">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="68d1b-572">Ключ</span><span class="sxs-lookup"><span data-stu-id="68d1b-572">Key</span></span>             | <span data-ttu-id="68d1b-573">Значение</span><span class="sxs-lookup"><span data-stu-id="68d1b-573">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="68d1b-574">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="68d1b-574">array:entries:0</span></span> | <span data-ttu-id="68d1b-575">value0</span><span class="sxs-lookup"><span data-stu-id="68d1b-575">value0</span></span> |
| <span data-ttu-id="68d1b-576">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="68d1b-576">array:entries:1</span></span> | <span data-ttu-id="68d1b-577">value1</span><span class="sxs-lookup"><span data-stu-id="68d1b-577">value1</span></span> |
| <span data-ttu-id="68d1b-578">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="68d1b-578">array:entries:2</span></span> | <span data-ttu-id="68d1b-579">value2</span><span class="sxs-lookup"><span data-stu-id="68d1b-579">value2</span></span> |
| <span data-ttu-id="68d1b-580">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="68d1b-580">array:entries:4</span></span> | <span data-ttu-id="68d1b-581">value4</span><span class="sxs-lookup"><span data-stu-id="68d1b-581">value4</span></span> |
| <span data-ttu-id="68d1b-582">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="68d1b-582">array:entries:5</span></span> | <span data-ttu-id="68d1b-583">value5</span><span class="sxs-lookup"><span data-stu-id="68d1b-583">value5</span></span> |

<span data-ttu-id="68d1b-584">Эти ключи и значения загружаются в пример приложения с помощью поставщика конфигурации памяти.</span><span class="sxs-lookup"><span data-stu-id="68d1b-584">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

<span data-ttu-id="68d1b-585">Массив пропускает значения для индекса &num;3.</span><span class="sxs-lookup"><span data-stu-id="68d1b-585">The array skips a value for index &num;3.</span></span> <span data-ttu-id="68d1b-586">Связующее свойство конфигурации не способно связывать нулевые значения или создавать нулевые записи в связанных объектах, что становится ясным в тот момент, когда демонстрируется результат привязки этого массива к объекту.</span><span class="sxs-lookup"><span data-stu-id="68d1b-586">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="68d1b-587">В примере приложения класс POCO доступен для хранения привязанных данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-587">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="68d1b-588">Данные конфигурации, связанные с объектом.</span><span class="sxs-lookup"><span data-stu-id="68d1b-588">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="68d1b-589">Синтаксис [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) также может использоваться для получения более компактного кода.</span><span class="sxs-lookup"><span data-stu-id="68d1b-589">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="68d1b-590">Связанный объект, экземпляр `ArrayExample`, получает данные массива из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-590">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="68d1b-591">Индекс `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="68d1b-591">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="68d1b-592">Значение `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="68d1b-592">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="68d1b-593">0</span><span class="sxs-lookup"><span data-stu-id="68d1b-593">0</span></span>                            | <span data-ttu-id="68d1b-594">value0</span><span class="sxs-lookup"><span data-stu-id="68d1b-594">value0</span></span>                       |
| <span data-ttu-id="68d1b-595">1</span><span class="sxs-lookup"><span data-stu-id="68d1b-595">1</span></span>                            | <span data-ttu-id="68d1b-596">value1</span><span class="sxs-lookup"><span data-stu-id="68d1b-596">value1</span></span>                       |
| <span data-ttu-id="68d1b-597">2</span><span class="sxs-lookup"><span data-stu-id="68d1b-597">2</span></span>                            | <span data-ttu-id="68d1b-598">value2</span><span class="sxs-lookup"><span data-stu-id="68d1b-598">value2</span></span>                       |
| <span data-ttu-id="68d1b-599">3</span><span class="sxs-lookup"><span data-stu-id="68d1b-599">3</span></span>                            | <span data-ttu-id="68d1b-600">value4</span><span class="sxs-lookup"><span data-stu-id="68d1b-600">value4</span></span>                       |
| <span data-ttu-id="68d1b-601">4</span><span class="sxs-lookup"><span data-stu-id="68d1b-601">4</span></span>                            | <span data-ttu-id="68d1b-602">value5</span><span class="sxs-lookup"><span data-stu-id="68d1b-602">value5</span></span>                       |

<span data-ttu-id="68d1b-603">Индекс &num;3 в связанном объекте содержит данные конфигурации для конфигурационного ключа `array:4` и его значения `value4`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-603">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="68d1b-604">При привязке данных конфигурации, содержащих массив индексов, в ключах конфигурации эти индексы просто используются для выполнения итерации данных конфигурации при создании объекта.</span><span class="sxs-lookup"><span data-stu-id="68d1b-604">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="68d1b-605">Когда массив ключей конфигурации пропускает один или несколько индексов, в данных конфигурации не может быть сохранено нулевое значение и в связанном объекте не создается нулевая запись.</span><span class="sxs-lookup"><span data-stu-id="68d1b-605">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="68d1b-606">Отсутствующий элемент конфигурации для индекса &num;3 может быть предоставлен перед привязкой к экземпляру `ArrayExample` любым поставщиком конфигурации, который создает правильную пару "ключ — значение" в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-606">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="68d1b-607">Если в образец включен дополнительный поставщик конфигурации JSON с отсутствующей парой "ключ — значение", то `ArrayExample.Entries` подойдет полному массиву конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-607">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="68d1b-608">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="68d1b-608">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="68d1b-609">В `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="68d1b-609">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="68d1b-610">Пары "ключ — значение", показанные в таблице, загружаются в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-610">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="68d1b-611">Ключ</span><span class="sxs-lookup"><span data-stu-id="68d1b-611">Key</span></span>             | <span data-ttu-id="68d1b-612">Значение</span><span class="sxs-lookup"><span data-stu-id="68d1b-612">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="68d1b-613">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="68d1b-613">array:entries:3</span></span> | <span data-ttu-id="68d1b-614">value3</span><span class="sxs-lookup"><span data-stu-id="68d1b-614">value3</span></span> |

<span data-ttu-id="68d1b-615">Если экземпляр класса `ArrayExample` связан после того, как поставщик конфигурации JSON включает запись для индекса &num;3, то массив `ArrayExample.Entries` включит значение.</span><span class="sxs-lookup"><span data-stu-id="68d1b-615">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="68d1b-616">Индекс `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="68d1b-616">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="68d1b-617">Значение `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="68d1b-617">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="68d1b-618">0</span><span class="sxs-lookup"><span data-stu-id="68d1b-618">0</span></span>                            | <span data-ttu-id="68d1b-619">value0</span><span class="sxs-lookup"><span data-stu-id="68d1b-619">value0</span></span>                       |
| <span data-ttu-id="68d1b-620">1</span><span class="sxs-lookup"><span data-stu-id="68d1b-620">1</span></span>                            | <span data-ttu-id="68d1b-621">value1</span><span class="sxs-lookup"><span data-stu-id="68d1b-621">value1</span></span>                       |
| <span data-ttu-id="68d1b-622">2</span><span class="sxs-lookup"><span data-stu-id="68d1b-622">2</span></span>                            | <span data-ttu-id="68d1b-623">value2</span><span class="sxs-lookup"><span data-stu-id="68d1b-623">value2</span></span>                       |
| <span data-ttu-id="68d1b-624">3</span><span class="sxs-lookup"><span data-stu-id="68d1b-624">3</span></span>                            | <span data-ttu-id="68d1b-625">value3</span><span class="sxs-lookup"><span data-stu-id="68d1b-625">value3</span></span>                       |
| <span data-ttu-id="68d1b-626">4</span><span class="sxs-lookup"><span data-stu-id="68d1b-626">4</span></span>                            | <span data-ttu-id="68d1b-627">value4</span><span class="sxs-lookup"><span data-stu-id="68d1b-627">value4</span></span>                       |
| <span data-ttu-id="68d1b-628">5</span><span class="sxs-lookup"><span data-stu-id="68d1b-628">5</span></span>                            | <span data-ttu-id="68d1b-629">value5</span><span class="sxs-lookup"><span data-stu-id="68d1b-629">value5</span></span>                       |

<span data-ttu-id="68d1b-630">**Обработка массива JSON**</span><span class="sxs-lookup"><span data-stu-id="68d1b-630">**JSON array processing**</span></span>

<span data-ttu-id="68d1b-631">Если JSON-файл содержит массив, ключи конфигурации будут созданы для элементов массива с индексом раздела, начиная с нуля.</span><span class="sxs-lookup"><span data-stu-id="68d1b-631">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="68d1b-632">В следующем файле конфигурации `subsection` — это массив.</span><span class="sxs-lookup"><span data-stu-id="68d1b-632">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="68d1b-633">Поставщик конфигурации JSON считывает данные конфигурации в следующие пары "ключ — значение".</span><span class="sxs-lookup"><span data-stu-id="68d1b-633">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="68d1b-634">Ключ</span><span class="sxs-lookup"><span data-stu-id="68d1b-634">Key</span></span>                     | <span data-ttu-id="68d1b-635">Значение</span><span class="sxs-lookup"><span data-stu-id="68d1b-635">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="68d1b-636">json_array:key</span><span class="sxs-lookup"><span data-stu-id="68d1b-636">json_array:key</span></span>          | <span data-ttu-id="68d1b-637">valueA</span><span class="sxs-lookup"><span data-stu-id="68d1b-637">valueA</span></span> |
| <span data-ttu-id="68d1b-638">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="68d1b-638">json_array:subsection:0</span></span> | <span data-ttu-id="68d1b-639">valueB</span><span class="sxs-lookup"><span data-stu-id="68d1b-639">valueB</span></span> |
| <span data-ttu-id="68d1b-640">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="68d1b-640">json_array:subsection:1</span></span> | <span data-ttu-id="68d1b-641">valueC</span><span class="sxs-lookup"><span data-stu-id="68d1b-641">valueC</span></span> |
| <span data-ttu-id="68d1b-642">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="68d1b-642">json_array:subsection:2</span></span> | <span data-ttu-id="68d1b-643">valueD</span><span class="sxs-lookup"><span data-stu-id="68d1b-643">valueD</span></span> |

<span data-ttu-id="68d1b-644">В примере приложения для привязки пар "ключ — значение" конфигурации доступен следующий класс POCO.</span><span class="sxs-lookup"><span data-stu-id="68d1b-644">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="68d1b-645">После выполнения привязки `JsonArrayExample.Key` содержит значение `valueA`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-645">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="68d1b-646">Подраздел значения хранится в свойстве массива POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-646">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="68d1b-647">Индекс `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="68d1b-647">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="68d1b-648">Значение `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="68d1b-648">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="68d1b-649">0</span><span class="sxs-lookup"><span data-stu-id="68d1b-649">0</span></span>                                   | <span data-ttu-id="68d1b-650">valueB</span><span class="sxs-lookup"><span data-stu-id="68d1b-650">valueB</span></span>                              |
| <span data-ttu-id="68d1b-651">1</span><span class="sxs-lookup"><span data-stu-id="68d1b-651">1</span></span>                                   | <span data-ttu-id="68d1b-652">valueC</span><span class="sxs-lookup"><span data-stu-id="68d1b-652">valueC</span></span>                              |
| <span data-ttu-id="68d1b-653">2</span><span class="sxs-lookup"><span data-stu-id="68d1b-653">2</span></span>                                   | <span data-ttu-id="68d1b-654">valueD</span><span class="sxs-lookup"><span data-stu-id="68d1b-654">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="68d1b-655">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="68d1b-655">Custom configuration provider</span></span>

<span data-ttu-id="68d1b-656">Пример приложения демонстрирует, как создать базовый поставщик конфигурации, который считывает пары "ключ — значение" конфигурации из базы данных, используя [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="68d1b-656">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="68d1b-657">Поставщик имеет следующие характеристики.</span><span class="sxs-lookup"><span data-stu-id="68d1b-657">The provider has the following characteristics:</span></span>

* <span data-ttu-id="68d1b-658">База данных в памяти EF используется для демонстрационных целей.</span><span class="sxs-lookup"><span data-stu-id="68d1b-658">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="68d1b-659">Чтобы использовать базу данных, для которой требуется строка подключения, выполните вторичный `ConfigurationBuilder`, чтобы предоставить строку подключения от другого поставщика конфигурации.</span><span class="sxs-lookup"><span data-stu-id="68d1b-659">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="68d1b-660">Поставщик считывает таблицу базы данных в конфигурации при запуске.</span><span class="sxs-lookup"><span data-stu-id="68d1b-660">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="68d1b-661">Поставщик не запрашивает базу данных для каждого ключа.</span><span class="sxs-lookup"><span data-stu-id="68d1b-661">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="68d1b-662">Функция перезагрузки на изменение не реализована, поэтому обновление базы данных после запуска приложения не влияет на конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="68d1b-662">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="68d1b-663">Определите сущность `EFConfigurationValue` для хранения значений конфигурации в базе данных.</span><span class="sxs-lookup"><span data-stu-id="68d1b-663">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="68d1b-664">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="68d1b-664">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="68d1b-665">Добавьте `EFConfigurationContext` в хранилище и обратитесь к настроенным значениям.</span><span class="sxs-lookup"><span data-stu-id="68d1b-665">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="68d1b-666">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="68d1b-666">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="68d1b-667">Создайте класс, реализующий <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="68d1b-667">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="68d1b-668">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="68d1b-668">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="68d1b-669">Создайте пользовательский поставщик конфигурации путем наследования от <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="68d1b-669">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="68d1b-670">Поставщик конфигурации инициализирует пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="68d1b-670">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="68d1b-671">Поскольку [конфигурационные ключи не учитывают регистр](#keys), словарь, используемый для инициализации базы данных, создается с помощью функции сравнения без учета регистра ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span><span class="sxs-lookup"><span data-stu-id="68d1b-671">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="68d1b-672">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="68d1b-672">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="68d1b-673">Метод расширения `AddEFConfiguration` позволяет добавить источник конфигурации к `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-673">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="68d1b-674">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="68d1b-674">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="68d1b-675">В следующем коде показано, как использовать пользовательский `EFConfigurationProvider` в *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="68d1b-675">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="68d1b-676">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="68d1b-676">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="68d1b-677">Добавьте `EFConfigurationContext` в хранилище и обратитесь к настроенным значениям.</span><span class="sxs-lookup"><span data-stu-id="68d1b-677">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="68d1b-678">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="68d1b-678">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="68d1b-679">Создайте класс, реализующий <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="68d1b-679">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="68d1b-680">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="68d1b-680">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="68d1b-681">Создайте пользовательский поставщик конфигурации путем наследования от <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="68d1b-681">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="68d1b-682">Поставщик конфигурации инициализирует пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="68d1b-682">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="68d1b-683">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="68d1b-683">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="68d1b-684">Метод расширения `AddEFConfiguration` позволяет добавить источник конфигурации к `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-684">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="68d1b-685">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="68d1b-685">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="68d1b-686">В следующем коде показано, как использовать пользовательский `EFConfigurationProvider` в *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="68d1b-686">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="68d1b-687">Доступ к конфигурации во время запуска</span><span class="sxs-lookup"><span data-stu-id="68d1b-687">Access configuration during startup</span></span>

<span data-ttu-id="68d1b-688">Внесите значение `IConfiguration` в конструктор `Startup` для доступа к значениям конфигурации в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-688">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="68d1b-689">Чтобы получить доступ к конфигурации в `Startup.Configure`, либо добавьте значение `IConfiguration` непосредственно в метод, либо используйте экземпляр из конструктора.</span><span class="sxs-lookup"><span data-stu-id="68d1b-689">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

```csharp
public class Startup
{
    private readonly IConfiguration _config;

    public Startup(IConfiguration config)
    {
        _config = config;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        var value = _config["key"];
    }

    public void Configure(IApplicationBuilder app, IConfiguration config)
    {
        var value = config["key"];
    }
}
```

<span data-ttu-id="68d1b-690">Дополнительные сведения см. в руководстве по [доступу к конфигурации с использованием удобных методов](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="68d1b-690">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="68d1b-691">Настройте доступ на странице Razor Pages или в представлении MVC</span><span class="sxs-lookup"><span data-stu-id="68d1b-691">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="68d1b-692">Для доступа к параметрам конфигурации на странице Razor Pages или в представлении MVC добавьте [использование директивы](xref:mvc/views/razor#using) ([Справочник по C#: использование директивы](/dotnet/csharp/language-reference/keywords/using-directive)) для [пространства имен Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) и вставьте <xref:Microsoft.Extensions.Configuration.IConfiguration> на страницу или в представление.</span><span class="sxs-lookup"><span data-stu-id="68d1b-692">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="68d1b-693">На странице Razor Pages</span><span class="sxs-lookup"><span data-stu-id="68d1b-693">In a Razor Pages page:</span></span>

```cshtml
@page
@model IndexModel
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="68d1b-694">В представлении MVC</span><span class="sxs-lookup"><span data-stu-id="68d1b-694">In an MVC view:</span></span>

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="68d1b-695">Добавление конфигурации из внешней сборки</span><span class="sxs-lookup"><span data-stu-id="68d1b-695">Add configuration from an external assembly</span></span>

<span data-ttu-id="68d1b-696">Реализация <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> позволяет при запуске добавлять в приложение улучшения из внешней сборки вне приложения класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="68d1b-696">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="68d1b-697">Для получения дополнительной информации см. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="68d1b-697">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="68d1b-698">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="68d1b-698">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
