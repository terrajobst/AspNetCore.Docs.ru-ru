---
title: Конфигурация в .NET Core
author: guardrex
description: Узнайте, как использовать API конфигурации для настройки приложения ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: 141ae5cda7672159032013cbda1ef4bfa7c142dd
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726981"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="5413c-103">Конфигурация в .NET Core</span><span class="sxs-lookup"><span data-stu-id="5413c-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="5413c-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="5413c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5413c-105">Конфигурация приложения в ASP.NET Core основана на парах "ключ — значение", установленных *поставщиками конфигурации*.</span><span class="sxs-lookup"><span data-stu-id="5413c-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="5413c-106">Поставщики конфигурации получают данные конфигурации в парах "ключ — значение" из различных источников:</span><span class="sxs-lookup"><span data-stu-id="5413c-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="5413c-107">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="5413c-107">Azure Key Vault</span></span>
* <span data-ttu-id="5413c-108">конфигурация приложения Azure;</span><span class="sxs-lookup"><span data-stu-id="5413c-108">Azure App Configuration</span></span>
* <span data-ttu-id="5413c-109">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="5413c-109">Command-line arguments</span></span>
* <span data-ttu-id="5413c-110">а также пользовательские поставщики (устанавливаемые или создаваемые).</span><span class="sxs-lookup"><span data-stu-id="5413c-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="5413c-111">справочных файлов;</span><span class="sxs-lookup"><span data-stu-id="5413c-111">Directory files</span></span>
* <span data-ttu-id="5413c-112">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="5413c-112">Environment variables</span></span>
* <span data-ttu-id="5413c-113">объектов .NET в памяти;</span><span class="sxs-lookup"><span data-stu-id="5413c-113">In-memory .NET objects</span></span>
* <span data-ttu-id="5413c-114">файлов параметров;</span><span class="sxs-lookup"><span data-stu-id="5413c-114">Settings files</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5413c-115">Пакеты конфигурации для распространенных вариантов провайдеров конфигурации ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) включаются платформой неявным образом.</span><span class="sxs-lookup"><span data-stu-id="5413c-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5413c-116">Пакеты конфигурации для распространенных вариантов провайдеров конфигурации ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) включаются в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="5413c-116">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="5413c-117">В приведенных ниже и представленных в образце приложения примерах кода используется пространство имен <xref:Microsoft.Extensions.Configuration>:</span><span class="sxs-lookup"><span data-stu-id="5413c-117">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="5413c-118">*Шаблон параметров* является расширением конфигурации основных понятий, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="5413c-118">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="5413c-119">Параметры используют классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="5413c-119">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="5413c-120">Для получения дополнительной информации см. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="5413c-120">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="5413c-121">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5413c-121">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="5413c-122">Конфигурация узла и приложения</span><span class="sxs-lookup"><span data-stu-id="5413c-122">Host versus app configuration</span></span>

<span data-ttu-id="5413c-123">Перед настройкой и запуском приложения настройте и запустите *узел*.</span><span class="sxs-lookup"><span data-stu-id="5413c-123">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="5413c-124">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="5413c-124">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="5413c-125">Как приложение, так и узел настраиваются с использованием поставщиков конфигурации, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="5413c-125">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="5413c-126">Пары "ключ — значение" конфигурации узлов также включаются в конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="5413c-126">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="5413c-127">Дополнительные сведения о том, как используются поставщики конфигурации при создании узла и как влияют источники конфигурации на узел, см. в разделе <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="5413c-127">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="5413c-128">Конфигурация по умолчанию</span><span class="sxs-lookup"><span data-stu-id="5413c-128">Default configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5413c-129">Веб-приложения на основе шаблонов [dotnet new](/dotnet/core/tools/dotnet-new) ASP.NET Core вызывают <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> при создании узла.</span><span class="sxs-lookup"><span data-stu-id="5413c-129">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="5413c-130">`CreateDefaultBuilder` предоставляет конфигурацию по умолчанию для приложения в следующем порядке:</span><span class="sxs-lookup"><span data-stu-id="5413c-130">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="5413c-131">Приведенные ниже сведения относятся к приложениям, использующим [универсальный узел](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="5413c-131">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="5413c-132">Подробные сведения о конфигурации по умолчанию при использовании [веб-узла](xref:fundamentals/host/web-host) см. в [разделе о версии ASP.NET Core 2.2 в этой статье](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="5413c-132">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="5413c-133">Существуют следующие способы предоставления конфигурации узла.</span><span class="sxs-lookup"><span data-stu-id="5413c-133">Host configuration is provided from:</span></span>
  * <span data-ttu-id="5413c-134">Переменные среды с префиксом `DOTNET_` (например, `DOTNET_ENVIRONMENT`), использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5413c-134">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="5413c-135">Префикс (`DOTNET_`) исключается при загрузке пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-135">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="5413c-136">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5413c-136">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="5413c-137">Устанавливается конфигурация веб-узла по умолчанию (`ConfigureWebHostDefaults`):</span><span class="sxs-lookup"><span data-stu-id="5413c-137">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="5413c-138">Kestrel используется в качестве веб-сервера и настраивается с помощью поставщиков конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="5413c-138">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="5413c-139">Добавьте ПО промежуточного слоя фильтрации узлов.</span><span class="sxs-lookup"><span data-stu-id="5413c-139">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="5413c-140">Если переменной среды `ASPNETCORE_FORWARDEDHEADERS_ENABLED` присвоено значение `true`, добавьте ПО промежуточного слоя перенаправления заголовков.</span><span class="sxs-lookup"><span data-stu-id="5413c-140">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="5413c-141">Включите интеграцию служб IIS.</span><span class="sxs-lookup"><span data-stu-id="5413c-141">Enable IIS integration.</span></span>
* <span data-ttu-id="5413c-142">Конфигурация приложения предоставляется из следующих ресурсов:</span><span class="sxs-lookup"><span data-stu-id="5413c-142">App configuration is provided from:</span></span>
  * <span data-ttu-id="5413c-143">Файл *appsettings.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5413c-143">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="5413c-144">Файл *appsettings.{Environment}.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5413c-144">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="5413c-145">[Менеджера секретов](xref:security/app-secrets), когда приложение выполняется в среде `Development` с использованием начальных сборок.</span><span class="sxs-lookup"><span data-stu-id="5413c-145">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="5413c-146">Переменные среды, использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5413c-146">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="5413c-147">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5413c-147">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5413c-148">Веб-приложения на основе шаблонов [dotnet new](/dotnet/core/tools/dotnet-new) ASP.NET Core вызывают <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> при создании узла.</span><span class="sxs-lookup"><span data-stu-id="5413c-148">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="5413c-149">`CreateDefaultBuilder` предоставляет конфигурацию по умолчанию для приложения в следующем порядке:</span><span class="sxs-lookup"><span data-stu-id="5413c-149">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="5413c-150">Приведенные ниже сведения относятся к приложениям, использующим [веб-узел](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="5413c-150">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="5413c-151">Подробные сведения о конфигурации по умолчанию при использовании [универсального узла](xref:fundamentals/host/generic-host) см. в [последней версии этой статьи](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="5413c-151">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="5413c-152">Существуют следующие способы предоставления конфигурации узла.</span><span class="sxs-lookup"><span data-stu-id="5413c-152">Host configuration is provided from:</span></span>
  * <span data-ttu-id="5413c-153">Переменные среды с префиксом `ASPNETCORE_` (например, `ASPNETCORE_ENVIRONMENT`), использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5413c-153">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="5413c-154">Префикс (`ASPNETCORE_`) исключается при загрузке пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-154">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="5413c-155">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5413c-155">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="5413c-156">Конфигурация приложения предоставляется из следующих ресурсов:</span><span class="sxs-lookup"><span data-stu-id="5413c-156">App configuration is provided from:</span></span>
  * <span data-ttu-id="5413c-157">Файл *appsettings.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5413c-157">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="5413c-158">Файл *appsettings.{Environment}.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5413c-158">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="5413c-159">[Менеджера секретов](xref:security/app-secrets), когда приложение выполняется в среде `Development` с использованием начальных сборок.</span><span class="sxs-lookup"><span data-stu-id="5413c-159">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="5413c-160">Переменные среды, использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5413c-160">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="5413c-161">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5413c-161">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

## <a name="security"></a><span data-ttu-id="5413c-162">Безопасность</span><span class="sxs-lookup"><span data-stu-id="5413c-162">Security</span></span>

<span data-ttu-id="5413c-163">Применяйте описанные ниже методики для защиты конфиденциальных данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-163">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="5413c-164">Никогда не храните пароли или другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации обычного текста.</span><span class="sxs-lookup"><span data-stu-id="5413c-164">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="5413c-165">Не используйте секреты рабочей среды в средах разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="5413c-165">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="5413c-166">Указывайте секреты вне проекта, чтобы их нельзя было случайно зафиксировать в репозитории с исходным кодом.</span><span class="sxs-lookup"><span data-stu-id="5413c-166">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="5413c-167">Дополнительные сведения см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="5413c-167">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="5413c-168"><xref:security/app-secrets> &ndash; содержит рекомендации по использованию переменных среды для хранения конфиденциальных данных.</span><span class="sxs-lookup"><span data-stu-id="5413c-168"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="5413c-169">Менеджер секретов использует поставщик конфигурации файла для хранения конфиденциальных данных пользователя в файле JSON в локальной системе.</span><span class="sxs-lookup"><span data-stu-id="5413c-169">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="5413c-170">Поставщик конфигурации файлов описан ниже в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="5413c-170">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="5413c-171">В [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) безопасно хранятся секреты приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5413c-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="5413c-172">Для получения дополнительной информации см. <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="5413c-172">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="5413c-173">Иерархическая модель конфигурации</span><span class="sxs-lookup"><span data-stu-id="5413c-173">Hierarchical configuration data</span></span>

