---
title: Конфигурация в .NET Core
author: guardrex
description: Узнайте, как использовать API конфигурации для настройки приложения ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/18/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 0a9b1a1a08617ef4ca8a36295cec8910ec111acd
ms.sourcegitcommit: a166291c6708f5949c417874108332856b53b6a9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/18/2019
ms.locfileid: "72589907"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="5366e-103">Конфигурация в .NET Core</span><span class="sxs-lookup"><span data-stu-id="5366e-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="5366e-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="5366e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5366e-105">Конфигурация приложения в ASP.NET Core основана на парах "ключ — значение", установленных *поставщиками конфигурации*.</span><span class="sxs-lookup"><span data-stu-id="5366e-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="5366e-106">Поставщики конфигурации получают данные конфигурации в парах "ключ — значение" из различных источников:</span><span class="sxs-lookup"><span data-stu-id="5366e-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="5366e-107">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="5366e-107">Azure Key Vault</span></span>
* <span data-ttu-id="5366e-108">Конфигурация приложений Azure</span><span class="sxs-lookup"><span data-stu-id="5366e-108">Azure App Configuration</span></span>
* <span data-ttu-id="5366e-109">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="5366e-109">Command-line arguments</span></span>
* <span data-ttu-id="5366e-110">а также пользовательские поставщики (устанавливаемые или создаваемые).</span><span class="sxs-lookup"><span data-stu-id="5366e-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="5366e-111">Справочные файлы</span><span class="sxs-lookup"><span data-stu-id="5366e-111">Directory files</span></span>
* <span data-ttu-id="5366e-112">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="5366e-112">Environment variables</span></span>
* <span data-ttu-id="5366e-113">объектов .NET в памяти;</span><span class="sxs-lookup"><span data-stu-id="5366e-113">In-memory .NET objects</span></span>
* <span data-ttu-id="5366e-114">файлов параметров;</span><span class="sxs-lookup"><span data-stu-id="5366e-114">Settings files</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5366e-115">Пакеты конфигурации для распространенных вариантов провайдеров конфигурации ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) включаются платформой неявным образом.</span><span class="sxs-lookup"><span data-stu-id="5366e-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5366e-116">Пакеты конфигурации для распространенных вариантов провайдеров конфигурации ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) включаются в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="5366e-116">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="5366e-117">В приведенных ниже и представленных в образце приложения примерах кода используется пространство имен <xref:Microsoft.Extensions.Configuration>:</span><span class="sxs-lookup"><span data-stu-id="5366e-117">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="5366e-118">*Шаблон параметров* является расширением конфигурации основных понятий, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="5366e-118">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="5366e-119">Параметры используют классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="5366e-119">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="5366e-120">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="5366e-120">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="5366e-121">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5366e-121">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="5366e-122">Конфигурация узла и приложения</span><span class="sxs-lookup"><span data-stu-id="5366e-122">Host versus app configuration</span></span>

<span data-ttu-id="5366e-123">Перед настройкой и запуском приложения настройте и запустите *узел*.</span><span class="sxs-lookup"><span data-stu-id="5366e-123">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="5366e-124">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="5366e-124">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="5366e-125">Как приложение, так и узел настраиваются с использованием поставщиков конфигурации, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="5366e-125">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="5366e-126">Пары "ключ — значение" конфигурации узлов также включаются в конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="5366e-126">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="5366e-127">Дополнительные сведения о том, как используются поставщики конфигурации при создании узла и как влияют источники конфигурации на узел, см. в разделе <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="5366e-127">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="5366e-128">Конфигурация по умолчанию</span><span class="sxs-lookup"><span data-stu-id="5366e-128">Default configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5366e-129">Веб-приложения на основе шаблонов [dotnet new](/dotnet/core/tools/dotnet-new) ASP.NET Core вызывают <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> при создании узла.</span><span class="sxs-lookup"><span data-stu-id="5366e-129">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="5366e-130">`CreateDefaultBuilder` предоставляет конфигурацию по умолчанию для приложения в следующем порядке:</span><span class="sxs-lookup"><span data-stu-id="5366e-130">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="5366e-131">Приведенные ниже сведения относятся к приложениям, использующим [универсальный узел](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="5366e-131">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="5366e-132">Подробные сведения о конфигурации по умолчанию при использовании [веб-узла](xref:fundamentals/host/web-host) см. в [разделе о версии ASP.NET Core 2.2 в этой статье](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="5366e-132">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="5366e-133">Существуют следующие способы предоставления конфигурации узла.</span><span class="sxs-lookup"><span data-stu-id="5366e-133">Host configuration is provided from:</span></span>
  * <span data-ttu-id="5366e-134">Переменные среды с префиксом `DOTNET_` (например, `DOTNET_ENVIRONMENT`), использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5366e-134">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="5366e-135">Префикс (`DOTNET_`) исключается при загрузке пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-135">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="5366e-136">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5366e-136">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="5366e-137">Устанавливается конфигурация веб-узла по умолчанию (`ConfigureWebHostDefaults`):</span><span class="sxs-lookup"><span data-stu-id="5366e-137">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="5366e-138">Kestrel используется в качестве веб-сервера и настраивается с помощью поставщиков конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="5366e-138">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="5366e-139">Добавьте ПО промежуточного слоя фильтрации узлов.</span><span class="sxs-lookup"><span data-stu-id="5366e-139">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="5366e-140">Если переменной среды `ASPNETCORE_FORWARDEDHEADERS_ENABLED` присвоено значение `true`, добавьте ПО промежуточного слоя перенаправления заголовков.</span><span class="sxs-lookup"><span data-stu-id="5366e-140">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="5366e-141">Включите интеграцию служб IIS.</span><span class="sxs-lookup"><span data-stu-id="5366e-141">Enable IIS integration.</span></span>
* <span data-ttu-id="5366e-142">Конфигурация приложения предоставляется из следующих ресурсов:</span><span class="sxs-lookup"><span data-stu-id="5366e-142">App configuration is provided from:</span></span>
  * <span data-ttu-id="5366e-143">Файл *appsettings.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5366e-143">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="5366e-144">Файл *appsettings.{Environment}.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5366e-144">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="5366e-145">[Менеджера секретов](xref:security/app-secrets), когда приложение выполняется в среде `Development` с использованием начальных сборок.</span><span class="sxs-lookup"><span data-stu-id="5366e-145">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="5366e-146">Переменные среды, использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5366e-146">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="5366e-147">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5366e-147">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5366e-148">Веб-приложения на основе шаблонов [dotnet new](/dotnet/core/tools/dotnet-new) ASP.NET Core вызывают <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> при создании узла.</span><span class="sxs-lookup"><span data-stu-id="5366e-148">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="5366e-149">`CreateDefaultBuilder` предоставляет конфигурацию по умолчанию для приложения в следующем порядке:</span><span class="sxs-lookup"><span data-stu-id="5366e-149">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="5366e-150">Приведенные ниже сведения относятся к приложениям, использующим [веб-узел](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="5366e-150">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="5366e-151">Подробные сведения о конфигурации по умолчанию при использовании [универсального узла](xref:fundamentals/host/generic-host) см. в [последней версии этой статьи](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="5366e-151">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="5366e-152">Существуют следующие способы предоставления конфигурации узла.</span><span class="sxs-lookup"><span data-stu-id="5366e-152">Host configuration is provided from:</span></span>
  * <span data-ttu-id="5366e-153">Переменные среды с префиксом `ASPNETCORE_` (например, `ASPNETCORE_ENVIRONMENT`), использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5366e-153">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="5366e-154">Префикс (`ASPNETCORE_`) исключается при загрузке пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-154">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="5366e-155">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5366e-155">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="5366e-156">Конфигурация приложения предоставляется из следующих ресурсов:</span><span class="sxs-lookup"><span data-stu-id="5366e-156">App configuration is provided from:</span></span>
  * <span data-ttu-id="5366e-157">Файл *appsettings.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5366e-157">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="5366e-158">Файл *appsettings.{Environment}.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5366e-158">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="5366e-159">[Менеджера секретов](xref:security/app-secrets), когда приложение выполняется в среде `Development` с использованием начальных сборок.</span><span class="sxs-lookup"><span data-stu-id="5366e-159">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="5366e-160">Переменные среды, использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5366e-160">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="5366e-161">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5366e-161">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

## <a name="security"></a><span data-ttu-id="5366e-162">Безопасность</span><span class="sxs-lookup"><span data-stu-id="5366e-162">Security</span></span>

<span data-ttu-id="5366e-163">Применяйте описанные ниже методики для защиты конфиденциальных данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-163">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="5366e-164">Никогда не храните пароли или другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации обычного текста.</span><span class="sxs-lookup"><span data-stu-id="5366e-164">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="5366e-165">Не используйте секреты рабочей среды в средах разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="5366e-165">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="5366e-166">Указывайте секреты вне проекта, чтобы их нельзя было случайно зафиксировать в репозитории с исходным кодом.</span><span class="sxs-lookup"><span data-stu-id="5366e-166">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="5366e-167">Дополнительные сведения см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="5366e-167">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="5366e-168"><xref:security/app-secrets> &ndash; содержит рекомендации по использованию переменных среды для хранения конфиденциальных данных.</span><span class="sxs-lookup"><span data-stu-id="5366e-168"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="5366e-169">Менеджер секретов использует поставщик конфигурации файла для хранения конфиденциальных данных пользователя в файле JSON в локальной системе.</span><span class="sxs-lookup"><span data-stu-id="5366e-169">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="5366e-170">Поставщик конфигурации файлов описан ниже в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="5366e-170">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="5366e-171">В [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) безопасно хранятся секреты приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5366e-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="5366e-172">Дополнительные сведения можно найти по адресу: <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="5366e-172">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="5366e-173">Иерархическая модель конфигурации</span><span class="sxs-lookup"><span data-stu-id="5366e-173">Hierarchical configuration data</span></span>

