---
title: Конфигурация в .NET Core
author: guardrex
description: Узнайте, как использовать API конфигурации для настройки приложения ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/29/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: c63609cfb91a1668b8e125c54fcfecf5f4ec259b
ms.sourcegitcommit: de0fc77487a4d342bcc30965ec5c142d10d22c03
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2019
ms.locfileid: "73143349"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="ce517-103">Конфигурация в .NET Core</span><span class="sxs-lookup"><span data-stu-id="ce517-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="ce517-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="ce517-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ce517-105">Конфигурация приложения в ASP.NET Core основана на парах "ключ — значение", установленных *поставщиками конфигурации*.</span><span class="sxs-lookup"><span data-stu-id="ce517-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="ce517-106">Поставщики конфигурации получают данные конфигурации в парах "ключ — значение" из различных источников:</span><span class="sxs-lookup"><span data-stu-id="ce517-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="ce517-107">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="ce517-107">Azure Key Vault</span></span>
* <span data-ttu-id="ce517-108">Конфигурация приложений Azure</span><span class="sxs-lookup"><span data-stu-id="ce517-108">Azure App Configuration</span></span>
* <span data-ttu-id="ce517-109">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="ce517-109">Command-line arguments</span></span>
* <span data-ttu-id="ce517-110">а также пользовательские поставщики (устанавливаемые или создаваемые).</span><span class="sxs-lookup"><span data-stu-id="ce517-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="ce517-111">Справочные файлы</span><span class="sxs-lookup"><span data-stu-id="ce517-111">Directory files</span></span>
* <span data-ttu-id="ce517-112">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="ce517-112">Environment variables</span></span>
* <span data-ttu-id="ce517-113">объектов .NET в памяти;</span><span class="sxs-lookup"><span data-stu-id="ce517-113">In-memory .NET objects</span></span>
* <span data-ttu-id="ce517-114">файлов параметров;</span><span class="sxs-lookup"><span data-stu-id="ce517-114">Settings files</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ce517-115">Пакеты конфигурации для распространенных вариантов провайдеров конфигурации ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) включаются платформой неявным образом.</span><span class="sxs-lookup"><span data-stu-id="ce517-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ce517-116">Пакеты конфигурации для распространенных вариантов провайдеров конфигурации ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) включаются в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="ce517-116">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="ce517-117">В приведенных ниже и представленных в образце приложения примерах кода используется пространство имен <xref:Microsoft.Extensions.Configuration>:</span><span class="sxs-lookup"><span data-stu-id="ce517-117">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="ce517-118">*Шаблон параметров* является расширением конфигурации основных понятий, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="ce517-118">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="ce517-119">Параметры используют классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="ce517-119">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="ce517-120">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="ce517-120">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="ce517-121">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ce517-121">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="ce517-122">Конфигурация узла и приложения</span><span class="sxs-lookup"><span data-stu-id="ce517-122">Host versus app configuration</span></span>

<span data-ttu-id="ce517-123">Перед настройкой и запуском приложения настройте и запустите *узел*.</span><span class="sxs-lookup"><span data-stu-id="ce517-123">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="ce517-124">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="ce517-124">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="ce517-125">Как приложение, так и узел настраиваются с использованием поставщиков конфигурации, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="ce517-125">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="ce517-126">Пары "ключ — значение" конфигурации узлов также включаются в конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="ce517-126">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="ce517-127">Дополнительные сведения о том, как используются поставщики конфигурации при создании узла и как влияют источники конфигурации на узел, см. в разделе <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="ce517-127">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="ce517-128">Конфигурация по умолчанию</span><span class="sxs-lookup"><span data-stu-id="ce517-128">Default configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ce517-129">Веб-приложения на основе шаблонов [dotnet new](/dotnet/core/tools/dotnet-new) ASP.NET Core вызывают <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> при создании узла.</span><span class="sxs-lookup"><span data-stu-id="ce517-129">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="ce517-130">`CreateDefaultBuilder` предоставляет конфигурацию по умолчанию для приложения в следующем порядке:</span><span class="sxs-lookup"><span data-stu-id="ce517-130">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="ce517-131">Приведенные ниже сведения относятся к приложениям, использующим [универсальный узел](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="ce517-131">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="ce517-132">Подробные сведения о конфигурации по умолчанию при использовании [веб-узла](xref:fundamentals/host/web-host) см. в [разделе о версии ASP.NET Core 2.2 в этой статье](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="ce517-132">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="ce517-133">Существуют следующие способы предоставления конфигурации узла.</span><span class="sxs-lookup"><span data-stu-id="ce517-133">Host configuration is provided from:</span></span>
  * <span data-ttu-id="ce517-134">Переменные среды с префиксом `DOTNET_` (например, `DOTNET_ENVIRONMENT`), использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ce517-134">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="ce517-135">Префикс (`DOTNET_`) исключается при загрузке пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-135">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="ce517-136">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ce517-136">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="ce517-137">Устанавливается конфигурация веб-узла по умолчанию (`ConfigureWebHostDefaults`):</span><span class="sxs-lookup"><span data-stu-id="ce517-137">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="ce517-138">Kestrel используется в качестве веб-сервера и настраивается с помощью поставщиков конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="ce517-138">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="ce517-139">Добавьте ПО промежуточного слоя фильтрации узлов.</span><span class="sxs-lookup"><span data-stu-id="ce517-139">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="ce517-140">Если переменной среды `ASPNETCORE_FORWARDEDHEADERS_ENABLED` присвоено значение `true`, добавьте ПО промежуточного слоя перенаправления заголовков.</span><span class="sxs-lookup"><span data-stu-id="ce517-140">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="ce517-141">Включите интеграцию служб IIS.</span><span class="sxs-lookup"><span data-stu-id="ce517-141">Enable IIS integration.</span></span>
* <span data-ttu-id="ce517-142">Конфигурация приложения предоставляется из следующих ресурсов:</span><span class="sxs-lookup"><span data-stu-id="ce517-142">App configuration is provided from:</span></span>
  * <span data-ttu-id="ce517-143">Файл *appsettings.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ce517-143">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="ce517-144">Файл *appsettings.{Environment}.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ce517-144">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="ce517-145">[Менеджера секретов](xref:security/app-secrets), когда приложение выполняется в среде `Development` с использованием начальных сборок.</span><span class="sxs-lookup"><span data-stu-id="ce517-145">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="ce517-146">Переменные среды, использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ce517-146">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="ce517-147">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ce517-147">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ce517-148">Веб-приложения на основе шаблонов [dotnet new](/dotnet/core/tools/dotnet-new) ASP.NET Core вызывают <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> при создании узла.</span><span class="sxs-lookup"><span data-stu-id="ce517-148">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="ce517-149">`CreateDefaultBuilder` предоставляет конфигурацию по умолчанию для приложения в следующем порядке:</span><span class="sxs-lookup"><span data-stu-id="ce517-149">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="ce517-150">Приведенные ниже сведения относятся к приложениям, использующим [веб-узел](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="ce517-150">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="ce517-151">Подробные сведения о конфигурации по умолчанию при использовании [универсального узла](xref:fundamentals/host/generic-host) см. в [последней версии этой статьи](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="ce517-151">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="ce517-152">Существуют следующие способы предоставления конфигурации узла.</span><span class="sxs-lookup"><span data-stu-id="ce517-152">Host configuration is provided from:</span></span>
  * <span data-ttu-id="ce517-153">Переменные среды с префиксом `ASPNETCORE_` (например, `ASPNETCORE_ENVIRONMENT`), использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ce517-153">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="ce517-154">Префикс (`ASPNETCORE_`) исключается при загрузке пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-154">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="ce517-155">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ce517-155">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="ce517-156">Конфигурация приложения предоставляется из следующих ресурсов:</span><span class="sxs-lookup"><span data-stu-id="ce517-156">App configuration is provided from:</span></span>
  * <span data-ttu-id="ce517-157">Файл *appsettings.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ce517-157">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="ce517-158">Файл *appsettings.{Environment}.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ce517-158">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="ce517-159">[Менеджера секретов](xref:security/app-secrets), когда приложение выполняется в среде `Development` с использованием начальных сборок.</span><span class="sxs-lookup"><span data-stu-id="ce517-159">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="ce517-160">Переменные среды, использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ce517-160">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="ce517-161">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ce517-161">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

## <a name="security"></a><span data-ttu-id="ce517-162">Безопасность</span><span class="sxs-lookup"><span data-stu-id="ce517-162">Security</span></span>

<span data-ttu-id="ce517-163">Применяйте описанные ниже методики для защиты конфиденциальных данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-163">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="ce517-164">Никогда не храните пароли или другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации обычного текста.</span><span class="sxs-lookup"><span data-stu-id="ce517-164">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="ce517-165">Не используйте секреты рабочей среды в средах разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="ce517-165">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="ce517-166">Указывайте секреты вне проекта, чтобы их нельзя было случайно зафиксировать в репозитории с исходным кодом.</span><span class="sxs-lookup"><span data-stu-id="ce517-166">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="ce517-167">Дополнительные сведения см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="ce517-167">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="ce517-168"><xref:security/app-secrets> &ndash; содержит рекомендации по использованию переменных среды для хранения конфиденциальных данных.</span><span class="sxs-lookup"><span data-stu-id="ce517-168"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="ce517-169">Менеджер секретов использует поставщик конфигурации файла для хранения конфиденциальных данных пользователя в файле JSON в локальной системе.</span><span class="sxs-lookup"><span data-stu-id="ce517-169">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="ce517-170">Поставщик конфигурации файлов описан ниже в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="ce517-170">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="ce517-171">В [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) безопасно хранятся секреты приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ce517-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="ce517-172">Дополнительные сведения можно найти по адресу: <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="ce517-172">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="ce517-173">Иерархическая модель конфигурации</span><span class="sxs-lookup"><span data-stu-id="ce517-173">Hierarchical configuration data</span></span>