<span data-ttu-id="5413c-174">API конфигурации способен поддерживать иерархические данные конфигурации, выполняя преобразование в плоскую структуру иерархических данных с использованием разделителя в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-174">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="5413c-175">В следующем файле JSON существуют четыре ключа в структурированной иерархии двух разделов.</span><span class="sxs-lookup"><span data-stu-id="5413c-175">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="5413c-176">При считывании файла в конфигурацию для сохранения исходной иерархической структуры данных источника конфигурации создаются уникальные ключи.</span><span class="sxs-lookup"><span data-stu-id="5413c-176">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="5413c-177">Разделы и ключи преобразовываются в плоскую структуру с использованием двоеточия (`:`) для сохранения исходной структуры.</span><span class="sxs-lookup"><span data-stu-id="5413c-177">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="5413c-178">section0:key0</span><span class="sxs-lookup"><span data-stu-id="5413c-178">section0:key0</span></span>
* <span data-ttu-id="5413c-179">section0:key1</span><span class="sxs-lookup"><span data-stu-id="5413c-179">section0:key1</span></span>
* <span data-ttu-id="5413c-180">section1:key0</span><span class="sxs-lookup"><span data-stu-id="5413c-180">section1:key0</span></span>
* <span data-ttu-id="5413c-181">section1:key1</span><span class="sxs-lookup"><span data-stu-id="5413c-181">section1:key1</span></span>

<span data-ttu-id="5413c-182">Методы <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> и <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> доступны для изолирования разделов и дочерних элементов раздела в данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-182"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="5413c-183">Эти методы описаны далее в разделе [GetSection, GetChildren и Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="5413c-183">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="5413c-184">Соглашения</span><span class="sxs-lookup"><span data-stu-id="5413c-184">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="5413c-185">Источники и поставщики</span><span class="sxs-lookup"><span data-stu-id="5413c-185">Sources and providers</span></span>

<span data-ttu-id="5413c-186">При запуске приложения источники конфигурации считываются в порядке, в котором были указаны их поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-186">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="5413c-187">Поставщики конфигурации, которые реализуют обнаружение изменений, имеют возможность перезагрузить конфигурацию при изменении базовых параметров.</span><span class="sxs-lookup"><span data-stu-id="5413c-187">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="5413c-188">Так, поставщик файла конфигурации (подробнее о нем далее в этой статье) и [поставщик конфигурации Azure Key Vault](xref:security/key-vault-configuration) реализуют обнаружение изменений.</span><span class="sxs-lookup"><span data-stu-id="5413c-188">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="5413c-189">Объект <xref:Microsoft.Extensions.Configuration.IConfiguration> доступен в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) приложения.</span><span class="sxs-lookup"><span data-stu-id="5413c-189"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="5413c-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> можно внедрить в <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> Razor Pages или <xref:Microsoft.AspNetCore.Mvc.Controller> MVC, чтобы получить конфигурацию для класса.</span><span class="sxs-lookup"><span data-stu-id="5413c-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="5413c-191">В следующих примерах поле `_config` используется для доступа к значениям конфигурации:</span><span class="sxs-lookup"><span data-stu-id="5413c-191">In the following examples, the `_config` field is used to access configuration values:</span></span>

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

<span data-ttu-id="5413c-192">Поставщики конфигурации не могут использовать контейнер DI, так как он недоступен при настройке узла.</span><span class="sxs-lookup"><span data-stu-id="5413c-192">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="5413c-193">Клавиши</span><span class="sxs-lookup"><span data-stu-id="5413c-193">Keys</span></span>

<span data-ttu-id="5413c-194">В ключах конфигурации приняты следующие соглашения.</span><span class="sxs-lookup"><span data-stu-id="5413c-194">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="5413c-195">В ключах не учитывается регистр символов.</span><span class="sxs-lookup"><span data-stu-id="5413c-195">Keys are case-insensitive.</span></span> <span data-ttu-id="5413c-196">Например `ConnectionString` и `connectionstring` обрабатываются как эквивалентные ключи.</span><span class="sxs-lookup"><span data-stu-id="5413c-196">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="5413c-197">Если значение для одного и того же ключа установлено одним и тем же или разными поставщиками конфигурации, последним значением, установленным на ключе, является используемое значение.</span><span class="sxs-lookup"><span data-stu-id="5413c-197">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="5413c-198">Иерархические ключи</span><span class="sxs-lookup"><span data-stu-id="5413c-198">Hierarchical keys</span></span>
  * <span data-ttu-id="5413c-199">При взаимодействии с API конфигурации разделитель-двоеточие (`:`) поддерживается на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="5413c-199">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="5413c-200">В переменных среды разделитель-двоеточие может не работать на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="5413c-200">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="5413c-201">Двойной знак подчеркивания (`__`) поддерживается на всех платформах и автоматически преобразовывается в двоеточие.</span><span class="sxs-lookup"><span data-stu-id="5413c-201">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="5413c-202">В хранилище ключей Azure иерархические ключи используют `--` (два дефиса) в качестве разделителя.</span><span class="sxs-lookup"><span data-stu-id="5413c-202">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="5413c-203">Чтобы заменить дефисы двоеточием, при загрузке секретов в конфигурацию приложения напишите код.</span><span class="sxs-lookup"><span data-stu-id="5413c-203">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="5413c-204"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-204">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="5413c-205">Привязка массива описана в разделе [Привязка массива к классу](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="5413c-205">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="5413c-206">Значения</span><span class="sxs-lookup"><span data-stu-id="5413c-206">Values</span></span>

<span data-ttu-id="5413c-207">В значениях конфигурации учитываются следующие соглашения.</span><span class="sxs-lookup"><span data-stu-id="5413c-207">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="5413c-208">Значения являются строками.</span><span class="sxs-lookup"><span data-stu-id="5413c-208">Values are strings.</span></span>
* <span data-ttu-id="5413c-209">Значение NULL не может храниться в конфигурации или быть привязанным к объектам.</span><span class="sxs-lookup"><span data-stu-id="5413c-209">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="5413c-210">Поставщики</span><span class="sxs-lookup"><span data-stu-id="5413c-210">Providers</span></span>

