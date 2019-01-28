---
title: Конфигурация в .NET Core
author: guardrex
description: Узнайте, как использовать API конфигурации для настройки приложения ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/25/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 2465570e469020ae2855508bd1bfc8528e188ebb
ms.sourcegitcommit: ca5f03210bedc61c6639a734ae5674bfe095dee8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/26/2019
ms.locfileid: "55073170"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="75f58-103">Конфигурация в .NET Core</span><span class="sxs-lookup"><span data-stu-id="75f58-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="75f58-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="75f58-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="75f58-105">Конфигурация приложения в ASP.NET Core основана на парах "ключ — значение", установленных *поставщиками конфигурации*.</span><span class="sxs-lookup"><span data-stu-id="75f58-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="75f58-106">Поставщики конфигурации получают данные конфигурации в парах "ключ — значение" из различных источников:</span><span class="sxs-lookup"><span data-stu-id="75f58-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="75f58-107">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="75f58-107">Azure Key Vault</span></span>
* <span data-ttu-id="75f58-108">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="75f58-108">Command-line arguments</span></span>
* <span data-ttu-id="75f58-109">а также пользовательские поставщики (устанавливаемые или создаваемые).</span><span class="sxs-lookup"><span data-stu-id="75f58-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="75f58-110">Справочные файлы</span><span class="sxs-lookup"><span data-stu-id="75f58-110">Directory files</span></span>
* <span data-ttu-id="75f58-111">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="75f58-111">Environment variables</span></span>
* <span data-ttu-id="75f58-112">объектов .NET в памяти;</span><span class="sxs-lookup"><span data-stu-id="75f58-112">In-memory .NET objects</span></span>
* <span data-ttu-id="75f58-113">файлов параметров;</span><span class="sxs-lookup"><span data-stu-id="75f58-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="75f58-114">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="75f58-114">Azure Key Vault</span></span>
* <span data-ttu-id="75f58-115">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="75f58-115">Command-line arguments</span></span>
* <span data-ttu-id="75f58-116">а также пользовательские поставщики (устанавливаемые или создаваемые).</span><span class="sxs-lookup"><span data-stu-id="75f58-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="75f58-117">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="75f58-117">Environment variables</span></span>
* <span data-ttu-id="75f58-118">объектов .NET в памяти;</span><span class="sxs-lookup"><span data-stu-id="75f58-118">In-memory .NET objects</span></span>
* <span data-ttu-id="75f58-119">файлов параметров;</span><span class="sxs-lookup"><span data-stu-id="75f58-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="75f58-120">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="75f58-120">Command-line arguments</span></span>
* <span data-ttu-id="75f58-121">а также пользовательские поставщики (устанавливаемые или создаваемые).</span><span class="sxs-lookup"><span data-stu-id="75f58-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="75f58-122">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="75f58-122">Environment variables</span></span>
* <span data-ttu-id="75f58-123">объектов .NET в памяти;</span><span class="sxs-lookup"><span data-stu-id="75f58-123">In-memory .NET objects</span></span>
* <span data-ttu-id="75f58-124">файлов параметров;</span><span class="sxs-lookup"><span data-stu-id="75f58-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="75f58-125">*Шаблон параметров* является расширением конфигурации основных понятий, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="75f58-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="75f58-126">Параметры используют классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="75f58-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="75f58-127">Дополнительные сведения об использовании шаблона параметров см. в разделе <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="75f58-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="75f58-128">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="75f58-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="75f58-129">Эти три пакета включены в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="75f58-129">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="75f58-130">Эти три пакета включены в [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="75f58-130">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="75f58-131">Конфигурация узла и приложения</span><span class="sxs-lookup"><span data-stu-id="75f58-131">Host vs. app configuration</span></span>

<span data-ttu-id="75f58-132">Перед настройкой и запуском приложения настройте и запустите *узел*.</span><span class="sxs-lookup"><span data-stu-id="75f58-132">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="75f58-133">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="75f58-133">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="75f58-134">Как приложение, так и узел настраиваются с использованием поставщиков конфигурации, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="75f58-134">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="75f58-135">Пары "ключ — значение" конфигурации узлов становятся частью глобальной конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="75f58-135">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="75f58-136">Дополнительные сведения о том, как используются поставщики конфигурации при создании узла и как влияют источники конфигурации на узел, см. в разделе <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="75f58-136">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="75f58-137">Конфигурация по умолчанию</span><span class="sxs-lookup"><span data-stu-id="75f58-137">Default configuration</span></span>

<span data-ttu-id="75f58-138">Веб-приложения на основе шаблонов [dotnet new](/dotnet/core/tools/dotnet-new) ASP.NET Core вызывают <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> при создании узла.</span><span class="sxs-lookup"><span data-stu-id="75f58-138">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="75f58-139">`CreateDefaultBuilder` предоставляет конфигурацию по умолчанию для приложения.</span><span class="sxs-lookup"><span data-stu-id="75f58-139">`CreateDefaultBuilder` provides default configuration for the app.</span></span>

* <span data-ttu-id="75f58-140">Существуют следующие способы предоставления конфигурации узла.</span><span class="sxs-lookup"><span data-stu-id="75f58-140">Host configuration is provided from:</span></span>
  * <span data-ttu-id="75f58-141">Переменные среды с префиксом `ASPNETCORE_` (например, `ASPNETCORE_ENVIRONMENT`), использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="75f58-141">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="75f58-142">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="75f58-142">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="75f58-143">Существуют следующие способы предоставления конфигурации приложений (в указанном порядке).</span><span class="sxs-lookup"><span data-stu-id="75f58-143">App configuration is provided from (in the following order):</span></span>
  * <span data-ttu-id="75f58-144">Файл *appsettings.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="75f58-144">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="75f58-145">Файл *appsettings.{Environment}.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="75f58-145">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="75f58-146">[Менеджера секретов](xref:security/app-secrets), когда приложение выполняется в среде `Development` с использованием начальных сборок.</span><span class="sxs-lookup"><span data-stu-id="75f58-146">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="75f58-147">Переменные среды, использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="75f58-147">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="75f58-148">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="75f58-148">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="75f58-149">Поставщики конфигурации описаны ниже в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="75f58-149">The configuration providers are explained later in this topic.</span></span> <span data-ttu-id="75f58-150">Дополнительные сведения об узле и `CreateDefaultBuilder`: <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="75f58-150">For more information on the host and `CreateDefaultBuilder`, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="security"></a><span data-ttu-id="75f58-151">Безопасность</span><span class="sxs-lookup"><span data-stu-id="75f58-151">Security</span></span>

<span data-ttu-id="75f58-152">Придерживайтесь следующих рекомендаций.</span><span class="sxs-lookup"><span data-stu-id="75f58-152">Adopt the following best practices:</span></span>

* <span data-ttu-id="75f58-153">Никогда не храните пароли или другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации обычного текста.</span><span class="sxs-lookup"><span data-stu-id="75f58-153">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="75f58-154">Не используйте секреты рабочей среды в средах разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="75f58-154">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="75f58-155">Указывайте секреты вне проекта, чтобы их нельзя было случайно зафиксировать в репозитории с исходным кодом.</span><span class="sxs-lookup"><span data-stu-id="75f58-155">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="75f58-156">Дополнительные сведения см. в статье [Использование нескольких сред в ASP.NET Core](xref:fundamentals/environments) и руководствуйтесь статьей [Безопасное хранение секретов приложения во время разработки в ASP.NET Core](xref:security/app-secrets) (включает рекомендации по использованию переменной среды для хранения конфиденциальных данных).</span><span class="sxs-lookup"><span data-stu-id="75f58-156">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="75f58-157">Менеджер секретов использует поставщик конфигурации файла для хранения конфиденциальных данных пользователя в файле JSON в локальной системе.</span><span class="sxs-lookup"><span data-stu-id="75f58-157">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="75f58-158">Поставщик конфигурации файлов описан ниже в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="75f58-158">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="75f58-159">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) — один из вариантов для безопасного хранения секретов приложения.</span><span class="sxs-lookup"><span data-stu-id="75f58-159">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="75f58-160">Для получения дополнительной информации см. <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="75f58-160">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="75f58-161">Иерархическая модель конфигурации</span><span class="sxs-lookup"><span data-stu-id="75f58-161">Hierarchical configuration data</span></span>

<span data-ttu-id="75f58-162">API конфигурации способен поддерживать иерархические данные конфигурации, выполняя преобразование в плоскую структуру иерархических данных с использованием разделителя в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="75f58-162">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="75f58-163">В следующем файле JSON существуют четыре ключа в структурированной иерархии двух разделов.</span><span class="sxs-lookup"><span data-stu-id="75f58-163">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="75f58-164">При считывании файла в конфигурацию для сохранения исходной иерархической структуры данных источника конфигурации создаются уникальные ключи.</span><span class="sxs-lookup"><span data-stu-id="75f58-164">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="75f58-165">Разделы и ключи преобразовываются в плоскую структуру с использованием двоеточия (`:`) для сохранения исходной структуры.</span><span class="sxs-lookup"><span data-stu-id="75f58-165">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="75f58-166">section0:key0</span><span class="sxs-lookup"><span data-stu-id="75f58-166">section0:key0</span></span>
* <span data-ttu-id="75f58-167">section0:key1</span><span class="sxs-lookup"><span data-stu-id="75f58-167">section0:key1</span></span>
* <span data-ttu-id="75f58-168">section1:key0</span><span class="sxs-lookup"><span data-stu-id="75f58-168">section1:key0</span></span>
* <span data-ttu-id="75f58-169">section1:key1</span><span class="sxs-lookup"><span data-stu-id="75f58-169">section1:key1</span></span>

