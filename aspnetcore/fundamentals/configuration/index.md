---
title: Конфигурация в .NET Core
author: guardrex
description: Узнайте, как использовать API конфигурации для настройки приложения ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/11/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 52450460af59a8da4fc6b24e3aa83d55385e8c77
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750960"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="b71fd-103">Конфигурация в .NET Core</span><span class="sxs-lookup"><span data-stu-id="b71fd-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="b71fd-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="b71fd-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b71fd-105">Конфигурация приложения в ASP.NET Core основана на парах "ключ — значение", установленных *поставщиками конфигурации*.</span><span class="sxs-lookup"><span data-stu-id="b71fd-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="b71fd-106">Поставщики конфигурации получают данные конфигурации в парах "ключ — значение" из различных источников:</span><span class="sxs-lookup"><span data-stu-id="b71fd-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="b71fd-107">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="b71fd-107">Azure Key Vault</span></span>
* <span data-ttu-id="b71fd-108">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="b71fd-108">Command-line arguments</span></span>
* <span data-ttu-id="b71fd-109">а также пользовательские поставщики (устанавливаемые или создаваемые).</span><span class="sxs-lookup"><span data-stu-id="b71fd-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="b71fd-110">Справочные файлы</span><span class="sxs-lookup"><span data-stu-id="b71fd-110">Directory files</span></span>
* <span data-ttu-id="b71fd-111">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="b71fd-111">Environment variables</span></span>
* <span data-ttu-id="b71fd-112">объектов .NET в памяти;</span><span class="sxs-lookup"><span data-stu-id="b71fd-112">In-memory .NET objects</span></span>
* <span data-ttu-id="b71fd-113">файлов параметров;</span><span class="sxs-lookup"><span data-stu-id="b71fd-113">Settings files</span></span>

<span data-ttu-id="b71fd-114">*Шаблон параметров* является расширением конфигурации основных понятий, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="b71fd-114">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="b71fd-115">Параметры используют классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="b71fd-115">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="b71fd-116">Дополнительные сведения об использовании шаблона параметров см. в разделе <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-116">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="b71fd-117">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b71fd-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b71fd-118">Эти три пакета включены в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b71fd-118">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="host-vs-app-configuration"></a><span data-ttu-id="b71fd-119">Конфигурация узла и приложения</span><span class="sxs-lookup"><span data-stu-id="b71fd-119">Host vs. app configuration</span></span>