<span data-ttu-id="5366e-174">API конфигурации способен поддерживать иерархические данные конфигурации, выполняя преобразование в плоскую структуру иерархических данных с использованием разделителя в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-174">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="5366e-175">В следующем файле JSON существуют четыре ключа в структурированной иерархии двух разделов.</span><span class="sxs-lookup"><span data-stu-id="5366e-175">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="5366e-176">При считывании файла в конфигурацию для сохранения исходной иерархической структуры данных источника конфигурации создаются уникальные ключи.</span><span class="sxs-lookup"><span data-stu-id="5366e-176">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="5366e-177">Разделы и ключи преобразовываются в плоскую структуру с использованием двоеточия (`:`) для сохранения исходной структуры.</span><span class="sxs-lookup"><span data-stu-id="5366e-177">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="5366e-178">section0:key0</span><span class="sxs-lookup"><span data-stu-id="5366e-178">section0:key0</span></span>
* <span data-ttu-id="5366e-179">section0:key1</span><span class="sxs-lookup"><span data-stu-id="5366e-179">section0:key1</span></span>
* <span data-ttu-id="5366e-180">section1:key0</span><span class="sxs-lookup"><span data-stu-id="5366e-180">section1:key0</span></span>
* <span data-ttu-id="5366e-181">section1:key1</span><span class="sxs-lookup"><span data-stu-id="5366e-181">section1:key1</span></span>

<span data-ttu-id="5366e-182">Методы <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> и <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> доступны для изолирования разделов и дочерних элементов раздела в данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-182"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="5366e-183">Эти методы описаны далее в разделе [GetSection, GetChildren и Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="5366e-183">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="5366e-184">Соглашения</span><span class="sxs-lookup"><span data-stu-id="5366e-184">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="5366e-185">Источники и поставщики</span><span class="sxs-lookup"><span data-stu-id="5366e-185">Sources and providers</span></span>

<span data-ttu-id="5366e-186">При запуске приложения источники конфигурации считываются в порядке, в котором были указаны их поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-186">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="5366e-187">Поставщики конфигурации, которые реализуют обнаружение изменений, имеют возможность перезагрузить конфигурацию при изменении базовых параметров.</span><span class="sxs-lookup"><span data-stu-id="5366e-187">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="5366e-188">Так, поставщик файла конфигурации (подробнее о нем далее в этой статье) и [поставщик конфигурации Azure Key Vault](xref:security/key-vault-configuration) реализуют обнаружение изменений.</span><span class="sxs-lookup"><span data-stu-id="5366e-188">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="5366e-189">Объект <xref:Microsoft.Extensions.Configuration.IConfiguration> доступен в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) приложения.</span><span class="sxs-lookup"><span data-stu-id="5366e-189"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="5366e-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> можно внедрить в <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> Razor Pages, чтобы получить конфигурацию для класса:</span><span class="sxs-lookup"><span data-stu-id="5366e-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

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

<span data-ttu-id="5366e-191">Поставщики конфигурации не могут использовать контейнер DI, так как он недоступен при настройке узла.</span><span class="sxs-lookup"><span data-stu-id="5366e-191">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="5366e-192">Клавиши</span><span class="sxs-lookup"><span data-stu-id="5366e-192">Keys</span></span>

<span data-ttu-id="5366e-193">В ключах конфигурации приняты следующие соглашения.</span><span class="sxs-lookup"><span data-stu-id="5366e-193">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="5366e-194">В ключах не учитывается регистр символов.</span><span class="sxs-lookup"><span data-stu-id="5366e-194">Keys are case-insensitive.</span></span> <span data-ttu-id="5366e-195">Например `ConnectionString` и `connectionstring` обрабатываются как эквивалентные ключи.</span><span class="sxs-lookup"><span data-stu-id="5366e-195">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="5366e-196">Если значение для одного и того же ключа установлено одним и тем же или разными поставщиками конфигурации, последним значением, установленным на ключе, является используемое значение.</span><span class="sxs-lookup"><span data-stu-id="5366e-196">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="5366e-197">Иерархические ключи</span><span class="sxs-lookup"><span data-stu-id="5366e-197">Hierarchical keys</span></span>
  * <span data-ttu-id="5366e-198">При взаимодействии с API конфигурации разделитель-двоеточие (`:`) поддерживается на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="5366e-198">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="5366e-199">В переменных среды разделитель-двоеточие может не работать на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="5366e-199">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="5366e-200">Двойной знак подчеркивания (`__`) поддерживается на всех платформах и автоматически преобразовывается в двоеточие.</span><span class="sxs-lookup"><span data-stu-id="5366e-200">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="5366e-201">В хранилище ключей Azure иерархические ключи используют `--` (два дефиса) в качестве разделителя.</span><span class="sxs-lookup"><span data-stu-id="5366e-201">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="5366e-202">Чтобы заменить дефисы двоеточием, при загрузке секретов в конфигурацию приложения необходимо указать код.</span><span class="sxs-lookup"><span data-stu-id="5366e-202">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="5366e-203"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-203">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="5366e-204">Привязка массива описана в разделе [Привязка массива к классу](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="5366e-204">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="5366e-205">Значения</span><span class="sxs-lookup"><span data-stu-id="5366e-205">Values</span></span>

<span data-ttu-id="5366e-206">В значениях конфигурации учитываются следующие соглашения.</span><span class="sxs-lookup"><span data-stu-id="5366e-206">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="5366e-207">Значения являются строками.</span><span class="sxs-lookup"><span data-stu-id="5366e-207">Values are strings.</span></span>
* <span data-ttu-id="5366e-208">Значение NULL не может храниться в конфигурации или быть привязанным к объектам.</span><span class="sxs-lookup"><span data-stu-id="5366e-208">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="5366e-209">Поставщики</span><span class="sxs-lookup"><span data-stu-id="5366e-209">Providers</span></span>