<span data-ttu-id="ce517-174">API конфигурации способен поддерживать иерархические данные конфигурации, выполняя преобразование в плоскую структуру иерархических данных с использованием разделителя в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-174">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="ce517-175">В следующем файле JSON существуют четыре ключа в структурированной иерархии двух разделов.</span><span class="sxs-lookup"><span data-stu-id="ce517-175">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="ce517-176">При считывании файла в конфигурацию для сохранения исходной иерархической структуры данных источника конфигурации создаются уникальные ключи.</span><span class="sxs-lookup"><span data-stu-id="ce517-176">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="ce517-177">Разделы и ключи преобразовываются в плоскую структуру с использованием двоеточия (`:`) для сохранения исходной структуры.</span><span class="sxs-lookup"><span data-stu-id="ce517-177">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="ce517-178">section0:key0</span><span class="sxs-lookup"><span data-stu-id="ce517-178">section0:key0</span></span>
* <span data-ttu-id="ce517-179">section0:key1</span><span class="sxs-lookup"><span data-stu-id="ce517-179">section0:key1</span></span>
* <span data-ttu-id="ce517-180">section1:key0</span><span class="sxs-lookup"><span data-stu-id="ce517-180">section1:key0</span></span>
* <span data-ttu-id="ce517-181">section1:key1</span><span class="sxs-lookup"><span data-stu-id="ce517-181">section1:key1</span></span>

<span data-ttu-id="ce517-182">Методы <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> и <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> доступны для изолирования разделов и дочерних элементов раздела в данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-182"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="ce517-183">Эти методы описаны далее в разделе [GetSection, GetChildren и Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="ce517-183">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="ce517-184">Соглашения</span><span class="sxs-lookup"><span data-stu-id="ce517-184">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="ce517-185">Источники и поставщики</span><span class="sxs-lookup"><span data-stu-id="ce517-185">Sources and providers</span></span>

<span data-ttu-id="ce517-186">При запуске приложения источники конфигурации считываются в порядке, в котором были указаны их поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-186">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="ce517-187">Поставщики конфигурации, которые реализуют обнаружение изменений, имеют возможность перезагрузить конфигурацию при изменении базовых параметров.</span><span class="sxs-lookup"><span data-stu-id="ce517-187">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="ce517-188">Так, поставщик файла конфигурации (подробнее о нем далее в этой статье) и [поставщик конфигурации Azure Key Vault](xref:security/key-vault-configuration) реализуют обнаружение изменений.</span><span class="sxs-lookup"><span data-stu-id="ce517-188">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="ce517-189">Объект <xref:Microsoft.Extensions.Configuration.IConfiguration> доступен в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) приложения.</span><span class="sxs-lookup"><span data-stu-id="ce517-189"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="ce517-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> можно внедрить в <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> Razor Pages, чтобы получить конфигурацию для класса:</span><span class="sxs-lookup"><span data-stu-id="ce517-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    // The _config local variable is used to obtain configuration 
    // throughout the class.
}
```

<span data-ttu-id="ce517-191">Поставщики конфигурации не могут использовать контейнер DI, так как он недоступен при настройке узла.</span><span class="sxs-lookup"><span data-stu-id="ce517-191">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="ce517-192">Клавиши</span><span class="sxs-lookup"><span data-stu-id="ce517-192">Keys</span></span>

<span data-ttu-id="ce517-193">В ключах конфигурации приняты следующие соглашения.</span><span class="sxs-lookup"><span data-stu-id="ce517-193">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="ce517-194">В ключах не учитывается регистр символов.</span><span class="sxs-lookup"><span data-stu-id="ce517-194">Keys are case-insensitive.</span></span> <span data-ttu-id="ce517-195">Например `ConnectionString` и `connectionstring` обрабатываются как эквивалентные ключи.</span><span class="sxs-lookup"><span data-stu-id="ce517-195">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="ce517-196">Если значение для одного и того же ключа установлено одним и тем же или разными поставщиками конфигурации, последним значением, установленным на ключе, является используемое значение.</span><span class="sxs-lookup"><span data-stu-id="ce517-196">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="ce517-197">Иерархические ключи</span><span class="sxs-lookup"><span data-stu-id="ce517-197">Hierarchical keys</span></span>
  * <span data-ttu-id="ce517-198">При взаимодействии с API конфигурации разделитель-двоеточие (`:`) поддерживается на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="ce517-198">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="ce517-199">В переменных среды разделитель-двоеточие может не работать на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="ce517-199">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="ce517-200">Двойной знак подчеркивания (`__`) поддерживается на всех платформах и автоматически преобразовывается в двоеточие.</span><span class="sxs-lookup"><span data-stu-id="ce517-200">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="ce517-201">В хранилище ключей Azure иерархические ключи используют `--` (два дефиса) в качестве разделителя.</span><span class="sxs-lookup"><span data-stu-id="ce517-201">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="ce517-202">Чтобы заменить дефисы двоеточием, при загрузке секретов в конфигурацию приложения необходимо указать код.</span><span class="sxs-lookup"><span data-stu-id="ce517-202">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="ce517-203"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-203">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="ce517-204">Привязка массива описана в разделе [Привязка массива к классу](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="ce517-204">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="ce517-205">Значения</span><span class="sxs-lookup"><span data-stu-id="ce517-205">Values</span></span>

<span data-ttu-id="ce517-206">В значениях конфигурации учитываются следующие соглашения.</span><span class="sxs-lookup"><span data-stu-id="ce517-206">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="ce517-207">Значения являются строками.</span><span class="sxs-lookup"><span data-stu-id="ce517-207">Values are strings.</span></span>
* <span data-ttu-id="ce517-208">Значение NULL не может храниться в конфигурации или быть привязанным к объектам.</span><span class="sxs-lookup"><span data-stu-id="ce517-208">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="ce517-209">Поставщики</span><span class="sxs-lookup"><span data-stu-id="ce517-209">Providers</span></span>

<span data-ttu-id="ce517-210">В следующей таблице показаны поставщики конфигурации, доступные для приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ce517-210">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="ce517-211">Поставщик</span><span class="sxs-lookup"><span data-stu-id="ce517-211">Provider</span></span> | <span data-ttu-id="ce517-212">Предоставляет конфигурацию из &hellip;</span><span class="sxs-lookup"><span data-stu-id="ce517-212">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="ce517-213">[Поставщик конфигурации хранилища ключей Azure](xref:security/key-vault-configuration) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="ce517-213">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="ce517-214">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="ce517-214">Azure Key Vault</span></span> |
| <span data-ttu-id="ce517-215">[Поставщик конфигурации приложений Azure](/azure/azure-app-configuration/quickstart-aspnet-core-app) (документация Azure)</span><span class="sxs-lookup"><span data-stu-id="ce517-215">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="ce517-216">Конфигурация приложений Azure</span><span class="sxs-lookup"><span data-stu-id="ce517-216">Azure App Configuration</span></span> |
| [<span data-ttu-id="ce517-217">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="ce517-217">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="ce517-218">Параметры командной строки</span><span class="sxs-lookup"><span data-stu-id="ce517-218">Command-line parameters</span></span> |
| [<span data-ttu-id="ce517-219">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="ce517-219">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="ce517-220">Источник пользователя</span><span class="sxs-lookup"><span data-stu-id="ce517-220">Custom source</span></span> |
| [<span data-ttu-id="ce517-221">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="ce517-221">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="ce517-222">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="ce517-222">Environment variables</span></span> |
| [<span data-ttu-id="ce517-223">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="ce517-223">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="ce517-224">Файлы (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="ce517-224">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="ce517-225">Поставщик конфигурации ключа для каждого файла</span><span class="sxs-lookup"><span data-stu-id="ce517-225">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="ce517-226">Справочные файлы</span><span class="sxs-lookup"><span data-stu-id="ce517-226">Directory files</span></span> |
| [<span data-ttu-id="ce517-227">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="ce517-227">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="ce517-228">Коллекции оперативной памяти</span><span class="sxs-lookup"><span data-stu-id="ce517-228">In-memory collections</span></span> |
| <span data-ttu-id="ce517-229">[Секреты пользователей (Менеджер секретов)](xref:security/app-secrets) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="ce517-229">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="ce517-230">Файл в каталоге профиля пользователя</span><span class="sxs-lookup"><span data-stu-id="ce517-230">File in the user profile directory</span></span> |

<span data-ttu-id="ce517-231">Источники конфигурации считываются в том порядке, в котором при запуске указываются их поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-231">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="ce517-232">Поставщики конфигурации в этом разделе описаны в алфавитном порядке, а не в необходимом вам порядке в коде.</span><span class="sxs-lookup"><span data-stu-id="ce517-232">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="ce517-233">Порядок поставщиков конфигурации в коде соответствует приоритетам ваших основных источников конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-233">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="ce517-234">Типичная последовательность поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-234">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="ce517-235">Файлы (*appsettings.json*, *appsettings.{Environment}.json*, где `{Environment}` — это текущая среда размещения приложения)</span><span class="sxs-lookup"><span data-stu-id="ce517-235">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. <span data-ttu-id="ce517-236">[Azure Key Vault](xref:security/key-vault-configuration);</span><span class="sxs-lookup"><span data-stu-id="ce517-236">[Azure Key Vault](xref:security/key-vault-configuration)</span></span>
1. <span data-ttu-id="ce517-237">[Секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки)</span><span class="sxs-lookup"><span data-stu-id="ce517-237">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="ce517-238">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="ce517-238">Environment variables</span></span>
1. <span data-ttu-id="ce517-239">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="ce517-239">Command-line arguments</span></span>

<span data-ttu-id="ce517-240">Общепринятой практикой является размещение поставщика конфигурации командной строки последним в ряду поставщиков, чтобы аргументы командной строки могли переопределять конфигурацию, установленную другими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="ce517-240">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="ce517-241">Предыдущая последовательность поставщиков используется при инициализации нового построителя узла с помощью `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ce517-241">The preceding sequence of providers is used when you initialize a new host builder with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="ce517-242">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="ce517-242">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="ce517-243">Настройка построителя узла с помощью ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="ce517-243">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="ce517-244">Чтобы настроить построитель узла, вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> и укажите конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="ce517-244">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="ce517-245">`ConfigureHostConfiguration` используется для инициализации <xref:Microsoft.Extensions.Hosting.IHostEnvironment> для дальнейшего использования в процессе сборки.</span><span class="sxs-lookup"><span data-stu-id="ce517-245">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="ce517-246">Метод `ConfigureHostConfiguration` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="ce517-246">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

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

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="ce517-247">Настройка построителя узла с помощью UseConfiguration</span><span class="sxs-lookup"><span data-stu-id="ce517-247">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="ce517-248">Чтобы настроить построитель узла, вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> в построителе узла с конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="ce517-248">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