<span data-ttu-id="75f58-170">Методы <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> и <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> доступны для изолирования разделов и дочерних элементов раздела в данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="75f58-170"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="75f58-171">Эти методы описаны далее в разделе [GetSection, GetChildren и Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="75f58-171">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span> <span data-ttu-id="75f58-172">`GetSection` находится в пакете [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="75f58-172">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="conventions"></a><span data-ttu-id="75f58-173">Соглашения</span><span class="sxs-lookup"><span data-stu-id="75f58-173">Conventions</span></span>

<span data-ttu-id="75f58-174">При запуске приложения источники конфигурации считываются в порядке, в котором были указаны их поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="75f58-174">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="75f58-175">Поставщики файлов конфигурации имеют возможность перезагрузить конфигурацию при изменении базового файла параметров после запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="75f58-175">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="75f58-176">Поставщик конфигурации файлов описан ниже в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="75f58-176">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="75f58-177"><xref:Microsoft.Extensions.Configuration.IConfiguration> доступен в контейнере [Dependency Injection (DI)](xref:fundamentals/dependency-injection) приложения.</span><span class="sxs-lookup"><span data-stu-id="75f58-177"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="75f58-178">Поставщики конфигурации не могут использовать контейнер DI, так как он недоступен при настройке узла.</span><span class="sxs-lookup"><span data-stu-id="75f58-178">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="75f58-179">В ключах конфигурации приняты следующие соглашения.</span><span class="sxs-lookup"><span data-stu-id="75f58-179">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="75f58-180">В ключах не учитывается регистр символов.</span><span class="sxs-lookup"><span data-stu-id="75f58-180">Keys are case-insensitive.</span></span> <span data-ttu-id="75f58-181">Например `ConnectionString` и `connectionstring` обрабатываются как эквивалентные ключи.</span><span class="sxs-lookup"><span data-stu-id="75f58-181">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="75f58-182">Если значение для одного и того же ключа установлено одним и тем же или разными поставщиками конфигурации, последним значением, установленным на ключе, является используемое значение.</span><span class="sxs-lookup"><span data-stu-id="75f58-182">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="75f58-183">Иерархические ключи</span><span class="sxs-lookup"><span data-stu-id="75f58-183">Hierarchical keys</span></span>
  * <span data-ttu-id="75f58-184">При взаимодействии с API конфигурации разделитель-двоеточие (`:`) поддерживается на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="75f58-184">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="75f58-185">В переменных среды разделитель-двоеточие может не работать на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="75f58-185">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="75f58-186">Двойной знак подчеркивания (`__`) поддерживается на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="75f58-186">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="75f58-187">В хранилище ключей Azure иерархические ключи используют `--` (два дефиса) в качестве разделителя.</span><span class="sxs-lookup"><span data-stu-id="75f58-187">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="75f58-188">Чтобы заменить дефисы двоеточием, при загрузке секретов в конфигурацию приложения необходимо указать код.</span><span class="sxs-lookup"><span data-stu-id="75f58-188">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="75f58-189"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="75f58-189">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="75f58-190">Привязка массива описана в разделе [Привязка массива к классу](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="75f58-190">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="75f58-191">В значениях конфигурации учитываются следующие соглашения.</span><span class="sxs-lookup"><span data-stu-id="75f58-191">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="75f58-192">Значения являются строками.</span><span class="sxs-lookup"><span data-stu-id="75f58-192">Values are strings.</span></span>
* <span data-ttu-id="75f58-193">Значение NULL не может храниться в конфигурации или быть привязанным к объектам.</span><span class="sxs-lookup"><span data-stu-id="75f58-193">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="75f58-194">Поставщики</span><span class="sxs-lookup"><span data-stu-id="75f58-194">Providers</span></span>

<span data-ttu-id="75f58-195">В следующей таблице показаны поставщики конфигурации, доступные для приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="75f58-195">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="75f58-196">Поставщик</span><span class="sxs-lookup"><span data-stu-id="75f58-196">Provider</span></span> | <span data-ttu-id="75f58-197">Предоставляет конфигурацию из &hellip;</span><span class="sxs-lookup"><span data-stu-id="75f58-197">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="75f58-198">[Поставщик конфигурации хранилища ключей Azure](xref:security/key-vault-configuration) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="75f58-198">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="75f58-199">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="75f58-199">Azure Key Vault</span></span> |
| [<span data-ttu-id="75f58-200">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="75f58-200">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="75f58-201">Параметры командной строки</span><span class="sxs-lookup"><span data-stu-id="75f58-201">Command-line parameters</span></span> |
| [<span data-ttu-id="75f58-202">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="75f58-202">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="75f58-203">Источник пользователя</span><span class="sxs-lookup"><span data-stu-id="75f58-203">Custom source</span></span> |
| [<span data-ttu-id="75f58-204">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="75f58-204">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="75f58-205">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="75f58-205">Environment variables</span></span> |
| [<span data-ttu-id="75f58-206">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="75f58-206">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="75f58-207">Файлы (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="75f58-207">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="75f58-208">Поставщик конфигурации ключа для каждого файла</span><span class="sxs-lookup"><span data-stu-id="75f58-208">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="75f58-209">Справочные файлы</span><span class="sxs-lookup"><span data-stu-id="75f58-209">Directory files</span></span> |
| [<span data-ttu-id="75f58-210">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="75f58-210">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="75f58-211">Коллекции оперативной памяти</span><span class="sxs-lookup"><span data-stu-id="75f58-211">In-memory collections</span></span> |
| <span data-ttu-id="75f58-212">[Секреты пользователей (Менеджер секретов)](xref:security/app-secrets) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="75f58-212">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="75f58-213">Файл в каталоге профиля пользователя</span><span class="sxs-lookup"><span data-stu-id="75f58-213">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="75f58-214">Поставщик</span><span class="sxs-lookup"><span data-stu-id="75f58-214">Provider</span></span> | <span data-ttu-id="75f58-215">Предоставляет конфигурацию из &hellip;</span><span class="sxs-lookup"><span data-stu-id="75f58-215">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="75f58-216">[Поставщик конфигурации хранилища ключей Azure](xref:security/key-vault-configuration) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="75f58-216">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="75f58-217">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="75f58-217">Azure Key Vault</span></span> |
| [<span data-ttu-id="75f58-218">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="75f58-218">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="75f58-219">Параметры командной строки</span><span class="sxs-lookup"><span data-stu-id="75f58-219">Command-line parameters</span></span> |
| [<span data-ttu-id="75f58-220">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="75f58-220">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="75f58-221">Источник пользователя</span><span class="sxs-lookup"><span data-stu-id="75f58-221">Custom source</span></span> |
| [<span data-ttu-id="75f58-222">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="75f58-222">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="75f58-223">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="75f58-223">Environment variables</span></span> |
| [<span data-ttu-id="75f58-224">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="75f58-224">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="75f58-225">Файлы (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="75f58-225">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="75f58-226">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="75f58-226">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="75f58-227">Коллекции оперативной памяти</span><span class="sxs-lookup"><span data-stu-id="75f58-227">In-memory collections</span></span> |
| <span data-ttu-id="75f58-228">[Секреты пользователей (Менеджер секретов)](xref:security/app-secrets) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="75f58-228">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="75f58-229">Файл в каталоге профиля пользователя</span><span class="sxs-lookup"><span data-stu-id="75f58-229">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="75f58-230">Поставщик</span><span class="sxs-lookup"><span data-stu-id="75f58-230">Provider</span></span> | <span data-ttu-id="75f58-231">Предоставляет конфигурацию из &hellip;</span><span class="sxs-lookup"><span data-stu-id="75f58-231">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="75f58-232">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="75f58-232">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="75f58-233">Параметры командной строки</span><span class="sxs-lookup"><span data-stu-id="75f58-233">Command-line parameters</span></span> |
| [<span data-ttu-id="75f58-234">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="75f58-234">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="75f58-235">Источник пользователя</span><span class="sxs-lookup"><span data-stu-id="75f58-235">Custom source</span></span> |
| [<span data-ttu-id="75f58-236">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="75f58-236">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="75f58-237">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="75f58-237">Environment variables</span></span> |
| [<span data-ttu-id="75f58-238">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="75f58-238">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="75f58-239">Файлы (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="75f58-239">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="75f58-240">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="75f58-240">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="75f58-241">Коллекции оперативной памяти</span><span class="sxs-lookup"><span data-stu-id="75f58-241">In-memory collections</span></span> |
| <span data-ttu-id="75f58-242">[Секреты пользователей (Менеджер секретов)](xref:security/app-secrets) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="75f58-242">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="75f58-243">Файл в каталоге профиля пользователя</span><span class="sxs-lookup"><span data-stu-id="75f58-243">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="75f58-244">Источники конфигурации считываются в том порядке, в котором при запуске указываются их поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="75f58-244">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="75f58-245">Поставщики конфигурации в этом разделе описаны в алфавитном порядке, а не в необходимом вам порядке в коде.</span><span class="sxs-lookup"><span data-stu-id="75f58-245">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="75f58-246">Порядок поставщиков конфигурации в коде соответствует приоритетам ваших основных источников конфигурации.</span><span class="sxs-lookup"><span data-stu-id="75f58-246">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="75f58-247">Типичная последовательность поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="75f58-247">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="75f58-248">Файлы (*appsettings.json*, *appsettings.{Environment}.json*, где `{Environment}` — это текущая среда размещения приложения)</span><span class="sxs-lookup"><span data-stu-id="75f58-248">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. <span data-ttu-id="75f58-249">[Azure Key Vault](xref:security/key-vault-configuration);</span><span class="sxs-lookup"><span data-stu-id="75f58-249">[Azure Key Vault](xref:security/key-vault-configuration)</span></span>
1. <span data-ttu-id="75f58-250">[Секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки)</span><span class="sxs-lookup"><span data-stu-id="75f58-250">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="75f58-251">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="75f58-251">Environment variables</span></span>
1. <span data-ttu-id="75f58-252">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="75f58-252">Command-line arguments</span></span>

<span data-ttu-id="75f58-253">Общепринятой практикой является размещение поставщика конфигурации командной строки последним в ряду поставщиков, чтобы аргументы командной строки могли переопределять конфигурацию, установленную другими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="75f58-253">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="75f58-254">Эта последовательность поставщиков помещается в месте, где происходит инициализация нового класса <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-254">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="75f58-255">Дополнительные сведения см. в руководстве по [настройке веб-узла](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="75f58-255">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="75f58-256">Эта последовательность поставщиков может быть создана для приложения (а не узла) с помощью <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> и вызова метода <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> в `Startup`.</span><span class="sxs-lookup"><span data-stu-id="75f58-256">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

```csharp
public Startup(IHostingEnvironment env)
{
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
        .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
            reloadOnChange: true);

    var appAssembly = Assembly.Load(new AssemblyName(env.ApplicationName));

    if (appAssembly != null)
    {
        builder.AddUserSecrets(appAssembly, optional: true);
    }

    builder.AddEnvironmentVariables();

    Configuration = builder.Build();
}

public IConfiguration Configuration { get; }

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IConfiguration>(Configuration);
}
```

<span data-ttu-id="75f58-257">В приведенном выше примере имя среды (`env.EnvironmentName`) и имя сборки приложения (`env.ApplicationName`) предоставляются <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="75f58-257">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="75f58-258">Для получения дополнительной информации см. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="75f58-258">For more information, see <xref:fundamentals/environments>.</span></span> <span data-ttu-id="75f58-259">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-259">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="75f58-260">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="75f58-260">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
<span data-ttu-id="75f58-261">.</span><span class="sxs-lookup"><span data-stu-id="75f58-261">.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a><span data-ttu-id="75f58-262">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="75f58-262">ConfigureAppConfiguration</span></span>

<span data-ttu-id="75f58-263">Вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> при сборке узла, чтобы указать поставщики конфигурации приложения в дополнение к тем, которые автоматически добавлены <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-263">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="75f58-264">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="75f58-264">Command-line Configuration Provider</span></span>

<span data-ttu-id="75f58-265"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> загружает конфигурацию из пары "ключ — значение" аргумента командной строки в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="75f58-265">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="75f58-266">Чтобы активировать конфигурацию командной строки, вызывается метод расширения <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> для экземпляра <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="75f58-266">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="75f58-267">`AddCommandLine` автоматически вызывается при инициализации нового <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-267">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="75f58-268">Дополнительные сведения см. в руководстве по [настройке веб-узла](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="75f58-268">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="75f58-269">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="75f58-269">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="75f58-270">дополнительную конфигурацию из *appsettings.json* и *appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="75f58-270">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="75f58-271">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки);</span><span class="sxs-lookup"><span data-stu-id="75f58-271">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="75f58-272">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="75f58-272">Environment variables.</span></span>

<span data-ttu-id="75f58-273">`CreateDefaultBuilder` добавляет последним поставщика конфигурации командной строки.</span><span class="sxs-lookup"><span data-stu-id="75f58-273">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="75f58-274">Аргументы командной строки передаются в набор конфигурации переопределения среды выполнения, установленной другими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="75f58-274">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="75f58-275">`CreateDefaultBuilder` действует, когда создается узел.</span><span class="sxs-lookup"><span data-stu-id="75f58-275">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="75f58-276">Поэтому конфигурация командной строки, активированная с помощью `CreateDefaultBuilder`, может повлиять на настройку узла.</span><span class="sxs-lookup"><span data-stu-id="75f58-276">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="75f58-277">Чтобы указать конфигурацию приложения, при сборке узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-277">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="75f58-278">Объект `AddCommandLine` уже был вызван методом `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="75f58-278">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="75f58-279">Если нужно предоставить конфигурацию приложения, сохранив возможность переопределить ее через аргументы командной строки, вызовите в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>дополнительные поставщики приложения, в завершение вызвав `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="75f58-279">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call other providers here and call AddCommandLine last.
                config.AddCommandLine(args);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="75f58-280">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="75f58-280">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="75f58-281">Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-281">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="75f58-282">Объект `AddCommandLine` уже был вызван методом `CreateDefaultBuilder` при вызове `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="75f58-282">`AddCommandLine` has already been called by `CreateDefaultBuilder` when `UseConfiguration` is called.</span></span> <span data-ttu-id="75f58-283">Если нужно предоставить конфигурацию приложения, сохранив возможность переопределить ее через аргументы командной строки, вызовите в `ConfigurationBuilder`дополнительные поставщики приложения, в завершение вызвав `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="75f58-283">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers on a `ConfigurationBuilder` and call `AddCommandLine` last.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            // Call other providers here and call AddCommandLine last.
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="75f58-284">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="75f58-284">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="75f58-285">Чтобы активировать конфигурацию командной строки, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="75f58-285">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="75f58-286">В этом случае аргументы командной строки, передаваемые во время выполнения, переопределяют конфигурацию, заданную другими вызванными ранее поставщиками конфигурации.</span><span class="sxs-lookup"><span data-stu-id="75f58-286">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="75f58-287">Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="75f58-287">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    // Call additional providers here as needed.
    // Call AddCommandLine last to allow arguments to override other configuration.
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="75f58-288">**Пример**</span><span class="sxs-lookup"><span data-stu-id="75f58-288">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="75f58-289">Пример приложения 2.x использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-289">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="75f58-290">Пример приложения 1.x вызывает образец <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> в <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="75f58-290">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="75f58-291">Откройте командную строку в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="75f58-291">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="75f58-292">Поставьте аргумент командной строки в команду `dotnet run`: `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="75f58-292">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="75f58-293">После запуска приложения откройте браузер в приложении по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="75f58-293">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="75f58-294">Обратите внимание, что вывод содержит пару "ключ — значение" для аргумента командной строки конфигурации, предоставленного для `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="75f58-294">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="75f58-295">Аргументы</span><span class="sxs-lookup"><span data-stu-id="75f58-295">Arguments</span></span>

<span data-ttu-id="75f58-296">Значение должно соответствовать знаку равенства (`=`), или ключ должен иметь префикс (`--` или `/`), когда значение следует за пробелом.</span><span class="sxs-lookup"><span data-stu-id="75f58-296">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="75f58-297">Значение может соответствовать NULL, если используется знак равенства (например, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="75f58-297">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="75f58-298">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="75f58-298">Key prefix</span></span>               | <span data-ttu-id="75f58-299">Пример</span><span class="sxs-lookup"><span data-stu-id="75f58-299">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="75f58-300">Без префикса</span><span class="sxs-lookup"><span data-stu-id="75f58-300">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="75f58-301">Два дефиса (`--`)</span><span class="sxs-lookup"><span data-stu-id="75f58-301">Two dashes (`--`)</span></span>        | <span data-ttu-id="75f58-302">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="75f58-302">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="75f58-303">Прямая косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="75f58-303">Forward slash (`/`)</span></span>      | <span data-ttu-id="75f58-304">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="75f58-304">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="75f58-305">В рамках одной и той же команды не смешивайте пары "ключ — значение" аргумента командной строки, которые используют знак равенства, с парами "ключ — значение", которые используют пробел.</span><span class="sxs-lookup"><span data-stu-id="75f58-305">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="75f58-306">Примеры команд.</span><span class="sxs-lookup"><span data-stu-id="75f58-306">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="75f58-307">Сопоставления переключений</span><span class="sxs-lookup"><span data-stu-id="75f58-307">Switch mappings</span></span>

<span data-ttu-id="75f58-308">Сопоставление параметров позволяет указать логику замены имен ключей.</span><span class="sxs-lookup"><span data-stu-id="75f58-308">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="75f58-309">Когда вручную создается конфигурация с помощью <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, методу <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> можно предоставить словарь сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="75f58-309">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="75f58-310">В словаре сопоставлений переключений выполняется поиск ключа, который совпадает с ключом, предоставляемым аргументом командной строки.</span><span class="sxs-lookup"><span data-stu-id="75f58-310">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="75f58-311">Если ключ в командной строке находится в словаре, значение словаря (замена ключа) передается обратно, чтобы установить пару "ключ — значение" в конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="75f58-311">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="75f58-312">Сопоставление переключений необходимо для любого ключа командной строки с префиксом из одного дефиса (`-`).</span><span class="sxs-lookup"><span data-stu-id="75f58-312">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="75f58-313">Правила ключей из словаря сопоставления переключений:</span><span class="sxs-lookup"><span data-stu-id="75f58-313">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="75f58-314">Переключения должны начинаться с дефиса (`-`) или двойного дефиса (`--`).</span><span class="sxs-lookup"><span data-stu-id="75f58-314">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="75f58-315">Словарь сопоставлений переключений не должен содержать повторяющиеся ключи.</span><span class="sxs-lookup"><span data-stu-id="75f58-315">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="75f58-316">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-316">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _switchMappings = 
        new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    // Do not pass the args to CreateDefaultBuilder
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder()
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddCommandLine(args, _switchMappings);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="75f58-317">Как показано в предыдущем примере, вызов `CreateDefaultBuilder` не должен передавать аргументы при использовании сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="75f58-317">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="75f58-318">Вызов команды `AddCommandLine` метода `CreateDefaultBuilder` не включает сопоставленные коммутаторы, и нет возможности передать словарь сопоставления переключений в `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="75f58-318">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="75f58-319">Если аргументы включают сопоставления переключений и передаются в `CreateDefaultBuilder`, его поставщик `AddCommandLine` не может инициализироваться с помощью <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="75f58-319">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="75f58-320">Чтобы решить эту проблему, нужно не передавать аргументы команде `CreateDefaultBuilder`, а позволить методу `AddCommandLine` метода `ConfigurationBuilder` обрабатывать как аргументы, так и словарь сопоставления параметров.</span><span class="sxs-lookup"><span data-stu-id="75f58-320">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var switchMappings = new Dictionary<string, string>
            {
                { "-CLKey1", "CommandLineKey1" },
                { "-CLKey2", "CommandLineKey2" }
            };

        var config = new ConfigurationBuilder()
            .AddCommandLine(args, switchMappings)
            .Build();

        // Do not pass the args to CreateDefaultBuilder
        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="75f58-321">Как показано в предыдущем примере, вызов `CreateDefaultBuilder` не должен передавать аргументы при использовании сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="75f58-321">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="75f58-322">Вызов команды `AddCommandLine` метода `CreateDefaultBuilder` не включает сопоставленные коммутаторы, и нет возможности передать словарь сопоставления переключений в `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="75f58-322">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="75f58-323">Если аргументы включают сопоставления переключений и передаются в `CreateDefaultBuilder`, его поставщик `AddCommandLine` не может инициализироваться с помощью <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="75f58-323">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="75f58-324">Чтобы решить эту проблему, нужно не передавать аргументы команде `CreateDefaultBuilder`, а позволить методу `AddCommandLine` метода `ConfigurationBuilder` обрабатывать как аргументы, так и словарь сопоставления параметров.</span><span class="sxs-lookup"><span data-stu-id="75f58-324">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    var switchMappings = new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    var config = new ConfigurationBuilder()
        .AddCommandLine(args, switchMappings)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .UseStartup<Startup>()
        .Start();

    using (host)
    {
        Console.ReadLine();
    }
}
```

::: moniker-end

<span data-ttu-id="75f58-325">Созданный словарь сопоставлений переключений содержит данные, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="75f58-325">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="75f58-326">Ключ</span><span class="sxs-lookup"><span data-stu-id="75f58-326">Key</span></span>       | <span data-ttu-id="75f58-327">Значение</span><span class="sxs-lookup"><span data-stu-id="75f58-327">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="75f58-328">Если ключи сопоставления переключений используются при запуске приложения, конфигурация принимает значение конфигурации в ключе, предоставленном в словаре.</span><span class="sxs-lookup"><span data-stu-id="75f58-328">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="75f58-329">После выполнения предыдущей команды конфигурация содержит значения, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="75f58-329">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="75f58-330">Ключ</span><span class="sxs-lookup"><span data-stu-id="75f58-330">Key</span></span>               | <span data-ttu-id="75f58-331">Значение</span><span class="sxs-lookup"><span data-stu-id="75f58-331">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="75f58-332">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="75f58-332">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="75f58-333"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> загружает конфигурацию из пары "ключ — значение" переменной среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="75f58-333">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="75f58-334">Чтобы активировать конфигурацию переменных среды, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="75f58-334">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="75f58-335">В переменных средах разделитель-двоеточие (`:`) может не работать на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="75f58-335">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="75f58-336">Двойной знак подчеркивания (`__`) поддерживается на всех платформах и его можно заменить двоеточием.</span><span class="sxs-lookup"><span data-stu-id="75f58-336">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="75f58-337">[Служба приложений Azure](https://azure.microsoft.com/services/app-service/) позволяет задать переменные среды на портале Azure, который может переопределить конфигурацию приложения, используя поставщик конфигурации переменных среды.</span><span class="sxs-lookup"><span data-stu-id="75f58-337">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="75f58-338">Дополнительные сведения см. в руководстве по [переопределению конфигурации приложения Azure с помощью портала Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="75f58-338">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="75f58-339">`AddEnvironmentVariables` автоматически вызывается для переменных среды, имеющих префикс `ASPNETCORE_`, при инициализации нового экземпляра <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="75f58-339">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="75f58-340">Дополнительные сведения см. в руководстве по [настройке веб-узла](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="75f58-340">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="75f58-341">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="75f58-341">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="75f58-342">конфигурация приложения на основе переменных среды без префикса путем вызова `AddEnvironmentVariables` без префикса;</span><span class="sxs-lookup"><span data-stu-id="75f58-342">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="75f58-343">дополнительную конфигурацию из *appsettings.json* и *appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="75f58-343">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="75f58-344">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки);</span><span class="sxs-lookup"><span data-stu-id="75f58-344">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="75f58-345">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="75f58-345">Command-line arguments.</span></span>

<span data-ttu-id="75f58-346">Поставщик конфигурации переменных среды вызывается после выполнения настройки с помощью секретов пользователя и файлов *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="75f58-346">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="75f58-347">Вызов поставщика в этой позиции разрешает чтение переменных среды выполнения, чтобы переопределить конфигурацию, заданную секретом пользователя и файлом *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="75f58-347">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="75f58-348">Чтобы указать конфигурацию приложения, при сборке узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-348">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="75f58-349">Если вам нужно добавить конфигурацию приложения из дополнительных переменных среды, вызовите дополнительные поставщики приложения в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, а затем вызовите `AddEnvironmentVariables` с префиксом.</span><span class="sxs-lookup"><span data-stu-id="75f58-349">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call additional providers here as needed.
                // Call AddEnvironmentVariables last if you need to allow environment
                // variables to override values from other providers.
                config.AddEnvironmentVariables(prefix: "PREFIX_");
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="75f58-350">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="75f58-350">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="75f58-351">Вызовите метод расширения `AddEnvironmentVariables` для экземпляра <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="75f58-351">Call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="75f58-352">Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-352">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="75f58-353">Если вам нужно добавить конфигурацию приложения из дополнительных переменных среды, вызовите дополнительные поставщики приложения в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, а затем вызовите `AddEnvironmentVariables` с префиксом.</span><span class="sxs-lookup"><span data-stu-id="75f58-353">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            // Call additional providers here as needed.
            // Call AddEnvironmentVariables last if you need to allow environment
            // variables to override values from other providers.
            .AddEnvironmentVariables(prefix: "PREFIX_")
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="75f58-354">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="75f58-354">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="75f58-355">Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="75f58-355">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="75f58-356">**Пример**</span><span class="sxs-lookup"><span data-stu-id="75f58-356">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="75f58-357">Пример приложения 2.x использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="75f58-357">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="75f58-358">Пример приложения 1.x вызывает образец `AddEnvironmentVariables` в `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="75f58-358">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="75f58-359">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="75f58-359">Run the sample app.</span></span> <span data-ttu-id="75f58-360">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="75f58-360">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="75f58-361">Обратите внимание, что выходные данные содержат пару "ключ — значение" для переменной среды `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="75f58-361">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="75f58-362">Значение отражает среду, в которой выполняется приложение, обычно при локальном запуске это `Development`.</span><span class="sxs-lookup"><span data-stu-id="75f58-362">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="75f58-363">Чтобы список переменных сред, отображаемый приложением, был коротким, приложение фильтрует переменные среды по следующим пунктам:</span><span class="sxs-lookup"><span data-stu-id="75f58-363">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="75f58-364">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="75f58-364">ASPNETCORE_</span></span>
* <span data-ttu-id="75f58-365">urls</span><span class="sxs-lookup"><span data-stu-id="75f58-365">urls</span></span>
* <span data-ttu-id="75f58-366">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="75f58-366">Logging</span></span>
* <span data-ttu-id="75f58-367">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="75f58-367">ENVIRONMENT</span></span>
* <span data-ttu-id="75f58-368">contentRoot</span><span class="sxs-lookup"><span data-stu-id="75f58-368">contentRoot</span></span>
* <span data-ttu-id="75f58-369">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="75f58-369">AllowedHosts</span></span>
* <span data-ttu-id="75f58-370">applicationName</span><span class="sxs-lookup"><span data-stu-id="75f58-370">applicationName</span></span>
* <span data-ttu-id="75f58-371">CommandLine</span><span class="sxs-lookup"><span data-stu-id="75f58-371">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="75f58-372">Если хотите просмотреть все переменные среды, доступные для приложения, измените значение `FilteredConfiguration` в *Pages/Index.cshtml.cs* на следующее:</span><span class="sxs-lookup"><span data-stu-id="75f58-372">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="75f58-373">Если хотите просмотреть все переменные среды, доступные для приложения, измените значение `FilteredConfiguration` в *Controllers/HomeController.cs* на следующее:</span><span class="sxs-lookup"><span data-stu-id="75f58-373">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="75f58-374">Префиксы</span><span class="sxs-lookup"><span data-stu-id="75f58-374">Prefixes</span></span>

<span data-ttu-id="75f58-375">Переменные среды, которые загружаются в конфигурацию приложения, фильтруются при добавлении префикса к методу `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="75f58-375">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="75f58-376">Например, чтобы отфильтровать переменные среды по префиксу `CUSTOM_`, введите префикс поставщику конфигурации:</span><span class="sxs-lookup"><span data-stu-id="75f58-376">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="75f58-377">Префикс отделяется при создании пары конфигурации "ключ — значение".</span><span class="sxs-lookup"><span data-stu-id="75f58-377">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="75f58-378">Статический удобный метод `CreateDefaultBuilder` создает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> для установления размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="75f58-378">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="75f58-379">Когда значение `WebHostBuilder` будет создано, оно найдет конфигурацию узла в переменных среды, которые начинаются с префикса `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="75f58-379">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="75f58-380">**Префиксы строк подключения**</span><span class="sxs-lookup"><span data-stu-id="75f58-380">**Connection string prefixes**</span></span>

<span data-ttu-id="75f58-381">API конфигурации имеет специальные правила обработки для четырех строк подключения переменных среды, связанных с настройкой строк подключения Azure для среды приложения.</span><span class="sxs-lookup"><span data-stu-id="75f58-381">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="75f58-382">Если префикс не указан в `AddEnvironmentVariables`, переменные среды с префиксами, указанными в таблице, загружаются в приложение.</span><span class="sxs-lookup"><span data-stu-id="75f58-382">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="75f58-383">Префикс строки подключения</span><span class="sxs-lookup"><span data-stu-id="75f58-383">Connection string prefix</span></span> | <span data-ttu-id="75f58-384">Поставщик</span><span class="sxs-lookup"><span data-stu-id="75f58-384">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="75f58-385">Поставщик пользователя</span><span class="sxs-lookup"><span data-stu-id="75f58-385">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="75f58-386">MySQL</span><span class="sxs-lookup"><span data-stu-id="75f58-386">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="75f58-387">База данных SQL Azure</span><span class="sxs-lookup"><span data-stu-id="75f58-387">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="75f58-388">SQL Server</span><span class="sxs-lookup"><span data-stu-id="75f58-388">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="75f58-389">Когда переменная среды обнаруживается и загружается в конфигурацию с одним из четырех префиксов, приведенных в таблице, происходит следующее.</span><span class="sxs-lookup"><span data-stu-id="75f58-389">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="75f58-390">Ключ конфигурации создается путем удаления префикса переменных среды и добавления ключа раздела конфигурации (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="75f58-390">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="75f58-391">Создается новая пара "ключ — значение" конфигурации, которая представляет поставщика подключения базы данных (за исключением `CUSTOMCONNSTR_`, который не имеет указанного поставщика).</span><span class="sxs-lookup"><span data-stu-id="75f58-391">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="75f58-392">Ключ переменной среды</span><span class="sxs-lookup"><span data-stu-id="75f58-392">Environment variable key</span></span> | <span data-ttu-id="75f58-393">Преобразованный ключ конфигурации</span><span class="sxs-lookup"><span data-stu-id="75f58-393">Converted configuration key</span></span> | <span data-ttu-id="75f58-394">Запись конфигурации поставщика</span><span class="sxs-lookup"><span data-stu-id="75f58-394">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="75f58-395">Запись конфигурации не создана.</span><span class="sxs-lookup"><span data-stu-id="75f58-395">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="75f58-396">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="75f58-396">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="75f58-397">Значение: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="75f58-397">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="75f58-398">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="75f58-398">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="75f58-399">Значение: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="75f58-399">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="75f58-400">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="75f58-400">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="75f58-401">Значение: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="75f58-401">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="75f58-402">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="75f58-402">File Configuration Provider</span></span>

<span data-ttu-id="75f58-403"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> является базовым классом для загрузки конфигурации из файловой системы.</span><span class="sxs-lookup"><span data-stu-id="75f58-403"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="75f58-404">Следующие поставщики конфигурации предназначены для определенных типов файлов:</span><span class="sxs-lookup"><span data-stu-id="75f58-404">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="75f58-405">Поставщик конфигурации INI</span><span class="sxs-lookup"><span data-stu-id="75f58-405">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="75f58-406">Поставщик конфигурации JSON</span><span class="sxs-lookup"><span data-stu-id="75f58-406">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="75f58-407">Поставщик конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="75f58-407">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="75f58-408">Поставщик конфигурации INI</span><span class="sxs-lookup"><span data-stu-id="75f58-408">INI Configuration Provider</span></span>

<span data-ttu-id="75f58-409"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> загружает конфигурацию из пары "ключ — значение" INI-файла во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="75f58-409">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="75f58-410">Чтобы активировать конфигурацию INI-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="75f58-410">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="75f58-411">Двоеточие можно использовать как разделитель раздела в конфигурации файла INI.</span><span class="sxs-lookup"><span data-stu-id="75f58-411">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="75f58-412">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="75f58-412">Overloads permit specifying:</span></span>

* <span data-ttu-id="75f58-413">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="75f58-413">Whether the file is optional.</span></span>
* <span data-ttu-id="75f58-414">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="75f58-414">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="75f58-415"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="75f58-415">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="75f58-416">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-416">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddIniFile("config.ini", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="75f58-417">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-417">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="75f58-418">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="75f58-418">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="75f58-419">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="75f58-419">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="75f58-420">Вызывая `CreateDefaultBuilder`, вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="75f58-420">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddIniFile("config.ini", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="75f58-421">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-421">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="75f58-422">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="75f58-422">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="75f58-423">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="75f58-423">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="75f58-424">Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="75f58-424">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddIniFile("config.ini", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="75f58-425">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-425">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="75f58-426">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="75f58-426">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="75f58-427">Общий пример конфигурации INI-файла.</span><span class="sxs-lookup"><span data-stu-id="75f58-427">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="75f58-428">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="75f58-428">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="75f58-429">section0:key0</span><span class="sxs-lookup"><span data-stu-id="75f58-429">section0:key0</span></span>
* <span data-ttu-id="75f58-430">section0:key1</span><span class="sxs-lookup"><span data-stu-id="75f58-430">section0:key1</span></span>
* <span data-ttu-id="75f58-431">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="75f58-431">section1:subsection:key</span></span>
* <span data-ttu-id="75f58-432">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="75f58-432">section2:subsection0:key</span></span>
* <span data-ttu-id="75f58-433">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="75f58-433">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="75f58-434">Поставщик конфигурации JSON</span><span class="sxs-lookup"><span data-stu-id="75f58-434">JSON Configuration Provider</span></span>

<span data-ttu-id="75f58-435"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> загружает конфигурацию из пары "ключ — значение" JSON-файла в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="75f58-435">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="75f58-436">Чтобы активировать конфигурацию JSON-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="75f58-436">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="75f58-437">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="75f58-437">Overloads permit specifying:</span></span>

* <span data-ttu-id="75f58-438">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="75f58-438">Whether the file is optional.</span></span>
* <span data-ttu-id="75f58-439">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="75f58-439">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="75f58-440"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="75f58-440">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="75f58-441">`AddJsonFile` автоматически вызывается дважды при инициализации нового <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-441">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="75f58-442">Метод вызывается для загрузки конфигурации из:</span><span class="sxs-lookup"><span data-stu-id="75f58-442">The method is called to load configuration from:</span></span>

* <span data-ttu-id="75f58-443">*appsettings.json* &ndash; первым читается этот файл.</span><span class="sxs-lookup"><span data-stu-id="75f58-443">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="75f58-444">Версия файла среды может переопределить значения, предоставленные *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="75f58-444">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="75f58-445">*appsettings.{Environment}.json* — версия среды файла загружается на основе [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="75f58-445">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="75f58-446">Дополнительные сведения см. в руководстве по [настройке веб-узла](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="75f58-446">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="75f58-447">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="75f58-447">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="75f58-448">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="75f58-448">Environment variables.</span></span>
* <span data-ttu-id="75f58-449">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки);</span><span class="sxs-lookup"><span data-stu-id="75f58-449">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="75f58-450">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="75f58-450">Command-line arguments.</span></span>

<span data-ttu-id="75f58-451">Сначала устанавливается поставщик конфигурации JSON-файлов.</span><span class="sxs-lookup"><span data-stu-id="75f58-451">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="75f58-452">Таким образом, секреты пользователя, переменные среды и аргументы командной строки переопределят конфигурацию, заданную файлами *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="75f58-452">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="75f58-453">Вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> при сборке узла, чтобы указать конфигурацию приложения для файлов, отличных от *appsettings.json* и *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="75f58-453">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddJsonFile("config.json", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="75f58-454">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-454">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="75f58-455">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="75f58-455">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="75f58-456">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="75f58-456">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="75f58-457">Вы можете также напрямую вызвать метод расширения `AddJsonFile` в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="75f58-457">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="75f58-458">Вызывая `CreateDefaultBuilder`, вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="75f58-458">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("config.json", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="75f58-459">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-459">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="75f58-460">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="75f58-460">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="75f58-461">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="75f58-461">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="75f58-462">Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="75f58-462">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("config.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="75f58-463">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-463">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="75f58-464">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="75f58-464">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="75f58-465">**Пример**</span><span class="sxs-lookup"><span data-stu-id="75f58-465">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="75f58-466">В примере приложения 2.x используется преимущество статически удобного метода `CreateDefaultBuilder` для создания узла, который включает два вызова в `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="75f58-466">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="75f58-467">Конфигурация загружена из *appsettings.json* и *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="75f58-467">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="75f58-468">Пример приложения 1.x вызывает образец `AddJsonFile` дважды в `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="75f58-468">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="75f58-469">Конфигурация загружена из *appsettings.json* и *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="75f58-469">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="75f58-470">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="75f58-470">Run the sample app.</span></span> <span data-ttu-id="75f58-471">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="75f58-471">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="75f58-472">Обратите внимание, что выходные данные содержат пары "ключ — значение" для конфигурации, представленной в таблице в зависимости от среды.</span><span class="sxs-lookup"><span data-stu-id="75f58-472">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="75f58-473">Ключи конфигурации ведения журнала используют двоеточие (`:`) как иерархический разделитель.</span><span class="sxs-lookup"><span data-stu-id="75f58-473">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="75f58-474">Ключ</span><span class="sxs-lookup"><span data-stu-id="75f58-474">Key</span></span>                        | <span data-ttu-id="75f58-475">Значение разработки</span><span class="sxs-lookup"><span data-stu-id="75f58-475">Development Value</span></span> | <span data-ttu-id="75f58-476">Рабочее значение</span><span class="sxs-lookup"><span data-stu-id="75f58-476">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="75f58-477">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="75f58-477">Logging:LogLevel:System</span></span>    | <span data-ttu-id="75f58-478">Сведения</span><span class="sxs-lookup"><span data-stu-id="75f58-478">Information</span></span>       | <span data-ttu-id="75f58-479">Сведения</span><span class="sxs-lookup"><span data-stu-id="75f58-479">Information</span></span>      |
| <span data-ttu-id="75f58-480">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="75f58-480">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="75f58-481">Сведения</span><span class="sxs-lookup"><span data-stu-id="75f58-481">Information</span></span>       | <span data-ttu-id="75f58-482">Сведения</span><span class="sxs-lookup"><span data-stu-id="75f58-482">Information</span></span>      |
| <span data-ttu-id="75f58-483">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="75f58-483">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="75f58-484">Отладка</span><span class="sxs-lookup"><span data-stu-id="75f58-484">Debug</span></span>             | <span data-ttu-id="75f58-485">Error</span><span class="sxs-lookup"><span data-stu-id="75f58-485">Error</span></span>            |
| <span data-ttu-id="75f58-486">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="75f58-486">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="75f58-487">Поставщик конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="75f58-487">XML Configuration Provider</span></span>

<span data-ttu-id="75f58-488"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> загружает конфигурацию из пары "ключ — значение" XML-файла в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="75f58-488">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="75f58-489">Чтобы активировать конфигурацию XML-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="75f58-489">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="75f58-490">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="75f58-490">Overloads permit specifying:</span></span>

* <span data-ttu-id="75f58-491">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="75f58-491">Whether the file is optional.</span></span>
* <span data-ttu-id="75f58-492">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="75f58-492">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="75f58-493"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="75f58-493">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="75f58-494">Корневой узел файла конфигурации не учитывается при создании пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="75f58-494">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="75f58-495">Не указывайте в файле Document Type Definition (определение типа документа, DTD) или пространство имен.</span><span class="sxs-lookup"><span data-stu-id="75f58-495">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="75f58-496">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-496">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddXmlFile("config.xml", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="75f58-497">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-497">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="75f58-498">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="75f58-498">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="75f58-499">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="75f58-499">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="75f58-500">Вызывая `CreateDefaultBuilder`, вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="75f58-500">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="75f58-501">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-501">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="75f58-502">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="75f58-502">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="75f58-503">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="75f58-503">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="75f58-504">Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="75f58-504">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="75f58-505">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-505">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="75f58-506">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="75f58-506">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="75f58-507">XML-файлы конфигурации могут использовать имена отдельных элементов для повторяющихся разделов.</span><span class="sxs-lookup"><span data-stu-id="75f58-507">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="75f58-508">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="75f58-508">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="75f58-509">section0:key0</span><span class="sxs-lookup"><span data-stu-id="75f58-509">section0:key0</span></span>
* <span data-ttu-id="75f58-510">section0:key1</span><span class="sxs-lookup"><span data-stu-id="75f58-510">section0:key1</span></span>
* <span data-ttu-id="75f58-511">section1:key0</span><span class="sxs-lookup"><span data-stu-id="75f58-511">section1:key0</span></span>
* <span data-ttu-id="75f58-512">section1:key1</span><span class="sxs-lookup"><span data-stu-id="75f58-512">section1:key1</span></span>

<span data-ttu-id="75f58-513">Повторяющиеся элементы, использующие то же имя элемента, работают, если атрибут `name` используется для различения элементов.</span><span class="sxs-lookup"><span data-stu-id="75f58-513">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="75f58-514">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="75f58-514">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="75f58-515">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="75f58-515">section:section0:key:key0</span></span>
* <span data-ttu-id="75f58-516">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="75f58-516">section:section0:key:key1</span></span>
* <span data-ttu-id="75f58-517">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="75f58-517">section:section1:key:key0</span></span>
* <span data-ttu-id="75f58-518">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="75f58-518">section:section1:key:key1</span></span>

<span data-ttu-id="75f58-519">Атрибуты можно использовать для предоставления значений.</span><span class="sxs-lookup"><span data-stu-id="75f58-519">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="75f58-520">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="75f58-520">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="75f58-521">key:attribute</span><span class="sxs-lookup"><span data-stu-id="75f58-521">key:attribute</span></span>
* <span data-ttu-id="75f58-522">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="75f58-522">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="75f58-523">Поставщик конфигурации ключа для каждого файла</span><span class="sxs-lookup"><span data-stu-id="75f58-523">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="75f58-524"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> использует файлы каталога как пары "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="75f58-524">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="75f58-525">Ключ является именем файла.</span><span class="sxs-lookup"><span data-stu-id="75f58-525">The key is the file name.</span></span> <span data-ttu-id="75f58-526">Значение содержит содержимое файла.</span><span class="sxs-lookup"><span data-stu-id="75f58-526">The value contains the file's contents.</span></span> <span data-ttu-id="75f58-527">Поставщик конфигурации ключа для каждого файла используется в сценариях размещения Docker.</span><span class="sxs-lookup"><span data-stu-id="75f58-527">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="75f58-528">Чтобы активировать конфигурацию ключа для каждого файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="75f58-528">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="75f58-529">Значение параметра `directoryPath` должно быть абсолютным путем к файлам.</span><span class="sxs-lookup"><span data-stu-id="75f58-529">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="75f58-530">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="75f58-530">Overloads permit specifying:</span></span>

* <span data-ttu-id="75f58-531">`Action<KeyPerFileConfigurationSource>` — делегат, который настраивает источник.</span><span class="sxs-lookup"><span data-stu-id="75f58-531">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="75f58-532">Обязательно ли указывать каталог и путь к каталогу.</span><span class="sxs-lookup"><span data-stu-id="75f58-532">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="75f58-533">Двойное подчеркивание (`__`) используется в качестве разделителя ключа конфигурации в именах файлов.</span><span class="sxs-lookup"><span data-stu-id="75f58-533">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="75f58-534">Например, в имени файла `Logging__LogLevel__System` создается ключ конфигурации `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="75f58-534">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="75f58-535">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-535">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
                config.AddKeyPerFile(directoryPath: path, optional: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="75f58-536">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-536">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="75f58-537">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="75f58-537">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="75f58-538">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="75f58-538">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
var config = new ConfigurationBuilder()
    .AddKeyPerFile(directoryPath: path, optional: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

::: moniker-end

## <a name="memory-configuration-provider"></a><span data-ttu-id="75f58-539">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="75f58-539">Memory Configuration Provider</span></span>

<span data-ttu-id="75f58-540"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> использует коллекцию памяти в качестве пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="75f58-540">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="75f58-541">Чтобы активировать конфигурацию коллекции в памяти, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="75f58-541">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="75f58-542">Поставщик конфигурации может инициализироваться значением `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="75f58-542">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="75f58-543">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="75f58-543">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _dict = 
        new Dictionary<string, string>
        {
            {"MemoryCollectionKey1", "value1"},
            {"MemoryCollectionKey2", "value2"}
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddInMemoryCollection(_dict);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="75f58-544">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="75f58-544">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="75f58-545">Вызывая `CreateDefaultBuilder`, вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="75f58-545">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

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
}
```

<span data-ttu-id="75f58-546">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="75f58-546">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="75f58-547">Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="75f58-547">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

var config = new ConfigurationBuilder()
    .AddInMemoryCollection(dict)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

## <a name="getvalue"></a><span data-ttu-id="75f58-548">GetValue</span><span class="sxs-lookup"><span data-stu-id="75f58-548">GetValue</span></span>

<span data-ttu-id="75f58-549">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) извлекает значение из конфигурации с указанным ключом и преобразует его в указанный тип.</span><span class="sxs-lookup"><span data-stu-id="75f58-549">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="75f58-550">Если ключ не найден, перегрузка позволяет предоставлять значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="75f58-550">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="75f58-551">В следующем примере извлекается строковое значение из конфигурации с ключом `NumberKey`, вводится значение `int` и сохраняется значение в переменной `intValue`.</span><span class="sxs-lookup"><span data-stu-id="75f58-551">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="75f58-552">Если `NumberKey` не обнаруживается в ключах конфигурации, `intValue` получает значение `99` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="75f58-552">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="75f58-553">GetSection, GetChildren и Exists</span><span class="sxs-lookup"><span data-stu-id="75f58-553">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="75f58-554">В следующих примерах рассмотрим файл JSON.</span><span class="sxs-lookup"><span data-stu-id="75f58-554">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="75f58-555">Четыре ключа находятся в двух разделах, один из которых содержит пару из подразделов.</span><span class="sxs-lookup"><span data-stu-id="75f58-555">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="75f58-556">Когда файл считывается в конфигурацию, для сохранения значений конфигурации создаются следующие уникальные иерархические ключи.</span><span class="sxs-lookup"><span data-stu-id="75f58-556">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="75f58-557">section0:key0</span><span class="sxs-lookup"><span data-stu-id="75f58-557">section0:key0</span></span>
* <span data-ttu-id="75f58-558">section0:key1</span><span class="sxs-lookup"><span data-stu-id="75f58-558">section0:key1</span></span>
* <span data-ttu-id="75f58-559">section1:key0</span><span class="sxs-lookup"><span data-stu-id="75f58-559">section1:key0</span></span>
* <span data-ttu-id="75f58-560">section1:key1</span><span class="sxs-lookup"><span data-stu-id="75f58-560">section1:key1</span></span>
* <span data-ttu-id="75f58-561">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="75f58-561">section2:subsection0:key0</span></span>
* <span data-ttu-id="75f58-562">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="75f58-562">section2:subsection0:key1</span></span>
* <span data-ttu-id="75f58-563">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="75f58-563">section2:subsection1:key0</span></span>
* <span data-ttu-id="75f58-564">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="75f58-564">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="75f58-565">GetSection</span><span class="sxs-lookup"><span data-stu-id="75f58-565">GetSection</span></span>

<span data-ttu-id="75f58-566">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) извлекает подраздел конфигурации с указанным ключом подраздела.</span><span class="sxs-lookup"><span data-stu-id="75f58-566">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span> <span data-ttu-id="75f58-567">`GetSection` находится в пакете [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="75f58-567">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="75f58-568">Чтобы вернуть <xref:Microsoft.Extensions.Configuration.IConfigurationSection>, содержащий только пары "ключ — значение" в `section1`, вызовите `GetSection` и укажите имя раздела.</span><span class="sxs-lookup"><span data-stu-id="75f58-568">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="75f58-569">`configSection` не содержит значение, только ключ и путь.</span><span class="sxs-lookup"><span data-stu-id="75f58-569">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="75f58-570">Аналогично, чтобы получить значения для ключей в `section2:subsection0`, вызовите `GetSection` и укажите путь к разделу.</span><span class="sxs-lookup"><span data-stu-id="75f58-570">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="75f58-571">Значение `GetSection` никогда не возвращает значение `null`.</span><span class="sxs-lookup"><span data-stu-id="75f58-571">`GetSection` never returns `null`.</span></span> <span data-ttu-id="75f58-572">Если соответствующий раздел не найден, возвращается пустой параметр `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="75f58-572">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="75f58-573">Когда `GetSection` возвращает соответствующий раздел, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> не заполняется.</span><span class="sxs-lookup"><span data-stu-id="75f58-573">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="75f58-574"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> и <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> возвращаются, если раздел существует.</span><span class="sxs-lookup"><span data-stu-id="75f58-574">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="75f58-575">GetChildren</span><span class="sxs-lookup"><span data-stu-id="75f58-575">GetChildren</span></span>

<span data-ttu-id="75f58-576">Вызов [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) в `section2` получает значение `IEnumerable<IConfigurationSection>`, которое включает:</span><span class="sxs-lookup"><span data-stu-id="75f58-576">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="75f58-577">Exists</span><span class="sxs-lookup"><span data-stu-id="75f58-577">Exists</span></span>

<span data-ttu-id="75f58-578">Используйте [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*), чтобы определить, существует ли раздел конфигурации.</span><span class="sxs-lookup"><span data-stu-id="75f58-578">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="75f58-579">Учитывая данные примера, `sectionExists` является `false`, потому что в данных конфигурации нет `section2:subsection2`.</span><span class="sxs-lookup"><span data-stu-id="75f58-579">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="75f58-580">Привязка к классу</span><span class="sxs-lookup"><span data-stu-id="75f58-580">Bind to a class</span></span>

<span data-ttu-id="75f58-581">Конфигурация может быть привязана к классам, которые представляют группы связанных параметров, используя шаблон *вариантов*.</span><span class="sxs-lookup"><span data-stu-id="75f58-581">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="75f58-582">Для получения дополнительной информации см. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="75f58-582">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="75f58-583">Значения конфигурации возвращаются как строки, но вызов <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> позволяет построить объекты [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="75f58-583">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="75f58-584">`Bind` находится в пакете [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="75f58-584">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="75f58-585">Пример приложения содержит модель `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="75f58-585">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="75f58-586">Раздел `starship` файла *starship.json* создает конфигурацию, когда образец приложения использует поставщик конфигурации JSON для загрузки конфигурации.</span><span class="sxs-lookup"><span data-stu-id="75f58-586">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="75f58-587">Создаются следующие пары "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="75f58-587">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="75f58-588">Ключ</span><span class="sxs-lookup"><span data-stu-id="75f58-588">Key</span></span>                   | <span data-ttu-id="75f58-589">Значение</span><span class="sxs-lookup"><span data-stu-id="75f58-589">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="75f58-590">starship:name</span><span class="sxs-lookup"><span data-stu-id="75f58-590">starship:name</span></span>         | <span data-ttu-id="75f58-591">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="75f58-591">USS Enterprise</span></span>                                    |
| <span data-ttu-id="75f58-592">starship:registry</span><span class="sxs-lookup"><span data-stu-id="75f58-592">starship:registry</span></span>     | <span data-ttu-id="75f58-593">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="75f58-593">NCC-1701</span></span>                                          |
| <span data-ttu-id="75f58-594">starship:class</span><span class="sxs-lookup"><span data-stu-id="75f58-594">starship:class</span></span>        | <span data-ttu-id="75f58-595">Constitution</span><span class="sxs-lookup"><span data-stu-id="75f58-595">Constitution</span></span>                                      |
| <span data-ttu-id="75f58-596">starship:length</span><span class="sxs-lookup"><span data-stu-id="75f58-596">starship:length</span></span>       | <span data-ttu-id="75f58-597">304.8</span><span class="sxs-lookup"><span data-stu-id="75f58-597">304.8</span></span>                                             |
| <span data-ttu-id="75f58-598">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="75f58-598">starship:commissioned</span></span> | <span data-ttu-id="75f58-599">False</span><span class="sxs-lookup"><span data-stu-id="75f58-599">False</span></span>                                             |
| <span data-ttu-id="75f58-600">trademark</span><span class="sxs-lookup"><span data-stu-id="75f58-600">trademark</span></span>             | <span data-ttu-id="75f58-601">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="75f58-601">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="75f58-602">Пример приложения вызывает `GetSection` с помощью ключа `starship`.</span><span class="sxs-lookup"><span data-stu-id="75f58-602">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="75f58-603">Пары "ключ — значение" `starship` изолированы.</span><span class="sxs-lookup"><span data-stu-id="75f58-603">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="75f58-604">Метод `Bind` вызывается в подразделе при прохождении в экземпляр класса `Starship`.</span><span class="sxs-lookup"><span data-stu-id="75f58-604">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="75f58-605">После привязки значения экземпляра экземпляру присваивается свойство для преобразования для просмотра.</span><span class="sxs-lookup"><span data-stu-id="75f58-605">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

<span data-ttu-id="75f58-606">`GetSection` находится в пакете [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="75f58-606">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="75f58-607">Привязка к графу объектов</span><span class="sxs-lookup"><span data-stu-id="75f58-607">Bind to an object graph</span></span>

<span data-ttu-id="75f58-608"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> способна связывать весь граф объекта POCO.</span><span class="sxs-lookup"><span data-stu-id="75f58-608"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="75f58-609">`Bind` находится в пакете [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="75f58-609">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="75f58-610">Образец содержит модель `TvShow`, в графе объектов которого находятся классы `Metadata` и `Actors` (*Модели/TvShow.cs*).</span><span class="sxs-lookup"><span data-stu-id="75f58-610">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="75f58-611">В примере приложения есть файл *tvshow.xml*, содержащий данные конфигурации.</span><span class="sxs-lookup"><span data-stu-id="75f58-611">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="75f58-612">Конфигурация привязана ко всему ​​графу объектов `TvShow` с помощью метода `Bind`.</span><span class="sxs-lookup"><span data-stu-id="75f58-612">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="75f58-613">Привязанный экземпляр присваивается свойству для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="75f58-613">The bound instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
viewModel.TvShow = tvShow;
```

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="75f58-614">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) привязывает и возвращает указанный тип.</span><span class="sxs-lookup"><span data-stu-id="75f58-614">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="75f58-615">Метод `Get<T>` может быть более удобным, чем использование `Bind`.</span><span class="sxs-lookup"><span data-stu-id="75f58-615">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="75f58-616">В следующем коде показано, как использовать `Get<T>` с предыдущим примером, который позволяет привязать связанный экземпляр непосредственно к свойству, используемому для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="75f58-616">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

<span data-ttu-id="75f58-617"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> находится в пакете [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="75f58-617"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="75f58-618">`Get<T>` доступно в ASP.NET Core 1.1 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="75f58-618">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="75f58-619">`GetSection` находится в пакете [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="75f58-619">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="75f58-620">Привязка массива к классу</span><span class="sxs-lookup"><span data-stu-id="75f58-620">Bind an array to a class</span></span>

<span data-ttu-id="75f58-621">*Пример приложения демонстрирует концепции, описанные в этом разделе.*</span><span class="sxs-lookup"><span data-stu-id="75f58-621">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="75f58-622"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="75f58-622">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="75f58-623">Любой формат массива, который предоставляет сегмент числового ключа (`:0:`, `:1:`, &hellip; `:{n}:`), способен привязать массив к массиву класса POCO.</span><span class="sxs-lookup"><span data-stu-id="75f58-623">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span> <span data-ttu-id="75f58-624">"Bind" находится в пакете [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="75f58-624">\`Bind\`\` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

> [!NOTE]
> <span data-ttu-id="75f58-625">Привязка предоставляется соглашением.</span><span class="sxs-lookup"><span data-stu-id="75f58-625">Binding is provided by convention.</span></span> <span data-ttu-id="75f58-626">Пользовательские поставщики конфигурации не обязаны реализовывать привязку массива.</span><span class="sxs-lookup"><span data-stu-id="75f58-626">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="75f58-627">**Обработка массива в оперативной памяти**</span><span class="sxs-lookup"><span data-stu-id="75f58-627">**In-memory array processing**</span></span>

<span data-ttu-id="75f58-628">Рассмотрите ключи и значения конфигурации, приведенные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="75f58-628">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="75f58-629">Ключ</span><span class="sxs-lookup"><span data-stu-id="75f58-629">Key</span></span>             | <span data-ttu-id="75f58-630">Значение</span><span class="sxs-lookup"><span data-stu-id="75f58-630">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="75f58-631">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="75f58-631">array:entries:0</span></span> | <span data-ttu-id="75f58-632">value0</span><span class="sxs-lookup"><span data-stu-id="75f58-632">value0</span></span> |
| <span data-ttu-id="75f58-633">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="75f58-633">array:entries:1</span></span> | <span data-ttu-id="75f58-634">value1</span><span class="sxs-lookup"><span data-stu-id="75f58-634">value1</span></span> |
| <span data-ttu-id="75f58-635">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="75f58-635">array:entries:2</span></span> | <span data-ttu-id="75f58-636">value2</span><span class="sxs-lookup"><span data-stu-id="75f58-636">value2</span></span> |
| <span data-ttu-id="75f58-637">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="75f58-637">array:entries:4</span></span> | <span data-ttu-id="75f58-638">value4</span><span class="sxs-lookup"><span data-stu-id="75f58-638">value4</span></span> |
| <span data-ttu-id="75f58-639">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="75f58-639">array:entries:5</span></span> | <span data-ttu-id="75f58-640">value5</span><span class="sxs-lookup"><span data-stu-id="75f58-640">value5</span></span> |

<span data-ttu-id="75f58-641">Эти ключи и значения загружаются в пример приложения с помощью поставщика конфигурации памяти.</span><span class="sxs-lookup"><span data-stu-id="75f58-641">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="75f58-642">Массив пропускает значения для индекса &num;3.</span><span class="sxs-lookup"><span data-stu-id="75f58-642">The array skips a value for index &num;3.</span></span> <span data-ttu-id="75f58-643">Связующее свойство конфигурации не способно связывать нулевые значения или создавать нулевые записи в связанных объектах, что становится ясным в тот момент, когда демонстрируется результат привязки этого массива к объекту.</span><span class="sxs-lookup"><span data-stu-id="75f58-643">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="75f58-644">В примере приложения класс POCO доступен для хранения привязанных данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="75f58-644">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="75f58-645">Данные конфигурации, связанные с объектом.</span><span class="sxs-lookup"><span data-stu-id="75f58-645">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="75f58-646">`GetSection` находится в пакете [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="75f58-646">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="75f58-647">Синтаксис [ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) также может использоваться для получения более компактного кода.</span><span class="sxs-lookup"><span data-stu-id="75f58-647">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="75f58-648">Связанный объект, экземпляр `ArrayExample`, получает данные массива из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="75f58-648">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="75f58-649">Индекс `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="75f58-649">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="75f58-650">Значение `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="75f58-650">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="75f58-651">0</span><span class="sxs-lookup"><span data-stu-id="75f58-651">0</span></span>                            | <span data-ttu-id="75f58-652">value0</span><span class="sxs-lookup"><span data-stu-id="75f58-652">value0</span></span>                       |
| <span data-ttu-id="75f58-653">1</span><span class="sxs-lookup"><span data-stu-id="75f58-653">1</span></span>                            | <span data-ttu-id="75f58-654">value1</span><span class="sxs-lookup"><span data-stu-id="75f58-654">value1</span></span>                       |
| <span data-ttu-id="75f58-655">2</span><span class="sxs-lookup"><span data-stu-id="75f58-655">2</span></span>                            | <span data-ttu-id="75f58-656">value2</span><span class="sxs-lookup"><span data-stu-id="75f58-656">value2</span></span>                       |
| <span data-ttu-id="75f58-657">3</span><span class="sxs-lookup"><span data-stu-id="75f58-657">3</span></span>                            | <span data-ttu-id="75f58-658">value4</span><span class="sxs-lookup"><span data-stu-id="75f58-658">value4</span></span>                       |
| <span data-ttu-id="75f58-659">4</span><span class="sxs-lookup"><span data-stu-id="75f58-659">4</span></span>                            | <span data-ttu-id="75f58-660">value5</span><span class="sxs-lookup"><span data-stu-id="75f58-660">value5</span></span>                       |

<span data-ttu-id="75f58-661">Индекс &num;3 в связанном объекте содержит данные конфигурации для конфигурационного ключа `array:4` и его значения `value4`.</span><span class="sxs-lookup"><span data-stu-id="75f58-661">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="75f58-662">При привязке данных конфигурации, содержащих массив индексов, в ключах конфигурации эти индексы просто используются для выполнения итерации данных конфигурации при создании объекта.</span><span class="sxs-lookup"><span data-stu-id="75f58-662">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="75f58-663">Когда массив ключей конфигурации пропускает один или несколько индексов, в данных конфигурации не может быть сохранено нулевое значение и в связанном объекте не создается нулевая запись.</span><span class="sxs-lookup"><span data-stu-id="75f58-663">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="75f58-664">Отсутствующий элемент конфигурации для индекса &num;3 может быть предоставлен перед привязкой к экземпляру `ArrayExample` любым поставщиком конфигурации, который создает правильную пару "ключ — значение" в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="75f58-664">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="75f58-665">Если в образец включен дополнительный поставщик конфигурации JSON с отсутствующей парой "ключ — значение", то `ArrayExample.Entries` подойдет полному массиву конфигурации.</span><span class="sxs-lookup"><span data-stu-id="75f58-665">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="75f58-666">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="75f58-666">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="75f58-667">В <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="75f58-667">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="75f58-668">В конструкторе `Startup`:</span><span class="sxs-lookup"><span data-stu-id="75f58-668">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="75f58-669">Пары "ключ — значение", показанные в таблице, загружаются в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="75f58-669">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="75f58-670">Ключ</span><span class="sxs-lookup"><span data-stu-id="75f58-670">Key</span></span>             | <span data-ttu-id="75f58-671">Значение</span><span class="sxs-lookup"><span data-stu-id="75f58-671">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="75f58-672">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="75f58-672">array:entries:3</span></span> | <span data-ttu-id="75f58-673">value3</span><span class="sxs-lookup"><span data-stu-id="75f58-673">value3</span></span> |

<span data-ttu-id="75f58-674">Если экземпляр класса `ArrayExample` связан после того, как поставщик конфигурации JSON включает запись для индекса &num;3, то массив `ArrayExample.Entries` включит значение.</span><span class="sxs-lookup"><span data-stu-id="75f58-674">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="75f58-675">Индекс `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="75f58-675">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="75f58-676">Значение `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="75f58-676">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="75f58-677">0</span><span class="sxs-lookup"><span data-stu-id="75f58-677">0</span></span>                            | <span data-ttu-id="75f58-678">value0</span><span class="sxs-lookup"><span data-stu-id="75f58-678">value0</span></span>                       |
| <span data-ttu-id="75f58-679">1</span><span class="sxs-lookup"><span data-stu-id="75f58-679">1</span></span>                            | <span data-ttu-id="75f58-680">value1</span><span class="sxs-lookup"><span data-stu-id="75f58-680">value1</span></span>                       |
| <span data-ttu-id="75f58-681">2</span><span class="sxs-lookup"><span data-stu-id="75f58-681">2</span></span>                            | <span data-ttu-id="75f58-682">value2</span><span class="sxs-lookup"><span data-stu-id="75f58-682">value2</span></span>                       |
| <span data-ttu-id="75f58-683">3</span><span class="sxs-lookup"><span data-stu-id="75f58-683">3</span></span>                            | <span data-ttu-id="75f58-684">value3</span><span class="sxs-lookup"><span data-stu-id="75f58-684">value3</span></span>                       |
| <span data-ttu-id="75f58-685">4</span><span class="sxs-lookup"><span data-stu-id="75f58-685">4</span></span>                            | <span data-ttu-id="75f58-686">value4</span><span class="sxs-lookup"><span data-stu-id="75f58-686">value4</span></span>                       |
| <span data-ttu-id="75f58-687">5</span><span class="sxs-lookup"><span data-stu-id="75f58-687">5</span></span>                            | <span data-ttu-id="75f58-688">value5</span><span class="sxs-lookup"><span data-stu-id="75f58-688">value5</span></span>                       |

<span data-ttu-id="75f58-689">**Обработка массива JSON**</span><span class="sxs-lookup"><span data-stu-id="75f58-689">**JSON array processing**</span></span>

<span data-ttu-id="75f58-690">Если JSON-файл содержит массив, ключи конфигурации будут созданы для элементов массива с индексом раздела, начиная с нуля.</span><span class="sxs-lookup"><span data-stu-id="75f58-690">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="75f58-691">В следующем файле конфигурации `subsection` — это массив.</span><span class="sxs-lookup"><span data-stu-id="75f58-691">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="75f58-692">Поставщик конфигурации JSON считывает данные конфигурации в следующие пары "ключ — значение".</span><span class="sxs-lookup"><span data-stu-id="75f58-692">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="75f58-693">Ключ</span><span class="sxs-lookup"><span data-stu-id="75f58-693">Key</span></span>                     | <span data-ttu-id="75f58-694">Значение</span><span class="sxs-lookup"><span data-stu-id="75f58-694">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="75f58-695">json_array:key</span><span class="sxs-lookup"><span data-stu-id="75f58-695">json_array:key</span></span>          | <span data-ttu-id="75f58-696">valueA</span><span class="sxs-lookup"><span data-stu-id="75f58-696">valueA</span></span> |
| <span data-ttu-id="75f58-697">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="75f58-697">json_array:subsection:0</span></span> | <span data-ttu-id="75f58-698">valueB</span><span class="sxs-lookup"><span data-stu-id="75f58-698">valueB</span></span> |
| <span data-ttu-id="75f58-699">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="75f58-699">json_array:subsection:1</span></span> | <span data-ttu-id="75f58-700">valueC</span><span class="sxs-lookup"><span data-stu-id="75f58-700">valueC</span></span> |
| <span data-ttu-id="75f58-701">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="75f58-701">json_array:subsection:2</span></span> | <span data-ttu-id="75f58-702">valueD</span><span class="sxs-lookup"><span data-stu-id="75f58-702">valueD</span></span> |

<span data-ttu-id="75f58-703">В примере приложения для привязки пар "ключ — значение" конфигурации доступен следующий класс POCO.</span><span class="sxs-lookup"><span data-stu-id="75f58-703">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="75f58-704">После выполнения привязки `JsonArrayExample.Key` содержит значение `valueA`.</span><span class="sxs-lookup"><span data-stu-id="75f58-704">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="75f58-705">Подраздел значения хранится в свойстве массива POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="75f58-705">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="75f58-706">Индекс `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="75f58-706">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="75f58-707">Значение `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="75f58-707">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="75f58-708">0</span><span class="sxs-lookup"><span data-stu-id="75f58-708">0</span></span>                                   | <span data-ttu-id="75f58-709">valueB</span><span class="sxs-lookup"><span data-stu-id="75f58-709">valueB</span></span>                              |
| <span data-ttu-id="75f58-710">1</span><span class="sxs-lookup"><span data-stu-id="75f58-710">1</span></span>                                   | <span data-ttu-id="75f58-711">valueC</span><span class="sxs-lookup"><span data-stu-id="75f58-711">valueC</span></span>                              |
| <span data-ttu-id="75f58-712">2</span><span class="sxs-lookup"><span data-stu-id="75f58-712">2</span></span>                                   | <span data-ttu-id="75f58-713">valueD</span><span class="sxs-lookup"><span data-stu-id="75f58-713">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="75f58-714">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="75f58-714">Custom configuration provider</span></span>

<span data-ttu-id="75f58-715">Пример приложения демонстрирует, как создать базовый поставщик конфигурации, который считывает пары "ключ — значение" конфигурации из базы данных, используя [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="75f58-715">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="75f58-716">Поставщик имеет следующие характеристики.</span><span class="sxs-lookup"><span data-stu-id="75f58-716">The provider has the following characteristics:</span></span>

* <span data-ttu-id="75f58-717">База данных в памяти EF используется для демонстрационных целей.</span><span class="sxs-lookup"><span data-stu-id="75f58-717">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="75f58-718">Чтобы использовать базу данных, для которой требуется строка подключения, выполните вторичный `ConfigurationBuilder`, чтобы предоставить строку подключения от другого поставщика конфигурации.</span><span class="sxs-lookup"><span data-stu-id="75f58-718">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="75f58-719">Поставщик считывает таблицу базы данных в конфигурации при запуске.</span><span class="sxs-lookup"><span data-stu-id="75f58-719">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="75f58-720">Поставщик не запрашивает базу данных для каждого ключа.</span><span class="sxs-lookup"><span data-stu-id="75f58-720">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="75f58-721">Функция перезагрузки на изменение не реализована, поэтому обновление базы данных после запуска приложения не влияет на конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="75f58-721">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="75f58-722">Определите сущность `EFConfigurationValue` для хранения значений конфигурации в базе данных.</span><span class="sxs-lookup"><span data-stu-id="75f58-722">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="75f58-723">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="75f58-723">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="75f58-724">Добавьте `EFConfigurationContext` в хранилище и обратитесь к настроенным значениям.</span><span class="sxs-lookup"><span data-stu-id="75f58-724">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="75f58-725">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="75f58-725">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="75f58-726">Создайте класс, реализующий <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="75f58-726">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="75f58-727">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="75f58-727">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="75f58-728">Создайте пользовательский поставщик конфигурации путем наследования от <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="75f58-728">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="75f58-729">Поставщик конфигурации инициализирует пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="75f58-729">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="75f58-730">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="75f58-730">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="75f58-731">Метод расширения `AddEFConfiguration` позволяет добавить источник конфигурации к `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="75f58-731">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="75f58-732">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="75f58-732">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="75f58-733">В следующем коде показано, как использовать пользовательский `EFConfigurationProvider` в *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="75f58-733">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="75f58-734">Доступ к конфигурации во время запуска</span><span class="sxs-lookup"><span data-stu-id="75f58-734">Access configuration during startup</span></span>

<span data-ttu-id="75f58-735">Внесите значение `IConfiguration` в конструктор `Startup` для доступа к значениям конфигурации в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="75f58-735">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="75f58-736">Чтобы получить доступ к конфигурации в `Startup.Configure`, либо добавьте значение `IConfiguration` непосредственно в метод, либо используйте экземпляр из конструктора.</span><span class="sxs-lookup"><span data-stu-id="75f58-736">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="75f58-737">Дополнительные сведения см. в руководстве по [доступу к конфигурации с использованием удобных методов](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="75f58-737">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="75f58-738">Настройте доступ на странице Razor Pages или в представлении MVC</span><span class="sxs-lookup"><span data-stu-id="75f58-738">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="75f58-739">Для доступа к параметрам конфигурации на странице Razor Pages или в представлении MVC добавьте [использование директивы](xref:mvc/views/razor#using) ([Справочник по C#: использование директивы](/dotnet/csharp/language-reference/keywords/using-directive)) для [пространства имен Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) и вставьте <xref:Microsoft.Extensions.Configuration.IConfiguration> на страницу или в представление.</span><span class="sxs-lookup"><span data-stu-id="75f58-739">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="75f58-740">На странице Razor Pages</span><span class="sxs-lookup"><span data-stu-id="75f58-740">In a Razor Pages page:</span></span>

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

<span data-ttu-id="75f58-741">В представлении MVC</span><span class="sxs-lookup"><span data-stu-id="75f58-741">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="75f58-742">Добавление конфигурации из внешней сборки</span><span class="sxs-lookup"><span data-stu-id="75f58-742">Add configuration from an external assembly</span></span>

<span data-ttu-id="75f58-743">Реализация <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> позволяет при запуске добавлять в приложение улучшения из внешней сборки вне приложения класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="75f58-743">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="75f58-744">Для получения дополнительной информации см. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="75f58-744">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="75f58-745">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="75f58-745">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* <span data-ttu-id="75f58-746">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/) (Подробные сведения о конфигурации Microsoft)</span><span class="sxs-lookup"><span data-stu-id="75f58-746">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)</span></span>