<span data-ttu-id="5366e-210">В следующей таблице показаны поставщики конфигурации, доступные для приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5366e-210">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="5366e-211">Поставщик</span><span class="sxs-lookup"><span data-stu-id="5366e-211">Provider</span></span> | <span data-ttu-id="5366e-212">Предоставляет конфигурацию из &hellip;</span><span class="sxs-lookup"><span data-stu-id="5366e-212">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="5366e-213">[Поставщик конфигурации хранилища ключей Azure](xref:security/key-vault-configuration) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="5366e-213">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="5366e-214">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="5366e-214">Azure Key Vault</span></span> |
| <span data-ttu-id="5366e-215">[Поставщик конфигурации приложений Azure](/azure/azure-app-configuration/quickstart-aspnet-core-app) (документация Azure)</span><span class="sxs-lookup"><span data-stu-id="5366e-215">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="5366e-216">Конфигурация приложений Azure</span><span class="sxs-lookup"><span data-stu-id="5366e-216">Azure App Configuration</span></span> |
| [<span data-ttu-id="5366e-217">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="5366e-217">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="5366e-218">Параметры командной строки</span><span class="sxs-lookup"><span data-stu-id="5366e-218">Command-line parameters</span></span> |
| [<span data-ttu-id="5366e-219">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="5366e-219">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="5366e-220">Источник пользователя</span><span class="sxs-lookup"><span data-stu-id="5366e-220">Custom source</span></span> |
| [<span data-ttu-id="5366e-221">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="5366e-221">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="5366e-222">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="5366e-222">Environment variables</span></span> |
| [<span data-ttu-id="5366e-223">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="5366e-223">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="5366e-224">Файлы (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="5366e-224">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="5366e-225">Поставщик конфигурации ключа для каждого файла</span><span class="sxs-lookup"><span data-stu-id="5366e-225">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="5366e-226">Справочные файлы</span><span class="sxs-lookup"><span data-stu-id="5366e-226">Directory files</span></span> |
| [<span data-ttu-id="5366e-227">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="5366e-227">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="5366e-228">Коллекции оперативной памяти</span><span class="sxs-lookup"><span data-stu-id="5366e-228">In-memory collections</span></span> |
| <span data-ttu-id="5366e-229">[Секреты пользователей (Менеджер секретов)](xref:security/app-secrets) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="5366e-229">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="5366e-230">Файл в каталоге профиля пользователя</span><span class="sxs-lookup"><span data-stu-id="5366e-230">File in the user profile directory</span></span> |

<span data-ttu-id="5366e-231">Источники конфигурации считываются в том порядке, в котором при запуске указываются их поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-231">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="5366e-232">Поставщики конфигурации в этом разделе описаны в алфавитном порядке, а не в необходимом вам порядке в коде.</span><span class="sxs-lookup"><span data-stu-id="5366e-232">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="5366e-233">Порядок поставщиков конфигурации в коде соответствует приоритетам ваших основных источников конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-233">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="5366e-234">Типичная последовательность поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-234">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="5366e-235">Файлы (*appsettings.json*, *appsettings.{Environment}.json*, где `{Environment}` — это текущая среда размещения приложения)</span><span class="sxs-lookup"><span data-stu-id="5366e-235">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. <span data-ttu-id="5366e-236">[Azure Key Vault](xref:security/key-vault-configuration);</span><span class="sxs-lookup"><span data-stu-id="5366e-236">[Azure Key Vault](xref:security/key-vault-configuration)</span></span>
1. <span data-ttu-id="5366e-237">[Секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки)</span><span class="sxs-lookup"><span data-stu-id="5366e-237">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="5366e-238">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="5366e-238">Environment variables</span></span>
1. <span data-ttu-id="5366e-239">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="5366e-239">Command-line arguments</span></span>

<span data-ttu-id="5366e-240">Общепринятой практикой является размещение поставщика конфигурации командной строки последним в ряду поставщиков, чтобы аргументы командной строки могли переопределять конфигурацию, установленную другими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="5366e-240">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="5366e-241">Предыдущая последовательность поставщиков используется при инициализации нового построителя узла с помощью `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5366e-241">The preceding sequence of providers is used when you initialize a new host builder with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="5366e-242">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="5366e-242">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="5366e-243">Настройка построителя узла с помощью ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="5366e-243">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="5366e-244">Чтобы настроить построитель узла, вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> и укажите конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="5366e-244">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="5366e-245">`ConfigureHostConfiguration` используется для инициализации <xref:Microsoft.Extensions.Hosting.IHostEnvironment> для дальнейшего использования в процессе сборки.</span><span class="sxs-lookup"><span data-stu-id="5366e-245">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="5366e-246">Метод `ConfigureHostConfiguration` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="5366e-246">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

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

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="5366e-247">Настройка построителя узла с помощью UseConfiguration</span><span class="sxs-lookup"><span data-stu-id="5366e-247">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="5366e-248">Чтобы настроить построитель узла, вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> в построителе узла с конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="5366e-248">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

## <a name="configureappconfiguration"></a><span data-ttu-id="5366e-249">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="5366e-249">ConfigureAppConfiguration</span></span>

<span data-ttu-id="5366e-250">Вызовите `ConfigureAppConfiguration` при сборке узла, чтобы указать поставщики конфигурации приложения в дополнение к тем, которые автоматически добавлены `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5366e-250">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="5366e-251">Переопределение предыдущей конфигурации с помощью аргументов командной строки</span><span class="sxs-lookup"><span data-stu-id="5366e-251">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="5366e-252">Чтобы предоставить конфигурацию приложения, которую можно переопределить с помощью аргументов командной строки, вызовите метод `AddCommandLine` последним:</span><span class="sxs-lookup"><span data-stu-id="5366e-252">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="5366e-253">Использование конфигурации во время запуска приложения</span><span class="sxs-lookup"><span data-stu-id="5366e-253">Consume configuration during app startup</span></span>

<span data-ttu-id="5366e-254">Конфигурация, предоставленная приложению в `ConfigureAppConfiguration`, доступна во время запуска приложения, включая `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5366e-254">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5366e-255">Дополнительные сведения см. в разделе [Доступ к конфигурации во время запуска](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="5366e-255">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="5366e-256">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="5366e-256">Command-line Configuration Provider</span></span>

<span data-ttu-id="5366e-257"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> загружает конфигурацию из пары "ключ — значение" аргумента командной строки в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="5366e-257">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="5366e-258">Чтобы активировать конфигурацию командной строки, вызывается метод расширения <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> для экземпляра <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5366e-258">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="5366e-259">`AddCommandLine` вызывается автоматически при вызове `CreateDefaultBuilder(string [])`.</span><span class="sxs-lookup"><span data-stu-id="5366e-259">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="5366e-260">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="5366e-260">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="5366e-261">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="5366e-261">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="5366e-262">дополнительную конфигурацию из файлов *appsettings.json* и *appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="5366e-262">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="5366e-263">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="5366e-263">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="5366e-264">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="5366e-264">Environment variables.</span></span>

<span data-ttu-id="5366e-265">`CreateDefaultBuilder` добавляет последним поставщика конфигурации командной строки.</span><span class="sxs-lookup"><span data-stu-id="5366e-265">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="5366e-266">Аргументы командной строки передаются в набор конфигурации переопределения среды выполнения, установленной другими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="5366e-266">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="5366e-267">`CreateDefaultBuilder` действует, когда создается узел.</span><span class="sxs-lookup"><span data-stu-id="5366e-267">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="5366e-268">Поэтому конфигурация командной строки, активированная с помощью `CreateDefaultBuilder`, может повлиять на настройку узла.</span><span class="sxs-lookup"><span data-stu-id="5366e-268">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="5366e-269">Для приложений на основе шаблонов ASP.NET Core метод `AddCommandLine` уже был вызван методом `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5366e-269">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="5366e-270">Чтобы добавить дополнительные поставщики конфигурации и обеспечить возможность переопределения конфигурации от этих поставщиков с помощью аргументов командной строки, вызовите дополнительные поставщики приложения в `ConfigureAppConfiguration` и вызовите `AddCommandLine` в последнюю очередь.</span><span class="sxs-lookup"><span data-stu-id="5366e-270">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="5366e-271">**Пример**</span><span class="sxs-lookup"><span data-stu-id="5366e-271">**Example**</span></span>

<span data-ttu-id="5366e-272">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="5366e-272">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="5366e-273">Откройте командную строку в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="5366e-273">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="5366e-274">Поставьте аргумент командной строки в команду `dotnet run`: `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="5366e-274">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="5366e-275">После запуска приложения откройте браузер в приложении по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="5366e-275">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="5366e-276">Обратите внимание, что вывод содержит пару "ключ — значение" для аргумента командной строки конфигурации, предоставленного для `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="5366e-276">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="5366e-277">Аргументы</span><span class="sxs-lookup"><span data-stu-id="5366e-277">Arguments</span></span>

<span data-ttu-id="5366e-278">Значение должно соответствовать знаку равенства (`=`), или ключ должен иметь префикс (`--` или `/`), когда значение следует за пробелом.</span><span class="sxs-lookup"><span data-stu-id="5366e-278">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="5366e-279">Значение не требуется, если используется знак равенства (например, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="5366e-279">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="5366e-280">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="5366e-280">Key prefix</span></span>               | <span data-ttu-id="5366e-281">Пример</span><span class="sxs-lookup"><span data-stu-id="5366e-281">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="5366e-282">Без префикса</span><span class="sxs-lookup"><span data-stu-id="5366e-282">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="5366e-283">Два дефиса (`--`)</span><span class="sxs-lookup"><span data-stu-id="5366e-283">Two dashes (`--`)</span></span>        | <span data-ttu-id="5366e-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="5366e-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="5366e-285">Прямая косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="5366e-285">Forward slash (`/`)</span></span>      | <span data-ttu-id="5366e-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="5366e-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="5366e-287">В рамках одной и той же команды не смешивайте пары "ключ — значение" аргумента командной строки, которые используют знак равенства, с парами "ключ — значение", которые используют пробел.</span><span class="sxs-lookup"><span data-stu-id="5366e-287">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="5366e-288">Примеры команд.</span><span class="sxs-lookup"><span data-stu-id="5366e-288">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="5366e-289">Сопоставления переключений</span><span class="sxs-lookup"><span data-stu-id="5366e-289">Switch mappings</span></span>

<span data-ttu-id="5366e-290">Сопоставление параметров позволяет указать логику замены имен ключей.</span><span class="sxs-lookup"><span data-stu-id="5366e-290">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="5366e-291">Когда вручную создается конфигурация с помощью <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, методу <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> можно предоставить словарь сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="5366e-291">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="5366e-292">В словаре сопоставлений переключений выполняется поиск ключа, который совпадает с ключом, предоставляемым аргументом командной строки.</span><span class="sxs-lookup"><span data-stu-id="5366e-292">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="5366e-293">Если ключ в командной строке находится в словаре, значение словаря (замена ключа) передается обратно, чтобы установить пару "ключ — значение" в конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="5366e-293">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="5366e-294">Сопоставление переключений необходимо для любого ключа командной строки с префиксом из одного дефиса (`-`).</span><span class="sxs-lookup"><span data-stu-id="5366e-294">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="5366e-295">Правила ключей из словаря сопоставления переключений:</span><span class="sxs-lookup"><span data-stu-id="5366e-295">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="5366e-296">Переключения должны начинаться с дефиса (`-`) или двойного дефиса (`--`).</span><span class="sxs-lookup"><span data-stu-id="5366e-296">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="5366e-297">Словарь сопоставлений переключений не должен содержать повторяющиеся ключи.</span><span class="sxs-lookup"><span data-stu-id="5366e-297">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="5366e-298">Создайте словарь сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="5366e-298">Create a switch mappings dictionary.</span></span> <span data-ttu-id="5366e-299">В следующем примере создаются два сопоставления переключений:</span><span class="sxs-lookup"><span data-stu-id="5366e-299">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="5366e-300">При сборке узла вызовите `AddCommandLine` со словарем сопоставлений переключений:</span><span class="sxs-lookup"><span data-stu-id="5366e-300">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="5366e-301">Для приложений, использующих сопоставления переключений, в вызове `CreateDefaultBuilder` аргументы передаваться не должны.</span><span class="sxs-lookup"><span data-stu-id="5366e-301">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="5366e-302">Вызов команды `AddCommandLine` метода `CreateDefaultBuilder` не включает сопоставленные переключения, и нет возможности передать словарь сопоставления переключений в `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5366e-302">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="5366e-303">Чтобы решить эту проблему, нужно не передавать аргументы команде `CreateDefaultBuilder`, а позволить методу `AddCommandLine` метода `ConfigurationBuilder` обрабатывать как аргументы, так и словарь сопоставления параметров.</span><span class="sxs-lookup"><span data-stu-id="5366e-303">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="5366e-304">Созданный словарь сопоставлений переключений содержит данные, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="5366e-304">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="5366e-305">Ключ</span><span class="sxs-lookup"><span data-stu-id="5366e-305">Key</span></span>       | <span data-ttu-id="5366e-306">Значение</span><span class="sxs-lookup"><span data-stu-id="5366e-306">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="5366e-307">Если ключи сопоставления переключений используются при запуске приложения, конфигурация принимает значение конфигурации в ключе, предоставленном в словаре.</span><span class="sxs-lookup"><span data-stu-id="5366e-307">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="5366e-308">После выполнения предыдущей команды конфигурация содержит значения, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="5366e-308">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="5366e-309">Ключ</span><span class="sxs-lookup"><span data-stu-id="5366e-309">Key</span></span>               | <span data-ttu-id="5366e-310">Значение</span><span class="sxs-lookup"><span data-stu-id="5366e-310">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="5366e-311">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="5366e-311">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="5366e-312"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> загружает конфигурацию из пары "ключ — значение" переменной среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="5366e-312">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="5366e-313">Чтобы активировать конфигурацию переменных среды, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5366e-313">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="5366e-314">[Служба приложений Azure](https://azure.microsoft.com/services/app-service/) позволяет задать переменные среды на портале Azure, который может переопределить конфигурацию приложения, используя поставщик конфигурации переменных среды.</span><span class="sxs-lookup"><span data-stu-id="5366e-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="5366e-315">Дополнительные сведения см. в руководстве по [переопределению конфигурации приложения Azure с помощью портала Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="5366e-315">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5366e-316">`AddEnvironmentVariables` используется для загрузки переменных среды, имеющих префикс `DOTNET_`, для [конфигурации узла](#host-versus-app-configuration) при инициализации нового построителя узла с [универсальным узлом](xref:fundamentals/host/generic-host) и вызове `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5366e-316">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="5366e-317">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="5366e-317">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5366e-318">`AddEnvironmentVariables` используется для загрузки переменных среды, имеющих префикс `ASPNETCORE_`, для [конфигурации узла](#host-versus-app-configuration) при инициализации нового построителя узла с [веб-узлом](xref:fundamentals/host/web-host) и вызове `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5366e-318">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="5366e-319">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="5366e-319">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

<span data-ttu-id="5366e-320">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="5366e-320">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="5366e-321">конфигурация приложения на основе переменных среды без префикса путем вызова `AddEnvironmentVariables` без префикса;</span><span class="sxs-lookup"><span data-stu-id="5366e-321">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="5366e-322">дополнительную конфигурацию из файлов *appsettings.json* и *appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="5366e-322">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="5366e-323">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="5366e-323">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="5366e-324">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="5366e-324">Command-line arguments.</span></span>

<span data-ttu-id="5366e-325">Поставщик конфигурации переменных среды вызывается после выполнения настройки с помощью секретов пользователя и файлов *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="5366e-325">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="5366e-326">Вызов поставщика в этой позиции разрешает чтение переменных среды выполнения, чтобы переопределить конфигурацию, заданную секретом пользователя и файлом *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="5366e-326">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="5366e-327">Если вам нужно добавить конфигурацию приложения из дополнительных переменных среды, вызовите дополнительные поставщики приложения в `ConfigureAppConfiguration`, а затем вызовите `AddEnvironmentVariables` с префиксом.</span><span class="sxs-lookup"><span data-stu-id="5366e-327">If you need to provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="5366e-328">**Пример**</span><span class="sxs-lookup"><span data-stu-id="5366e-328">**Example**</span></span>

<span data-ttu-id="5366e-329">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="5366e-329">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="5366e-330">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="5366e-330">Run the sample app.</span></span> <span data-ttu-id="5366e-331">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="5366e-331">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="5366e-332">Обратите внимание, что выходные данные содержат пару "ключ — значение" для переменной среды `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="5366e-332">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="5366e-333">Значение отражает среду, в которой выполняется приложение, обычно при локальном запуске это `Development`.</span><span class="sxs-lookup"><span data-stu-id="5366e-333">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="5366e-334">Чтобы список переменных среды, отображаемый приложением, был коротким, приложение фильтрует переменные среды.</span><span class="sxs-lookup"><span data-stu-id="5366e-334">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="5366e-335">См. пример файла *Pages/Index.cshtml.cs* приложения.</span><span class="sxs-lookup"><span data-stu-id="5366e-335">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="5366e-336">Если хотите просмотреть все переменные среды, доступные для приложения, измените значение `FilteredConfiguration` в *Pages/Index.cshtml.cs* на следующее:</span><span class="sxs-lookup"><span data-stu-id="5366e-336">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="5366e-337">Префиксы</span><span class="sxs-lookup"><span data-stu-id="5366e-337">Prefixes</span></span>

<span data-ttu-id="5366e-338">Переменные среды, которые загружаются в конфигурацию приложения, фильтруются при добавлении префикса к методу `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="5366e-338">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="5366e-339">Например, чтобы отфильтровать переменные среды по префиксу `CUSTOM_`, введите префикс поставщику конфигурации:</span><span class="sxs-lookup"><span data-stu-id="5366e-339">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="5366e-340">Префикс отделяется при создании пары конфигурации "ключ — значение".</span><span class="sxs-lookup"><span data-stu-id="5366e-340">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="5366e-341">При создании построителя узла конфигурация узла предоставляется переменными среды.</span><span class="sxs-lookup"><span data-stu-id="5366e-341">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="5366e-342">Дополнительные сведения о префиксе, используемом для этих переменных среды, см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="5366e-342">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="5366e-343">**Префиксы строк подключения**</span><span class="sxs-lookup"><span data-stu-id="5366e-343">**Connection string prefixes**</span></span>

<span data-ttu-id="5366e-344">API конфигурации имеет специальные правила обработки для четырех строк подключения переменных среды, связанных с настройкой строк подключения Azure для среды приложения.</span><span class="sxs-lookup"><span data-stu-id="5366e-344">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="5366e-345">Если префикс не указан в `AddEnvironmentVariables`, переменные среды с префиксами, указанными в таблице, загружаются в приложение.</span><span class="sxs-lookup"><span data-stu-id="5366e-345">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="5366e-346">Префикс строки подключения</span><span class="sxs-lookup"><span data-stu-id="5366e-346">Connection string prefix</span></span> | <span data-ttu-id="5366e-347">Поставщик</span><span class="sxs-lookup"><span data-stu-id="5366e-347">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="5366e-348">Поставщик пользователя</span><span class="sxs-lookup"><span data-stu-id="5366e-348">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="5366e-349">MySQL</span><span class="sxs-lookup"><span data-stu-id="5366e-349">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="5366e-350">База данных SQL Azure</span><span class="sxs-lookup"><span data-stu-id="5366e-350">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="5366e-351">SQL Server</span><span class="sxs-lookup"><span data-stu-id="5366e-351">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="5366e-352">Когда переменная среды обнаруживается и загружается в конфигурацию с одним из четырех префиксов, приведенных в таблице, происходит следующее.</span><span class="sxs-lookup"><span data-stu-id="5366e-352">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="5366e-353">Ключ конфигурации создается путем удаления префикса переменных среды и добавления ключа раздела конфигурации (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="5366e-353">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="5366e-354">Создается новая пара "ключ — значение" конфигурации, которая представляет поставщика подключения базы данных (за исключением `CUSTOMCONNSTR_`, который не имеет указанного поставщика).</span><span class="sxs-lookup"><span data-stu-id="5366e-354">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="5366e-355">Ключ переменной среды</span><span class="sxs-lookup"><span data-stu-id="5366e-355">Environment variable key</span></span> | <span data-ttu-id="5366e-356">Преобразованный ключ конфигурации</span><span class="sxs-lookup"><span data-stu-id="5366e-356">Converted configuration key</span></span> | <span data-ttu-id="5366e-357">Запись конфигурации поставщика</span><span class="sxs-lookup"><span data-stu-id="5366e-357">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="5366e-358">Запись конфигурации не создана.</span><span class="sxs-lookup"><span data-stu-id="5366e-358">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="5366e-359">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="5366e-359">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="5366e-360">Значение: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="5366e-360">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="5366e-361">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="5366e-361">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="5366e-362">Значение: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="5366e-362">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="5366e-363">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="5366e-363">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="5366e-364">Значение: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="5366e-364">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="5366e-365">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="5366e-365">File Configuration Provider</span></span>

<span data-ttu-id="5366e-366"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> является базовым классом для загрузки конфигурации из файловой системы.</span><span class="sxs-lookup"><span data-stu-id="5366e-366"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="5366e-367">Следующие поставщики конфигурации предназначены для определенных типов файлов:</span><span class="sxs-lookup"><span data-stu-id="5366e-367">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="5366e-368">Поставщик конфигурации INI</span><span class="sxs-lookup"><span data-stu-id="5366e-368">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="5366e-369">Поставщик конфигурации JSON</span><span class="sxs-lookup"><span data-stu-id="5366e-369">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="5366e-370">Поставщик конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="5366e-370">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="5366e-371">Поставщик конфигурации INI</span><span class="sxs-lookup"><span data-stu-id="5366e-371">INI Configuration Provider</span></span>

<span data-ttu-id="5366e-372"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> загружает конфигурацию из пары "ключ — значение" INI-файла во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="5366e-372">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="5366e-373">Чтобы активировать конфигурацию INI-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5366e-373">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="5366e-374">Двоеточие можно использовать как разделитель раздела в конфигурации файла INI.</span><span class="sxs-lookup"><span data-stu-id="5366e-374">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="5366e-375">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="5366e-375">Overloads permit specifying:</span></span>

* <span data-ttu-id="5366e-376">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="5366e-376">Whether the file is optional.</span></span>
* <span data-ttu-id="5366e-377">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="5366e-377">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="5366e-378"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="5366e-378">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="5366e-379">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="5366e-379">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.SetBasePath(Directory.GetCurrentDirectory());
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="5366e-380">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="5366e-380">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span>

<span data-ttu-id="5366e-381">Общий пример конфигурации INI-файла.</span><span class="sxs-lookup"><span data-stu-id="5366e-381">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="5366e-382">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="5366e-382">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="5366e-383">section0:key0</span><span class="sxs-lookup"><span data-stu-id="5366e-383">section0:key0</span></span>
* <span data-ttu-id="5366e-384">section0:key1</span><span class="sxs-lookup"><span data-stu-id="5366e-384">section0:key1</span></span>
* <span data-ttu-id="5366e-385">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="5366e-385">section1:subsection:key</span></span>
* <span data-ttu-id="5366e-386">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="5366e-386">section2:subsection0:key</span></span>
* <span data-ttu-id="5366e-387">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="5366e-387">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="5366e-388">Поставщик конфигурации JSON</span><span class="sxs-lookup"><span data-stu-id="5366e-388">JSON Configuration Provider</span></span>

<span data-ttu-id="5366e-389"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> загружает конфигурацию из пары "ключ — значение" JSON-файла в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="5366e-389">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="5366e-390">Чтобы активировать конфигурацию JSON-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5366e-390">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="5366e-391">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="5366e-391">Overloads permit specifying:</span></span>

* <span data-ttu-id="5366e-392">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="5366e-392">Whether the file is optional.</span></span>
* <span data-ttu-id="5366e-393">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="5366e-393">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="5366e-394"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="5366e-394">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="5366e-395">`AddJsonFile` автоматически вызывается дважды при инициализации нового построителя узла с `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5366e-395">`AddJsonFile` is automatically called twice when you initialize a new host builder with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="5366e-396">Метод вызывается для загрузки конфигурации из:</span><span class="sxs-lookup"><span data-stu-id="5366e-396">The method is called to load configuration from:</span></span>

* <span data-ttu-id="5366e-397">*appsettings.json* &ndash; первым читается этот файл.</span><span class="sxs-lookup"><span data-stu-id="5366e-397">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="5366e-398">Версия файла среды может переопределить значения, предоставленные *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="5366e-398">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="5366e-399">*appsettings.{Environment}.json* — версия среды файла загружается на основе [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="5366e-399">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="5366e-400">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="5366e-400">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="5366e-401">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="5366e-401">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="5366e-402">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="5366e-402">Environment variables.</span></span>
* <span data-ttu-id="5366e-403">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="5366e-403">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="5366e-404">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="5366e-404">Command-line arguments.</span></span>

<span data-ttu-id="5366e-405">Сначала устанавливается поставщик конфигурации JSON-файлов.</span><span class="sxs-lookup"><span data-stu-id="5366e-405">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="5366e-406">Таким образом, секреты пользователя, переменные среды и аргументы командной строки переопределят конфигурацию, заданную файлами *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="5366e-406">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="5366e-407">Вызовите `ConfigureAppConfiguration` при сборке узла, чтобы указать конфигурацию приложения для файлов, отличных от *appsettings.json* и *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="5366e-407">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.SetBasePath(Directory.GetCurrentDirectory());
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="5366e-408">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="5366e-408">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span>

<span data-ttu-id="5366e-409">**Пример**</span><span class="sxs-lookup"><span data-stu-id="5366e-409">**Example**</span></span>

<span data-ttu-id="5366e-410">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает два вызова `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="5366e-410">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="5366e-411">Конфигурация загружена из *appsettings.json* и *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="5366e-411">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="5366e-412">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="5366e-412">Run the sample app.</span></span> <span data-ttu-id="5366e-413">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="5366e-413">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="5366e-414">Обратите внимание, что выходные данные содержат пары "ключ — значение" для конфигурации, представленной в таблице в зависимости от среды.</span><span class="sxs-lookup"><span data-stu-id="5366e-414">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="5366e-415">Ключи конфигурации ведения журнала используют двоеточие (`:`) как иерархический разделитель.</span><span class="sxs-lookup"><span data-stu-id="5366e-415">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="5366e-416">Ключ</span><span class="sxs-lookup"><span data-stu-id="5366e-416">Key</span></span>                        | <span data-ttu-id="5366e-417">Значение разработки</span><span class="sxs-lookup"><span data-stu-id="5366e-417">Development Value</span></span> | <span data-ttu-id="5366e-418">Рабочее значение</span><span class="sxs-lookup"><span data-stu-id="5366e-418">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="5366e-419">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="5366e-419">Logging:LogLevel:System</span></span>    | <span data-ttu-id="5366e-420">Сведения</span><span class="sxs-lookup"><span data-stu-id="5366e-420">Information</span></span>       | <span data-ttu-id="5366e-421">Сведения</span><span class="sxs-lookup"><span data-stu-id="5366e-421">Information</span></span>      |
| <span data-ttu-id="5366e-422">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="5366e-422">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="5366e-423">Сведения</span><span class="sxs-lookup"><span data-stu-id="5366e-423">Information</span></span>       | <span data-ttu-id="5366e-424">Сведения</span><span class="sxs-lookup"><span data-stu-id="5366e-424">Information</span></span>      |
| <span data-ttu-id="5366e-425">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="5366e-425">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="5366e-426">Отладка</span><span class="sxs-lookup"><span data-stu-id="5366e-426">Debug</span></span>             | <span data-ttu-id="5366e-427">Ошибка</span><span class="sxs-lookup"><span data-stu-id="5366e-427">Error</span></span>            |
| <span data-ttu-id="5366e-428">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="5366e-428">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="5366e-429">Поставщик конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="5366e-429">XML Configuration Provider</span></span>

<span data-ttu-id="5366e-430"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> загружает конфигурацию из пары "ключ — значение" XML-файла в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="5366e-430">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="5366e-431">Чтобы активировать конфигурацию XML-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5366e-431">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="5366e-432">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="5366e-432">Overloads permit specifying:</span></span>

* <span data-ttu-id="5366e-433">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="5366e-433">Whether the file is optional.</span></span>
* <span data-ttu-id="5366e-434">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="5366e-434">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="5366e-435"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="5366e-435">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="5366e-436">Корневой узел файла конфигурации не учитывается при создании пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-436">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="5366e-437">Не указывайте в файле Document Type Definition (определение типа документа, DTD) или пространство имен.</span><span class="sxs-lookup"><span data-stu-id="5366e-437">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="5366e-438">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="5366e-438">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.SetBasePath(Directory.GetCurrentDirectory());
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="5366e-439">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="5366e-439">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span>

<span data-ttu-id="5366e-440">XML-файлы конфигурации могут использовать имена отдельных элементов для повторяющихся разделов.</span><span class="sxs-lookup"><span data-stu-id="5366e-440">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="5366e-441">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="5366e-441">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="5366e-442">section0:key0</span><span class="sxs-lookup"><span data-stu-id="5366e-442">section0:key0</span></span>
* <span data-ttu-id="5366e-443">section0:key1</span><span class="sxs-lookup"><span data-stu-id="5366e-443">section0:key1</span></span>
* <span data-ttu-id="5366e-444">section1:key0</span><span class="sxs-lookup"><span data-stu-id="5366e-444">section1:key0</span></span>
* <span data-ttu-id="5366e-445">section1:key1</span><span class="sxs-lookup"><span data-stu-id="5366e-445">section1:key1</span></span>

<span data-ttu-id="5366e-446">Повторяющиеся элементы, использующие то же имя элемента, работают, если атрибут `name` используется для различения элементов.</span><span class="sxs-lookup"><span data-stu-id="5366e-446">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="5366e-447">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="5366e-447">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="5366e-448">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="5366e-448">section:section0:key:key0</span></span>
* <span data-ttu-id="5366e-449">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="5366e-449">section:section0:key:key1</span></span>
* <span data-ttu-id="5366e-450">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="5366e-450">section:section1:key:key0</span></span>
* <span data-ttu-id="5366e-451">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="5366e-451">section:section1:key:key1</span></span>

<span data-ttu-id="5366e-452">Атрибуты можно использовать для предоставления значений.</span><span class="sxs-lookup"><span data-stu-id="5366e-452">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="5366e-453">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="5366e-453">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="5366e-454">key:attribute</span><span class="sxs-lookup"><span data-stu-id="5366e-454">key:attribute</span></span>
* <span data-ttu-id="5366e-455">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="5366e-455">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="5366e-456">Поставщик конфигурации ключа для каждого файла</span><span class="sxs-lookup"><span data-stu-id="5366e-456">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="5366e-457"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> использует файлы каталога как пары "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-457">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="5366e-458">Ключ является именем файла.</span><span class="sxs-lookup"><span data-stu-id="5366e-458">The key is the file name.</span></span> <span data-ttu-id="5366e-459">Значение содержит содержимое файла.</span><span class="sxs-lookup"><span data-stu-id="5366e-459">The value contains the file's contents.</span></span> <span data-ttu-id="5366e-460">Поставщик конфигурации ключа для каждого файла используется в сценариях размещения Docker.</span><span class="sxs-lookup"><span data-stu-id="5366e-460">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="5366e-461">Чтобы активировать конфигурацию ключа для каждого файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5366e-461">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="5366e-462">Значение параметра `directoryPath` должно быть абсолютным путем к файлам.</span><span class="sxs-lookup"><span data-stu-id="5366e-462">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="5366e-463">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="5366e-463">Overloads permit specifying:</span></span>

* <span data-ttu-id="5366e-464">`Action<KeyPerFileConfigurationSource>` — делегат, который настраивает источник.</span><span class="sxs-lookup"><span data-stu-id="5366e-464">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="5366e-465">Обязательно ли указывать каталог и путь к каталогу.</span><span class="sxs-lookup"><span data-stu-id="5366e-465">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="5366e-466">Двойное подчеркивание (`__`) используется в качестве разделителя ключа конфигурации в именах файлов.</span><span class="sxs-lookup"><span data-stu-id="5366e-466">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="5366e-467">Например, в имени файла `Logging__LogLevel__System` создается ключ конфигурации `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="5366e-467">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="5366e-468">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="5366e-468">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.SetBasePath(Directory.GetCurrentDirectory());
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

<span data-ttu-id="5366e-469">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="5366e-469">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span>

## <a name="memory-configuration-provider"></a><span data-ttu-id="5366e-470">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="5366e-470">Memory Configuration Provider</span></span>

<span data-ttu-id="5366e-471"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> использует коллекцию памяти в качестве пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-471">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="5366e-472">Чтобы активировать конфигурацию коллекции в памяти, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5366e-472">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="5366e-473">Поставщик конфигурации может инициализироваться значением `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="5366e-473">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="5366e-474">Чтобы указать конфигурацию приложения, при сборке узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="5366e-474">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="5366e-475">В следующем примере создается словарь конфигурации:</span><span class="sxs-lookup"><span data-stu-id="5366e-475">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="5366e-476">Словарь используется с вызовом `AddInMemoryCollection` для предоставления конфигурации:</span><span class="sxs-lookup"><span data-stu-id="5366e-476">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="5366e-477">GetValue</span><span class="sxs-lookup"><span data-stu-id="5366e-477">GetValue</span></span>

<span data-ttu-id="5366e-478">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) извлекает одно значение из конфигурации с указанным ключом и преобразует его в указанный тип, не являющийся коллекцией.</span><span class="sxs-lookup"><span data-stu-id="5366e-478">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="5366e-479">Перегрузка принимает значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5366e-479">An overload accepts a default value.</span></span>

<span data-ttu-id="5366e-480">В следующем примере происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="5366e-480">The following example:</span></span>

* <span data-ttu-id="5366e-481">Из конфигурации извлекается строковое значение с ключом `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="5366e-481">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="5366e-482">Если `NumberKey` отсутствует в ключах конфигурации, используется значение по умолчанию `99`.</span><span class="sxs-lookup"><span data-stu-id="5366e-482">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="5366e-483">Значение получает тип `int`.</span><span class="sxs-lookup"><span data-stu-id="5366e-483">Types the value as an `int`.</span></span>
* <span data-ttu-id="5366e-484">Значение сохраняется в свойстве `NumberConfig` для использования на странице.</span><span class="sxs-lookup"><span data-stu-id="5366e-484">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="5366e-485">GetSection, GetChildren и Exists</span><span class="sxs-lookup"><span data-stu-id="5366e-485">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="5366e-486">В следующих примерах рассмотрим файл JSON.</span><span class="sxs-lookup"><span data-stu-id="5366e-486">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="5366e-487">Четыре ключа находятся в двух разделах, один из которых содержит пару из подразделов.</span><span class="sxs-lookup"><span data-stu-id="5366e-487">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="5366e-488">Когда файл считывается в конфигурацию, для сохранения значений конфигурации создаются следующие уникальные иерархические ключи.</span><span class="sxs-lookup"><span data-stu-id="5366e-488">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="5366e-489">section0:key0</span><span class="sxs-lookup"><span data-stu-id="5366e-489">section0:key0</span></span>
* <span data-ttu-id="5366e-490">section0:key1</span><span class="sxs-lookup"><span data-stu-id="5366e-490">section0:key1</span></span>
* <span data-ttu-id="5366e-491">section1:key0</span><span class="sxs-lookup"><span data-stu-id="5366e-491">section1:key0</span></span>
* <span data-ttu-id="5366e-492">section1:key1</span><span class="sxs-lookup"><span data-stu-id="5366e-492">section1:key1</span></span>
* <span data-ttu-id="5366e-493">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="5366e-493">section2:subsection0:key0</span></span>
* <span data-ttu-id="5366e-494">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="5366e-494">section2:subsection0:key1</span></span>
* <span data-ttu-id="5366e-495">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="5366e-495">section2:subsection1:key0</span></span>
* <span data-ttu-id="5366e-496">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="5366e-496">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="5366e-497">GetSection</span><span class="sxs-lookup"><span data-stu-id="5366e-497">GetSection</span></span>

<span data-ttu-id="5366e-498">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) извлекает подраздел конфигурации с указанным ключом подраздела.</span><span class="sxs-lookup"><span data-stu-id="5366e-498">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="5366e-499">Чтобы вернуть <xref:Microsoft.Extensions.Configuration.IConfigurationSection>, содержащий только пары "ключ — значение" в `section1`, вызовите `GetSection` и укажите имя раздела.</span><span class="sxs-lookup"><span data-stu-id="5366e-499">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="5366e-500">`configSection` не содержит значение, только ключ и путь.</span><span class="sxs-lookup"><span data-stu-id="5366e-500">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="5366e-501">Аналогично, чтобы получить значения для ключей в `section2:subsection0`, вызовите `GetSection` и укажите путь к разделу.</span><span class="sxs-lookup"><span data-stu-id="5366e-501">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="5366e-502">Значение `GetSection` никогда не возвращает значение `null`.</span><span class="sxs-lookup"><span data-stu-id="5366e-502">`GetSection` never returns `null`.</span></span> <span data-ttu-id="5366e-503">Если соответствующий раздел не найден, возвращается пустой параметр `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="5366e-503">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="5366e-504">Когда `GetSection` возвращает соответствующий раздел, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> не заполняется.</span><span class="sxs-lookup"><span data-stu-id="5366e-504">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="5366e-505"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> и <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> возвращаются, если раздел существует.</span><span class="sxs-lookup"><span data-stu-id="5366e-505">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="5366e-506">GetChildren</span><span class="sxs-lookup"><span data-stu-id="5366e-506">GetChildren</span></span>

<span data-ttu-id="5366e-507">Вызов [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) в `section2` получает значение `IEnumerable<IConfigurationSection>`, которое включает:</span><span class="sxs-lookup"><span data-stu-id="5366e-507">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="5366e-508">Exists</span><span class="sxs-lookup"><span data-stu-id="5366e-508">Exists</span></span>

<span data-ttu-id="5366e-509">Используйте [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*), чтобы определить, существует ли раздел конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-509">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="5366e-510">Учитывая данные примера, `sectionExists` является `false`, потому что в данных конфигурации нет `section2:subsection2`.</span><span class="sxs-lookup"><span data-stu-id="5366e-510">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="5366e-511">Привязка к классу</span><span class="sxs-lookup"><span data-stu-id="5366e-511">Bind to a class</span></span>

<span data-ttu-id="5366e-512">Конфигурация может быть привязана к классам, которые представляют группы связанных параметров, используя шаблон *вариантов*.</span><span class="sxs-lookup"><span data-stu-id="5366e-512">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="5366e-513">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="5366e-513">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="5366e-514">Значения конфигурации возвращаются как строки, но вызов <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> позволяет построить объекты [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="5366e-514">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="5366e-515">Пример приложения содержит модель `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="5366e-515">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5366e-516">Раздел `starship` файла *starship.json* создает конфигурацию, когда образец приложения использует поставщик конфигурации JSON для загрузки конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-516">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="5366e-517">Создаются следующие пары "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-517">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="5366e-518">Ключ</span><span class="sxs-lookup"><span data-stu-id="5366e-518">Key</span></span>                   | <span data-ttu-id="5366e-519">Значение</span><span class="sxs-lookup"><span data-stu-id="5366e-519">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="5366e-520">starship:name</span><span class="sxs-lookup"><span data-stu-id="5366e-520">starship:name</span></span>         | <span data-ttu-id="5366e-521">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="5366e-521">USS Enterprise</span></span>                                    |
| <span data-ttu-id="5366e-522">starship:registry</span><span class="sxs-lookup"><span data-stu-id="5366e-522">starship:registry</span></span>     | <span data-ttu-id="5366e-523">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="5366e-523">NCC-1701</span></span>                                          |
| <span data-ttu-id="5366e-524">starship:class</span><span class="sxs-lookup"><span data-stu-id="5366e-524">starship:class</span></span>        | <span data-ttu-id="5366e-525">Constitution</span><span class="sxs-lookup"><span data-stu-id="5366e-525">Constitution</span></span>                                      |
| <span data-ttu-id="5366e-526">starship:length</span><span class="sxs-lookup"><span data-stu-id="5366e-526">starship:length</span></span>       | <span data-ttu-id="5366e-527">304.8</span><span class="sxs-lookup"><span data-stu-id="5366e-527">304.8</span></span>                                             |
| <span data-ttu-id="5366e-528">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="5366e-528">starship:commissioned</span></span> | <span data-ttu-id="5366e-529">False</span><span class="sxs-lookup"><span data-stu-id="5366e-529">False</span></span>                                             |
| <span data-ttu-id="5366e-530">trademark</span><span class="sxs-lookup"><span data-stu-id="5366e-530">trademark</span></span>             | <span data-ttu-id="5366e-531">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="5366e-531">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="5366e-532">Пример приложения вызывает `GetSection` с помощью ключа `starship`.</span><span class="sxs-lookup"><span data-stu-id="5366e-532">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="5366e-533">Пары "ключ — значение" `starship` изолированы.</span><span class="sxs-lookup"><span data-stu-id="5366e-533">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="5366e-534">Метод `Bind` вызывается в подразделе при прохождении в экземпляр класса `Starship`.</span><span class="sxs-lookup"><span data-stu-id="5366e-534">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="5366e-535">После привязки значения экземпляра экземпляру присваивается свойство для преобразования для просмотра.</span><span class="sxs-lookup"><span data-stu-id="5366e-535">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="5366e-536">Привязка к графу объектов</span><span class="sxs-lookup"><span data-stu-id="5366e-536">Bind to an object graph</span></span>

<span data-ttu-id="5366e-537"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> способна связывать весь граф объекта POCO.</span><span class="sxs-lookup"><span data-stu-id="5366e-537"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="5366e-538">Образец содержит модель `TvShow`, в графе объектов которого находятся классы `Metadata` и `Actors` (*Модели/TvShow.cs*).</span><span class="sxs-lookup"><span data-stu-id="5366e-538">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5366e-539">В примере приложения есть файл *tvshow.xml*, содержащий данные конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-539">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="5366e-540">Конфигурация привязана ко всему ​​графу объектов `TvShow` с помощью метода `Bind`.</span><span class="sxs-lookup"><span data-stu-id="5366e-540">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="5366e-541">Привязанный экземпляр присваивается свойству для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="5366e-541">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="5366e-542">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) привязывает и возвращает указанный тип.</span><span class="sxs-lookup"><span data-stu-id="5366e-542">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="5366e-543">Метод `Get<T>` может быть более удобным, чем использование `Bind`.</span><span class="sxs-lookup"><span data-stu-id="5366e-543">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="5366e-544">В следующем коде показано, как использовать `Get<T>` с предыдущим примером, который позволяет привязать связанный экземпляр непосредственно к свойству, используемому для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="5366e-544">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="5366e-545">Привязка массива к классу</span><span class="sxs-lookup"><span data-stu-id="5366e-545">Bind an array to a class</span></span>

<span data-ttu-id="5366e-546">*Пример приложения демонстрирует концепции, описанные в этом разделе.*</span><span class="sxs-lookup"><span data-stu-id="5366e-546">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="5366e-547"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-547">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="5366e-548">Любой формат массива, который предоставляет сегмент числового ключа (`:0:`, `:1:`, &hellip; `:{n}:`), способен привязать массив к массиву класса POCO.</span><span class="sxs-lookup"><span data-stu-id="5366e-548">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="5366e-549">Привязка предоставляется соглашением.</span><span class="sxs-lookup"><span data-stu-id="5366e-549">Binding is provided by convention.</span></span> <span data-ttu-id="5366e-550">Пользовательские поставщики конфигурации не обязаны реализовывать привязку массива.</span><span class="sxs-lookup"><span data-stu-id="5366e-550">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="5366e-551">**Обработка массива в оперативной памяти**</span><span class="sxs-lookup"><span data-stu-id="5366e-551">**In-memory array processing**</span></span>

<span data-ttu-id="5366e-552">Рассмотрите ключи и значения конфигурации, приведенные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="5366e-552">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="5366e-553">Ключ</span><span class="sxs-lookup"><span data-stu-id="5366e-553">Key</span></span>             | <span data-ttu-id="5366e-554">Значение</span><span class="sxs-lookup"><span data-stu-id="5366e-554">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="5366e-555">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="5366e-555">array:entries:0</span></span> | <span data-ttu-id="5366e-556">value0</span><span class="sxs-lookup"><span data-stu-id="5366e-556">value0</span></span> |
| <span data-ttu-id="5366e-557">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="5366e-557">array:entries:1</span></span> | <span data-ttu-id="5366e-558">value1</span><span class="sxs-lookup"><span data-stu-id="5366e-558">value1</span></span> |
| <span data-ttu-id="5366e-559">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="5366e-559">array:entries:2</span></span> | <span data-ttu-id="5366e-560">value2</span><span class="sxs-lookup"><span data-stu-id="5366e-560">value2</span></span> |
| <span data-ttu-id="5366e-561">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="5366e-561">array:entries:4</span></span> | <span data-ttu-id="5366e-562">value4</span><span class="sxs-lookup"><span data-stu-id="5366e-562">value4</span></span> |
| <span data-ttu-id="5366e-563">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="5366e-563">array:entries:5</span></span> | <span data-ttu-id="5366e-564">value5</span><span class="sxs-lookup"><span data-stu-id="5366e-564">value5</span></span> |

<span data-ttu-id="5366e-565">Эти ключи и значения загружаются в пример приложения с помощью поставщика конфигурации памяти.</span><span class="sxs-lookup"><span data-stu-id="5366e-565">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,23)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,23)]

::: moniker-end

<span data-ttu-id="5366e-566">Массив пропускает значения для индекса &num;3.</span><span class="sxs-lookup"><span data-stu-id="5366e-566">The array skips a value for index &num;3.</span></span> <span data-ttu-id="5366e-567">Связующее свойство конфигурации не способно связывать нулевые значения или создавать нулевые записи в связанных объектах, что становится ясным в тот момент, когда демонстрируется результат привязки этого массива к объекту.</span><span class="sxs-lookup"><span data-stu-id="5366e-567">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="5366e-568">В примере приложения класс POCO доступен для хранения привязанных данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-568">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5366e-569">Данные конфигурации, связанные с объектом.</span><span class="sxs-lookup"><span data-stu-id="5366e-569">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="5366e-570">Синтаксис [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) также может использоваться для получения более компактного кода.</span><span class="sxs-lookup"><span data-stu-id="5366e-570">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="5366e-571">Связанный объект, экземпляр `ArrayExample`, получает данные массива из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-571">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="5366e-572">Индекс `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="5366e-572">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="5366e-573">Значение `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="5366e-573">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="5366e-574">0</span><span class="sxs-lookup"><span data-stu-id="5366e-574">0</span></span>                            | <span data-ttu-id="5366e-575">value0</span><span class="sxs-lookup"><span data-stu-id="5366e-575">value0</span></span>                       |
| <span data-ttu-id="5366e-576">1</span><span class="sxs-lookup"><span data-stu-id="5366e-576">1</span></span>                            | <span data-ttu-id="5366e-577">value1</span><span class="sxs-lookup"><span data-stu-id="5366e-577">value1</span></span>                       |
| <span data-ttu-id="5366e-578">2</span><span class="sxs-lookup"><span data-stu-id="5366e-578">2</span></span>                            | <span data-ttu-id="5366e-579">value2</span><span class="sxs-lookup"><span data-stu-id="5366e-579">value2</span></span>                       |
| <span data-ttu-id="5366e-580">3</span><span class="sxs-lookup"><span data-stu-id="5366e-580">3</span></span>                            | <span data-ttu-id="5366e-581">value4</span><span class="sxs-lookup"><span data-stu-id="5366e-581">value4</span></span>                       |
| <span data-ttu-id="5366e-582">4</span><span class="sxs-lookup"><span data-stu-id="5366e-582">4</span></span>                            | <span data-ttu-id="5366e-583">value5</span><span class="sxs-lookup"><span data-stu-id="5366e-583">value5</span></span>                       |

<span data-ttu-id="5366e-584">Индекс &num;3 в связанном объекте содержит данные конфигурации для конфигурационного ключа `array:4` и его значения `value4`.</span><span class="sxs-lookup"><span data-stu-id="5366e-584">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="5366e-585">При привязке данных конфигурации, содержащих массив индексов, в ключах конфигурации эти индексы просто используются для выполнения итерации данных конфигурации при создании объекта.</span><span class="sxs-lookup"><span data-stu-id="5366e-585">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="5366e-586">Когда массив ключей конфигурации пропускает один или несколько индексов, в данных конфигурации не может быть сохранено нулевое значение и в связанном объекте не создается нулевая запись.</span><span class="sxs-lookup"><span data-stu-id="5366e-586">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="5366e-587">Отсутствующий элемент конфигурации для индекса &num;3 может быть предоставлен перед привязкой к экземпляру `ArrayExample` любым поставщиком конфигурации, который создает правильную пару "ключ — значение" в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-587">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="5366e-588">Если в образец включен дополнительный поставщик конфигурации JSON с отсутствующей парой "ключ — значение", то `ArrayExample.Entries` подойдет полному массиву конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-588">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="5366e-589">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="5366e-589">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="5366e-590">В `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="5366e-590">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="5366e-591">Пары "ключ — значение", показанные в таблице, загружаются в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-591">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="5366e-592">Ключ</span><span class="sxs-lookup"><span data-stu-id="5366e-592">Key</span></span>             | <span data-ttu-id="5366e-593">Значение</span><span class="sxs-lookup"><span data-stu-id="5366e-593">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="5366e-594">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="5366e-594">array:entries:3</span></span> | <span data-ttu-id="5366e-595">value3</span><span class="sxs-lookup"><span data-stu-id="5366e-595">value3</span></span> |

<span data-ttu-id="5366e-596">Если экземпляр класса `ArrayExample` связан после того, как поставщик конфигурации JSON включает запись для индекса &num;3, то массив `ArrayExample.Entries` включит значение.</span><span class="sxs-lookup"><span data-stu-id="5366e-596">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="5366e-597">Индекс `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="5366e-597">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="5366e-598">Значение `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="5366e-598">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="5366e-599">0</span><span class="sxs-lookup"><span data-stu-id="5366e-599">0</span></span>                            | <span data-ttu-id="5366e-600">value0</span><span class="sxs-lookup"><span data-stu-id="5366e-600">value0</span></span>                       |
| <span data-ttu-id="5366e-601">1</span><span class="sxs-lookup"><span data-stu-id="5366e-601">1</span></span>                            | <span data-ttu-id="5366e-602">value1</span><span class="sxs-lookup"><span data-stu-id="5366e-602">value1</span></span>                       |
| <span data-ttu-id="5366e-603">2</span><span class="sxs-lookup"><span data-stu-id="5366e-603">2</span></span>                            | <span data-ttu-id="5366e-604">value2</span><span class="sxs-lookup"><span data-stu-id="5366e-604">value2</span></span>                       |
| <span data-ttu-id="5366e-605">3</span><span class="sxs-lookup"><span data-stu-id="5366e-605">3</span></span>                            | <span data-ttu-id="5366e-606">value3</span><span class="sxs-lookup"><span data-stu-id="5366e-606">value3</span></span>                       |
| <span data-ttu-id="5366e-607">4</span><span class="sxs-lookup"><span data-stu-id="5366e-607">4</span></span>                            | <span data-ttu-id="5366e-608">value4</span><span class="sxs-lookup"><span data-stu-id="5366e-608">value4</span></span>                       |
| <span data-ttu-id="5366e-609">5</span><span class="sxs-lookup"><span data-stu-id="5366e-609">5</span></span>                            | <span data-ttu-id="5366e-610">value5</span><span class="sxs-lookup"><span data-stu-id="5366e-610">value5</span></span>                       |

<span data-ttu-id="5366e-611">**Обработка массива JSON**</span><span class="sxs-lookup"><span data-stu-id="5366e-611">**JSON array processing**</span></span>

<span data-ttu-id="5366e-612">Если JSON-файл содержит массив, ключи конфигурации будут созданы для элементов массива с индексом раздела, начиная с нуля.</span><span class="sxs-lookup"><span data-stu-id="5366e-612">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="5366e-613">В следующем файле конфигурации `subsection` — это массив.</span><span class="sxs-lookup"><span data-stu-id="5366e-613">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="5366e-614">Поставщик конфигурации JSON считывает данные конфигурации в следующие пары "ключ — значение".</span><span class="sxs-lookup"><span data-stu-id="5366e-614">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="5366e-615">Ключ</span><span class="sxs-lookup"><span data-stu-id="5366e-615">Key</span></span>                     | <span data-ttu-id="5366e-616">Значение</span><span class="sxs-lookup"><span data-stu-id="5366e-616">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="5366e-617">json_array:key</span><span class="sxs-lookup"><span data-stu-id="5366e-617">json_array:key</span></span>          | <span data-ttu-id="5366e-618">valueA</span><span class="sxs-lookup"><span data-stu-id="5366e-618">valueA</span></span> |
| <span data-ttu-id="5366e-619">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="5366e-619">json_array:subsection:0</span></span> | <span data-ttu-id="5366e-620">valueB</span><span class="sxs-lookup"><span data-stu-id="5366e-620">valueB</span></span> |
| <span data-ttu-id="5366e-621">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="5366e-621">json_array:subsection:1</span></span> | <span data-ttu-id="5366e-622">valueC</span><span class="sxs-lookup"><span data-stu-id="5366e-622">valueC</span></span> |
| <span data-ttu-id="5366e-623">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="5366e-623">json_array:subsection:2</span></span> | <span data-ttu-id="5366e-624">valueD</span><span class="sxs-lookup"><span data-stu-id="5366e-624">valueD</span></span> |

<span data-ttu-id="5366e-625">В примере приложения для привязки пар "ключ — значение" конфигурации доступен следующий класс POCO.</span><span class="sxs-lookup"><span data-stu-id="5366e-625">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5366e-626">После выполнения привязки `JsonArrayExample.Key` содержит значение `valueA`.</span><span class="sxs-lookup"><span data-stu-id="5366e-626">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="5366e-627">Подраздел значения хранится в свойстве массива POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="5366e-627">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="5366e-628">Индекс `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="5366e-628">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="5366e-629">Значение `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="5366e-629">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="5366e-630">0</span><span class="sxs-lookup"><span data-stu-id="5366e-630">0</span></span>                                   | <span data-ttu-id="5366e-631">valueB</span><span class="sxs-lookup"><span data-stu-id="5366e-631">valueB</span></span>                              |
| <span data-ttu-id="5366e-632">1</span><span class="sxs-lookup"><span data-stu-id="5366e-632">1</span></span>                                   | <span data-ttu-id="5366e-633">valueC</span><span class="sxs-lookup"><span data-stu-id="5366e-633">valueC</span></span>                              |
| <span data-ttu-id="5366e-634">2</span><span class="sxs-lookup"><span data-stu-id="5366e-634">2</span></span>                                   | <span data-ttu-id="5366e-635">valueD</span><span class="sxs-lookup"><span data-stu-id="5366e-635">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="5366e-636">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="5366e-636">Custom configuration provider</span></span>

<span data-ttu-id="5366e-637">Пример приложения демонстрирует, как создать базовый поставщик конфигурации, который считывает пары "ключ — значение" конфигурации из базы данных, используя [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="5366e-637">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="5366e-638">Поставщик имеет следующие характеристики.</span><span class="sxs-lookup"><span data-stu-id="5366e-638">The provider has the following characteristics:</span></span>

* <span data-ttu-id="5366e-639">База данных в памяти EF используется для демонстрационных целей.</span><span class="sxs-lookup"><span data-stu-id="5366e-639">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="5366e-640">Чтобы использовать базу данных, для которой требуется строка подключения, выполните вторичный `ConfigurationBuilder`, чтобы предоставить строку подключения от другого поставщика конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5366e-640">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="5366e-641">Поставщик считывает таблицу базы данных в конфигурации при запуске.</span><span class="sxs-lookup"><span data-stu-id="5366e-641">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="5366e-642">Поставщик не запрашивает базу данных для каждого ключа.</span><span class="sxs-lookup"><span data-stu-id="5366e-642">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="5366e-643">Функция перезагрузки на изменение не реализована, поэтому обновление базы данных после запуска приложения не влияет на конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="5366e-643">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="5366e-644">Определите сущность `EFConfigurationValue` для хранения значений конфигурации в базе данных.</span><span class="sxs-lookup"><span data-stu-id="5366e-644">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5366e-645">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="5366e-645">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="5366e-646">Добавьте `EFConfigurationContext` в хранилище и обратитесь к настроенным значениям.</span><span class="sxs-lookup"><span data-stu-id="5366e-646">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="5366e-647">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="5366e-647">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="5366e-648">Создайте класс, реализующий <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="5366e-648">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="5366e-649">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="5366e-649">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="5366e-650">Создайте пользовательский поставщик конфигурации путем наследования от <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="5366e-650">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="5366e-651">Поставщик конфигурации инициализирует пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="5366e-651">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="5366e-652">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="5366e-652">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="5366e-653">Метод расширения `AddEFConfiguration` позволяет добавить источник конфигурации к `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5366e-653">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="5366e-654">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="5366e-654">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="5366e-655">В следующем коде показано, как использовать пользовательский `EFConfigurationProvider` в *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="5366e-655">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=30-31)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5366e-656">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="5366e-656">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="5366e-657">Добавьте `EFConfigurationContext` в хранилище и обратитесь к настроенным значениям.</span><span class="sxs-lookup"><span data-stu-id="5366e-657">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="5366e-658">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="5366e-658">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="5366e-659">Создайте класс, реализующий <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="5366e-659">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="5366e-660">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="5366e-660">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="5366e-661">Создайте пользовательский поставщик конфигурации путем наследования от <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="5366e-661">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="5366e-662">Поставщик конфигурации инициализирует пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="5366e-662">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="5366e-663">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="5366e-663">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="5366e-664">Метод расширения `AddEFConfiguration` позволяет добавить источник конфигурации к `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5366e-664">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="5366e-665">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="5366e-665">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="5366e-666">В следующем коде показано, как использовать пользовательский `EFConfigurationProvider` в *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="5366e-666">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=30-31)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="5366e-667">Доступ к конфигурации во время запуска</span><span class="sxs-lookup"><span data-stu-id="5366e-667">Access configuration during startup</span></span>

<span data-ttu-id="5366e-668">Внесите значение `IConfiguration` в конструктор `Startup` для доступа к значениям конфигурации в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5366e-668">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5366e-669">Чтобы получить доступ к конфигурации в `Startup.Configure`, либо добавьте значение `IConfiguration` непосредственно в метод, либо используйте экземпляр из конструктора.</span><span class="sxs-lookup"><span data-stu-id="5366e-669">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="5366e-670">Дополнительные сведения см. в руководстве по [доступу к конфигурации с использованием удобных методов](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="5366e-670">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="5366e-671">Настройте доступ на странице Razor Pages или в представлении MVC</span><span class="sxs-lookup"><span data-stu-id="5366e-671">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="5366e-672">Для доступа к параметрам конфигурации на странице Razor Pages или в представлении MVC добавьте [использование директивы](xref:mvc/views/razor#using) ([Справочник по C#: использование директивы](/dotnet/csharp/language-reference/keywords/using-directive)) для [пространства имен Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) и вставьте <xref:Microsoft.Extensions.Configuration.IConfiguration> на страницу или в представление.</span><span class="sxs-lookup"><span data-stu-id="5366e-672">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="5366e-673">На странице Razor Pages</span><span class="sxs-lookup"><span data-stu-id="5366e-673">In a Razor Pages page:</span></span>

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

<span data-ttu-id="5366e-674">В представлении MVC</span><span class="sxs-lookup"><span data-stu-id="5366e-674">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="5366e-675">Добавление конфигурации из внешней сборки</span><span class="sxs-lookup"><span data-stu-id="5366e-675">Add configuration from an external assembly</span></span>

<span data-ttu-id="5366e-676">Реализация <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> позволяет при запуске добавлять в приложение улучшения из внешней сборки вне приложения класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="5366e-676">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="5366e-677">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="5366e-677">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5366e-678">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="5366e-678">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