## <a name="configureappconfiguration"></a><span data-ttu-id="ce517-249">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="ce517-249">ConfigureAppConfiguration</span></span>

<span data-ttu-id="ce517-250">Вызовите `ConfigureAppConfiguration` при сборке узла, чтобы указать поставщики конфигурации приложения в дополнение к тем, которые автоматически добавлены `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ce517-250">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="ce517-251">Переопределение предыдущей конфигурации с помощью аргументов командной строки</span><span class="sxs-lookup"><span data-stu-id="ce517-251">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="ce517-252">Чтобы предоставить конфигурацию приложения, которую можно переопределить с помощью аргументов командной строки, вызовите метод `AddCommandLine` последним:</span><span class="sxs-lookup"><span data-stu-id="ce517-252">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="ce517-253">Удаление поставщиков, добавленных CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="ce517-253">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="ce517-254">Чтобы удалить поставщиков, добавленных `CreateDefaultBuilder`, сначала вызовите [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) в [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources):</span><span class="sxs-lookup"><span data-stu-id="ce517-254">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="ce517-255">Использование конфигурации во время запуска приложения</span><span class="sxs-lookup"><span data-stu-id="ce517-255">Consume configuration during app startup</span></span>

<span data-ttu-id="ce517-256">Конфигурация, предоставленная приложению в `ConfigureAppConfiguration`, доступна во время запуска приложения, включая `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ce517-256">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="ce517-257">Дополнительные сведения см. в разделе [Доступ к конфигурации во время запуска](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="ce517-257">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="ce517-258">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="ce517-258">Command-line Configuration Provider</span></span>

<span data-ttu-id="ce517-259"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> загружает конфигурацию из пары "ключ — значение" аргумента командной строки в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="ce517-259">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="ce517-260">Чтобы активировать конфигурацию командной строки, вызывается метод расширения <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> для экземпляра <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="ce517-260">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="ce517-261">`AddCommandLine` вызывается автоматически при вызове `CreateDefaultBuilder(string [])`.</span><span class="sxs-lookup"><span data-stu-id="ce517-261">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="ce517-262">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="ce517-262">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="ce517-263">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="ce517-263">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="ce517-264">дополнительную конфигурацию из файлов *appsettings.json* и *appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="ce517-264">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="ce517-265">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="ce517-265">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="ce517-266">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="ce517-266">Environment variables.</span></span>

<span data-ttu-id="ce517-267">`CreateDefaultBuilder` добавляет последним поставщика конфигурации командной строки.</span><span class="sxs-lookup"><span data-stu-id="ce517-267">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="ce517-268">Аргументы командной строки передаются в набор конфигурации переопределения среды выполнения, установленной другими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="ce517-268">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="ce517-269">`CreateDefaultBuilder` действует, когда создается узел.</span><span class="sxs-lookup"><span data-stu-id="ce517-269">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="ce517-270">Поэтому конфигурация командной строки, активированная с помощью `CreateDefaultBuilder`, может повлиять на настройку узла.</span><span class="sxs-lookup"><span data-stu-id="ce517-270">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="ce517-271">Для приложений на основе шаблонов ASP.NET Core метод `AddCommandLine` уже был вызван методом `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ce517-271">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="ce517-272">Чтобы добавить дополнительные поставщики конфигурации и обеспечить возможность переопределения конфигурации от этих поставщиков с помощью аргументов командной строки, вызовите дополнительные поставщики приложения в `ConfigureAppConfiguration` и вызовите `AddCommandLine` в последнюю очередь.</span><span class="sxs-lookup"><span data-stu-id="ce517-272">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="ce517-273">**Пример**</span><span class="sxs-lookup"><span data-stu-id="ce517-273">**Example**</span></span>

<span data-ttu-id="ce517-274">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="ce517-274">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="ce517-275">Откройте командную строку в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="ce517-275">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="ce517-276">Поставьте аргумент командной строки в команду `dotnet run`: `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="ce517-276">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="ce517-277">После запуска приложения откройте браузер в приложении по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="ce517-277">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="ce517-278">Обратите внимание, что вывод содержит пару "ключ — значение" для аргумента командной строки конфигурации, предоставленного для `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="ce517-278">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="ce517-279">Аргументы</span><span class="sxs-lookup"><span data-stu-id="ce517-279">Arguments</span></span>

<span data-ttu-id="ce517-280">Значение должно соответствовать знаку равенства (`=`), или ключ должен иметь префикс (`--` или `/`), когда значение следует за пробелом.</span><span class="sxs-lookup"><span data-stu-id="ce517-280">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="ce517-281">Значение не требуется, если используется знак равенства (например, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="ce517-281">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="ce517-282">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="ce517-282">Key prefix</span></span>               | <span data-ttu-id="ce517-283">Пример</span><span class="sxs-lookup"><span data-stu-id="ce517-283">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="ce517-284">Без префикса</span><span class="sxs-lookup"><span data-stu-id="ce517-284">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="ce517-285">Два дефиса (`--`)</span><span class="sxs-lookup"><span data-stu-id="ce517-285">Two dashes (`--`)</span></span>        | <span data-ttu-id="ce517-286">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="ce517-286">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="ce517-287">Прямая косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="ce517-287">Forward slash (`/`)</span></span>      | <span data-ttu-id="ce517-288">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="ce517-288">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="ce517-289">В рамках одной и той же команды не смешивайте пары "ключ — значение" аргумента командной строки, которые используют знак равенства, с парами "ключ — значение", которые используют пробел.</span><span class="sxs-lookup"><span data-stu-id="ce517-289">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="ce517-290">Примеры команд.</span><span class="sxs-lookup"><span data-stu-id="ce517-290">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="ce517-291">Сопоставления переключений</span><span class="sxs-lookup"><span data-stu-id="ce517-291">Switch mappings</span></span>

<span data-ttu-id="ce517-292">Сопоставление параметров позволяет указать логику замены имен ключей.</span><span class="sxs-lookup"><span data-stu-id="ce517-292">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="ce517-293">Когда вручную создается конфигурация с помощью <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, методу <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> можно предоставить словарь сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="ce517-293">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="ce517-294">В словаре сопоставлений переключений выполняется поиск ключа, который совпадает с ключом, предоставляемым аргументом командной строки.</span><span class="sxs-lookup"><span data-stu-id="ce517-294">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="ce517-295">Если ключ в командной строке находится в словаре, значение словаря (замена ключа) передается обратно, чтобы установить пару "ключ — значение" в конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="ce517-295">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="ce517-296">Сопоставление переключений необходимо для любого ключа командной строки с префиксом из одного дефиса (`-`).</span><span class="sxs-lookup"><span data-stu-id="ce517-296">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="ce517-297">Правила ключей из словаря сопоставления переключений:</span><span class="sxs-lookup"><span data-stu-id="ce517-297">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="ce517-298">Переключения должны начинаться с дефиса (`-`) или двойного дефиса (`--`).</span><span class="sxs-lookup"><span data-stu-id="ce517-298">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="ce517-299">Словарь сопоставлений переключений не должен содержать повторяющиеся ключи.</span><span class="sxs-lookup"><span data-stu-id="ce517-299">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="ce517-300">Создайте словарь сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="ce517-300">Create a switch mappings dictionary.</span></span> <span data-ttu-id="ce517-301">В следующем примере создаются два сопоставления переключений:</span><span class="sxs-lookup"><span data-stu-id="ce517-301">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="ce517-302">При сборке узла вызовите `AddCommandLine` со словарем сопоставлений переключений:</span><span class="sxs-lookup"><span data-stu-id="ce517-302">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="ce517-303">Для приложений, использующих сопоставления переключений, в вызове `CreateDefaultBuilder` аргументы передаваться не должны.</span><span class="sxs-lookup"><span data-stu-id="ce517-303">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="ce517-304">Вызов команды `AddCommandLine` метода `CreateDefaultBuilder` не включает сопоставленные переключения, и нет возможности передать словарь сопоставления переключений в `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ce517-304">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="ce517-305">Чтобы решить эту проблему, нужно не передавать аргументы команде `CreateDefaultBuilder`, а позволить методу `AddCommandLine` метода `ConfigurationBuilder` обрабатывать как аргументы, так и словарь сопоставления параметров.</span><span class="sxs-lookup"><span data-stu-id="ce517-305">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="ce517-306">Созданный словарь сопоставлений переключений содержит данные, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="ce517-306">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="ce517-307">Ключ</span><span class="sxs-lookup"><span data-stu-id="ce517-307">Key</span></span>       | <span data-ttu-id="ce517-308">Значение</span><span class="sxs-lookup"><span data-stu-id="ce517-308">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="ce517-309">Если ключи сопоставления переключений используются при запуске приложения, конфигурация принимает значение конфигурации в ключе, предоставленном в словаре.</span><span class="sxs-lookup"><span data-stu-id="ce517-309">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="ce517-310">После выполнения предыдущей команды конфигурация содержит значения, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="ce517-310">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="ce517-311">Ключ</span><span class="sxs-lookup"><span data-stu-id="ce517-311">Key</span></span>               | <span data-ttu-id="ce517-312">Значение</span><span class="sxs-lookup"><span data-stu-id="ce517-312">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="ce517-313">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="ce517-313">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="ce517-314"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> загружает конфигурацию из пары "ключ — значение" переменной среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="ce517-314">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="ce517-315">Чтобы активировать конфигурацию переменных среды, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="ce517-315">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="ce517-316">[Служба приложений Azure](https://azure.microsoft.com/services/app-service/) позволяет задать переменные среды на портале Azure, который может переопределить конфигурацию приложения, используя поставщик конфигурации переменных среды.</span><span class="sxs-lookup"><span data-stu-id="ce517-316">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="ce517-317">Дополнительные сведения см. в руководстве по [переопределению конфигурации приложения Azure с помощью портала Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="ce517-317">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ce517-318">`AddEnvironmentVariables` используется для загрузки переменных среды, имеющих префикс `DOTNET_`, для [конфигурации узла](#host-versus-app-configuration) при инициализации нового построителя узла с [универсальным узлом](xref:fundamentals/host/generic-host) и вызове `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ce517-318">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="ce517-319">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="ce517-319">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ce517-320">`AddEnvironmentVariables` используется для загрузки переменных среды, имеющих префикс `ASPNETCORE_`, для [конфигурации узла](#host-versus-app-configuration) при инициализации нового построителя узла с [веб-узлом](xref:fundamentals/host/web-host) и вызове `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ce517-320">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="ce517-321">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="ce517-321">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

<span data-ttu-id="ce517-322">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="ce517-322">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="ce517-323">конфигурация приложения на основе переменных среды без префикса путем вызова `AddEnvironmentVariables` без префикса;</span><span class="sxs-lookup"><span data-stu-id="ce517-323">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="ce517-324">дополнительную конфигурацию из файлов *appsettings.json* и *appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="ce517-324">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="ce517-325">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="ce517-325">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="ce517-326">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="ce517-326">Command-line arguments.</span></span>

<span data-ttu-id="ce517-327">Поставщик конфигурации переменных среды вызывается после выполнения настройки с помощью секретов пользователя и файлов *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="ce517-327">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="ce517-328">Вызов поставщика в этой позиции разрешает чтение переменных среды выполнения, чтобы переопределить конфигурацию, заданную секретом пользователя и файлом *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="ce517-328">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="ce517-329">Если вам нужно добавить конфигурацию приложения из дополнительных переменных среды, вызовите дополнительные поставщики приложения в `ConfigureAppConfiguration`, а затем вызовите `AddEnvironmentVariables` с префиксом.</span><span class="sxs-lookup"><span data-stu-id="ce517-329">If you need to provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call additional providers here as needed.
    // Call AddEnvironmentVariables last if you need to allow
    // environment variables to override values from other 
    // providers.
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
}
```

<span data-ttu-id="ce517-330">**Пример**</span><span class="sxs-lookup"><span data-stu-id="ce517-330">**Example**</span></span>

<span data-ttu-id="ce517-331">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="ce517-331">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="ce517-332">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="ce517-332">Run the sample app.</span></span> <span data-ttu-id="ce517-333">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="ce517-333">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="ce517-334">Обратите внимание, что выходные данные содержат пару "ключ — значение" для переменной среды `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="ce517-334">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="ce517-335">Значение отражает среду, в которой выполняется приложение, обычно при локальном запуске это `Development`.</span><span class="sxs-lookup"><span data-stu-id="ce517-335">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="ce517-336">Чтобы список переменных среды, отображаемый приложением, был коротким, приложение фильтрует переменные среды.</span><span class="sxs-lookup"><span data-stu-id="ce517-336">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="ce517-337">См. пример файла *Pages/Index.cshtml.cs* приложения.</span><span class="sxs-lookup"><span data-stu-id="ce517-337">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="ce517-338">Если хотите просмотреть все переменные среды, доступные для приложения, измените значение `FilteredConfiguration` в *Pages/Index.cshtml.cs* на следующее:</span><span class="sxs-lookup"><span data-stu-id="ce517-338">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="ce517-339">Префиксы</span><span class="sxs-lookup"><span data-stu-id="ce517-339">Prefixes</span></span>