<span data-ttu-id="b71fd-120">Перед настройкой и запуском приложения настройте и запустите *узел*.</span><span class="sxs-lookup"><span data-stu-id="b71fd-120">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="b71fd-121">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="b71fd-121">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="b71fd-122">Как приложение, так и узел настраиваются с использованием поставщиков конфигурации, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="b71fd-122">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="b71fd-123">Пары "ключ — значение" конфигурации узлов становятся частью глобальной конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="b71fd-123">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="b71fd-124">Дополнительные сведения о том, как используются поставщики конфигурации при создании узла и как источники конфигурации влияют на его настройку, см. в разделе [Узел](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="b71fd-124">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see [The host](xref:fundamentals/index#host).</span></span>

## <a name="default-configuration"></a><span data-ttu-id="b71fd-125">Конфигурация по умолчанию</span><span class="sxs-lookup"><span data-stu-id="b71fd-125">Default configuration</span></span>

<span data-ttu-id="b71fd-126">Веб-приложения на основе шаблонов [dotnet new](/dotnet/core/tools/dotnet-new) ASP.NET Core вызывают <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> при создании узла.</span><span class="sxs-lookup"><span data-stu-id="b71fd-126">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="b71fd-127"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> предоставляет конфигурацию по умолчанию для приложения в следующем порядке:</span><span class="sxs-lookup"><span data-stu-id="b71fd-127"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> provides default configuration for the app in the following order:</span></span>

* <span data-ttu-id="b71fd-128">Существуют следующие способы предоставления конфигурации узла.</span><span class="sxs-lookup"><span data-stu-id="b71fd-128">Host configuration is provided from:</span></span>
  * <span data-ttu-id="b71fd-129">Переменные среды с префиксом `ASPNETCORE_` (например, `ASPNETCORE_ENVIRONMENT`), использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="b71fd-129">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="b71fd-130">Префикс (`ASPNETCORE_`) исключается при загрузке пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-130">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="b71fd-131">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="b71fd-131">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="b71fd-132">Конфигурация приложения предоставляется из следующих ресурсов:</span><span class="sxs-lookup"><span data-stu-id="b71fd-132">App configuration is provided from:</span></span>
  * <span data-ttu-id="b71fd-133">Файл *appsettings.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="b71fd-133">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="b71fd-134">Файл *appsettings.{Environment}.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="b71fd-134">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="b71fd-135">[Менеджера секретов](xref:security/app-secrets), когда приложение выполняется в среде `Development` с использованием начальных сборок.</span><span class="sxs-lookup"><span data-stu-id="b71fd-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="b71fd-136">Переменные среды, использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="b71fd-136">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="b71fd-137">Если используется пользовательский префикс (например, `PREFIX_` с `.AddEnvironmentVariables(prefix: "PREFIX_")`), такой префикс исключается при загрузке пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-137">If a custom prefix is used (for example, `PREFIX_` with `.AddEnvironmentVariables(prefix: "PREFIX_")`), the prefix is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="b71fd-138">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="b71fd-138">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="b71fd-139">Поставщики конфигурации описаны ниже в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="b71fd-139">The configuration providers are explained later in this topic.</span></span> <span data-ttu-id="b71fd-140">Дополнительные сведения об узле и <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>: <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-140">For more information on the host and <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="security"></a><span data-ttu-id="b71fd-141">Безопасность</span><span class="sxs-lookup"><span data-stu-id="b71fd-141">Security</span></span>

<span data-ttu-id="b71fd-142">Придерживайтесь следующих рекомендаций.</span><span class="sxs-lookup"><span data-stu-id="b71fd-142">Adopt the following best practices:</span></span>

* <span data-ttu-id="b71fd-143">Никогда не храните пароли или другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации обычного текста.</span><span class="sxs-lookup"><span data-stu-id="b71fd-143">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="b71fd-144">Не используйте секреты рабочей среды в средах разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="b71fd-144">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="b71fd-145">Указывайте секреты вне проекта, чтобы их нельзя было случайно зафиксировать в репозитории с исходным кодом.</span><span class="sxs-lookup"><span data-stu-id="b71fd-145">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="b71fd-146">Дополнительные сведения см. в статье [Использование нескольких сред в ASP.NET Core](xref:fundamentals/environments) и руководствуйтесь статьей [Безопасное хранение секретов приложения во время разработки в ASP.NET Core](xref:security/app-secrets) (включает рекомендации по использованию переменной среды для хранения конфиденциальных данных).</span><span class="sxs-lookup"><span data-stu-id="b71fd-146">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="b71fd-147">Менеджер секретов использует поставщик конфигурации файла для хранения конфиденциальных данных пользователя в файле JSON в локальной системе.</span><span class="sxs-lookup"><span data-stu-id="b71fd-147">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="b71fd-148">Поставщик конфигурации файлов описан ниже в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="b71fd-148">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="b71fd-149">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) — один из вариантов для безопасного хранения секретов приложения.</span><span class="sxs-lookup"><span data-stu-id="b71fd-149">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="b71fd-150">Для получения дополнительной информации см. <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-150">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="b71fd-151">Иерархическая модель конфигурации</span><span class="sxs-lookup"><span data-stu-id="b71fd-151">Hierarchical configuration data</span></span>

<span data-ttu-id="b71fd-152">API конфигурации способен поддерживать иерархические данные конфигурации, выполняя преобразование в плоскую структуру иерархических данных с использованием разделителя в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-152">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="b71fd-153">В следующем файле JSON существуют четыре ключа в структурированной иерархии двух разделов.</span><span class="sxs-lookup"><span data-stu-id="b71fd-153">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="b71fd-154">При считывании файла в конфигурацию для сохранения исходной иерархической структуры данных источника конфигурации создаются уникальные ключи.</span><span class="sxs-lookup"><span data-stu-id="b71fd-154">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="b71fd-155">Разделы и ключи преобразовываются в плоскую структуру с использованием двоеточия (`:`) для сохранения исходной структуры.</span><span class="sxs-lookup"><span data-stu-id="b71fd-155">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="b71fd-156">section0:key0</span><span class="sxs-lookup"><span data-stu-id="b71fd-156">section0:key0</span></span>
* <span data-ttu-id="b71fd-157">section0:key1</span><span class="sxs-lookup"><span data-stu-id="b71fd-157">section0:key1</span></span>
* <span data-ttu-id="b71fd-158">section1:key0</span><span class="sxs-lookup"><span data-stu-id="b71fd-158">section1:key0</span></span>
* <span data-ttu-id="b71fd-159">section1:key1</span><span class="sxs-lookup"><span data-stu-id="b71fd-159">section1:key1</span></span>

<span data-ttu-id="b71fd-160">Методы <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> и <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> доступны для изолирования разделов и дочерних элементов раздела в данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-160"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="b71fd-161">Эти методы описаны далее в разделе [GetSection, GetChildren и Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="b71fd-161">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span> <span data-ttu-id="b71fd-162">`GetSection` находится в пакете [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b71fd-162">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="conventions"></a><span data-ttu-id="b71fd-163">Соглашения</span><span class="sxs-lookup"><span data-stu-id="b71fd-163">Conventions</span></span>

<span data-ttu-id="b71fd-164">При запуске приложения источники конфигурации считываются в порядке, в котором были указаны их поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-164">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="b71fd-165">Поставщики файлов конфигурации имеют возможность перезагрузить конфигурацию при изменении базового файла параметров после запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="b71fd-165">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="b71fd-166">Поставщик конфигурации файлов описан ниже в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="b71fd-166">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="b71fd-167">Объект <xref:Microsoft.Extensions.Configuration.IConfiguration> доступен в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) приложения.</span><span class="sxs-lookup"><span data-stu-id="b71fd-167"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="b71fd-168"><xref:Microsoft.Extensions.Configuration.IConfiguration> можно внедрить в <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> Razor Pages, чтобы получить конфигурацию для класса:</span><span class="sxs-lookup"><span data-stu-id="b71fd-168"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

```csharp
// using Microsoft.Extensions.Configuration;

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

<span data-ttu-id="b71fd-169">Поставщики конфигурации не могут использовать контейнер DI, так как он недоступен при настройке узла.</span><span class="sxs-lookup"><span data-stu-id="b71fd-169">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="b71fd-170">В ключах конфигурации приняты следующие соглашения.</span><span class="sxs-lookup"><span data-stu-id="b71fd-170">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="b71fd-171">В ключах не учитывается регистр символов.</span><span class="sxs-lookup"><span data-stu-id="b71fd-171">Keys are case-insensitive.</span></span> <span data-ttu-id="b71fd-172">Например `ConnectionString` и `connectionstring` обрабатываются как эквивалентные ключи.</span><span class="sxs-lookup"><span data-stu-id="b71fd-172">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="b71fd-173">Если значение для одного и того же ключа установлено одним и тем же или разными поставщиками конфигурации, последним значением, установленным на ключе, является используемое значение.</span><span class="sxs-lookup"><span data-stu-id="b71fd-173">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="b71fd-174">Иерархические ключи</span><span class="sxs-lookup"><span data-stu-id="b71fd-174">Hierarchical keys</span></span>
  * <span data-ttu-id="b71fd-175">При взаимодействии с API конфигурации разделитель-двоеточие (`:`) поддерживается на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="b71fd-175">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="b71fd-176">В переменных среды разделитель-двоеточие может не работать на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="b71fd-176">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="b71fd-177">Двойной знак подчеркивания (`__`) поддерживается на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="b71fd-177">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="b71fd-178">В хранилище ключей Azure иерархические ключи используют `--` (два дефиса) в качестве разделителя.</span><span class="sxs-lookup"><span data-stu-id="b71fd-178">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="b71fd-179">Чтобы заменить дефисы двоеточием, при загрузке секретов в конфигурацию приложения необходимо указать код.</span><span class="sxs-lookup"><span data-stu-id="b71fd-179">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="b71fd-180"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-180">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="b71fd-181">Привязка массива описана в разделе [Привязка массива к классу](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="b71fd-181">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="b71fd-182">В значениях конфигурации учитываются следующие соглашения.</span><span class="sxs-lookup"><span data-stu-id="b71fd-182">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="b71fd-183">Значения являются строками.</span><span class="sxs-lookup"><span data-stu-id="b71fd-183">Values are strings.</span></span>
* <span data-ttu-id="b71fd-184">Значение NULL не может храниться в конфигурации или быть привязанным к объектам.</span><span class="sxs-lookup"><span data-stu-id="b71fd-184">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="b71fd-185">Поставщики</span><span class="sxs-lookup"><span data-stu-id="b71fd-185">Providers</span></span>

<span data-ttu-id="b71fd-186">В следующей таблице показаны поставщики конфигурации, доступные для приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b71fd-186">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="b71fd-187">Поставщик</span><span class="sxs-lookup"><span data-stu-id="b71fd-187">Provider</span></span> | <span data-ttu-id="b71fd-188">Предоставляет конфигурацию из &hellip;</span><span class="sxs-lookup"><span data-stu-id="b71fd-188">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="b71fd-189">[Поставщик конфигурации хранилища ключей Azure](xref:security/key-vault-configuration) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="b71fd-189">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="b71fd-190">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="b71fd-190">Azure Key Vault</span></span> |
| [<span data-ttu-id="b71fd-191">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="b71fd-191">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="b71fd-192">Параметры командной строки</span><span class="sxs-lookup"><span data-stu-id="b71fd-192">Command-line parameters</span></span> |
| [<span data-ttu-id="b71fd-193">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="b71fd-193">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="b71fd-194">Источник пользователя</span><span class="sxs-lookup"><span data-stu-id="b71fd-194">Custom source</span></span> |
| [<span data-ttu-id="b71fd-195">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="b71fd-195">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="b71fd-196">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="b71fd-196">Environment variables</span></span> |
| [<span data-ttu-id="b71fd-197">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="b71fd-197">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="b71fd-198">Файлы (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="b71fd-198">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="b71fd-199">Поставщик конфигурации ключа для каждого файла</span><span class="sxs-lookup"><span data-stu-id="b71fd-199">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="b71fd-200">Справочные файлы</span><span class="sxs-lookup"><span data-stu-id="b71fd-200">Directory files</span></span> |
| [<span data-ttu-id="b71fd-201">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="b71fd-201">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="b71fd-202">Коллекции оперативной памяти</span><span class="sxs-lookup"><span data-stu-id="b71fd-202">In-memory collections</span></span> |
| <span data-ttu-id="b71fd-203">[Секреты пользователей (Менеджер секретов)](xref:security/app-secrets) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="b71fd-203">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="b71fd-204">Файл в каталоге профиля пользователя</span><span class="sxs-lookup"><span data-stu-id="b71fd-204">File in the user profile directory</span></span> |

<span data-ttu-id="b71fd-205">Источники конфигурации считываются в том порядке, в котором при запуске указываются их поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-205">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="b71fd-206">Поставщики конфигурации в этом разделе описаны в алфавитном порядке, а не в необходимом вам порядке в коде.</span><span class="sxs-lookup"><span data-stu-id="b71fd-206">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="b71fd-207">Порядок поставщиков конфигурации в коде соответствует приоритетам ваших основных источников конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-207">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="b71fd-208">Типичная последовательность поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-208">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="b71fd-209">Файлы (*appsettings.json*, *appsettings.{Environment}.json*, где `{Environment}` — это текущая среда размещения приложения)</span><span class="sxs-lookup"><span data-stu-id="b71fd-209">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. <span data-ttu-id="b71fd-210">[Azure Key Vault](xref:security/key-vault-configuration);</span><span class="sxs-lookup"><span data-stu-id="b71fd-210">[Azure Key Vault](xref:security/key-vault-configuration)</span></span>
1. <span data-ttu-id="b71fd-211">[Секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки)</span><span class="sxs-lookup"><span data-stu-id="b71fd-211">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="b71fd-212">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="b71fd-212">Environment variables</span></span>
1. <span data-ttu-id="b71fd-213">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="b71fd-213">Command-line arguments</span></span>

<span data-ttu-id="b71fd-214">Общепринятой практикой является размещение поставщика конфигурации командной строки последним в ряду поставщиков, чтобы аргументы командной строки могли переопределять конфигурацию, установленную другими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="b71fd-214">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="b71fd-215">Эта последовательность поставщиков помещается в месте, где происходит инициализация нового класса <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-215">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="b71fd-216">Дополнительные сведения см. в руководстве по [настройке веб-узла](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="b71fd-216">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

## <a name="configureappconfiguration"></a><span data-ttu-id="b71fd-217">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="b71fd-217">ConfigureAppConfiguration</span></span>

<span data-ttu-id="b71fd-218">Вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> при сборке узла, чтобы указать поставщики конфигурации приложения в дополнение к тем, которые автоматически добавлены <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-218">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

<span data-ttu-id="b71fd-219">Конфигурация, предоставленная приложению в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, доступна во время запуска приложения, включая `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-219">Configuration supplied to the app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b71fd-220">Дополнительные сведения см. в разделе [Доступ к конфигурации во время запуска](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="b71fd-220">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="b71fd-221">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="b71fd-221">Command-line Configuration Provider</span></span>

<span data-ttu-id="b71fd-222"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> загружает конфигурацию из пары "ключ — значение" аргумента командной строки в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="b71fd-222">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="b71fd-223">Чтобы активировать конфигурацию командной строки, вызывается метод расширения <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> для экземпляра <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-223">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b71fd-224">`AddCommandLine` автоматически вызывается при инициализации нового <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-224">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="b71fd-225">Дополнительные сведения см. в руководстве по [настройке веб-узла](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="b71fd-225">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="b71fd-226">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="b71fd-226">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="b71fd-227">дополнительную конфигурацию из *appsettings.json* и *appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="b71fd-227">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="b71fd-228">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки);</span><span class="sxs-lookup"><span data-stu-id="b71fd-228">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="b71fd-229">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="b71fd-229">Environment variables.</span></span>

<span data-ttu-id="b71fd-230">`CreateDefaultBuilder` добавляет последним поставщика конфигурации командной строки.</span><span class="sxs-lookup"><span data-stu-id="b71fd-230">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="b71fd-231">Аргументы командной строки передаются в набор конфигурации переопределения среды выполнения, установленной другими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="b71fd-231">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="b71fd-232">`CreateDefaultBuilder` действует, когда создается узел.</span><span class="sxs-lookup"><span data-stu-id="b71fd-232">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="b71fd-233">Поэтому конфигурация командной строки, активированная с помощью `CreateDefaultBuilder`, может повлиять на настройку узла.</span><span class="sxs-lookup"><span data-stu-id="b71fd-233">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="b71fd-234">Чтобы указать конфигурацию приложения, при сборке узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-234">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="b71fd-235">Объект `AddCommandLine` уже был вызван методом `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-235">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="b71fd-236">Если нужно предоставить конфигурацию приложения, сохранив возможность переопределить ее через аргументы командной строки, вызовите в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>дополнительные поставщики приложения, в завершение вызвав `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-236">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="b71fd-237">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="b71fd-237">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="b71fd-238">**Пример**</span><span class="sxs-lookup"><span data-stu-id="b71fd-238">**Example**</span></span>

<span data-ttu-id="b71fd-239">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-239">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="b71fd-240">Откройте командную строку в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="b71fd-240">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="b71fd-241">Поставьте аргумент командной строки в команду `dotnet run`: `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-241">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="b71fd-242">После запуска приложения откройте браузер в приложении по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-242">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="b71fd-243">Обратите внимание, что вывод содержит пару "ключ — значение" для аргумента командной строки конфигурации, предоставленного для `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-243">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="b71fd-244">Аргументы</span><span class="sxs-lookup"><span data-stu-id="b71fd-244">Arguments</span></span>

<span data-ttu-id="b71fd-245">Значение должно соответствовать знаку равенства (`=`), или ключ должен иметь префикс (`--` или `/`), когда значение следует за пробелом.</span><span class="sxs-lookup"><span data-stu-id="b71fd-245">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="b71fd-246">Значение может соответствовать NULL, если используется знак равенства (например, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="b71fd-246">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="b71fd-247">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="b71fd-247">Key prefix</span></span>               | <span data-ttu-id="b71fd-248">Пример</span><span class="sxs-lookup"><span data-stu-id="b71fd-248">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="b71fd-249">Без префикса</span><span class="sxs-lookup"><span data-stu-id="b71fd-249">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="b71fd-250">Два дефиса (`--`)</span><span class="sxs-lookup"><span data-stu-id="b71fd-250">Two dashes (`--`)</span></span>        | <span data-ttu-id="b71fd-251">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="b71fd-251">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="b71fd-252">Прямая косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="b71fd-252">Forward slash (`/`)</span></span>      | <span data-ttu-id="b71fd-253">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="b71fd-253">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="b71fd-254">В рамках одной и той же команды не смешивайте пары "ключ — значение" аргумента командной строки, которые используют знак равенства, с парами "ключ — значение", которые используют пробел.</span><span class="sxs-lookup"><span data-stu-id="b71fd-254">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="b71fd-255">Примеры команд.</span><span class="sxs-lookup"><span data-stu-id="b71fd-255">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="b71fd-256">Сопоставления переключений</span><span class="sxs-lookup"><span data-stu-id="b71fd-256">Switch mappings</span></span>

<span data-ttu-id="b71fd-257">Сопоставление параметров позволяет указать логику замены имен ключей.</span><span class="sxs-lookup"><span data-stu-id="b71fd-257">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="b71fd-258">Когда вручную создается конфигурация с помощью <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, методу <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> можно предоставить словарь сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="b71fd-258">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="b71fd-259">В словаре сопоставлений переключений выполняется поиск ключа, который совпадает с ключом, предоставляемым аргументом командной строки.</span><span class="sxs-lookup"><span data-stu-id="b71fd-259">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="b71fd-260">Если ключ в командной строке находится в словаре, значение словаря (замена ключа) передается обратно, чтобы установить пару "ключ — значение" в конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="b71fd-260">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="b71fd-261">Сопоставление переключений необходимо для любого ключа командной строки с префиксом из одного дефиса (`-`).</span><span class="sxs-lookup"><span data-stu-id="b71fd-261">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="b71fd-262">Правила ключей из словаря сопоставления переключений:</span><span class="sxs-lookup"><span data-stu-id="b71fd-262">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="b71fd-263">Переключения должны начинаться с дефиса (`-`) или двойного дефиса (`--`).</span><span class="sxs-lookup"><span data-stu-id="b71fd-263">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="b71fd-264">Словарь сопоставлений переключений не должен содержать повторяющиеся ключи.</span><span class="sxs-lookup"><span data-stu-id="b71fd-264">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="b71fd-265">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-265">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="b71fd-266">Как показано в предыдущем примере, вызов `CreateDefaultBuilder` не должен передавать аргументы при использовании сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="b71fd-266">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="b71fd-267">Вызов команды `AddCommandLine` метода `CreateDefaultBuilder` не включает сопоставленные коммутаторы, и нет возможности передать словарь сопоставления переключений в `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-267">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="b71fd-268">Если аргументы включают сопоставления переключений и передаются в `CreateDefaultBuilder`, его поставщик `AddCommandLine` не может инициализироваться с помощью <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-268">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="b71fd-269">Чтобы решить эту проблему, нужно не передавать аргументы команде `CreateDefaultBuilder`, а позволить методу `AddCommandLine` метода `ConfigurationBuilder` обрабатывать как аргументы, так и словарь сопоставления параметров.</span><span class="sxs-lookup"><span data-stu-id="b71fd-269">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="b71fd-270">Созданный словарь сопоставлений переключений содержит данные, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="b71fd-270">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="b71fd-271">Ключ</span><span class="sxs-lookup"><span data-stu-id="b71fd-271">Key</span></span>       | <span data-ttu-id="b71fd-272">Значение</span><span class="sxs-lookup"><span data-stu-id="b71fd-272">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="b71fd-273">Если ключи сопоставления переключений используются при запуске приложения, конфигурация принимает значение конфигурации в ключе, предоставленном в словаре.</span><span class="sxs-lookup"><span data-stu-id="b71fd-273">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="b71fd-274">После выполнения предыдущей команды конфигурация содержит значения, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="b71fd-274">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="b71fd-275">Ключ</span><span class="sxs-lookup"><span data-stu-id="b71fd-275">Key</span></span>               | <span data-ttu-id="b71fd-276">Значение</span><span class="sxs-lookup"><span data-stu-id="b71fd-276">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="b71fd-277">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="b71fd-277">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="b71fd-278"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> загружает конфигурацию из пары "ключ — значение" переменной среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="b71fd-278">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="b71fd-279">Чтобы активировать конфигурацию переменных среды, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-279">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="b71fd-280">[Служба приложений Azure](https://azure.microsoft.com/services/app-service/) позволяет задать переменные среды на портале Azure, который может переопределить конфигурацию приложения, используя поставщик конфигурации переменных среды.</span><span class="sxs-lookup"><span data-stu-id="b71fd-280">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="b71fd-281">Дополнительные сведения см. в руководстве по [переопределению конфигурации приложения Azure с помощью портала Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="b71fd-281">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="b71fd-282">`AddEnvironmentVariables` автоматически вызывается для переменных среды, имеющих префикс `ASPNETCORE_`, при инициализации нового экземпляра <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-282">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="b71fd-283">Дополнительные сведения см. в руководстве по [настройке веб-узла](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="b71fd-283">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="b71fd-284">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="b71fd-284">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="b71fd-285">конфигурация приложения на основе переменных среды без префикса путем вызова `AddEnvironmentVariables` без префикса;</span><span class="sxs-lookup"><span data-stu-id="b71fd-285">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="b71fd-286">дополнительную конфигурацию из *appsettings.json* и *appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="b71fd-286">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="b71fd-287">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки);</span><span class="sxs-lookup"><span data-stu-id="b71fd-287">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="b71fd-288">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="b71fd-288">Command-line arguments.</span></span>

<span data-ttu-id="b71fd-289">Поставщик конфигурации переменных среды вызывается после выполнения настройки с помощью секретов пользователя и файлов *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="b71fd-289">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="b71fd-290">Вызов поставщика в этой позиции разрешает чтение переменных среды выполнения, чтобы переопределить конфигурацию, заданную секретом пользователя и файлом *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="b71fd-290">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="b71fd-291">Чтобы указать конфигурацию приложения, при сборке узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-291">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="b71fd-292">Если вам нужно добавить конфигурацию приложения из дополнительных переменных среды, вызовите дополнительные поставщики приложения в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, а затем вызовите `AddEnvironmentVariables` с префиксом.</span><span class="sxs-lookup"><span data-stu-id="b71fd-292">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="b71fd-293">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="b71fd-293">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="b71fd-294">**Пример**</span><span class="sxs-lookup"><span data-stu-id="b71fd-294">**Example**</span></span>

<span data-ttu-id="b71fd-295">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-295">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="b71fd-296">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="b71fd-296">Run the sample app.</span></span> <span data-ttu-id="b71fd-297">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-297">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="b71fd-298">Обратите внимание, что выходные данные содержат пару "ключ — значение" для переменной среды `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-298">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="b71fd-299">Значение отражает среду, в которой выполняется приложение, обычно при локальном запуске это `Development`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-299">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="b71fd-300">Чтобы список переменных сред, отображаемый приложением, был коротким, приложение фильтрует переменные среды по следующим пунктам:</span><span class="sxs-lookup"><span data-stu-id="b71fd-300">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="b71fd-301">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="b71fd-301">ASPNETCORE_</span></span>
* <span data-ttu-id="b71fd-302">urls</span><span class="sxs-lookup"><span data-stu-id="b71fd-302">urls</span></span>
* <span data-ttu-id="b71fd-303">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="b71fd-303">Logging</span></span>
* <span data-ttu-id="b71fd-304">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="b71fd-304">ENVIRONMENT</span></span>
* <span data-ttu-id="b71fd-305">contentRoot</span><span class="sxs-lookup"><span data-stu-id="b71fd-305">contentRoot</span></span>
* <span data-ttu-id="b71fd-306">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="b71fd-306">AllowedHosts</span></span>
* <span data-ttu-id="b71fd-307">applicationName</span><span class="sxs-lookup"><span data-stu-id="b71fd-307">applicationName</span></span>
* <span data-ttu-id="b71fd-308">CommandLine</span><span class="sxs-lookup"><span data-stu-id="b71fd-308">CommandLine</span></span>

<span data-ttu-id="b71fd-309">Если хотите просмотреть все переменные среды, доступные для приложения, измените значение `FilteredConfiguration` в *Pages/Index.cshtml.cs* на следующее:</span><span class="sxs-lookup"><span data-stu-id="b71fd-309">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="b71fd-310">Префиксы</span><span class="sxs-lookup"><span data-stu-id="b71fd-310">Prefixes</span></span>

<span data-ttu-id="b71fd-311">Переменные среды, которые загружаются в конфигурацию приложения, фильтруются при добавлении префикса к методу `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-311">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="b71fd-312">Например, чтобы отфильтровать переменные среды по префиксу `CUSTOM_`, введите префикс поставщику конфигурации:</span><span class="sxs-lookup"><span data-stu-id="b71fd-312">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="b71fd-313">Префикс отделяется при создании пары конфигурации "ключ — значение".</span><span class="sxs-lookup"><span data-stu-id="b71fd-313">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="b71fd-314">Статический удобный метод `CreateDefaultBuilder` создает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> для установления размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="b71fd-314">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="b71fd-315">Когда значение `WebHostBuilder` будет создано, оно найдет конфигурацию узла в переменных среды, которые начинаются с префикса `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-315">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

<span data-ttu-id="b71fd-316">**Префиксы строк подключения**</span><span class="sxs-lookup"><span data-stu-id="b71fd-316">**Connection string prefixes**</span></span>

<span data-ttu-id="b71fd-317">API конфигурации имеет специальные правила обработки для четырех строк подключения переменных среды, связанных с настройкой строк подключения Azure для среды приложения.</span><span class="sxs-lookup"><span data-stu-id="b71fd-317">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="b71fd-318">Если префикс не указан в `AddEnvironmentVariables`, переменные среды с префиксами, указанными в таблице, загружаются в приложение.</span><span class="sxs-lookup"><span data-stu-id="b71fd-318">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="b71fd-319">Префикс строки подключения</span><span class="sxs-lookup"><span data-stu-id="b71fd-319">Connection string prefix</span></span> | <span data-ttu-id="b71fd-320">Поставщик</span><span class="sxs-lookup"><span data-stu-id="b71fd-320">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="b71fd-321">Поставщик пользователя</span><span class="sxs-lookup"><span data-stu-id="b71fd-321">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="b71fd-322">MySQL</span><span class="sxs-lookup"><span data-stu-id="b71fd-322">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="b71fd-323">База данных SQL Azure</span><span class="sxs-lookup"><span data-stu-id="b71fd-323">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="b71fd-324">SQL Server</span><span class="sxs-lookup"><span data-stu-id="b71fd-324">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="b71fd-325">Когда переменная среды обнаруживается и загружается в конфигурацию с одним из четырех префиксов, приведенных в таблице, происходит следующее.</span><span class="sxs-lookup"><span data-stu-id="b71fd-325">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="b71fd-326">Ключ конфигурации создается путем удаления префикса переменных среды и добавления ключа раздела конфигурации (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="b71fd-326">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="b71fd-327">Создается новая пара "ключ — значение" конфигурации, которая представляет поставщика подключения базы данных (за исключением `CUSTOMCONNSTR_`, который не имеет указанного поставщика).</span><span class="sxs-lookup"><span data-stu-id="b71fd-327">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="b71fd-328">Ключ переменной среды</span><span class="sxs-lookup"><span data-stu-id="b71fd-328">Environment variable key</span></span> | <span data-ttu-id="b71fd-329">Преобразованный ключ конфигурации</span><span class="sxs-lookup"><span data-stu-id="b71fd-329">Converted configuration key</span></span> | <span data-ttu-id="b71fd-330">Запись конфигурации поставщика</span><span class="sxs-lookup"><span data-stu-id="b71fd-330">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="b71fd-331">Запись конфигурации не создана.</span><span class="sxs-lookup"><span data-stu-id="b71fd-331">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="b71fd-332">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="b71fd-332">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="b71fd-333">Значение: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="b71fd-333">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="b71fd-334">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="b71fd-334">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="b71fd-335">Значение: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="b71fd-335">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="b71fd-336">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="b71fd-336">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="b71fd-337">Значение: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="b71fd-337">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="b71fd-338">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="b71fd-338">File Configuration Provider</span></span>

<span data-ttu-id="b71fd-339"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> является базовым классом для загрузки конфигурации из файловой системы.</span><span class="sxs-lookup"><span data-stu-id="b71fd-339"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="b71fd-340">Следующие поставщики конфигурации предназначены для определенных типов файлов:</span><span class="sxs-lookup"><span data-stu-id="b71fd-340">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="b71fd-341">Поставщик конфигурации INI</span><span class="sxs-lookup"><span data-stu-id="b71fd-341">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="b71fd-342">Поставщик конфигурации JSON</span><span class="sxs-lookup"><span data-stu-id="b71fd-342">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="b71fd-343">Поставщик конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="b71fd-343">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="b71fd-344">Поставщик конфигурации INI</span><span class="sxs-lookup"><span data-stu-id="b71fd-344">INI Configuration Provider</span></span>

<span data-ttu-id="b71fd-345"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> загружает конфигурацию из пары "ключ — значение" INI-файла во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="b71fd-345">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="b71fd-346">Чтобы активировать конфигурацию INI-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-346">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b71fd-347">Двоеточие можно использовать как разделитель раздела в конфигурации файла INI.</span><span class="sxs-lookup"><span data-stu-id="b71fd-347">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="b71fd-348">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="b71fd-348">Overloads permit specifying:</span></span>

* <span data-ttu-id="b71fd-349">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="b71fd-349">Whether the file is optional.</span></span>
* <span data-ttu-id="b71fd-350">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="b71fd-350">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="b71fd-351"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="b71fd-351">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="b71fd-352">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-352">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="b71fd-353">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-353">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b71fd-354">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b71fd-354">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b71fd-355">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="b71fd-355">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="b71fd-356">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-356">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b71fd-357">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b71fd-357">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b71fd-358">Общий пример конфигурации INI-файла.</span><span class="sxs-lookup"><span data-stu-id="b71fd-358">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="b71fd-359">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-359">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="b71fd-360">section0:key0</span><span class="sxs-lookup"><span data-stu-id="b71fd-360">section0:key0</span></span>
* <span data-ttu-id="b71fd-361">section0:key1</span><span class="sxs-lookup"><span data-stu-id="b71fd-361">section0:key1</span></span>
* <span data-ttu-id="b71fd-362">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="b71fd-362">section1:subsection:key</span></span>
* <span data-ttu-id="b71fd-363">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="b71fd-363">section2:subsection0:key</span></span>
* <span data-ttu-id="b71fd-364">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="b71fd-364">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="b71fd-365">Поставщик конфигурации JSON</span><span class="sxs-lookup"><span data-stu-id="b71fd-365">JSON Configuration Provider</span></span>

<span data-ttu-id="b71fd-366"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> загружает конфигурацию из пары "ключ — значение" JSON-файла в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="b71fd-366">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="b71fd-367">Чтобы активировать конфигурацию JSON-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-367">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b71fd-368">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="b71fd-368">Overloads permit specifying:</span></span>

* <span data-ttu-id="b71fd-369">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="b71fd-369">Whether the file is optional.</span></span>
* <span data-ttu-id="b71fd-370">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="b71fd-370">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="b71fd-371"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="b71fd-371">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="b71fd-372">`AddJsonFile` автоматически вызывается дважды при инициализации нового <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-372">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="b71fd-373">Метод вызывается для загрузки конфигурации из:</span><span class="sxs-lookup"><span data-stu-id="b71fd-373">The method is called to load configuration from:</span></span>

* <span data-ttu-id="b71fd-374">*appsettings.json* &ndash; первым читается этот файл.</span><span class="sxs-lookup"><span data-stu-id="b71fd-374">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="b71fd-375">Версия файла среды может переопределить значения, предоставленные *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="b71fd-375">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="b71fd-376">*appsettings.{Environment}.json* — версия среды файла загружается на основе [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="b71fd-376">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="b71fd-377">Дополнительные сведения см. в руководстве по [настройке веб-узла](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="b71fd-377">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="b71fd-378">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="b71fd-378">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="b71fd-379">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="b71fd-379">Environment variables.</span></span>
* <span data-ttu-id="b71fd-380">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки);</span><span class="sxs-lookup"><span data-stu-id="b71fd-380">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="b71fd-381">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="b71fd-381">Command-line arguments.</span></span>

<span data-ttu-id="b71fd-382">Сначала устанавливается поставщик конфигурации JSON-файлов.</span><span class="sxs-lookup"><span data-stu-id="b71fd-382">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="b71fd-383">Таким образом, секреты пользователя, переменные среды и аргументы командной строки переопределят конфигурацию, заданную файлами *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="b71fd-383">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="b71fd-384">Вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> при сборке узла, чтобы указать конфигурацию приложения для файлов, отличных от *appsettings.json* и *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="b71fd-384">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

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

<span data-ttu-id="b71fd-385">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-385">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b71fd-386">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b71fd-386">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b71fd-387">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="b71fd-387">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="b71fd-388">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-388">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b71fd-389">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b71fd-389">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b71fd-390">**Пример**</span><span class="sxs-lookup"><span data-stu-id="b71fd-390">**Example**</span></span>

<span data-ttu-id="b71fd-391">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает два вызова `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-391">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="b71fd-392">Конфигурация загружена из *appsettings.json* и *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="b71fd-392">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="b71fd-393">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="b71fd-393">Run the sample app.</span></span> <span data-ttu-id="b71fd-394">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-394">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="b71fd-395">Обратите внимание, что выходные данные содержат пары "ключ — значение" для конфигурации, представленной в таблице в зависимости от среды.</span><span class="sxs-lookup"><span data-stu-id="b71fd-395">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="b71fd-396">Ключи конфигурации ведения журнала используют двоеточие (`:`) как иерархический разделитель.</span><span class="sxs-lookup"><span data-stu-id="b71fd-396">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="b71fd-397">Ключ</span><span class="sxs-lookup"><span data-stu-id="b71fd-397">Key</span></span>                        | <span data-ttu-id="b71fd-398">Значение разработки</span><span class="sxs-lookup"><span data-stu-id="b71fd-398">Development Value</span></span> | <span data-ttu-id="b71fd-399">Рабочее значение</span><span class="sxs-lookup"><span data-stu-id="b71fd-399">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="b71fd-400">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="b71fd-400">Logging:LogLevel:System</span></span>    | <span data-ttu-id="b71fd-401">Сведения</span><span class="sxs-lookup"><span data-stu-id="b71fd-401">Information</span></span>       | <span data-ttu-id="b71fd-402">Сведения</span><span class="sxs-lookup"><span data-stu-id="b71fd-402">Information</span></span>      |
| <span data-ttu-id="b71fd-403">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="b71fd-403">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="b71fd-404">Сведения</span><span class="sxs-lookup"><span data-stu-id="b71fd-404">Information</span></span>       | <span data-ttu-id="b71fd-405">Сведения</span><span class="sxs-lookup"><span data-stu-id="b71fd-405">Information</span></span>      |
| <span data-ttu-id="b71fd-406">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="b71fd-406">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="b71fd-407">Отладка</span><span class="sxs-lookup"><span data-stu-id="b71fd-407">Debug</span></span>             | <span data-ttu-id="b71fd-408">Error</span><span class="sxs-lookup"><span data-stu-id="b71fd-408">Error</span></span>            |
| <span data-ttu-id="b71fd-409">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="b71fd-409">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="b71fd-410">Поставщик конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="b71fd-410">XML Configuration Provider</span></span>

<span data-ttu-id="b71fd-411"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> загружает конфигурацию из пары "ключ — значение" XML-файла в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="b71fd-411">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="b71fd-412">Чтобы активировать конфигурацию XML-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-412">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b71fd-413">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="b71fd-413">Overloads permit specifying:</span></span>

* <span data-ttu-id="b71fd-414">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="b71fd-414">Whether the file is optional.</span></span>
* <span data-ttu-id="b71fd-415">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="b71fd-415">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="b71fd-416"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="b71fd-416">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="b71fd-417">Корневой узел файла конфигурации не учитывается при создании пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-417">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="b71fd-418">Не указывайте в файле Document Type Definition (определение типа документа, DTD) или пространство имен.</span><span class="sxs-lookup"><span data-stu-id="b71fd-418">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="b71fd-419">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-419">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="b71fd-420">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-420">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b71fd-421">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b71fd-421">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b71fd-422">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="b71fd-422">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="b71fd-423">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-423">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b71fd-424">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b71fd-424">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b71fd-425">XML-файлы конфигурации могут использовать имена отдельных элементов для повторяющихся разделов.</span><span class="sxs-lookup"><span data-stu-id="b71fd-425">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="b71fd-426">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-426">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="b71fd-427">section0:key0</span><span class="sxs-lookup"><span data-stu-id="b71fd-427">section0:key0</span></span>
* <span data-ttu-id="b71fd-428">section0:key1</span><span class="sxs-lookup"><span data-stu-id="b71fd-428">section0:key1</span></span>
* <span data-ttu-id="b71fd-429">section1:key0</span><span class="sxs-lookup"><span data-stu-id="b71fd-429">section1:key0</span></span>
* <span data-ttu-id="b71fd-430">section1:key1</span><span class="sxs-lookup"><span data-stu-id="b71fd-430">section1:key1</span></span>

<span data-ttu-id="b71fd-431">Повторяющиеся элементы, использующие то же имя элемента, работают, если атрибут `name` используется для различения элементов.</span><span class="sxs-lookup"><span data-stu-id="b71fd-431">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="b71fd-432">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-432">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="b71fd-433">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="b71fd-433">section:section0:key:key0</span></span>
* <span data-ttu-id="b71fd-434">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="b71fd-434">section:section0:key:key1</span></span>
* <span data-ttu-id="b71fd-435">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="b71fd-435">section:section1:key:key0</span></span>
* <span data-ttu-id="b71fd-436">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="b71fd-436">section:section1:key:key1</span></span>

<span data-ttu-id="b71fd-437">Атрибуты можно использовать для предоставления значений.</span><span class="sxs-lookup"><span data-stu-id="b71fd-437">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="b71fd-438">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-438">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="b71fd-439">key:attribute</span><span class="sxs-lookup"><span data-stu-id="b71fd-439">key:attribute</span></span>
* <span data-ttu-id="b71fd-440">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="b71fd-440">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="b71fd-441">Поставщик конфигурации ключа для каждого файла</span><span class="sxs-lookup"><span data-stu-id="b71fd-441">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="b71fd-442"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> использует файлы каталога как пары "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-442">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="b71fd-443">Ключ является именем файла.</span><span class="sxs-lookup"><span data-stu-id="b71fd-443">The key is the file name.</span></span> <span data-ttu-id="b71fd-444">Значение содержит содержимое файла.</span><span class="sxs-lookup"><span data-stu-id="b71fd-444">The value contains the file's contents.</span></span> <span data-ttu-id="b71fd-445">Поставщик конфигурации ключа для каждого файла используется в сценариях размещения Docker.</span><span class="sxs-lookup"><span data-stu-id="b71fd-445">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="b71fd-446">Чтобы активировать конфигурацию ключа для каждого файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-446">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="b71fd-447">Значение параметра `directoryPath` должно быть абсолютным путем к файлам.</span><span class="sxs-lookup"><span data-stu-id="b71fd-447">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="b71fd-448">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="b71fd-448">Overloads permit specifying:</span></span>

* <span data-ttu-id="b71fd-449">`Action<KeyPerFileConfigurationSource>` — делегат, который настраивает источник.</span><span class="sxs-lookup"><span data-stu-id="b71fd-449">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="b71fd-450">Обязательно ли указывать каталог и путь к каталогу.</span><span class="sxs-lookup"><span data-stu-id="b71fd-450">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="b71fd-451">Двойное подчеркивание (`__`) используется в качестве разделителя ключа конфигурации в именах файлов.</span><span class="sxs-lookup"><span data-stu-id="b71fd-451">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="b71fd-452">Например, в имени файла `Logging__LogLevel__System` создается ключ конфигурации `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-452">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="b71fd-453">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-453">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="b71fd-454">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-454">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b71fd-455">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b71fd-455">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b71fd-456">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="b71fd-456">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="b71fd-457">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="b71fd-457">Memory Configuration Provider</span></span>

<span data-ttu-id="b71fd-458"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> использует коллекцию памяти в качестве пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-458">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="b71fd-459">Чтобы активировать конфигурацию коллекции в памяти, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-459">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b71fd-460">Поставщик конфигурации может инициализироваться значением `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-460">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="b71fd-461">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-461">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="b71fd-462">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="b71fd-462">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="b71fd-463">GetValue</span><span class="sxs-lookup"><span data-stu-id="b71fd-463">GetValue</span></span>

<span data-ttu-id="b71fd-464">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) извлекает значение из конфигурации с указанным ключом и преобразует его в указанный тип.</span><span class="sxs-lookup"><span data-stu-id="b71fd-464">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="b71fd-465">Если ключ не найден, перегрузка позволяет предоставлять значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b71fd-465">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="b71fd-466">В следующем примере происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="b71fd-466">The following example:</span></span>

* <span data-ttu-id="b71fd-467">Из конфигурации извлекается строковое значение с ключом `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-467">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="b71fd-468">Если `NumberKey` отсутствует в ключах конфигурации, используется значение по умолчанию `99`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-468">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="b71fd-469">Значение получает тип `int`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-469">Types the value as an `int`.</span></span>
* <span data-ttu-id="b71fd-470">Значение сохраняется в свойстве `NumberConfig` для использования на странице.</span><span class="sxs-lookup"><span data-stu-id="b71fd-470">Stores the value in the `NumberConfig` property for use by the page.</span></span>

```csharp
// using Microsoft.Extensions.Configuration;

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="b71fd-471">GetSection, GetChildren и Exists</span><span class="sxs-lookup"><span data-stu-id="b71fd-471">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="b71fd-472">В следующих примерах рассмотрим файл JSON.</span><span class="sxs-lookup"><span data-stu-id="b71fd-472">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="b71fd-473">Четыре ключа находятся в двух разделах, один из которых содержит пару из подразделов.</span><span class="sxs-lookup"><span data-stu-id="b71fd-473">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="b71fd-474">Когда файл считывается в конфигурацию, для сохранения значений конфигурации создаются следующие уникальные иерархические ключи.</span><span class="sxs-lookup"><span data-stu-id="b71fd-474">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="b71fd-475">section0:key0</span><span class="sxs-lookup"><span data-stu-id="b71fd-475">section0:key0</span></span>
* <span data-ttu-id="b71fd-476">section0:key1</span><span class="sxs-lookup"><span data-stu-id="b71fd-476">section0:key1</span></span>
* <span data-ttu-id="b71fd-477">section1:key0</span><span class="sxs-lookup"><span data-stu-id="b71fd-477">section1:key0</span></span>
* <span data-ttu-id="b71fd-478">section1:key1</span><span class="sxs-lookup"><span data-stu-id="b71fd-478">section1:key1</span></span>
* <span data-ttu-id="b71fd-479">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="b71fd-479">section2:subsection0:key0</span></span>
* <span data-ttu-id="b71fd-480">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="b71fd-480">section2:subsection0:key1</span></span>
* <span data-ttu-id="b71fd-481">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="b71fd-481">section2:subsection1:key0</span></span>
* <span data-ttu-id="b71fd-482">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="b71fd-482">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="b71fd-483">GetSection</span><span class="sxs-lookup"><span data-stu-id="b71fd-483">GetSection</span></span>

<span data-ttu-id="b71fd-484">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) извлекает подраздел конфигурации с указанным ключом подраздела.</span><span class="sxs-lookup"><span data-stu-id="b71fd-484">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span> <span data-ttu-id="b71fd-485">`GetSection` находится в пакете [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b71fd-485">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b71fd-486">Чтобы вернуть <xref:Microsoft.Extensions.Configuration.IConfigurationSection>, содержащий только пары "ключ — значение" в `section1`, вызовите `GetSection` и укажите имя раздела.</span><span class="sxs-lookup"><span data-stu-id="b71fd-486">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="b71fd-487">`configSection` не содержит значение, только ключ и путь.</span><span class="sxs-lookup"><span data-stu-id="b71fd-487">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="b71fd-488">Аналогично, чтобы получить значения для ключей в `section2:subsection0`, вызовите `GetSection` и укажите путь к разделу.</span><span class="sxs-lookup"><span data-stu-id="b71fd-488">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="b71fd-489">Значение `GetSection` никогда не возвращает значение `null`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-489">`GetSection` never returns `null`.</span></span> <span data-ttu-id="b71fd-490">Если соответствующий раздел не найден, возвращается пустой параметр `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-490">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="b71fd-491">Когда `GetSection` возвращает соответствующий раздел, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> не заполняется.</span><span class="sxs-lookup"><span data-stu-id="b71fd-491">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="b71fd-492"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> и <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> возвращаются, если раздел существует.</span><span class="sxs-lookup"><span data-stu-id="b71fd-492">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="b71fd-493">GetChildren</span><span class="sxs-lookup"><span data-stu-id="b71fd-493">GetChildren</span></span>

<span data-ttu-id="b71fd-494">Вызов [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) в `section2` получает значение `IEnumerable<IConfigurationSection>`, которое включает:</span><span class="sxs-lookup"><span data-stu-id="b71fd-494">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="b71fd-495">Exists</span><span class="sxs-lookup"><span data-stu-id="b71fd-495">Exists</span></span>

<span data-ttu-id="b71fd-496">Используйте [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*), чтобы определить, существует ли раздел конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-496">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="b71fd-497">Учитывая данные примера, `sectionExists` является `false`, потому что в данных конфигурации нет `section2:subsection2`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-497">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="b71fd-498">Привязка к классу</span><span class="sxs-lookup"><span data-stu-id="b71fd-498">Bind to a class</span></span>

<span data-ttu-id="b71fd-499">Конфигурация может быть привязана к классам, которые представляют группы связанных параметров, используя шаблон *вариантов*.</span><span class="sxs-lookup"><span data-stu-id="b71fd-499">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="b71fd-500">Для получения дополнительной информации см. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-500">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="b71fd-501">Значения конфигурации возвращаются как строки, но вызов <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> позволяет построить объекты [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="b71fd-501">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="b71fd-502">`Bind` находится в пакете [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b71fd-502">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b71fd-503">Пример приложения содержит модель `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="b71fd-503">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="b71fd-504">Раздел `starship` файла *starship.json* создает конфигурацию, когда образец приложения использует поставщик конфигурации JSON для загрузки конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-504">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

<span data-ttu-id="b71fd-505">Создаются следующие пары "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-505">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="b71fd-506">Ключ</span><span class="sxs-lookup"><span data-stu-id="b71fd-506">Key</span></span>                   | <span data-ttu-id="b71fd-507">Значение</span><span class="sxs-lookup"><span data-stu-id="b71fd-507">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="b71fd-508">starship:name</span><span class="sxs-lookup"><span data-stu-id="b71fd-508">starship:name</span></span>         | <span data-ttu-id="b71fd-509">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="b71fd-509">USS Enterprise</span></span>                                    |
| <span data-ttu-id="b71fd-510">starship:registry</span><span class="sxs-lookup"><span data-stu-id="b71fd-510">starship:registry</span></span>     | <span data-ttu-id="b71fd-511">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="b71fd-511">NCC-1701</span></span>                                          |
| <span data-ttu-id="b71fd-512">starship:class</span><span class="sxs-lookup"><span data-stu-id="b71fd-512">starship:class</span></span>        | <span data-ttu-id="b71fd-513">Constitution</span><span class="sxs-lookup"><span data-stu-id="b71fd-513">Constitution</span></span>                                      |
| <span data-ttu-id="b71fd-514">starship:length</span><span class="sxs-lookup"><span data-stu-id="b71fd-514">starship:length</span></span>       | <span data-ttu-id="b71fd-515">304.8</span><span class="sxs-lookup"><span data-stu-id="b71fd-515">304.8</span></span>                                             |
| <span data-ttu-id="b71fd-516">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="b71fd-516">starship:commissioned</span></span> | <span data-ttu-id="b71fd-517">False</span><span class="sxs-lookup"><span data-stu-id="b71fd-517">False</span></span>                                             |
| <span data-ttu-id="b71fd-518">trademark</span><span class="sxs-lookup"><span data-stu-id="b71fd-518">trademark</span></span>             | <span data-ttu-id="b71fd-519">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="b71fd-519">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="b71fd-520">Пример приложения вызывает `GetSection` с помощью ключа `starship`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-520">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="b71fd-521">Пары "ключ — значение" `starship` изолированы.</span><span class="sxs-lookup"><span data-stu-id="b71fd-521">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="b71fd-522">Метод `Bind` вызывается в подразделе при прохождении в экземпляр класса `Starship`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-522">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="b71fd-523">После привязки значения экземпляра экземпляру присваивается свойство для преобразования для просмотра.</span><span class="sxs-lookup"><span data-stu-id="b71fd-523">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

<span data-ttu-id="b71fd-524">`GetSection` находится в пакете [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b71fd-524">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="b71fd-525">Привязка к графу объектов</span><span class="sxs-lookup"><span data-stu-id="b71fd-525">Bind to an object graph</span></span>

<span data-ttu-id="b71fd-526"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> способна связывать весь граф объекта POCO.</span><span class="sxs-lookup"><span data-stu-id="b71fd-526"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="b71fd-527">`Bind` находится в пакете [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b71fd-527">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b71fd-528">Образец содержит модель `TvShow`, в графе объектов которого находятся классы `Metadata` и `Actors` (*Модели/TvShow.cs*).</span><span class="sxs-lookup"><span data-stu-id="b71fd-528">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="b71fd-529">В примере приложения есть файл *tvshow.xml*, содержащий данные конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-529">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="b71fd-530">Конфигурация привязана ко всему ​​графу объектов `TvShow` с помощью метода `Bind`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-530">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="b71fd-531">Привязанный экземпляр присваивается свойству для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="b71fd-531">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="b71fd-532">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) привязывает и возвращает указанный тип.</span><span class="sxs-lookup"><span data-stu-id="b71fd-532">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="b71fd-533">Метод `Get<T>` может быть более удобным, чем использование `Bind`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-533">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="b71fd-534">В следующем коде показано, как использовать `Get<T>` с предыдущим примером, который позволяет привязать связанный экземпляр непосредственно к свойству, используемому для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="b71fd-534">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

<span data-ttu-id="b71fd-535"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> находится в пакете [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b71fd-535"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="b71fd-536">`Get<T>` доступно в ASP.NET Core 1.1 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="b71fd-536">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="b71fd-537">`GetSection` находится в пакете [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b71fd-537">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="b71fd-538">Привязка массива к классу</span><span class="sxs-lookup"><span data-stu-id="b71fd-538">Bind an array to a class</span></span>

<span data-ttu-id="b71fd-539">*Пример приложения демонстрирует концепции, описанные в этом разделе.*</span><span class="sxs-lookup"><span data-stu-id="b71fd-539">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="b71fd-540"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-540">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="b71fd-541">Любой формат массива, который предоставляет сегмент числового ключа (`:0:`, `:1:`, &hellip; `:{n}:`), способен привязать массив к массиву класса POCO.</span><span class="sxs-lookup"><span data-stu-id="b71fd-541">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span> <span data-ttu-id="b71fd-542">"Bind" находится в пакете [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b71fd-542">\`Bind\`\` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

> [!NOTE]
> <span data-ttu-id="b71fd-543">Привязка предоставляется соглашением.</span><span class="sxs-lookup"><span data-stu-id="b71fd-543">Binding is provided by convention.</span></span> <span data-ttu-id="b71fd-544">Пользовательские поставщики конфигурации не обязаны реализовывать привязку массива.</span><span class="sxs-lookup"><span data-stu-id="b71fd-544">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="b71fd-545">**Обработка массива в оперативной памяти**</span><span class="sxs-lookup"><span data-stu-id="b71fd-545">**In-memory array processing**</span></span>

<span data-ttu-id="b71fd-546">Рассмотрите ключи и значения конфигурации, приведенные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="b71fd-546">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="b71fd-547">Ключ</span><span class="sxs-lookup"><span data-stu-id="b71fd-547">Key</span></span>             | <span data-ttu-id="b71fd-548">Значение</span><span class="sxs-lookup"><span data-stu-id="b71fd-548">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="b71fd-549">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="b71fd-549">array:entries:0</span></span> | <span data-ttu-id="b71fd-550">value0</span><span class="sxs-lookup"><span data-stu-id="b71fd-550">value0</span></span> |
| <span data-ttu-id="b71fd-551">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="b71fd-551">array:entries:1</span></span> | <span data-ttu-id="b71fd-552">value1</span><span class="sxs-lookup"><span data-stu-id="b71fd-552">value1</span></span> |
| <span data-ttu-id="b71fd-553">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="b71fd-553">array:entries:2</span></span> | <span data-ttu-id="b71fd-554">value2</span><span class="sxs-lookup"><span data-stu-id="b71fd-554">value2</span></span> |
| <span data-ttu-id="b71fd-555">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="b71fd-555">array:entries:4</span></span> | <span data-ttu-id="b71fd-556">value4</span><span class="sxs-lookup"><span data-stu-id="b71fd-556">value4</span></span> |
| <span data-ttu-id="b71fd-557">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="b71fd-557">array:entries:5</span></span> | <span data-ttu-id="b71fd-558">value5</span><span class="sxs-lookup"><span data-stu-id="b71fd-558">value5</span></span> |

<span data-ttu-id="b71fd-559">Эти ключи и значения загружаются в пример приложения с помощью поставщика конфигурации памяти.</span><span class="sxs-lookup"><span data-stu-id="b71fd-559">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

<span data-ttu-id="b71fd-560">Массив пропускает значения для индекса &num;3.</span><span class="sxs-lookup"><span data-stu-id="b71fd-560">The array skips a value for index &num;3.</span></span> <span data-ttu-id="b71fd-561">Связующее свойство конфигурации не способно связывать нулевые значения или создавать нулевые записи в связанных объектах, что становится ясным в тот момент, когда демонстрируется результат привязки этого массива к объекту.</span><span class="sxs-lookup"><span data-stu-id="b71fd-561">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="b71fd-562">В примере приложения класс POCO доступен для хранения привязанных данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-562">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="b71fd-563">Данные конфигурации, связанные с объектом.</span><span class="sxs-lookup"><span data-stu-id="b71fd-563">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="b71fd-564">`GetSection` находится в пакете [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b71fd-564">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b71fd-565">Синтаксис [ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) также может использоваться для получения более компактного кода.</span><span class="sxs-lookup"><span data-stu-id="b71fd-565">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="b71fd-566">Связанный объект, экземпляр `ArrayExample`, получает данные массива из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-566">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="b71fd-567">Индекс `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="b71fd-567">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="b71fd-568">Значение `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="b71fd-568">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="b71fd-569">0</span><span class="sxs-lookup"><span data-stu-id="b71fd-569">0</span></span>                            | <span data-ttu-id="b71fd-570">value0</span><span class="sxs-lookup"><span data-stu-id="b71fd-570">value0</span></span>                       |
| <span data-ttu-id="b71fd-571">1</span><span class="sxs-lookup"><span data-stu-id="b71fd-571">1</span></span>                            | <span data-ttu-id="b71fd-572">value1</span><span class="sxs-lookup"><span data-stu-id="b71fd-572">value1</span></span>                       |
| <span data-ttu-id="b71fd-573">2</span><span class="sxs-lookup"><span data-stu-id="b71fd-573">2</span></span>                            | <span data-ttu-id="b71fd-574">value2</span><span class="sxs-lookup"><span data-stu-id="b71fd-574">value2</span></span>                       |
| <span data-ttu-id="b71fd-575">3</span><span class="sxs-lookup"><span data-stu-id="b71fd-575">3</span></span>                            | <span data-ttu-id="b71fd-576">value4</span><span class="sxs-lookup"><span data-stu-id="b71fd-576">value4</span></span>                       |
| <span data-ttu-id="b71fd-577">4</span><span class="sxs-lookup"><span data-stu-id="b71fd-577">4</span></span>                            | <span data-ttu-id="b71fd-578">value5</span><span class="sxs-lookup"><span data-stu-id="b71fd-578">value5</span></span>                       |

<span data-ttu-id="b71fd-579">Индекс &num;3 в связанном объекте содержит данные конфигурации для конфигурационного ключа `array:4` и его значения `value4`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-579">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="b71fd-580">При привязке данных конфигурации, содержащих массив индексов, в ключах конфигурации эти индексы просто используются для выполнения итерации данных конфигурации при создании объекта.</span><span class="sxs-lookup"><span data-stu-id="b71fd-580">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="b71fd-581">Когда массив ключей конфигурации пропускает один или несколько индексов, в данных конфигурации не может быть сохранено нулевое значение и в связанном объекте не создается нулевая запись.</span><span class="sxs-lookup"><span data-stu-id="b71fd-581">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="b71fd-582">Отсутствующий элемент конфигурации для индекса &num;3 может быть предоставлен перед привязкой к экземпляру `ArrayExample` любым поставщиком конфигурации, который создает правильную пару "ключ — значение" в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-582">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="b71fd-583">Если в образец включен дополнительный поставщик конфигурации JSON с отсутствующей парой "ключ — значение", то `ArrayExample.Entries` подойдет полному массиву конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-583">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="b71fd-584">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="b71fd-584">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="b71fd-585">В <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="b71fd-585">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="b71fd-586">Пары "ключ — значение", показанные в таблице, загружаются в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-586">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="b71fd-587">Ключ</span><span class="sxs-lookup"><span data-stu-id="b71fd-587">Key</span></span>             | <span data-ttu-id="b71fd-588">Значение</span><span class="sxs-lookup"><span data-stu-id="b71fd-588">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="b71fd-589">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="b71fd-589">array:entries:3</span></span> | <span data-ttu-id="b71fd-590">value3</span><span class="sxs-lookup"><span data-stu-id="b71fd-590">value3</span></span> |

<span data-ttu-id="b71fd-591">Если экземпляр класса `ArrayExample` связан после того, как поставщик конфигурации JSON включает запись для индекса &num;3, то массив `ArrayExample.Entries` включит значение.</span><span class="sxs-lookup"><span data-stu-id="b71fd-591">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="b71fd-592">Индекс `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="b71fd-592">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="b71fd-593">Значение `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="b71fd-593">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="b71fd-594">0</span><span class="sxs-lookup"><span data-stu-id="b71fd-594">0</span></span>                            | <span data-ttu-id="b71fd-595">value0</span><span class="sxs-lookup"><span data-stu-id="b71fd-595">value0</span></span>                       |
| <span data-ttu-id="b71fd-596">1</span><span class="sxs-lookup"><span data-stu-id="b71fd-596">1</span></span>                            | <span data-ttu-id="b71fd-597">value1</span><span class="sxs-lookup"><span data-stu-id="b71fd-597">value1</span></span>                       |
| <span data-ttu-id="b71fd-598">2</span><span class="sxs-lookup"><span data-stu-id="b71fd-598">2</span></span>                            | <span data-ttu-id="b71fd-599">value2</span><span class="sxs-lookup"><span data-stu-id="b71fd-599">value2</span></span>                       |
| <span data-ttu-id="b71fd-600">3</span><span class="sxs-lookup"><span data-stu-id="b71fd-600">3</span></span>                            | <span data-ttu-id="b71fd-601">value3</span><span class="sxs-lookup"><span data-stu-id="b71fd-601">value3</span></span>                       |
| <span data-ttu-id="b71fd-602">4</span><span class="sxs-lookup"><span data-stu-id="b71fd-602">4</span></span>                            | <span data-ttu-id="b71fd-603">value4</span><span class="sxs-lookup"><span data-stu-id="b71fd-603">value4</span></span>                       |
| <span data-ttu-id="b71fd-604">5</span><span class="sxs-lookup"><span data-stu-id="b71fd-604">5</span></span>                            | <span data-ttu-id="b71fd-605">value5</span><span class="sxs-lookup"><span data-stu-id="b71fd-605">value5</span></span>                       |

<span data-ttu-id="b71fd-606">**Обработка массива JSON**</span><span class="sxs-lookup"><span data-stu-id="b71fd-606">**JSON array processing**</span></span>

<span data-ttu-id="b71fd-607">Если JSON-файл содержит массив, ключи конфигурации будут созданы для элементов массива с индексом раздела, начиная с нуля.</span><span class="sxs-lookup"><span data-stu-id="b71fd-607">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="b71fd-608">В следующем файле конфигурации `subsection` — это массив.</span><span class="sxs-lookup"><span data-stu-id="b71fd-608">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="b71fd-609">Поставщик конфигурации JSON считывает данные конфигурации в следующие пары "ключ — значение".</span><span class="sxs-lookup"><span data-stu-id="b71fd-609">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="b71fd-610">Ключ</span><span class="sxs-lookup"><span data-stu-id="b71fd-610">Key</span></span>                     | <span data-ttu-id="b71fd-611">Значение</span><span class="sxs-lookup"><span data-stu-id="b71fd-611">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="b71fd-612">json_array:key</span><span class="sxs-lookup"><span data-stu-id="b71fd-612">json_array:key</span></span>          | <span data-ttu-id="b71fd-613">valueA</span><span class="sxs-lookup"><span data-stu-id="b71fd-613">valueA</span></span> |
| <span data-ttu-id="b71fd-614">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="b71fd-614">json_array:subsection:0</span></span> | <span data-ttu-id="b71fd-615">valueB</span><span class="sxs-lookup"><span data-stu-id="b71fd-615">valueB</span></span> |
| <span data-ttu-id="b71fd-616">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="b71fd-616">json_array:subsection:1</span></span> | <span data-ttu-id="b71fd-617">valueC</span><span class="sxs-lookup"><span data-stu-id="b71fd-617">valueC</span></span> |
| <span data-ttu-id="b71fd-618">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="b71fd-618">json_array:subsection:2</span></span> | <span data-ttu-id="b71fd-619">valueD</span><span class="sxs-lookup"><span data-stu-id="b71fd-619">valueD</span></span> |

<span data-ttu-id="b71fd-620">В примере приложения для привязки пар "ключ — значение" конфигурации доступен следующий класс POCO.</span><span class="sxs-lookup"><span data-stu-id="b71fd-620">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="b71fd-621">После выполнения привязки `JsonArrayExample.Key` содержит значение `valueA`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-621">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="b71fd-622">Подраздел значения хранится в свойстве массива POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-622">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="b71fd-623">Индекс `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="b71fd-623">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="b71fd-624">Значение `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="b71fd-624">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="b71fd-625">0</span><span class="sxs-lookup"><span data-stu-id="b71fd-625">0</span></span>                                   | <span data-ttu-id="b71fd-626">valueB</span><span class="sxs-lookup"><span data-stu-id="b71fd-626">valueB</span></span>                              |
| <span data-ttu-id="b71fd-627">1</span><span class="sxs-lookup"><span data-stu-id="b71fd-627">1</span></span>                                   | <span data-ttu-id="b71fd-628">valueC</span><span class="sxs-lookup"><span data-stu-id="b71fd-628">valueC</span></span>                              |
| <span data-ttu-id="b71fd-629">2</span><span class="sxs-lookup"><span data-stu-id="b71fd-629">2</span></span>                                   | <span data-ttu-id="b71fd-630">valueD</span><span class="sxs-lookup"><span data-stu-id="b71fd-630">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="b71fd-631">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="b71fd-631">Custom configuration provider</span></span>

<span data-ttu-id="b71fd-632">Пример приложения демонстрирует, как создать базовый поставщик конфигурации, который считывает пары "ключ — значение" конфигурации из базы данных, используя [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="b71fd-632">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="b71fd-633">Поставщик имеет следующие характеристики.</span><span class="sxs-lookup"><span data-stu-id="b71fd-633">The provider has the following characteristics:</span></span>

* <span data-ttu-id="b71fd-634">База данных в памяти EF используется для демонстрационных целей.</span><span class="sxs-lookup"><span data-stu-id="b71fd-634">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="b71fd-635">Чтобы использовать базу данных, для которой требуется строка подключения, выполните вторичный `ConfigurationBuilder`, чтобы предоставить строку подключения от другого поставщика конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b71fd-635">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="b71fd-636">Поставщик считывает таблицу базы данных в конфигурации при запуске.</span><span class="sxs-lookup"><span data-stu-id="b71fd-636">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="b71fd-637">Поставщик не запрашивает базу данных для каждого ключа.</span><span class="sxs-lookup"><span data-stu-id="b71fd-637">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="b71fd-638">Функция перезагрузки на изменение не реализована, поэтому обновление базы данных после запуска приложения не влияет на конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="b71fd-638">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="b71fd-639">Определите сущность `EFConfigurationValue` для хранения значений конфигурации в базе данных.</span><span class="sxs-lookup"><span data-stu-id="b71fd-639">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="b71fd-640">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="b71fd-640">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="b71fd-641">Добавьте `EFConfigurationContext` в хранилище и обратитесь к настроенным значениям.</span><span class="sxs-lookup"><span data-stu-id="b71fd-641">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="b71fd-642">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="b71fd-642">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="b71fd-643">Создайте класс, реализующий <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-643">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="b71fd-644">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="b71fd-644">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="b71fd-645">Создайте пользовательский поставщик конфигурации путем наследования от <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-645">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="b71fd-646">Поставщик конфигурации инициализирует пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="b71fd-646">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="b71fd-647">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="b71fd-647">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="b71fd-648">Метод расширения `AddEFConfiguration` позволяет добавить источник конфигурации к `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-648">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="b71fd-649">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="b71fd-649">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="b71fd-650">В следующем коде показано, как использовать пользовательский `EFConfigurationProvider` в *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="b71fd-650">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="b71fd-651">Доступ к конфигурации во время запуска</span><span class="sxs-lookup"><span data-stu-id="b71fd-651">Access configuration during startup</span></span>

<span data-ttu-id="b71fd-652">Внесите значение `IConfiguration` в конструктор `Startup` для доступа к значениям конфигурации в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-652">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b71fd-653">Чтобы получить доступ к конфигурации в `Startup.Configure`, либо добавьте значение `IConfiguration` непосредственно в метод, либо используйте экземпляр из конструктора.</span><span class="sxs-lookup"><span data-stu-id="b71fd-653">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="b71fd-654">Дополнительные сведения см. в руководстве по [доступу к конфигурации с использованием удобных методов](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="b71fd-654">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="b71fd-655">Настройте доступ на странице Razor Pages или в представлении MVC</span><span class="sxs-lookup"><span data-stu-id="b71fd-655">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="b71fd-656">Для доступа к параметрам конфигурации на странице Razor Pages или в представлении MVC добавьте [использование директивы](xref:mvc/views/razor#using) ([Справочник по C#: использование директивы](/dotnet/csharp/language-reference/keywords/using-directive)) для [пространства имен Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) и вставьте <xref:Microsoft.Extensions.Configuration.IConfiguration> на страницу или в представление.</span><span class="sxs-lookup"><span data-stu-id="b71fd-656">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="b71fd-657">На странице Razor Pages</span><span class="sxs-lookup"><span data-stu-id="b71fd-657">In a Razor Pages page:</span></span>

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

<span data-ttu-id="b71fd-658">В представлении MVC</span><span class="sxs-lookup"><span data-stu-id="b71fd-658">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="b71fd-659">Добавление конфигурации из внешней сборки</span><span class="sxs-lookup"><span data-stu-id="b71fd-659">Add configuration from an external assembly</span></span>

<span data-ttu-id="b71fd-660">Реализация <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> позволяет при запуске добавлять в приложение улучшения из внешней сборки вне приложения класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="b71fd-660">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="b71fd-661">Для получения дополнительной информации см. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="b71fd-661">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b71fd-662">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="b71fd-662">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* <span data-ttu-id="b71fd-663">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/) (Подробные сведения о конфигурации Microsoft)</span><span class="sxs-lookup"><span data-stu-id="b71fd-663">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)</span></span>