<span data-ttu-id="5413c-211">В следующей таблице показаны поставщики конфигурации, доступные для приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5413c-211">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="5413c-212">Поставщик</span><span class="sxs-lookup"><span data-stu-id="5413c-212">Provider</span></span> | <span data-ttu-id="5413c-213">Предоставляет конфигурацию из &hellip;</span><span class="sxs-lookup"><span data-stu-id="5413c-213">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="5413c-214">[Поставщик конфигурации хранилища ключей Azure](xref:security/key-vault-configuration) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="5413c-214">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="5413c-215">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="5413c-215">Azure Key Vault</span></span> |
| <span data-ttu-id="5413c-216">[Поставщик конфигурации приложений Azure](/azure/azure-app-configuration/quickstart-aspnet-core-app) (документация Azure)</span><span class="sxs-lookup"><span data-stu-id="5413c-216">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="5413c-217">конфигурация приложения Azure;</span><span class="sxs-lookup"><span data-stu-id="5413c-217">Azure App Configuration</span></span> |
| [<span data-ttu-id="5413c-218">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="5413c-218">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="5413c-219">Параметры командной строки</span><span class="sxs-lookup"><span data-stu-id="5413c-219">Command-line parameters</span></span> |
| [<span data-ttu-id="5413c-220">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="5413c-220">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="5413c-221">Источник пользователя</span><span class="sxs-lookup"><span data-stu-id="5413c-221">Custom source</span></span> |
| [<span data-ttu-id="5413c-222">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="5413c-222">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="5413c-223">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="5413c-223">Environment variables</span></span> |
| [<span data-ttu-id="5413c-224">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="5413c-224">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="5413c-225">Файлы (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="5413c-225">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="5413c-226">Поставщик конфигурации ключа для каждого файла</span><span class="sxs-lookup"><span data-stu-id="5413c-226">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="5413c-227">Справочные файлы</span><span class="sxs-lookup"><span data-stu-id="5413c-227">Directory files</span></span> |
| [<span data-ttu-id="5413c-228">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="5413c-228">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="5413c-229">Коллекции оперативной памяти</span><span class="sxs-lookup"><span data-stu-id="5413c-229">In-memory collections</span></span> |
| <span data-ttu-id="5413c-230">[Секреты пользователей (Менеджер секретов)](xref:security/app-secrets) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="5413c-230">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="5413c-231">Файл в каталоге профиля пользователя</span><span class="sxs-lookup"><span data-stu-id="5413c-231">File in the user profile directory</span></span> |

<span data-ttu-id="5413c-232">Источники конфигурации считываются в том порядке, в котором при запуске указываются их поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-232">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="5413c-233">Поставщики конфигурации в этом разделе описаны в алфавитном порядке, а не в необходимом вам порядке в коде.</span><span class="sxs-lookup"><span data-stu-id="5413c-233">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="5413c-234">Порядок поставщиков конфигурации в коде соответствует приоритетам ваших основных источников конфигурации, требуемых приложением.</span><span class="sxs-lookup"><span data-stu-id="5413c-234">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="5413c-235">Типичная последовательность поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-235">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="5413c-236">Файлы (*appsettings.json*, *appsettings.{Environment}.json*, где `{Environment}` — это текущая среда размещения приложения)</span><span class="sxs-lookup"><span data-stu-id="5413c-236">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. <span data-ttu-id="5413c-237">[Azure Key Vault](xref:security/key-vault-configuration);</span><span class="sxs-lookup"><span data-stu-id="5413c-237">[Azure Key Vault](xref:security/key-vault-configuration)</span></span>
1. <span data-ttu-id="5413c-238">[Секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки)</span><span class="sxs-lookup"><span data-stu-id="5413c-238">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="5413c-239">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="5413c-239">Environment variables</span></span>
1. <span data-ttu-id="5413c-240">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="5413c-240">Command-line arguments</span></span>

<span data-ttu-id="5413c-241">Общепринятой практикой является размещение поставщика конфигурации командной строки последним в ряду поставщиков, чтобы аргументы командной строки могли переопределять конфигурацию, установленную другими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="5413c-241">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="5413c-242">Предыдущая последовательность поставщиков используется при инициализации нового построителя узла с помощью `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5413c-242">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="5413c-243">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="5413c-243">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="5413c-244">Настройка построителя узла с помощью ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="5413c-244">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="5413c-245">Чтобы настроить построитель узла, вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> и укажите конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="5413c-245">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="5413c-246">`ConfigureHostConfiguration` используется для инициализации <xref:Microsoft.Extensions.Hosting.IHostEnvironment> для дальнейшего использования в процессе сборки.</span><span class="sxs-lookup"><span data-stu-id="5413c-246">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="5413c-247">Метод `ConfigureHostConfiguration` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="5413c-247">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

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

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="5413c-248">Настройка построителя узла с помощью UseConfiguration</span><span class="sxs-lookup"><span data-stu-id="5413c-248">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="5413c-249">Чтобы настроить построитель узла, вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> в построителе узла с конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="5413c-249">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

## <a name="configureappconfiguration"></a><span data-ttu-id="5413c-250">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="5413c-250">ConfigureAppConfiguration</span></span>

<span data-ttu-id="5413c-251">Вызовите `ConfigureAppConfiguration` при сборке узла, чтобы указать поставщики конфигурации приложения в дополнение к тем, которые автоматически добавлены `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5413c-251">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="5413c-252">Переопределение предыдущей конфигурации с помощью аргументов командной строки</span><span class="sxs-lookup"><span data-stu-id="5413c-252">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="5413c-253">Чтобы предоставить конфигурацию приложения, которую можно переопределить с помощью аргументов командной строки, вызовите метод `AddCommandLine` последним:</span><span class="sxs-lookup"><span data-stu-id="5413c-253">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="5413c-254">Удаление поставщиков, добавленных CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="5413c-254">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="5413c-255">Чтобы удалить поставщиков, добавленных `CreateDefaultBuilder`, сначала вызовите [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) в [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources):</span><span class="sxs-lookup"><span data-stu-id="5413c-255">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="5413c-256">Использование конфигурации во время запуска приложения</span><span class="sxs-lookup"><span data-stu-id="5413c-256">Consume configuration during app startup</span></span>

<span data-ttu-id="5413c-257">Конфигурация, предоставленная приложению в `ConfigureAppConfiguration`, доступна во время запуска приложения, включая `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5413c-257">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5413c-258">Дополнительные сведения см. в разделе [Доступ к конфигурации во время запуска](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="5413c-258">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="5413c-259">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="5413c-259">Command-line Configuration Provider</span></span>

<span data-ttu-id="5413c-260"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> загружает конфигурацию из пары "ключ — значение" аргумента командной строки в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="5413c-260">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="5413c-261">Чтобы активировать конфигурацию командной строки, вызывается метод расширения <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> для экземпляра <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5413c-261">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="5413c-262">`AddCommandLine` вызывается автоматически при вызове `CreateDefaultBuilder(string [])`.</span><span class="sxs-lookup"><span data-stu-id="5413c-262">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="5413c-263">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="5413c-263">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="5413c-264">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="5413c-264">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="5413c-265">дополнительную конфигурацию из файлов *appsettings.json* и *appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="5413c-265">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="5413c-266">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="5413c-266">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="5413c-267">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="5413c-267">Environment variables.</span></span>

<span data-ttu-id="5413c-268">`CreateDefaultBuilder` добавляет последним поставщика конфигурации командной строки.</span><span class="sxs-lookup"><span data-stu-id="5413c-268">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="5413c-269">Аргументы командной строки передаются в набор конфигурации переопределения среды выполнения, установленной другими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="5413c-269">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="5413c-270">`CreateDefaultBuilder` действует, когда создается узел.</span><span class="sxs-lookup"><span data-stu-id="5413c-270">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="5413c-271">Поэтому конфигурация командной строки, активированная с помощью `CreateDefaultBuilder`, может повлиять на настройку узла.</span><span class="sxs-lookup"><span data-stu-id="5413c-271">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="5413c-272">Для приложений на основе шаблонов ASP.NET Core метод `AddCommandLine` уже был вызван методом `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5413c-272">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="5413c-273">Чтобы добавить дополнительные поставщики конфигурации и обеспечить возможность переопределения конфигурации от этих поставщиков с помощью аргументов командной строки, вызовите дополнительные поставщики приложения в `ConfigureAppConfiguration` и вызовите `AddCommandLine` в последнюю очередь.</span><span class="sxs-lookup"><span data-stu-id="5413c-273">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="5413c-274">**Пример**</span><span class="sxs-lookup"><span data-stu-id="5413c-274">**Example**</span></span>

<span data-ttu-id="5413c-275">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="5413c-275">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="5413c-276">Откройте командную строку в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="5413c-276">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="5413c-277">Поставьте аргумент командной строки в команду `dotnet run`: `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="5413c-277">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="5413c-278">После запуска приложения откройте браузер в приложении по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="5413c-278">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="5413c-279">Обратите внимание, что вывод содержит пару "ключ — значение" для аргумента командной строки конфигурации, предоставленного для `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="5413c-279">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="5413c-280">Аргументы</span><span class="sxs-lookup"><span data-stu-id="5413c-280">Arguments</span></span>

<span data-ttu-id="5413c-281">Значение должно соответствовать знаку равенства (`=`), или ключ должен иметь префикс (`--` или `/`), когда значение следует за пробелом.</span><span class="sxs-lookup"><span data-stu-id="5413c-281">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="5413c-282">Значение не требуется, если используется знак равенства (например, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="5413c-282">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="5413c-283">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="5413c-283">Key prefix</span></span>               | <span data-ttu-id="5413c-284">Пример</span><span class="sxs-lookup"><span data-stu-id="5413c-284">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="5413c-285">Без префикса</span><span class="sxs-lookup"><span data-stu-id="5413c-285">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="5413c-286">Два дефиса (`--`)</span><span class="sxs-lookup"><span data-stu-id="5413c-286">Two dashes (`--`)</span></span>        | <span data-ttu-id="5413c-287">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="5413c-287">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="5413c-288">Прямая косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="5413c-288">Forward slash (`/`)</span></span>      | <span data-ttu-id="5413c-289">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="5413c-289">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="5413c-290">В рамках одной и той же команды не смешивайте пары "ключ — значение" аргумента командной строки, которые используют знак равенства, с парами "ключ — значение", которые используют пробел.</span><span class="sxs-lookup"><span data-stu-id="5413c-290">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="5413c-291">Примеры команд.</span><span class="sxs-lookup"><span data-stu-id="5413c-291">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="5413c-292">Сопоставления переключений</span><span class="sxs-lookup"><span data-stu-id="5413c-292">Switch mappings</span></span>

<span data-ttu-id="5413c-293">Сопоставление параметров позволяет указать логику замены имен ключей.</span><span class="sxs-lookup"><span data-stu-id="5413c-293">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="5413c-294">Когда вручную создается конфигурация с помощью <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, методу <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> можно предоставить словарь сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="5413c-294">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="5413c-295">В словаре сопоставлений переключений выполняется поиск ключа, который совпадает с ключом, предоставляемым аргументом командной строки.</span><span class="sxs-lookup"><span data-stu-id="5413c-295">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="5413c-296">Если ключ в командной строке находится в словаре, значение словаря (замена ключа) передается обратно, чтобы установить пару "ключ — значение" в конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="5413c-296">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="5413c-297">Сопоставление переключений необходимо для любого ключа командной строки с префиксом из одного дефиса (`-`).</span><span class="sxs-lookup"><span data-stu-id="5413c-297">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="5413c-298">Правила ключей из словаря сопоставления переключений:</span><span class="sxs-lookup"><span data-stu-id="5413c-298">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="5413c-299">Переключения должны начинаться с дефиса (`-`) или двойного дефиса (`--`).</span><span class="sxs-lookup"><span data-stu-id="5413c-299">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="5413c-300">Словарь сопоставлений переключений не должен содержать повторяющиеся ключи.</span><span class="sxs-lookup"><span data-stu-id="5413c-300">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="5413c-301">Создайте словарь сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="5413c-301">Create a switch mappings dictionary.</span></span> <span data-ttu-id="5413c-302">В следующем примере создаются два сопоставления переключений:</span><span class="sxs-lookup"><span data-stu-id="5413c-302">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="5413c-303">При сборке узла вызовите `AddCommandLine` со словарем сопоставлений переключений:</span><span class="sxs-lookup"><span data-stu-id="5413c-303">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="5413c-304">Для приложений, использующих сопоставления переключений, в вызове `CreateDefaultBuilder` аргументы передаваться не должны.</span><span class="sxs-lookup"><span data-stu-id="5413c-304">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="5413c-305">Вызов команды `AddCommandLine` метода `CreateDefaultBuilder` не включает сопоставленные переключения, и нет возможности передать словарь сопоставления переключений в `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5413c-305">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="5413c-306">Чтобы решить эту проблему, нужно не передавать аргументы команде `CreateDefaultBuilder`, а позволить методу `AddCommandLine` метода `ConfigurationBuilder` обрабатывать как аргументы, так и словарь сопоставления параметров.</span><span class="sxs-lookup"><span data-stu-id="5413c-306">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="5413c-307">Созданный словарь сопоставлений переключений содержит данные, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="5413c-307">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="5413c-308">Ключ</span><span class="sxs-lookup"><span data-stu-id="5413c-308">Key</span></span>       | <span data-ttu-id="5413c-309">Значение</span><span class="sxs-lookup"><span data-stu-id="5413c-309">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="5413c-310">Если ключи сопоставления переключений используются при запуске приложения, конфигурация принимает значение конфигурации в ключе, предоставленном в словаре.</span><span class="sxs-lookup"><span data-stu-id="5413c-310">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="5413c-311">После выполнения предыдущей команды конфигурация содержит значения, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="5413c-311">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="5413c-312">Ключ</span><span class="sxs-lookup"><span data-stu-id="5413c-312">Key</span></span>               | <span data-ttu-id="5413c-313">Значение</span><span class="sxs-lookup"><span data-stu-id="5413c-313">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="5413c-314">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="5413c-314">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="5413c-315"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> загружает конфигурацию из пары "ключ — значение" переменной среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="5413c-315">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="5413c-316">Чтобы активировать конфигурацию переменных среды, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5413c-316">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="5413c-317">[Служба приложений Azure](https://azure.microsoft.com/services/app-service/) позволяет задать переменные среды на портале Azure, который может переопределить конфигурацию приложения, используя поставщик конфигурации переменных среды.</span><span class="sxs-lookup"><span data-stu-id="5413c-317">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="5413c-318">Дополнительные сведения см. в руководстве по [переопределению конфигурации приложения Azure с помощью портала Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="5413c-318">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5413c-319">`AddEnvironmentVariables` используется для загрузки переменных среды, имеющих префикс `DOTNET_`, для [конфигурации узла](#host-versus-app-configuration) при инициализации нового построителя узла с [универсальным узлом](xref:fundamentals/host/generic-host) и вызове `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5413c-319">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="5413c-320">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="5413c-320">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5413c-321">`AddEnvironmentVariables` используется для загрузки переменных среды, имеющих префикс `ASPNETCORE_`, для [конфигурации узла](#host-versus-app-configuration) при инициализации нового построителя узла с [веб-узлом](xref:fundamentals/host/web-host) и вызове `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5413c-321">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="5413c-322">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="5413c-322">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

<span data-ttu-id="5413c-323">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="5413c-323">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="5413c-324">конфигурация приложения на основе переменных среды без префикса путем вызова `AddEnvironmentVariables` без префикса;</span><span class="sxs-lookup"><span data-stu-id="5413c-324">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="5413c-325">дополнительную конфигурацию из файлов *appsettings.json* и *appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="5413c-325">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="5413c-326">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="5413c-326">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="5413c-327">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="5413c-327">Command-line arguments.</span></span>

<span data-ttu-id="5413c-328">Поставщик конфигурации переменных среды вызывается после выполнения настройки с помощью секретов пользователя и файлов *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="5413c-328">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="5413c-329">Вызов поставщика в этой позиции разрешает чтение переменных среды выполнения, чтобы переопределить конфигурацию, заданную секретом пользователя и файлом *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="5413c-329">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="5413c-330">Чтобы добавить конфигурацию приложения из дополнительных переменных среды, вызовите дополнительные поставщики приложения в `ConfigureAppConfiguration`, а затем вызовите `AddEnvironmentVariables` с префиксом:</span><span class="sxs-lookup"><span data-stu-id="5413c-330">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="5413c-331">Вызовите `AddEnvironmentVariables` последним, чтобы разрешить переменным среды с заданным префиксом переопределять значения от других поставщиков.</span><span class="sxs-lookup"><span data-stu-id="5413c-331">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="5413c-332">**Пример**</span><span class="sxs-lookup"><span data-stu-id="5413c-332">**Example**</span></span>

<span data-ttu-id="5413c-333">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="5413c-333">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="5413c-334">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="5413c-334">Run the sample app.</span></span> <span data-ttu-id="5413c-335">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="5413c-335">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="5413c-336">Обратите внимание, что выходные данные содержат пару "ключ — значение" для переменной среды `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="5413c-336">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="5413c-337">Значение отражает среду, в которой выполняется приложение, обычно при локальном запуске это `Development`.</span><span class="sxs-lookup"><span data-stu-id="5413c-337">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="5413c-338">Чтобы список переменных среды, отображаемый приложением, был коротким, приложение фильтрует переменные среды.</span><span class="sxs-lookup"><span data-stu-id="5413c-338">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="5413c-339">См. пример файла *Pages/Index.cshtml.cs* приложения.</span><span class="sxs-lookup"><span data-stu-id="5413c-339">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="5413c-340">Чтобы просмотреть все переменные среды, доступные для приложения, измените значение `FilteredConfiguration` в *Pages/Index.cshtml.cs* на следующее:</span><span class="sxs-lookup"><span data-stu-id="5413c-340">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="5413c-341">Префиксы</span><span class="sxs-lookup"><span data-stu-id="5413c-341">Prefixes</span></span>

<span data-ttu-id="5413c-342">Переменные среды, которые загружаются в конфигурацию приложения, фильтруются при добавлении префикса к методу `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="5413c-342">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="5413c-343">Например, чтобы отфильтровать переменные среды по префиксу `CUSTOM_`, введите префикс поставщику конфигурации:</span><span class="sxs-lookup"><span data-stu-id="5413c-343">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="5413c-344">Префикс отделяется при создании пары конфигурации "ключ — значение".</span><span class="sxs-lookup"><span data-stu-id="5413c-344">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="5413c-345">При создании построителя узла конфигурация узла предоставляется переменными среды.</span><span class="sxs-lookup"><span data-stu-id="5413c-345">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="5413c-346">Дополнительные сведения о префиксе, используемом для этих переменных среды, см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="5413c-346">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="5413c-347">**Префиксы строк подключения**</span><span class="sxs-lookup"><span data-stu-id="5413c-347">**Connection string prefixes**</span></span>

<span data-ttu-id="5413c-348">API конфигурации имеет специальные правила обработки для четырех строк подключения переменных среды, связанных с настройкой строк подключения Azure для среды приложения.</span><span class="sxs-lookup"><span data-stu-id="5413c-348">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="5413c-349">Если префикс не указан в `AddEnvironmentVariables`, переменные среды с префиксами, указанными в таблице, загружаются в приложение.</span><span class="sxs-lookup"><span data-stu-id="5413c-349">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="5413c-350">Префикс строки подключения</span><span class="sxs-lookup"><span data-stu-id="5413c-350">Connection string prefix</span></span> | <span data-ttu-id="5413c-351">Поставщик</span><span class="sxs-lookup"><span data-stu-id="5413c-351">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="5413c-352">Поставщик пользователя</span><span class="sxs-lookup"><span data-stu-id="5413c-352">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="5413c-353">MySQL</span><span class="sxs-lookup"><span data-stu-id="5413c-353">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="5413c-354">База данных SQL Azure</span><span class="sxs-lookup"><span data-stu-id="5413c-354">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="5413c-355">SQL Server</span><span class="sxs-lookup"><span data-stu-id="5413c-355">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="5413c-356">Когда переменная среды обнаруживается и загружается в конфигурацию с одним из четырех префиксов, приведенных в таблице, происходит следующее.</span><span class="sxs-lookup"><span data-stu-id="5413c-356">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="5413c-357">Ключ конфигурации создается путем удаления префикса переменных среды и добавления ключа раздела конфигурации (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="5413c-357">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="5413c-358">Создается новая пара "ключ — значение" конфигурации, которая представляет поставщика подключения базы данных (за исключением `CUSTOMCONNSTR_`, который не имеет указанного поставщика).</span><span class="sxs-lookup"><span data-stu-id="5413c-358">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="5413c-359">Ключ переменной среды</span><span class="sxs-lookup"><span data-stu-id="5413c-359">Environment variable key</span></span> | <span data-ttu-id="5413c-360">Преобразованный ключ конфигурации</span><span class="sxs-lookup"><span data-stu-id="5413c-360">Converted configuration key</span></span> | <span data-ttu-id="5413c-361">Запись конфигурации поставщика</span><span class="sxs-lookup"><span data-stu-id="5413c-361">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="5413c-362">Запись конфигурации не создана.</span><span class="sxs-lookup"><span data-stu-id="5413c-362">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="5413c-363">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="5413c-363">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="5413c-364">Значение: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="5413c-364">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="5413c-365">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="5413c-365">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="5413c-366">Значение: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="5413c-366">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="5413c-367">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="5413c-367">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="5413c-368">Значение: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="5413c-368">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="5413c-369">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="5413c-369">File Configuration Provider</span></span>

<span data-ttu-id="5413c-370"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> является базовым классом для загрузки конфигурации из файловой системы.</span><span class="sxs-lookup"><span data-stu-id="5413c-370"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="5413c-371">Следующие поставщики конфигурации предназначены для определенных типов файлов:</span><span class="sxs-lookup"><span data-stu-id="5413c-371">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="5413c-372">Поставщик конфигурации INI</span><span class="sxs-lookup"><span data-stu-id="5413c-372">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="5413c-373">Поставщик конфигурации JSON</span><span class="sxs-lookup"><span data-stu-id="5413c-373">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="5413c-374">Поставщик конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="5413c-374">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="5413c-375">Поставщик конфигурации INI</span><span class="sxs-lookup"><span data-stu-id="5413c-375">INI Configuration Provider</span></span>

<span data-ttu-id="5413c-376"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> загружает конфигурацию из пары "ключ — значение" INI-файла во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="5413c-376">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="5413c-377">Чтобы активировать конфигурацию INI-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5413c-377">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="5413c-378">Двоеточие можно использовать как разделитель раздела в конфигурации файла INI.</span><span class="sxs-lookup"><span data-stu-id="5413c-378">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="5413c-379">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="5413c-379">Overloads permit specifying:</span></span>

* <span data-ttu-id="5413c-380">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="5413c-380">Whether the file is optional.</span></span>
* <span data-ttu-id="5413c-381">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="5413c-381">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="5413c-382"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="5413c-382">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="5413c-383">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="5413c-383">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="5413c-384">Общий пример конфигурации INI-файла.</span><span class="sxs-lookup"><span data-stu-id="5413c-384">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="5413c-385">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="5413c-385">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="5413c-386">section0:key0</span><span class="sxs-lookup"><span data-stu-id="5413c-386">section0:key0</span></span>
* <span data-ttu-id="5413c-387">section0:key1</span><span class="sxs-lookup"><span data-stu-id="5413c-387">section0:key1</span></span>
* <span data-ttu-id="5413c-388">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="5413c-388">section1:subsection:key</span></span>
* <span data-ttu-id="5413c-389">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="5413c-389">section2:subsection0:key</span></span>
* <span data-ttu-id="5413c-390">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="5413c-390">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="5413c-391">Поставщик конфигурации JSON</span><span class="sxs-lookup"><span data-stu-id="5413c-391">JSON Configuration Provider</span></span>

<span data-ttu-id="5413c-392"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> загружает конфигурацию из пары "ключ — значение" JSON-файла в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="5413c-392">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="5413c-393">Чтобы активировать конфигурацию JSON-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5413c-393">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="5413c-394">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="5413c-394">Overloads permit specifying:</span></span>

* <span data-ttu-id="5413c-395">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="5413c-395">Whether the file is optional.</span></span>
* <span data-ttu-id="5413c-396">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="5413c-396">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="5413c-397"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="5413c-397">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="5413c-398">`AddJsonFile` автоматически вызывается дважды при инициализации нового построителя узла с `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5413c-398">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="5413c-399">Метод вызывается для загрузки конфигурации из:</span><span class="sxs-lookup"><span data-stu-id="5413c-399">The method is called to load configuration from:</span></span>

* <span data-ttu-id="5413c-400">*appsettings.json* &ndash; первым читается этот файл.</span><span class="sxs-lookup"><span data-stu-id="5413c-400">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="5413c-401">Версия файла среды может переопределить значения, предоставленные *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="5413c-401">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="5413c-402">*appsettings.{Environment}.json* &ndash; версия среды файла загружается на основе [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="5413c-402">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="5413c-403">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="5413c-403">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="5413c-404">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="5413c-404">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="5413c-405">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="5413c-405">Environment variables.</span></span>
* <span data-ttu-id="5413c-406">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="5413c-406">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="5413c-407">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="5413c-407">Command-line arguments.</span></span>

<span data-ttu-id="5413c-408">Сначала устанавливается поставщик конфигурации JSON-файлов.</span><span class="sxs-lookup"><span data-stu-id="5413c-408">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="5413c-409">Таким образом, секреты пользователя, переменные среды и аргументы командной строки переопределят конфигурацию, заданную файлами *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="5413c-409">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="5413c-410">Вызовите `ConfigureAppConfiguration` при сборке узла, чтобы указать конфигурацию приложения для файлов, отличных от *appsettings.json* и *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="5413c-410">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="5413c-411">**Пример**</span><span class="sxs-lookup"><span data-stu-id="5413c-411">**Example**</span></span>

<span data-ttu-id="5413c-412">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает два вызова `AddJsonFile`:</span><span class="sxs-lookup"><span data-stu-id="5413c-412">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="5413c-413">Первый вызов `AddJsonFile` загружает конфигурацию из *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="5413c-413">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="5413c-414">Второй вызов `AddJsonFile` загружает конфигурацию из *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="5413c-414">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="5413c-415">Для *appsettings.Development.json* в примере приложения загружается следующий файл:</span><span class="sxs-lookup"><span data-stu-id="5413c-415">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="5413c-416">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="5413c-416">Run the sample app.</span></span> <span data-ttu-id="5413c-417">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="5413c-417">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="5413c-418">Выходные данные содержат пары "ключ-значение" для конфигурации в зависимости от среды приложения.</span><span class="sxs-lookup"><span data-stu-id="5413c-418">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="5413c-419">Уровень ведения журнала для ключа `Logging:LogLevel:Default` имеет значение `Debug` при запуске приложения в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="5413c-419">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="5413c-420">Запустите пример приложения еще раз в рабочей среде:</span><span class="sxs-lookup"><span data-stu-id="5413c-420">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="5413c-421">Откройте файл *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="5413c-421">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="5413c-422">В профиле `ConfigurationSample` измените значение переменной среды `ASPNETCORE_ENVIRONMENT` на `Production`.</span><span class="sxs-lookup"><span data-stu-id="5413c-422">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="5413c-423">Сохраните файл и запустите приложение с помощью `dotnet run` в командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="5413c-423">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="5413c-424">Параметры в *appsettings.Development.json* больше не переопределяют параметры в *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="5413c-424">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="5413c-425">Уровень ведения журнала для ключа `Logging:LogLevel:Default` имеет значение `Information`.</span><span class="sxs-lookup"><span data-stu-id="5413c-425">The log level for the key `Logging:LogLevel:Default` is `Information`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="5413c-426">Первый вызов `AddJsonFile` загружает конфигурацию из *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="5413c-426">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="5413c-427">Второй вызов `AddJsonFile` загружает конфигурацию из *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="5413c-427">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="5413c-428">Для *appsettings.Development.json* в примере приложения загружается следующий файл:</span><span class="sxs-lookup"><span data-stu-id="5413c-428">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="5413c-429">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="5413c-429">Run the sample app.</span></span> <span data-ttu-id="5413c-430">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="5413c-430">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="5413c-431">Выходные данные содержат пары "ключ-значение" для конфигурации в зависимости от среды приложения.</span><span class="sxs-lookup"><span data-stu-id="5413c-431">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="5413c-432">Уровень ведения журнала для ключа `Logging:LogLevel:Default` имеет значение `Debug` при запуске приложения в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="5413c-432">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="5413c-433">Запустите пример приложения еще раз в рабочей среде:</span><span class="sxs-lookup"><span data-stu-id="5413c-433">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="5413c-434">Откройте файл *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="5413c-434">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="5413c-435">В профиле `ConfigurationSample` измените значение переменной среды `ASPNETCORE_ENVIRONMENT` на `Production`.</span><span class="sxs-lookup"><span data-stu-id="5413c-435">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="5413c-436">Сохраните файл и запустите приложение с помощью `dotnet run` в командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="5413c-436">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="5413c-437">Параметры в *appsettings.Development.json* больше не переопределяют параметры в *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="5413c-437">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="5413c-438">Уровень ведения журнала для ключа `Logging:LogLevel:Default` имеет значение `Warning`.</span><span class="sxs-lookup"><span data-stu-id="5413c-438">The log level for the key `Logging:LogLevel:Default` is `Warning`.</span></span>

::: moniker-end

### <a name="xml-configuration-provider"></a><span data-ttu-id="5413c-439">Поставщик конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="5413c-439">XML Configuration Provider</span></span>

<span data-ttu-id="5413c-440"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> загружает конфигурацию из пары "ключ — значение" XML-файла в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="5413c-440">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="5413c-441">Чтобы активировать конфигурацию XML-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5413c-441">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="5413c-442">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="5413c-442">Overloads permit specifying:</span></span>

* <span data-ttu-id="5413c-443">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="5413c-443">Whether the file is optional.</span></span>
* <span data-ttu-id="5413c-444">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="5413c-444">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="5413c-445"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="5413c-445">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="5413c-446">Корневой узел файла конфигурации не учитывается при создании пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-446">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="5413c-447">Не указывайте в файле Document Type Definition (определение типа документа, DTD) или пространство имен.</span><span class="sxs-lookup"><span data-stu-id="5413c-447">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="5413c-448">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="5413c-448">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="5413c-449">XML-файлы конфигурации могут использовать имена отдельных элементов для повторяющихся разделов.</span><span class="sxs-lookup"><span data-stu-id="5413c-449">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="5413c-450">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="5413c-450">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="5413c-451">section0:key0</span><span class="sxs-lookup"><span data-stu-id="5413c-451">section0:key0</span></span>
* <span data-ttu-id="5413c-452">section0:key1</span><span class="sxs-lookup"><span data-stu-id="5413c-452">section0:key1</span></span>
* <span data-ttu-id="5413c-453">section1:key0</span><span class="sxs-lookup"><span data-stu-id="5413c-453">section1:key0</span></span>
* <span data-ttu-id="5413c-454">section1:key1</span><span class="sxs-lookup"><span data-stu-id="5413c-454">section1:key1</span></span>

<span data-ttu-id="5413c-455">Повторяющиеся элементы, использующие то же имя элемента, работают, если атрибут `name` используется для различения элементов.</span><span class="sxs-lookup"><span data-stu-id="5413c-455">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="5413c-456">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="5413c-456">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="5413c-457">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="5413c-457">section:section0:key:key0</span></span>
* <span data-ttu-id="5413c-458">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="5413c-458">section:section0:key:key1</span></span>
* <span data-ttu-id="5413c-459">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="5413c-459">section:section1:key:key0</span></span>
* <span data-ttu-id="5413c-460">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="5413c-460">section:section1:key:key1</span></span>

<span data-ttu-id="5413c-461">Атрибуты можно использовать для предоставления значений.</span><span class="sxs-lookup"><span data-stu-id="5413c-461">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="5413c-462">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="5413c-462">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="5413c-463">key:attribute</span><span class="sxs-lookup"><span data-stu-id="5413c-463">key:attribute</span></span>
* <span data-ttu-id="5413c-464">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="5413c-464">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="5413c-465">Поставщик конфигурации ключа для каждого файла</span><span class="sxs-lookup"><span data-stu-id="5413c-465">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="5413c-466"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> использует файлы каталога как пары "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-466">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="5413c-467">Ключ является именем файла.</span><span class="sxs-lookup"><span data-stu-id="5413c-467">The key is the file name.</span></span> <span data-ttu-id="5413c-468">Значение содержит содержимое файла.</span><span class="sxs-lookup"><span data-stu-id="5413c-468">The value contains the file's contents.</span></span> <span data-ttu-id="5413c-469">Поставщик конфигурации ключа для каждого файла используется в сценариях размещения Docker.</span><span class="sxs-lookup"><span data-stu-id="5413c-469">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="5413c-470">Чтобы активировать конфигурацию ключа для каждого файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5413c-470">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="5413c-471">Значение параметра `directoryPath` должно быть абсолютным путем к файлам.</span><span class="sxs-lookup"><span data-stu-id="5413c-471">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="5413c-472">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="5413c-472">Overloads permit specifying:</span></span>

* <span data-ttu-id="5413c-473">`Action<KeyPerFileConfigurationSource>` — делегат, который настраивает источник.</span><span class="sxs-lookup"><span data-stu-id="5413c-473">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="5413c-474">Обязательно ли указывать каталог и путь к каталогу.</span><span class="sxs-lookup"><span data-stu-id="5413c-474">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="5413c-475">Двойное подчеркивание (`__`) используется в качестве разделителя ключа конфигурации в именах файлов.</span><span class="sxs-lookup"><span data-stu-id="5413c-475">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="5413c-476">Например, в имени файла `Logging__LogLevel__System` создается ключ конфигурации `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="5413c-476">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="5413c-477">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="5413c-477">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="5413c-478">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="5413c-478">Memory Configuration Provider</span></span>

<span data-ttu-id="5413c-479"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> использует коллекцию памяти в качестве пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-479">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="5413c-480">Чтобы активировать конфигурацию коллекции в памяти, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5413c-480">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="5413c-481">Поставщик конфигурации может инициализироваться значением `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="5413c-481">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="5413c-482">Чтобы указать конфигурацию приложения, при сборке узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="5413c-482">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="5413c-483">В следующем примере создается словарь конфигурации:</span><span class="sxs-lookup"><span data-stu-id="5413c-483">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="5413c-484">Словарь используется с вызовом `AddInMemoryCollection` для предоставления конфигурации:</span><span class="sxs-lookup"><span data-stu-id="5413c-484">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="5413c-485">GetValue</span><span class="sxs-lookup"><span data-stu-id="5413c-485">GetValue</span></span>

<span data-ttu-id="5413c-486">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) извлекает одно значение из конфигурации с указанным ключом и преобразует его в указанный тип, не являющийся коллекцией.</span><span class="sxs-lookup"><span data-stu-id="5413c-486">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="5413c-487">Перегрузка принимает значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5413c-487">An overload accepts a default value.</span></span>

<span data-ttu-id="5413c-488">В следующем примере происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="5413c-488">The following example:</span></span>

* <span data-ttu-id="5413c-489">Из конфигурации извлекается строковое значение с ключом `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="5413c-489">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="5413c-490">Если `NumberKey` отсутствует в ключах конфигурации, используется значение по умолчанию `99`.</span><span class="sxs-lookup"><span data-stu-id="5413c-490">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="5413c-491">Значение получает тип `int`.</span><span class="sxs-lookup"><span data-stu-id="5413c-491">Types the value as an `int`.</span></span>
* <span data-ttu-id="5413c-492">Значение сохраняется в свойстве `NumberConfig` для использования на странице.</span><span class="sxs-lookup"><span data-stu-id="5413c-492">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="5413c-493">GetSection, GetChildren и Exists</span><span class="sxs-lookup"><span data-stu-id="5413c-493">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="5413c-494">В следующих примерах рассмотрим файл JSON.</span><span class="sxs-lookup"><span data-stu-id="5413c-494">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="5413c-495">Четыре ключа находятся в двух разделах, один из которых содержит пару из подразделов.</span><span class="sxs-lookup"><span data-stu-id="5413c-495">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="5413c-496">Когда файл считывается в конфигурацию, для сохранения значений конфигурации создаются следующие уникальные иерархические ключи.</span><span class="sxs-lookup"><span data-stu-id="5413c-496">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="5413c-497">section0:key0</span><span class="sxs-lookup"><span data-stu-id="5413c-497">section0:key0</span></span>
* <span data-ttu-id="5413c-498">section0:key1</span><span class="sxs-lookup"><span data-stu-id="5413c-498">section0:key1</span></span>
* <span data-ttu-id="5413c-499">section1:key0</span><span class="sxs-lookup"><span data-stu-id="5413c-499">section1:key0</span></span>
* <span data-ttu-id="5413c-500">section1:key1</span><span class="sxs-lookup"><span data-stu-id="5413c-500">section1:key1</span></span>
* <span data-ttu-id="5413c-501">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="5413c-501">section2:subsection0:key0</span></span>
* <span data-ttu-id="5413c-502">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="5413c-502">section2:subsection0:key1</span></span>
* <span data-ttu-id="5413c-503">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="5413c-503">section2:subsection1:key0</span></span>
* <span data-ttu-id="5413c-504">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="5413c-504">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="5413c-505">GetSection</span><span class="sxs-lookup"><span data-stu-id="5413c-505">GetSection</span></span>

<span data-ttu-id="5413c-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) извлекает подраздел конфигурации с указанным ключом подраздела.</span><span class="sxs-lookup"><span data-stu-id="5413c-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="5413c-507">Чтобы вернуть <xref:Microsoft.Extensions.Configuration.IConfigurationSection>, содержащий только пары "ключ — значение" в `section1`, вызовите `GetSection` и укажите имя раздела.</span><span class="sxs-lookup"><span data-stu-id="5413c-507">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="5413c-508">`configSection` не содержит значение, только ключ и путь.</span><span class="sxs-lookup"><span data-stu-id="5413c-508">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="5413c-509">Аналогично, чтобы получить значения для ключей в `section2:subsection0`, вызовите `GetSection` и укажите путь к разделу.</span><span class="sxs-lookup"><span data-stu-id="5413c-509">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="5413c-510">Значение `GetSection` никогда не возвращает значение `null`.</span><span class="sxs-lookup"><span data-stu-id="5413c-510">`GetSection` never returns `null`.</span></span> <span data-ttu-id="5413c-511">Если соответствующий раздел не найден, возвращается пустой параметр `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="5413c-511">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="5413c-512">Когда `GetSection` возвращает соответствующий раздел, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> не заполняется.</span><span class="sxs-lookup"><span data-stu-id="5413c-512">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="5413c-513"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> и <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> возвращаются, если раздел существует.</span><span class="sxs-lookup"><span data-stu-id="5413c-513">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="5413c-514">GetChildren</span><span class="sxs-lookup"><span data-stu-id="5413c-514">GetChildren</span></span>

<span data-ttu-id="5413c-515">Вызов [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) в `section2` получает значение `IEnumerable<IConfigurationSection>`, которое включает:</span><span class="sxs-lookup"><span data-stu-id="5413c-515">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="5413c-516">Exists</span><span class="sxs-lookup"><span data-stu-id="5413c-516">Exists</span></span>

<span data-ttu-id="5413c-517">Используйте [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*), чтобы определить, существует ли раздел конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-517">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="5413c-518">Учитывая данные примера, `sectionExists` является `false`, потому что в данных конфигурации нет `section2:subsection2`.</span><span class="sxs-lookup"><span data-stu-id="5413c-518">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="5413c-519">Привязка к классу</span><span class="sxs-lookup"><span data-stu-id="5413c-519">Bind to a class</span></span>

<span data-ttu-id="5413c-520">Конфигурация может быть привязана к классам, которые представляют группы связанных параметров, используя шаблон *вариантов*.</span><span class="sxs-lookup"><span data-stu-id="5413c-520">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="5413c-521">Для получения дополнительной информации см. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="5413c-521">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="5413c-522">Значения конфигурации возвращаются как строки, но вызов <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> позволяет построить объекты [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="5413c-522">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="5413c-523">Модуль привязки привязывает значения ко всем открытым свойствам чтения и записи предоставленного типа.</span><span class="sxs-lookup"><span data-stu-id="5413c-523">The binder binds values to all of the public read/write properties of the type provided.</span></span> <span data-ttu-id="5413c-524">Поля **не** привязаны.</span><span class="sxs-lookup"><span data-stu-id="5413c-524">Fields are **not** bound.</span></span>

<span data-ttu-id="5413c-525">Пример приложения содержит модель `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="5413c-525">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5413c-526">Раздел `starship` файла *starship.json* создает конфигурацию, когда образец приложения использует поставщик конфигурации JSON для загрузки конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-526">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="5413c-527">Создаются следующие пары "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-527">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="5413c-528">Ключ</span><span class="sxs-lookup"><span data-stu-id="5413c-528">Key</span></span>                   | <span data-ttu-id="5413c-529">Значение</span><span class="sxs-lookup"><span data-stu-id="5413c-529">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="5413c-530">starship:name</span><span class="sxs-lookup"><span data-stu-id="5413c-530">starship:name</span></span>         | <span data-ttu-id="5413c-531">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="5413c-531">USS Enterprise</span></span>                                    |
| <span data-ttu-id="5413c-532">starship:registry</span><span class="sxs-lookup"><span data-stu-id="5413c-532">starship:registry</span></span>     | <span data-ttu-id="5413c-533">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="5413c-533">NCC-1701</span></span>                                          |
| <span data-ttu-id="5413c-534">starship:class</span><span class="sxs-lookup"><span data-stu-id="5413c-534">starship:class</span></span>        | <span data-ttu-id="5413c-535">Constitution</span><span class="sxs-lookup"><span data-stu-id="5413c-535">Constitution</span></span>                                      |
| <span data-ttu-id="5413c-536">starship:length</span><span class="sxs-lookup"><span data-stu-id="5413c-536">starship:length</span></span>       | <span data-ttu-id="5413c-537">304.8</span><span class="sxs-lookup"><span data-stu-id="5413c-537">304.8</span></span>                                             |
| <span data-ttu-id="5413c-538">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="5413c-538">starship:commissioned</span></span> | <span data-ttu-id="5413c-539">False</span><span class="sxs-lookup"><span data-stu-id="5413c-539">False</span></span>                                             |
| <span data-ttu-id="5413c-540">trademark</span><span class="sxs-lookup"><span data-stu-id="5413c-540">trademark</span></span>             | <span data-ttu-id="5413c-541">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="5413c-541">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="5413c-542">Пример приложения вызывает `GetSection` с помощью ключа `starship`.</span><span class="sxs-lookup"><span data-stu-id="5413c-542">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="5413c-543">Пары "ключ — значение" `starship` изолированы.</span><span class="sxs-lookup"><span data-stu-id="5413c-543">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="5413c-544">Метод `Bind` вызывается в подразделе при прохождении в экземпляр класса `Starship`.</span><span class="sxs-lookup"><span data-stu-id="5413c-544">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="5413c-545">После привязки значения экземпляра экземпляру присваивается свойство для преобразования для просмотра.</span><span class="sxs-lookup"><span data-stu-id="5413c-545">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="5413c-546">Привязка к графу объектов</span><span class="sxs-lookup"><span data-stu-id="5413c-546">Bind to an object graph</span></span>

<span data-ttu-id="5413c-547"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> способна связывать весь граф объекта POCO.</span><span class="sxs-lookup"><span data-stu-id="5413c-547"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="5413c-548">Как и при привязке простого объекта, привязываются только открытые свойства чтения и записи.</span><span class="sxs-lookup"><span data-stu-id="5413c-548">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="5413c-549">Образец содержит модель `TvShow`, в графе объектов которого находятся классы `Metadata` и `Actors` (*Модели/TvShow.cs*).</span><span class="sxs-lookup"><span data-stu-id="5413c-549">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5413c-550">В примере приложения есть файл *tvshow.xml*, содержащий данные конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-550">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="5413c-551">Конфигурация привязана ко всему ​​графу объектов `TvShow` с помощью метода `Bind`.</span><span class="sxs-lookup"><span data-stu-id="5413c-551">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="5413c-552">Привязанный экземпляр присваивается свойству для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="5413c-552">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="5413c-553">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) привязывает и возвращает указанный тип.</span><span class="sxs-lookup"><span data-stu-id="5413c-553">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="5413c-554">Метод `Get<T>` может быть более удобным, чем использование `Bind`.</span><span class="sxs-lookup"><span data-stu-id="5413c-554">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="5413c-555">В следующем коде показано, как использовать `Get<T>` с предыдущим примером, который позволяет привязать связанный экземпляр непосредственно к свойству, используемому для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="5413c-555">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="5413c-556">Привязка массива к классу</span><span class="sxs-lookup"><span data-stu-id="5413c-556">Bind an array to a class</span></span>

<span data-ttu-id="5413c-557">*Пример приложения демонстрирует концепции, описанные в этом разделе.*</span><span class="sxs-lookup"><span data-stu-id="5413c-557">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="5413c-558"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-558">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="5413c-559">Любой формат массива, который предоставляет сегмент числового ключа (`:0:`, `:1:`, &hellip; `:{n}:`), способен привязать массив к массиву класса POCO.</span><span class="sxs-lookup"><span data-stu-id="5413c-559">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="5413c-560">Привязка предоставляется соглашением.</span><span class="sxs-lookup"><span data-stu-id="5413c-560">Binding is provided by convention.</span></span> <span data-ttu-id="5413c-561">Пользовательские поставщики конфигурации не обязаны реализовывать привязку массива.</span><span class="sxs-lookup"><span data-stu-id="5413c-561">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="5413c-562">**Обработка массива в оперативной памяти**</span><span class="sxs-lookup"><span data-stu-id="5413c-562">**In-memory array processing**</span></span>

<span data-ttu-id="5413c-563">Рассмотрите ключи и значения конфигурации, приведенные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="5413c-563">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="5413c-564">Ключ</span><span class="sxs-lookup"><span data-stu-id="5413c-564">Key</span></span>             | <span data-ttu-id="5413c-565">Значение</span><span class="sxs-lookup"><span data-stu-id="5413c-565">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="5413c-566">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="5413c-566">array:entries:0</span></span> | <span data-ttu-id="5413c-567">value0</span><span class="sxs-lookup"><span data-stu-id="5413c-567">value0</span></span> |
| <span data-ttu-id="5413c-568">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="5413c-568">array:entries:1</span></span> | <span data-ttu-id="5413c-569">value1</span><span class="sxs-lookup"><span data-stu-id="5413c-569">value1</span></span> |
| <span data-ttu-id="5413c-570">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="5413c-570">array:entries:2</span></span> | <span data-ttu-id="5413c-571">value2</span><span class="sxs-lookup"><span data-stu-id="5413c-571">value2</span></span> |
| <span data-ttu-id="5413c-572">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="5413c-572">array:entries:4</span></span> | <span data-ttu-id="5413c-573">value4</span><span class="sxs-lookup"><span data-stu-id="5413c-573">value4</span></span> |
| <span data-ttu-id="5413c-574">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="5413c-574">array:entries:5</span></span> | <span data-ttu-id="5413c-575">value5</span><span class="sxs-lookup"><span data-stu-id="5413c-575">value5</span></span> |

<span data-ttu-id="5413c-576">Эти ключи и значения загружаются в пример приложения с помощью поставщика конфигурации памяти.</span><span class="sxs-lookup"><span data-stu-id="5413c-576">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

<span data-ttu-id="5413c-577">Массив пропускает значения для индекса &num;3.</span><span class="sxs-lookup"><span data-stu-id="5413c-577">The array skips a value for index &num;3.</span></span> <span data-ttu-id="5413c-578">Связующее свойство конфигурации не способно связывать нулевые значения или создавать нулевые записи в связанных объектах, что становится ясным в тот момент, когда демонстрируется результат привязки этого массива к объекту.</span><span class="sxs-lookup"><span data-stu-id="5413c-578">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="5413c-579">В примере приложения класс POCO доступен для хранения привязанных данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-579">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5413c-580">Данные конфигурации, связанные с объектом.</span><span class="sxs-lookup"><span data-stu-id="5413c-580">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="5413c-581">Синтаксис [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) также может использоваться для получения более компактного кода.</span><span class="sxs-lookup"><span data-stu-id="5413c-581">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="5413c-582">Связанный объект, экземпляр `ArrayExample`, получает данные массива из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-582">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="5413c-583">Индекс `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="5413c-583">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="5413c-584">Значение `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="5413c-584">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="5413c-585">0</span><span class="sxs-lookup"><span data-stu-id="5413c-585">0</span></span>                            | <span data-ttu-id="5413c-586">value0</span><span class="sxs-lookup"><span data-stu-id="5413c-586">value0</span></span>                       |
| <span data-ttu-id="5413c-587">1</span><span class="sxs-lookup"><span data-stu-id="5413c-587">1</span></span>                            | <span data-ttu-id="5413c-588">value1</span><span class="sxs-lookup"><span data-stu-id="5413c-588">value1</span></span>                       |
| <span data-ttu-id="5413c-589">2</span><span class="sxs-lookup"><span data-stu-id="5413c-589">2</span></span>                            | <span data-ttu-id="5413c-590">value2</span><span class="sxs-lookup"><span data-stu-id="5413c-590">value2</span></span>                       |
| <span data-ttu-id="5413c-591">3</span><span class="sxs-lookup"><span data-stu-id="5413c-591">3</span></span>                            | <span data-ttu-id="5413c-592">value4</span><span class="sxs-lookup"><span data-stu-id="5413c-592">value4</span></span>                       |
| <span data-ttu-id="5413c-593">4</span><span class="sxs-lookup"><span data-stu-id="5413c-593">4</span></span>                            | <span data-ttu-id="5413c-594">value5</span><span class="sxs-lookup"><span data-stu-id="5413c-594">value5</span></span>                       |

<span data-ttu-id="5413c-595">Индекс &num;3 в связанном объекте содержит данные конфигурации для конфигурационного ключа `array:4` и его значения `value4`.</span><span class="sxs-lookup"><span data-stu-id="5413c-595">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="5413c-596">При привязке данных конфигурации, содержащих массив индексов, в ключах конфигурации эти индексы просто используются для выполнения итерации данных конфигурации при создании объекта.</span><span class="sxs-lookup"><span data-stu-id="5413c-596">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="5413c-597">Когда массив ключей конфигурации пропускает один или несколько индексов, в данных конфигурации не может быть сохранено нулевое значение и в связанном объекте не создается нулевая запись.</span><span class="sxs-lookup"><span data-stu-id="5413c-597">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="5413c-598">Отсутствующий элемент конфигурации для индекса &num;3 может быть предоставлен перед привязкой к экземпляру `ArrayExample` любым поставщиком конфигурации, который создает правильную пару "ключ — значение" в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-598">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="5413c-599">Если в образец включен дополнительный поставщик конфигурации JSON с отсутствующей парой "ключ — значение", то `ArrayExample.Entries` подойдет полному массиву конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-599">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="5413c-600">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="5413c-600">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="5413c-601">В `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="5413c-601">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="5413c-602">Пары "ключ — значение", показанные в таблице, загружаются в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-602">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="5413c-603">Ключ</span><span class="sxs-lookup"><span data-stu-id="5413c-603">Key</span></span>             | <span data-ttu-id="5413c-604">Значение</span><span class="sxs-lookup"><span data-stu-id="5413c-604">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="5413c-605">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="5413c-605">array:entries:3</span></span> | <span data-ttu-id="5413c-606">value3</span><span class="sxs-lookup"><span data-stu-id="5413c-606">value3</span></span> |

<span data-ttu-id="5413c-607">Если экземпляр класса `ArrayExample` связан после того, как поставщик конфигурации JSON включает запись для индекса &num;3, то массив `ArrayExample.Entries` включит значение.</span><span class="sxs-lookup"><span data-stu-id="5413c-607">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="5413c-608">Индекс `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="5413c-608">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="5413c-609">Значение `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="5413c-609">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="5413c-610">0</span><span class="sxs-lookup"><span data-stu-id="5413c-610">0</span></span>                            | <span data-ttu-id="5413c-611">value0</span><span class="sxs-lookup"><span data-stu-id="5413c-611">value0</span></span>                       |
| <span data-ttu-id="5413c-612">1</span><span class="sxs-lookup"><span data-stu-id="5413c-612">1</span></span>                            | <span data-ttu-id="5413c-613">value1</span><span class="sxs-lookup"><span data-stu-id="5413c-613">value1</span></span>                       |
| <span data-ttu-id="5413c-614">2</span><span class="sxs-lookup"><span data-stu-id="5413c-614">2</span></span>                            | <span data-ttu-id="5413c-615">value2</span><span class="sxs-lookup"><span data-stu-id="5413c-615">value2</span></span>                       |
| <span data-ttu-id="5413c-616">3</span><span class="sxs-lookup"><span data-stu-id="5413c-616">3</span></span>                            | <span data-ttu-id="5413c-617">value3</span><span class="sxs-lookup"><span data-stu-id="5413c-617">value3</span></span>                       |
| <span data-ttu-id="5413c-618">4</span><span class="sxs-lookup"><span data-stu-id="5413c-618">4</span></span>                            | <span data-ttu-id="5413c-619">value4</span><span class="sxs-lookup"><span data-stu-id="5413c-619">value4</span></span>                       |
| <span data-ttu-id="5413c-620">5</span><span class="sxs-lookup"><span data-stu-id="5413c-620">5</span></span>                            | <span data-ttu-id="5413c-621">value5</span><span class="sxs-lookup"><span data-stu-id="5413c-621">value5</span></span>                       |

<span data-ttu-id="5413c-622">**Обработка массива JSON**</span><span class="sxs-lookup"><span data-stu-id="5413c-622">**JSON array processing**</span></span>

<span data-ttu-id="5413c-623">Если JSON-файл содержит массив, ключи конфигурации будут созданы для элементов массива с индексом раздела, начиная с нуля.</span><span class="sxs-lookup"><span data-stu-id="5413c-623">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="5413c-624">В следующем файле конфигурации `subsection` — это массив.</span><span class="sxs-lookup"><span data-stu-id="5413c-624">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="5413c-625">Поставщик конфигурации JSON считывает данные конфигурации в следующие пары "ключ — значение".</span><span class="sxs-lookup"><span data-stu-id="5413c-625">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="5413c-626">Ключ</span><span class="sxs-lookup"><span data-stu-id="5413c-626">Key</span></span>                     | <span data-ttu-id="5413c-627">Значение</span><span class="sxs-lookup"><span data-stu-id="5413c-627">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="5413c-628">json_array:key</span><span class="sxs-lookup"><span data-stu-id="5413c-628">json_array:key</span></span>          | <span data-ttu-id="5413c-629">valueA</span><span class="sxs-lookup"><span data-stu-id="5413c-629">valueA</span></span> |
| <span data-ttu-id="5413c-630">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="5413c-630">json_array:subsection:0</span></span> | <span data-ttu-id="5413c-631">valueB</span><span class="sxs-lookup"><span data-stu-id="5413c-631">valueB</span></span> |
| <span data-ttu-id="5413c-632">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="5413c-632">json_array:subsection:1</span></span> | <span data-ttu-id="5413c-633">valueC</span><span class="sxs-lookup"><span data-stu-id="5413c-633">valueC</span></span> |
| <span data-ttu-id="5413c-634">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="5413c-634">json_array:subsection:2</span></span> | <span data-ttu-id="5413c-635">valueD</span><span class="sxs-lookup"><span data-stu-id="5413c-635">valueD</span></span> |

<span data-ttu-id="5413c-636">В примере приложения для привязки пар "ключ — значение" конфигурации доступен следующий класс POCO.</span><span class="sxs-lookup"><span data-stu-id="5413c-636">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5413c-637">После выполнения привязки `JsonArrayExample.Key` содержит значение `valueA`.</span><span class="sxs-lookup"><span data-stu-id="5413c-637">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="5413c-638">Подраздел значения хранится в свойстве массива POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="5413c-638">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="5413c-639">Индекс `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="5413c-639">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="5413c-640">Значение `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="5413c-640">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="5413c-641">0</span><span class="sxs-lookup"><span data-stu-id="5413c-641">0</span></span>                                   | <span data-ttu-id="5413c-642">valueB</span><span class="sxs-lookup"><span data-stu-id="5413c-642">valueB</span></span>                              |
| <span data-ttu-id="5413c-643">1</span><span class="sxs-lookup"><span data-stu-id="5413c-643">1</span></span>                                   | <span data-ttu-id="5413c-644">valueC</span><span class="sxs-lookup"><span data-stu-id="5413c-644">valueC</span></span>                              |
| <span data-ttu-id="5413c-645">2</span><span class="sxs-lookup"><span data-stu-id="5413c-645">2</span></span>                                   | <span data-ttu-id="5413c-646">valueD</span><span class="sxs-lookup"><span data-stu-id="5413c-646">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="5413c-647">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="5413c-647">Custom configuration provider</span></span>

<span data-ttu-id="5413c-648">Пример приложения демонстрирует, как создать базовый поставщик конфигурации, который считывает пары "ключ — значение" конфигурации из базы данных, используя [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="5413c-648">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="5413c-649">Поставщик имеет следующие характеристики.</span><span class="sxs-lookup"><span data-stu-id="5413c-649">The provider has the following characteristics:</span></span>

* <span data-ttu-id="5413c-650">База данных в памяти EF используется для демонстрационных целей.</span><span class="sxs-lookup"><span data-stu-id="5413c-650">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="5413c-651">Чтобы использовать базу данных, для которой требуется строка подключения, выполните вторичный `ConfigurationBuilder`, чтобы предоставить строку подключения от другого поставщика конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5413c-651">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="5413c-652">Поставщик считывает таблицу базы данных в конфигурации при запуске.</span><span class="sxs-lookup"><span data-stu-id="5413c-652">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="5413c-653">Поставщик не запрашивает базу данных для каждого ключа.</span><span class="sxs-lookup"><span data-stu-id="5413c-653">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="5413c-654">Функция перезагрузки на изменение не реализована, поэтому обновление базы данных после запуска приложения не влияет на конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="5413c-654">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="5413c-655">Определите сущность `EFConfigurationValue` для хранения значений конфигурации в базе данных.</span><span class="sxs-lookup"><span data-stu-id="5413c-655">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5413c-656">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="5413c-656">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="5413c-657">Добавьте `EFConfigurationContext` в хранилище и обратитесь к настроенным значениям.</span><span class="sxs-lookup"><span data-stu-id="5413c-657">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="5413c-658">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="5413c-658">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="5413c-659">Создайте класс, реализующий <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="5413c-659">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="5413c-660">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="5413c-660">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="5413c-661">Создайте пользовательский поставщик конфигурации путем наследования от <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="5413c-661">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="5413c-662">Поставщик конфигурации инициализирует пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="5413c-662">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="5413c-663">Поскольку [конфигурационные ключи не учитывают регистр](#keys), словарь, используемый для инициализации базы данных, создается с помощью функции сравнения без учета регистра ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span><span class="sxs-lookup"><span data-stu-id="5413c-663">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="5413c-664">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="5413c-664">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="5413c-665">Метод расширения `AddEFConfiguration` позволяет добавить источник конфигурации к `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5413c-665">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="5413c-666">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="5413c-666">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="5413c-667">В следующем коде показано, как использовать пользовательский `EFConfigurationProvider` в *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="5413c-667">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5413c-668">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="5413c-668">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="5413c-669">Добавьте `EFConfigurationContext` в хранилище и обратитесь к настроенным значениям.</span><span class="sxs-lookup"><span data-stu-id="5413c-669">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="5413c-670">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="5413c-670">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="5413c-671">Создайте класс, реализующий <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="5413c-671">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="5413c-672">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="5413c-672">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="5413c-673">Создайте пользовательский поставщик конфигурации путем наследования от <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="5413c-673">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="5413c-674">Поставщик конфигурации инициализирует пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="5413c-674">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="5413c-675">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="5413c-675">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="5413c-676">Метод расширения `AddEFConfiguration` позволяет добавить источник конфигурации к `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5413c-676">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="5413c-677">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="5413c-677">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="5413c-678">В следующем коде показано, как использовать пользовательский `EFConfigurationProvider` в *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="5413c-678">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="5413c-679">Доступ к конфигурации во время запуска</span><span class="sxs-lookup"><span data-stu-id="5413c-679">Access configuration during startup</span></span>

<span data-ttu-id="5413c-680">Внесите значение `IConfiguration` в конструктор `Startup` для доступа к значениям конфигурации в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5413c-680">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5413c-681">Чтобы получить доступ к конфигурации в `Startup.Configure`, либо добавьте значение `IConfiguration` непосредственно в метод, либо используйте экземпляр из конструктора.</span><span class="sxs-lookup"><span data-stu-id="5413c-681">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="5413c-682">Дополнительные сведения см. в руководстве по [доступу к конфигурации с использованием удобных методов](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="5413c-682">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="5413c-683">Настройте доступ на странице Razor Pages или в представлении MVC</span><span class="sxs-lookup"><span data-stu-id="5413c-683">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="5413c-684">Для доступа к параметрам конфигурации на странице Razor Pages или в представлении MVC добавьте [использование директивы](xref:mvc/views/razor#using) ([Справочник по C#: использование директивы](/dotnet/csharp/language-reference/keywords/using-directive)) для [пространства имен Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) и вставьте <xref:Microsoft.Extensions.Configuration.IConfiguration> на страницу или в представление.</span><span class="sxs-lookup"><span data-stu-id="5413c-684">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="5413c-685">На странице Razor Pages</span><span class="sxs-lookup"><span data-stu-id="5413c-685">In a Razor Pages page:</span></span>

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

<span data-ttu-id="5413c-686">В представлении MVC</span><span class="sxs-lookup"><span data-stu-id="5413c-686">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="5413c-687">Добавление конфигурации из внешней сборки</span><span class="sxs-lookup"><span data-stu-id="5413c-687">Add configuration from an external assembly</span></span>

<span data-ttu-id="5413c-688">Реализация <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> позволяет при запуске добавлять в приложение улучшения из внешней сборки вне приложения класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="5413c-688">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="5413c-689">Для получения дополнительной информации см. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="5413c-689">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5413c-690">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="5413c-690">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