<span data-ttu-id="ce517-340">Переменные среды, которые загружаются в конфигурацию приложения, фильтруются при добавлении префикса к методу `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="ce517-340">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="ce517-341">Например, чтобы отфильтровать переменные среды по префиксу `CUSTOM_`, введите префикс поставщику конфигурации:</span><span class="sxs-lookup"><span data-stu-id="ce517-341">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="ce517-342">Префикс отделяется при создании пары конфигурации "ключ — значение".</span><span class="sxs-lookup"><span data-stu-id="ce517-342">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="ce517-343">При создании построителя узла конфигурация узла предоставляется переменными среды.</span><span class="sxs-lookup"><span data-stu-id="ce517-343">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="ce517-344">Дополнительные сведения о префиксе, используемом для этих переменных среды, см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="ce517-344">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="ce517-345">**Префиксы строк подключения**</span><span class="sxs-lookup"><span data-stu-id="ce517-345">**Connection string prefixes**</span></span>

<span data-ttu-id="ce517-346">API конфигурации имеет специальные правила обработки для четырех строк подключения переменных среды, связанных с настройкой строк подключения Azure для среды приложения.</span><span class="sxs-lookup"><span data-stu-id="ce517-346">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="ce517-347">Если префикс не указан в `AddEnvironmentVariables`, переменные среды с префиксами, указанными в таблице, загружаются в приложение.</span><span class="sxs-lookup"><span data-stu-id="ce517-347">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="ce517-348">Префикс строки подключения</span><span class="sxs-lookup"><span data-stu-id="ce517-348">Connection string prefix</span></span> | <span data-ttu-id="ce517-349">Поставщик</span><span class="sxs-lookup"><span data-stu-id="ce517-349">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="ce517-350">Поставщик пользователя</span><span class="sxs-lookup"><span data-stu-id="ce517-350">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="ce517-351">MySQL</span><span class="sxs-lookup"><span data-stu-id="ce517-351">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="ce517-352">База данных SQL Azure</span><span class="sxs-lookup"><span data-stu-id="ce517-352">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="ce517-353">SQL Server</span><span class="sxs-lookup"><span data-stu-id="ce517-353">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="ce517-354">Когда переменная среды обнаруживается и загружается в конфигурацию с одним из четырех префиксов, приведенных в таблице, происходит следующее.</span><span class="sxs-lookup"><span data-stu-id="ce517-354">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="ce517-355">Ключ конфигурации создается путем удаления префикса переменных среды и добавления ключа раздела конфигурации (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="ce517-355">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="ce517-356">Создается новая пара "ключ — значение" конфигурации, которая представляет поставщика подключения базы данных (за исключением `CUSTOMCONNSTR_`, который не имеет указанного поставщика).</span><span class="sxs-lookup"><span data-stu-id="ce517-356">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="ce517-357">Ключ переменной среды</span><span class="sxs-lookup"><span data-stu-id="ce517-357">Environment variable key</span></span> | <span data-ttu-id="ce517-358">Преобразованный ключ конфигурации</span><span class="sxs-lookup"><span data-stu-id="ce517-358">Converted configuration key</span></span> | <span data-ttu-id="ce517-359">Запись конфигурации поставщика</span><span class="sxs-lookup"><span data-stu-id="ce517-359">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="ce517-360">Запись конфигурации не создана.</span><span class="sxs-lookup"><span data-stu-id="ce517-360">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="ce517-361">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="ce517-361">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="ce517-362">Значение: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="ce517-362">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="ce517-363">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="ce517-363">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="ce517-364">Значение: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="ce517-364">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="ce517-365">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="ce517-365">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="ce517-366">Значение: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="ce517-366">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="ce517-367">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="ce517-367">File Configuration Provider</span></span>

<span data-ttu-id="ce517-368"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> является базовым классом для загрузки конфигурации из файловой системы.</span><span class="sxs-lookup"><span data-stu-id="ce517-368"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="ce517-369">Следующие поставщики конфигурации предназначены для определенных типов файлов:</span><span class="sxs-lookup"><span data-stu-id="ce517-369">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="ce517-370">Поставщик конфигурации INI</span><span class="sxs-lookup"><span data-stu-id="ce517-370">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="ce517-371">Поставщик конфигурации JSON</span><span class="sxs-lookup"><span data-stu-id="ce517-371">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="ce517-372">Поставщик конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="ce517-372">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="ce517-373">Поставщик конфигурации INI</span><span class="sxs-lookup"><span data-stu-id="ce517-373">INI Configuration Provider</span></span>

<span data-ttu-id="ce517-374"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> загружает конфигурацию из пары "ключ — значение" INI-файла во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="ce517-374">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="ce517-375">Чтобы активировать конфигурацию INI-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="ce517-375">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="ce517-376">Двоеточие можно использовать как разделитель раздела в конфигурации файла INI.</span><span class="sxs-lookup"><span data-stu-id="ce517-376">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="ce517-377">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="ce517-377">Overloads permit specifying:</span></span>

* <span data-ttu-id="ce517-378">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="ce517-378">Whether the file is optional.</span></span>
* <span data-ttu-id="ce517-379">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="ce517-379">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="ce517-380"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="ce517-380">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="ce517-381">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="ce517-381">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="ce517-382">Общий пример конфигурации INI-файла.</span><span class="sxs-lookup"><span data-stu-id="ce517-382">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="ce517-383">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="ce517-383">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="ce517-384">section0:key0</span><span class="sxs-lookup"><span data-stu-id="ce517-384">section0:key0</span></span>
* <span data-ttu-id="ce517-385">section0:key1</span><span class="sxs-lookup"><span data-stu-id="ce517-385">section0:key1</span></span>
* <span data-ttu-id="ce517-386">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="ce517-386">section1:subsection:key</span></span>
* <span data-ttu-id="ce517-387">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="ce517-387">section2:subsection0:key</span></span>
* <span data-ttu-id="ce517-388">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="ce517-388">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="ce517-389">Поставщик конфигурации JSON</span><span class="sxs-lookup"><span data-stu-id="ce517-389">JSON Configuration Provider</span></span>

<span data-ttu-id="ce517-390"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> загружает конфигурацию из пары "ключ — значение" JSON-файла в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="ce517-390">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="ce517-391">Чтобы активировать конфигурацию JSON-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="ce517-391">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="ce517-392">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="ce517-392">Overloads permit specifying:</span></span>

* <span data-ttu-id="ce517-393">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="ce517-393">Whether the file is optional.</span></span>
* <span data-ttu-id="ce517-394">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="ce517-394">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="ce517-395"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="ce517-395">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="ce517-396">`AddJsonFile` автоматически вызывается дважды при инициализации нового построителя узла с `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ce517-396">`AddJsonFile` is automatically called twice when you initialize a new host builder with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="ce517-397">Метод вызывается для загрузки конфигурации из:</span><span class="sxs-lookup"><span data-stu-id="ce517-397">The method is called to load configuration from:</span></span>

* <span data-ttu-id="ce517-398">*appsettings.json* &ndash; первым читается этот файл.</span><span class="sxs-lookup"><span data-stu-id="ce517-398">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="ce517-399">Версия файла среды может переопределить значения, предоставленные *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="ce517-399">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="ce517-400">*appsettings.{Environment}.json* — версия среды файла загружается на основе [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="ce517-400">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="ce517-401">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="ce517-401">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="ce517-402">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="ce517-402">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="ce517-403">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="ce517-403">Environment variables.</span></span>
* <span data-ttu-id="ce517-404">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="ce517-404">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="ce517-405">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="ce517-405">Command-line arguments.</span></span>

<span data-ttu-id="ce517-406">Сначала устанавливается поставщик конфигурации JSON-файлов.</span><span class="sxs-lookup"><span data-stu-id="ce517-406">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="ce517-407">Таким образом, секреты пользователя, переменные среды и аргументы командной строки переопределят конфигурацию, заданную файлами *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="ce517-407">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="ce517-408">Вызовите `ConfigureAppConfiguration` при сборке узла, чтобы указать конфигурацию приложения для файлов, отличных от *appsettings.json* и *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="ce517-408">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="ce517-409">**Пример**</span><span class="sxs-lookup"><span data-stu-id="ce517-409">**Example**</span></span>

<span data-ttu-id="ce517-410">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает два вызова `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="ce517-410">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="ce517-411">Конфигурация загружена из *appsettings.json* и *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="ce517-411">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="ce517-412">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="ce517-412">Run the sample app.</span></span> <span data-ttu-id="ce517-413">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="ce517-413">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="ce517-414">Обратите внимание, что выходные данные содержат пары "ключ — значение" для конфигурации, представленной в таблице в зависимости от среды.</span><span class="sxs-lookup"><span data-stu-id="ce517-414">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="ce517-415">Ключи конфигурации ведения журнала используют двоеточие (`:`) как иерархический разделитель.</span><span class="sxs-lookup"><span data-stu-id="ce517-415">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="ce517-416">Ключ</span><span class="sxs-lookup"><span data-stu-id="ce517-416">Key</span></span>                        | <span data-ttu-id="ce517-417">Значение разработки</span><span class="sxs-lookup"><span data-stu-id="ce517-417">Development Value</span></span> | <span data-ttu-id="ce517-418">Рабочее значение</span><span class="sxs-lookup"><span data-stu-id="ce517-418">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="ce517-419">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="ce517-419">Logging:LogLevel:System</span></span>    | <span data-ttu-id="ce517-420">Сведения</span><span class="sxs-lookup"><span data-stu-id="ce517-420">Information</span></span>       | <span data-ttu-id="ce517-421">Сведения</span><span class="sxs-lookup"><span data-stu-id="ce517-421">Information</span></span>      |
| <span data-ttu-id="ce517-422">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="ce517-422">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="ce517-423">Сведения</span><span class="sxs-lookup"><span data-stu-id="ce517-423">Information</span></span>       | <span data-ttu-id="ce517-424">Сведения</span><span class="sxs-lookup"><span data-stu-id="ce517-424">Information</span></span>      |
| <span data-ttu-id="ce517-425">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="ce517-425">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="ce517-426">Отладка</span><span class="sxs-lookup"><span data-stu-id="ce517-426">Debug</span></span>             | <span data-ttu-id="ce517-427">Ошибка</span><span class="sxs-lookup"><span data-stu-id="ce517-427">Error</span></span>            |
| <span data-ttu-id="ce517-428">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="ce517-428">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="ce517-429">Поставщик конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="ce517-429">XML Configuration Provider</span></span>

<span data-ttu-id="ce517-430"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> загружает конфигурацию из пары "ключ — значение" XML-файла в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="ce517-430">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="ce517-431">Чтобы активировать конфигурацию XML-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="ce517-431">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="ce517-432">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="ce517-432">Overloads permit specifying:</span></span>

* <span data-ttu-id="ce517-433">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="ce517-433">Whether the file is optional.</span></span>
* <span data-ttu-id="ce517-434">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="ce517-434">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="ce517-435"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="ce517-435">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="ce517-436">Корневой узел файла конфигурации не учитывается при создании пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-436">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="ce517-437">Не указывайте в файле Document Type Definition (определение типа документа, DTD) или пространство имен.</span><span class="sxs-lookup"><span data-stu-id="ce517-437">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="ce517-438">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="ce517-438">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="ce517-439">XML-файлы конфигурации могут использовать имена отдельных элементов для повторяющихся разделов.</span><span class="sxs-lookup"><span data-stu-id="ce517-439">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="ce517-440">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="ce517-440">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="ce517-441">section0:key0</span><span class="sxs-lookup"><span data-stu-id="ce517-441">section0:key0</span></span>
* <span data-ttu-id="ce517-442">section0:key1</span><span class="sxs-lookup"><span data-stu-id="ce517-442">section0:key1</span></span>
* <span data-ttu-id="ce517-443">section1:key0</span><span class="sxs-lookup"><span data-stu-id="ce517-443">section1:key0</span></span>
* <span data-ttu-id="ce517-444">section1:key1</span><span class="sxs-lookup"><span data-stu-id="ce517-444">section1:key1</span></span>

<span data-ttu-id="ce517-445">Повторяющиеся элементы, использующие то же имя элемента, работают, если атрибут `name` используется для различения элементов.</span><span class="sxs-lookup"><span data-stu-id="ce517-445">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="ce517-446">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="ce517-446">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="ce517-447">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="ce517-447">section:section0:key:key0</span></span>
* <span data-ttu-id="ce517-448">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="ce517-448">section:section0:key:key1</span></span>
* <span data-ttu-id="ce517-449">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="ce517-449">section:section1:key:key0</span></span>
* <span data-ttu-id="ce517-450">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="ce517-450">section:section1:key:key1</span></span>

<span data-ttu-id="ce517-451">Атрибуты можно использовать для предоставления значений.</span><span class="sxs-lookup"><span data-stu-id="ce517-451">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="ce517-452">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="ce517-452">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="ce517-453">key:attribute</span><span class="sxs-lookup"><span data-stu-id="ce517-453">key:attribute</span></span>
* <span data-ttu-id="ce517-454">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="ce517-454">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="ce517-455">Поставщик конфигурации ключа для каждого файла</span><span class="sxs-lookup"><span data-stu-id="ce517-455">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="ce517-456"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> использует файлы каталога как пары "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-456">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="ce517-457">Ключ является именем файла.</span><span class="sxs-lookup"><span data-stu-id="ce517-457">The key is the file name.</span></span> <span data-ttu-id="ce517-458">Значение содержит содержимое файла.</span><span class="sxs-lookup"><span data-stu-id="ce517-458">The value contains the file's contents.</span></span> <span data-ttu-id="ce517-459">Поставщик конфигурации ключа для каждого файла используется в сценариях размещения Docker.</span><span class="sxs-lookup"><span data-stu-id="ce517-459">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="ce517-460">Чтобы активировать конфигурацию ключа для каждого файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="ce517-460">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="ce517-461">Значение параметра `directoryPath` должно быть абсолютным путем к файлам.</span><span class="sxs-lookup"><span data-stu-id="ce517-461">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="ce517-462">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="ce517-462">Overloads permit specifying:</span></span>

* <span data-ttu-id="ce517-463">`Action<KeyPerFileConfigurationSource>` — делегат, который настраивает источник.</span><span class="sxs-lookup"><span data-stu-id="ce517-463">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="ce517-464">Обязательно ли указывать каталог и путь к каталогу.</span><span class="sxs-lookup"><span data-stu-id="ce517-464">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="ce517-465">Двойное подчеркивание (`__`) используется в качестве разделителя ключа конфигурации в именах файлов.</span><span class="sxs-lookup"><span data-stu-id="ce517-465">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="ce517-466">Например, в имени файла `Logging__LogLevel__System` создается ключ конфигурации `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="ce517-466">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="ce517-467">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="ce517-467">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="ce517-468">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="ce517-468">Memory Configuration Provider</span></span>

<span data-ttu-id="ce517-469"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> использует коллекцию памяти в качестве пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-469">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="ce517-470">Чтобы активировать конфигурацию коллекции в памяти, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="ce517-470">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="ce517-471">Поставщик конфигурации может инициализироваться значением `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="ce517-471">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="ce517-472">Чтобы указать конфигурацию приложения, при сборке узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="ce517-472">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="ce517-473">В следующем примере создается словарь конфигурации:</span><span class="sxs-lookup"><span data-stu-id="ce517-473">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="ce517-474">Словарь используется с вызовом `AddInMemoryCollection` для предоставления конфигурации:</span><span class="sxs-lookup"><span data-stu-id="ce517-474">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="ce517-475">GetValue</span><span class="sxs-lookup"><span data-stu-id="ce517-475">GetValue</span></span>

<span data-ttu-id="ce517-476">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) извлекает одно значение из конфигурации с указанным ключом и преобразует его в указанный тип, не являющийся коллекцией.</span><span class="sxs-lookup"><span data-stu-id="ce517-476">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="ce517-477">Перегрузка принимает значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="ce517-477">An overload accepts a default value.</span></span>

<span data-ttu-id="ce517-478">В следующем примере происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="ce517-478">The following example:</span></span>

* <span data-ttu-id="ce517-479">Из конфигурации извлекается строковое значение с ключом `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="ce517-479">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="ce517-480">Если `NumberKey` отсутствует в ключах конфигурации, используется значение по умолчанию `99`.</span><span class="sxs-lookup"><span data-stu-id="ce517-480">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="ce517-481">Значение получает тип `int`.</span><span class="sxs-lookup"><span data-stu-id="ce517-481">Types the value as an `int`.</span></span>
* <span data-ttu-id="ce517-482">Значение сохраняется в свойстве `NumberConfig` для использования на странице.</span><span class="sxs-lookup"><span data-stu-id="ce517-482">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="ce517-483">GetSection, GetChildren и Exists</span><span class="sxs-lookup"><span data-stu-id="ce517-483">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="ce517-484">В следующих примерах рассмотрим файл JSON.</span><span class="sxs-lookup"><span data-stu-id="ce517-484">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="ce517-485">Четыре ключа находятся в двух разделах, один из которых содержит пару из подразделов.</span><span class="sxs-lookup"><span data-stu-id="ce517-485">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="ce517-486">Когда файл считывается в конфигурацию, для сохранения значений конфигурации создаются следующие уникальные иерархические ключи.</span><span class="sxs-lookup"><span data-stu-id="ce517-486">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="ce517-487">section0:key0</span><span class="sxs-lookup"><span data-stu-id="ce517-487">section0:key0</span></span>
* <span data-ttu-id="ce517-488">section0:key1</span><span class="sxs-lookup"><span data-stu-id="ce517-488">section0:key1</span></span>
* <span data-ttu-id="ce517-489">section1:key0</span><span class="sxs-lookup"><span data-stu-id="ce517-489">section1:key0</span></span>
* <span data-ttu-id="ce517-490">section1:key1</span><span class="sxs-lookup"><span data-stu-id="ce517-490">section1:key1</span></span>
* <span data-ttu-id="ce517-491">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="ce517-491">section2:subsection0:key0</span></span>
* <span data-ttu-id="ce517-492">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="ce517-492">section2:subsection0:key1</span></span>
* <span data-ttu-id="ce517-493">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="ce517-493">section2:subsection1:key0</span></span>
* <span data-ttu-id="ce517-494">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="ce517-494">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="ce517-495">GetSection</span><span class="sxs-lookup"><span data-stu-id="ce517-495">GetSection</span></span>

<span data-ttu-id="ce517-496">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) извлекает подраздел конфигурации с указанным ключом подраздела.</span><span class="sxs-lookup"><span data-stu-id="ce517-496">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="ce517-497">Чтобы вернуть <xref:Microsoft.Extensions.Configuration.IConfigurationSection>, содержащий только пары "ключ — значение" в `section1`, вызовите `GetSection` и укажите имя раздела.</span><span class="sxs-lookup"><span data-stu-id="ce517-497">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="ce517-498">`configSection` не содержит значение, только ключ и путь.</span><span class="sxs-lookup"><span data-stu-id="ce517-498">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="ce517-499">Аналогично, чтобы получить значения для ключей в `section2:subsection0`, вызовите `GetSection` и укажите путь к разделу.</span><span class="sxs-lookup"><span data-stu-id="ce517-499">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="ce517-500">Значение `GetSection` никогда не возвращает значение `null`.</span><span class="sxs-lookup"><span data-stu-id="ce517-500">`GetSection` never returns `null`.</span></span> <span data-ttu-id="ce517-501">Если соответствующий раздел не найден, возвращается пустой параметр `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="ce517-501">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="ce517-502">Когда `GetSection` возвращает соответствующий раздел, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> не заполняется.</span><span class="sxs-lookup"><span data-stu-id="ce517-502">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="ce517-503"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> и <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> возвращаются, если раздел существует.</span><span class="sxs-lookup"><span data-stu-id="ce517-503">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="ce517-504">GetChildren</span><span class="sxs-lookup"><span data-stu-id="ce517-504">GetChildren</span></span>

<span data-ttu-id="ce517-505">Вызов [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) в `section2` получает значение `IEnumerable<IConfigurationSection>`, которое включает:</span><span class="sxs-lookup"><span data-stu-id="ce517-505">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="ce517-506">Exists</span><span class="sxs-lookup"><span data-stu-id="ce517-506">Exists</span></span>

<span data-ttu-id="ce517-507">Используйте [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*), чтобы определить, существует ли раздел конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-507">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="ce517-508">Учитывая данные примера, `sectionExists` является `false`, потому что в данных конфигурации нет `section2:subsection2`.</span><span class="sxs-lookup"><span data-stu-id="ce517-508">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="ce517-509">Привязка к классу</span><span class="sxs-lookup"><span data-stu-id="ce517-509">Bind to a class</span></span>

<span data-ttu-id="ce517-510">Конфигурация может быть привязана к классам, которые представляют группы связанных параметров, используя шаблон *вариантов*.</span><span class="sxs-lookup"><span data-stu-id="ce517-510">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="ce517-511">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="ce517-511">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="ce517-512">Значения конфигурации возвращаются как строки, но вызов <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> позволяет построить объекты [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="ce517-512">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="ce517-513">Пример приложения содержит модель `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="ce517-513">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="ce517-514">Раздел `starship` файла *starship.json* создает конфигурацию, когда образец приложения использует поставщик конфигурации JSON для загрузки конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-514">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="ce517-515">Создаются следующие пары "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-515">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="ce517-516">Ключ</span><span class="sxs-lookup"><span data-stu-id="ce517-516">Key</span></span>                   | <span data-ttu-id="ce517-517">Значение</span><span class="sxs-lookup"><span data-stu-id="ce517-517">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="ce517-518">starship:name</span><span class="sxs-lookup"><span data-stu-id="ce517-518">starship:name</span></span>         | <span data-ttu-id="ce517-519">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="ce517-519">USS Enterprise</span></span>                                    |
| <span data-ttu-id="ce517-520">starship:registry</span><span class="sxs-lookup"><span data-stu-id="ce517-520">starship:registry</span></span>     | <span data-ttu-id="ce517-521">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="ce517-521">NCC-1701</span></span>                                          |
| <span data-ttu-id="ce517-522">starship:class</span><span class="sxs-lookup"><span data-stu-id="ce517-522">starship:class</span></span>        | <span data-ttu-id="ce517-523">Constitution</span><span class="sxs-lookup"><span data-stu-id="ce517-523">Constitution</span></span>                                      |
| <span data-ttu-id="ce517-524">starship:length</span><span class="sxs-lookup"><span data-stu-id="ce517-524">starship:length</span></span>       | <span data-ttu-id="ce517-525">304.8</span><span class="sxs-lookup"><span data-stu-id="ce517-525">304.8</span></span>                                             |
| <span data-ttu-id="ce517-526">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="ce517-526">starship:commissioned</span></span> | <span data-ttu-id="ce517-527">False</span><span class="sxs-lookup"><span data-stu-id="ce517-527">False</span></span>                                             |
| <span data-ttu-id="ce517-528">trademark</span><span class="sxs-lookup"><span data-stu-id="ce517-528">trademark</span></span>             | <span data-ttu-id="ce517-529">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="ce517-529">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="ce517-530">Пример приложения вызывает `GetSection` с помощью ключа `starship`.</span><span class="sxs-lookup"><span data-stu-id="ce517-530">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="ce517-531">Пары "ключ — значение" `starship` изолированы.</span><span class="sxs-lookup"><span data-stu-id="ce517-531">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="ce517-532">Метод `Bind` вызывается в подразделе при прохождении в экземпляр класса `Starship`.</span><span class="sxs-lookup"><span data-stu-id="ce517-532">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="ce517-533">После привязки значения экземпляра экземпляру присваивается свойство для преобразования для просмотра.</span><span class="sxs-lookup"><span data-stu-id="ce517-533">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="ce517-534">Привязка к графу объектов</span><span class="sxs-lookup"><span data-stu-id="ce517-534">Bind to an object graph</span></span>

<span data-ttu-id="ce517-535"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> способна связывать весь граф объекта POCO.</span><span class="sxs-lookup"><span data-stu-id="ce517-535"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="ce517-536">Образец содержит модель `TvShow`, в графе объектов которого находятся классы `Metadata` и `Actors` (*Модели/TvShow.cs*).</span><span class="sxs-lookup"><span data-stu-id="ce517-536">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="ce517-537">В примере приложения есть файл *tvshow.xml*, содержащий данные конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-537">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="ce517-538">Конфигурация привязана ко всему ​​графу объектов `TvShow` с помощью метода `Bind`.</span><span class="sxs-lookup"><span data-stu-id="ce517-538">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="ce517-539">Привязанный экземпляр присваивается свойству для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="ce517-539">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="ce517-540">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) привязывает и возвращает указанный тип.</span><span class="sxs-lookup"><span data-stu-id="ce517-540">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="ce517-541">Метод `Get<T>` может быть более удобным, чем использование `Bind`.</span><span class="sxs-lookup"><span data-stu-id="ce517-541">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="ce517-542">В следующем коде показано, как использовать `Get<T>` с предыдущим примером, который позволяет привязать связанный экземпляр непосредственно к свойству, используемому для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="ce517-542">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="ce517-543">Привязка массива к классу</span><span class="sxs-lookup"><span data-stu-id="ce517-543">Bind an array to a class</span></span>

<span data-ttu-id="ce517-544">*Пример приложения демонстрирует концепции, описанные в этом разделе.*</span><span class="sxs-lookup"><span data-stu-id="ce517-544">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="ce517-545"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-545">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="ce517-546">Любой формат массива, который предоставляет сегмент числового ключа (`:0:`, `:1:`, &hellip; `:{n}:`), способен привязать массив к массиву класса POCO.</span><span class="sxs-lookup"><span data-stu-id="ce517-546">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="ce517-547">Привязка предоставляется соглашением.</span><span class="sxs-lookup"><span data-stu-id="ce517-547">Binding is provided by convention.</span></span> <span data-ttu-id="ce517-548">Пользовательские поставщики конфигурации не обязаны реализовывать привязку массива.</span><span class="sxs-lookup"><span data-stu-id="ce517-548">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="ce517-549">**Обработка массива в оперативной памяти**</span><span class="sxs-lookup"><span data-stu-id="ce517-549">**In-memory array processing**</span></span>

<span data-ttu-id="ce517-550">Рассмотрите ключи и значения конфигурации, приведенные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="ce517-550">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="ce517-551">Ключ</span><span class="sxs-lookup"><span data-stu-id="ce517-551">Key</span></span>             | <span data-ttu-id="ce517-552">Значение</span><span class="sxs-lookup"><span data-stu-id="ce517-552">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="ce517-553">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="ce517-553">array:entries:0</span></span> | <span data-ttu-id="ce517-554">value0</span><span class="sxs-lookup"><span data-stu-id="ce517-554">value0</span></span> |
| <span data-ttu-id="ce517-555">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="ce517-555">array:entries:1</span></span> | <span data-ttu-id="ce517-556">value1</span><span class="sxs-lookup"><span data-stu-id="ce517-556">value1</span></span> |
| <span data-ttu-id="ce517-557">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="ce517-557">array:entries:2</span></span> | <span data-ttu-id="ce517-558">value2</span><span class="sxs-lookup"><span data-stu-id="ce517-558">value2</span></span> |
| <span data-ttu-id="ce517-559">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="ce517-559">array:entries:4</span></span> | <span data-ttu-id="ce517-560">value4</span><span class="sxs-lookup"><span data-stu-id="ce517-560">value4</span></span> |
| <span data-ttu-id="ce517-561">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="ce517-561">array:entries:5</span></span> | <span data-ttu-id="ce517-562">value5</span><span class="sxs-lookup"><span data-stu-id="ce517-562">value5</span></span> |

<span data-ttu-id="ce517-563">Эти ключи и значения загружаются в пример приложения с помощью поставщика конфигурации памяти.</span><span class="sxs-lookup"><span data-stu-id="ce517-563">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

<span data-ttu-id="ce517-564">Массив пропускает значения для индекса &num;3.</span><span class="sxs-lookup"><span data-stu-id="ce517-564">The array skips a value for index &num;3.</span></span> <span data-ttu-id="ce517-565">Связующее свойство конфигурации не способно связывать нулевые значения или создавать нулевые записи в связанных объектах, что становится ясным в тот момент, когда демонстрируется результат привязки этого массива к объекту.</span><span class="sxs-lookup"><span data-stu-id="ce517-565">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="ce517-566">В примере приложения класс POCO доступен для хранения привязанных данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-566">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="ce517-567">Данные конфигурации, связанные с объектом.</span><span class="sxs-lookup"><span data-stu-id="ce517-567">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="ce517-568">Синтаксис [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) также может использоваться для получения более компактного кода.</span><span class="sxs-lookup"><span data-stu-id="ce517-568">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="ce517-569">Связанный объект, экземпляр `ArrayExample`, получает данные массива из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-569">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="ce517-570">Индекс `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="ce517-570">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="ce517-571">Значение `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="ce517-571">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="ce517-572">0</span><span class="sxs-lookup"><span data-stu-id="ce517-572">0</span></span>                            | <span data-ttu-id="ce517-573">value0</span><span class="sxs-lookup"><span data-stu-id="ce517-573">value0</span></span>                       |
| <span data-ttu-id="ce517-574">1</span><span class="sxs-lookup"><span data-stu-id="ce517-574">1</span></span>                            | <span data-ttu-id="ce517-575">value1</span><span class="sxs-lookup"><span data-stu-id="ce517-575">value1</span></span>                       |
| <span data-ttu-id="ce517-576">2</span><span class="sxs-lookup"><span data-stu-id="ce517-576">2</span></span>                            | <span data-ttu-id="ce517-577">value2</span><span class="sxs-lookup"><span data-stu-id="ce517-577">value2</span></span>                       |
| <span data-ttu-id="ce517-578">3</span><span class="sxs-lookup"><span data-stu-id="ce517-578">3</span></span>                            | <span data-ttu-id="ce517-579">value4</span><span class="sxs-lookup"><span data-stu-id="ce517-579">value4</span></span>                       |
| <span data-ttu-id="ce517-580">4</span><span class="sxs-lookup"><span data-stu-id="ce517-580">4</span></span>                            | <span data-ttu-id="ce517-581">value5</span><span class="sxs-lookup"><span data-stu-id="ce517-581">value5</span></span>                       |

<span data-ttu-id="ce517-582">Индекс &num;3 в связанном объекте содержит данные конфигурации для конфигурационного ключа `array:4` и его значения `value4`.</span><span class="sxs-lookup"><span data-stu-id="ce517-582">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="ce517-583">При привязке данных конфигурации, содержащих массив индексов, в ключах конфигурации эти индексы просто используются для выполнения итерации данных конфигурации при создании объекта.</span><span class="sxs-lookup"><span data-stu-id="ce517-583">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="ce517-584">Когда массив ключей конфигурации пропускает один или несколько индексов, в данных конфигурации не может быть сохранено нулевое значение и в связанном объекте не создается нулевая запись.</span><span class="sxs-lookup"><span data-stu-id="ce517-584">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="ce517-585">Отсутствующий элемент конфигурации для индекса &num;3 может быть предоставлен перед привязкой к экземпляру `ArrayExample` любым поставщиком конфигурации, который создает правильную пару "ключ — значение" в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-585">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="ce517-586">Если в образец включен дополнительный поставщик конфигурации JSON с отсутствующей парой "ключ — значение", то `ArrayExample.Entries` подойдет полному массиву конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-586">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="ce517-587">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="ce517-587">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="ce517-588">В `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="ce517-588">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="ce517-589">Пары "ключ — значение", показанные в таблице, загружаются в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-589">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="ce517-590">Ключ</span><span class="sxs-lookup"><span data-stu-id="ce517-590">Key</span></span>             | <span data-ttu-id="ce517-591">Значение</span><span class="sxs-lookup"><span data-stu-id="ce517-591">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="ce517-592">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="ce517-592">array:entries:3</span></span> | <span data-ttu-id="ce517-593">value3</span><span class="sxs-lookup"><span data-stu-id="ce517-593">value3</span></span> |

<span data-ttu-id="ce517-594">Если экземпляр класса `ArrayExample` связан после того, как поставщик конфигурации JSON включает запись для индекса &num;3, то массив `ArrayExample.Entries` включит значение.</span><span class="sxs-lookup"><span data-stu-id="ce517-594">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="ce517-595">Индекс `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="ce517-595">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="ce517-596">Значение `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="ce517-596">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="ce517-597">0</span><span class="sxs-lookup"><span data-stu-id="ce517-597">0</span></span>                            | <span data-ttu-id="ce517-598">value0</span><span class="sxs-lookup"><span data-stu-id="ce517-598">value0</span></span>                       |
| <span data-ttu-id="ce517-599">1</span><span class="sxs-lookup"><span data-stu-id="ce517-599">1</span></span>                            | <span data-ttu-id="ce517-600">value1</span><span class="sxs-lookup"><span data-stu-id="ce517-600">value1</span></span>                       |
| <span data-ttu-id="ce517-601">2</span><span class="sxs-lookup"><span data-stu-id="ce517-601">2</span></span>                            | <span data-ttu-id="ce517-602">value2</span><span class="sxs-lookup"><span data-stu-id="ce517-602">value2</span></span>                       |
| <span data-ttu-id="ce517-603">3</span><span class="sxs-lookup"><span data-stu-id="ce517-603">3</span></span>                            | <span data-ttu-id="ce517-604">value3</span><span class="sxs-lookup"><span data-stu-id="ce517-604">value3</span></span>                       |
| <span data-ttu-id="ce517-605">4</span><span class="sxs-lookup"><span data-stu-id="ce517-605">4</span></span>                            | <span data-ttu-id="ce517-606">value4</span><span class="sxs-lookup"><span data-stu-id="ce517-606">value4</span></span>                       |
| <span data-ttu-id="ce517-607">5</span><span class="sxs-lookup"><span data-stu-id="ce517-607">5</span></span>                            | <span data-ttu-id="ce517-608">value5</span><span class="sxs-lookup"><span data-stu-id="ce517-608">value5</span></span>                       |

<span data-ttu-id="ce517-609">**Обработка массива JSON**</span><span class="sxs-lookup"><span data-stu-id="ce517-609">**JSON array processing**</span></span>

<span data-ttu-id="ce517-610">Если JSON-файл содержит массив, ключи конфигурации будут созданы для элементов массива с индексом раздела, начиная с нуля.</span><span class="sxs-lookup"><span data-stu-id="ce517-610">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="ce517-611">В следующем файле конфигурации `subsection` — это массив.</span><span class="sxs-lookup"><span data-stu-id="ce517-611">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="ce517-612">Поставщик конфигурации JSON считывает данные конфигурации в следующие пары "ключ — значение".</span><span class="sxs-lookup"><span data-stu-id="ce517-612">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="ce517-613">Ключ</span><span class="sxs-lookup"><span data-stu-id="ce517-613">Key</span></span>                     | <span data-ttu-id="ce517-614">Значение</span><span class="sxs-lookup"><span data-stu-id="ce517-614">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="ce517-615">json_array:key</span><span class="sxs-lookup"><span data-stu-id="ce517-615">json_array:key</span></span>          | <span data-ttu-id="ce517-616">valueA</span><span class="sxs-lookup"><span data-stu-id="ce517-616">valueA</span></span> |
| <span data-ttu-id="ce517-617">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="ce517-617">json_array:subsection:0</span></span> | <span data-ttu-id="ce517-618">valueB</span><span class="sxs-lookup"><span data-stu-id="ce517-618">valueB</span></span> |
| <span data-ttu-id="ce517-619">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="ce517-619">json_array:subsection:1</span></span> | <span data-ttu-id="ce517-620">valueC</span><span class="sxs-lookup"><span data-stu-id="ce517-620">valueC</span></span> |
| <span data-ttu-id="ce517-621">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="ce517-621">json_array:subsection:2</span></span> | <span data-ttu-id="ce517-622">valueD</span><span class="sxs-lookup"><span data-stu-id="ce517-622">valueD</span></span> |

<span data-ttu-id="ce517-623">В примере приложения для привязки пар "ключ — значение" конфигурации доступен следующий класс POCO.</span><span class="sxs-lookup"><span data-stu-id="ce517-623">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="ce517-624">После выполнения привязки `JsonArrayExample.Key` содержит значение `valueA`.</span><span class="sxs-lookup"><span data-stu-id="ce517-624">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="ce517-625">Подраздел значения хранится в свойстве массива POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="ce517-625">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="ce517-626">Индекс `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="ce517-626">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="ce517-627">Значение `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="ce517-627">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="ce517-628">0</span><span class="sxs-lookup"><span data-stu-id="ce517-628">0</span></span>                                   | <span data-ttu-id="ce517-629">valueB</span><span class="sxs-lookup"><span data-stu-id="ce517-629">valueB</span></span>                              |
| <span data-ttu-id="ce517-630">1</span><span class="sxs-lookup"><span data-stu-id="ce517-630">1</span></span>                                   | <span data-ttu-id="ce517-631">valueC</span><span class="sxs-lookup"><span data-stu-id="ce517-631">valueC</span></span>                              |
| <span data-ttu-id="ce517-632">2</span><span class="sxs-lookup"><span data-stu-id="ce517-632">2</span></span>                                   | <span data-ttu-id="ce517-633">valueD</span><span class="sxs-lookup"><span data-stu-id="ce517-633">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="ce517-634">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="ce517-634">Custom configuration provider</span></span>

<span data-ttu-id="ce517-635">Пример приложения демонстрирует, как создать базовый поставщик конфигурации, который считывает пары "ключ — значение" конфигурации из базы данных, используя [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="ce517-635">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="ce517-636">Поставщик имеет следующие характеристики.</span><span class="sxs-lookup"><span data-stu-id="ce517-636">The provider has the following characteristics:</span></span>

* <span data-ttu-id="ce517-637">База данных в памяти EF используется для демонстрационных целей.</span><span class="sxs-lookup"><span data-stu-id="ce517-637">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="ce517-638">Чтобы использовать базу данных, для которой требуется строка подключения, выполните вторичный `ConfigurationBuilder`, чтобы предоставить строку подключения от другого поставщика конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce517-638">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="ce517-639">Поставщик считывает таблицу базы данных в конфигурации при запуске.</span><span class="sxs-lookup"><span data-stu-id="ce517-639">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="ce517-640">Поставщик не запрашивает базу данных для каждого ключа.</span><span class="sxs-lookup"><span data-stu-id="ce517-640">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="ce517-641">Функция перезагрузки на изменение не реализована, поэтому обновление базы данных после запуска приложения не влияет на конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="ce517-641">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="ce517-642">Определите сущность `EFConfigurationValue` для хранения значений конфигурации в базе данных.</span><span class="sxs-lookup"><span data-stu-id="ce517-642">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ce517-643">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="ce517-643">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="ce517-644">Добавьте `EFConfigurationContext` в хранилище и обратитесь к настроенным значениям.</span><span class="sxs-lookup"><span data-stu-id="ce517-644">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="ce517-645">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="ce517-645">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="ce517-646">Создайте класс, реализующий <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="ce517-646">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="ce517-647">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="ce517-647">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="ce517-648">Создайте пользовательский поставщик конфигурации путем наследования от <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="ce517-648">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="ce517-649">Поставщик конфигурации инициализирует пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="ce517-649">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="ce517-650">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="ce517-650">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="ce517-651">Метод расширения `AddEFConfiguration` позволяет добавить источник конфигурации к `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ce517-651">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="ce517-652">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="ce517-652">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="ce517-653">В следующем коде показано, как использовать пользовательский `EFConfigurationProvider` в *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="ce517-653">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ce517-654">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="ce517-654">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="ce517-655">Добавьте `EFConfigurationContext` в хранилище и обратитесь к настроенным значениям.</span><span class="sxs-lookup"><span data-stu-id="ce517-655">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="ce517-656">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="ce517-656">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="ce517-657">Создайте класс, реализующий <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="ce517-657">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="ce517-658">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="ce517-658">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="ce517-659">Создайте пользовательский поставщик конфигурации путем наследования от <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="ce517-659">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="ce517-660">Поставщик конфигурации инициализирует пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="ce517-660">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="ce517-661">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="ce517-661">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="ce517-662">Метод расширения `AddEFConfiguration` позволяет добавить источник конфигурации к `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ce517-662">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="ce517-663">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="ce517-663">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="ce517-664">В следующем коде показано, как использовать пользовательский `EFConfigurationProvider` в *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="ce517-664">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="ce517-665">Доступ к конфигурации во время запуска</span><span class="sxs-lookup"><span data-stu-id="ce517-665">Access configuration during startup</span></span>

<span data-ttu-id="ce517-666">Внесите значение `IConfiguration` в конструктор `Startup` для доступа к значениям конфигурации в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ce517-666">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="ce517-667">Чтобы получить доступ к конфигурации в `Startup.Configure`, либо добавьте значение `IConfiguration` непосредственно в метод, либо используйте экземпляр из конструктора.</span><span class="sxs-lookup"><span data-stu-id="ce517-667">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="ce517-668">Дополнительные сведения см. в руководстве по [доступу к конфигурации с использованием удобных методов](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="ce517-668">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="ce517-669">Настройте доступ на странице Razor Pages или в представлении MVC</span><span class="sxs-lookup"><span data-stu-id="ce517-669">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="ce517-670">Для доступа к параметрам конфигурации на странице Razor Pages или в представлении MVC добавьте [использование директивы](xref:mvc/views/razor#using) ([Справочник по C#: использование директивы](/dotnet/csharp/language-reference/keywords/using-directive)) для [пространства имен Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) и вставьте <xref:Microsoft.Extensions.Configuration.IConfiguration> на страницу или в представление.</span><span class="sxs-lookup"><span data-stu-id="ce517-670">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="ce517-671">На странице Razor Pages</span><span class="sxs-lookup"><span data-stu-id="ce517-671">In a Razor Pages page:</span></span>

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

<span data-ttu-id="ce517-672">В представлении MVC</span><span class="sxs-lookup"><span data-stu-id="ce517-672">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="ce517-673">Добавление конфигурации из внешней сборки</span><span class="sxs-lookup"><span data-stu-id="ce517-673">Add configuration from an external assembly</span></span>

<span data-ttu-id="ce517-674">Реализация <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> позволяет при запуске добавлять в приложение улучшения из внешней сборки вне приложения класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="ce517-674">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="ce517-675">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="ce517-675">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ce517-676">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ce517-676">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
