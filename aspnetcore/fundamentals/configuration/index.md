---
title: Конфигурация в .NET Core
author: guardrex
description: Узнайте, как использовать API конфигурации для настройки приложения ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: a0c57e75b28bc7c5590d20a8fa59b00b6bb9af4e
ms.sourcegitcommit: 25150f4398de83132965a89f12d3a030f6cce48d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/25/2018
ms.locfileid: "42927882"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="8aeeb-103">Конфигурация в .NET Core</span><span class="sxs-lookup"><span data-stu-id="8aeeb-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="8aeeb-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="8aeeb-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8aeeb-105">Конфигурация приложения в ASP.NET Core основана на парах "ключ — значение", установленных *поставщиками конфигурации*.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="8aeeb-106">Поставщики конфигурации получают данные конфигурации в парах "ключ — значение" из различных источников:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="8aeeb-107">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="8aeeb-107">Azure Key Vault</span></span>
* <span data-ttu-id="8aeeb-108">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="8aeeb-108">Command-line arguments</span></span>
* <span data-ttu-id="8aeeb-109">а также пользовательские поставщики (устанавливаемые или создаваемые).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="8aeeb-110">справочных файлов;</span><span class="sxs-lookup"><span data-stu-id="8aeeb-110">Directory files</span></span>
* <span data-ttu-id="8aeeb-111">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="8aeeb-111">Environment variables</span></span>
* <span data-ttu-id="8aeeb-112">объектов .NET в памяти;</span><span class="sxs-lookup"><span data-stu-id="8aeeb-112">In-memory .NET objects</span></span>
* <span data-ttu-id="8aeeb-113">файлов параметров;</span><span class="sxs-lookup"><span data-stu-id="8aeeb-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="8aeeb-114">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="8aeeb-114">Azure Key Vault</span></span>
* <span data-ttu-id="8aeeb-115">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="8aeeb-115">Command-line arguments</span></span>
* <span data-ttu-id="8aeeb-116">а также пользовательские поставщики (устанавливаемые или создаваемые).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="8aeeb-117">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="8aeeb-117">Environment variables</span></span>
* <span data-ttu-id="8aeeb-118">объектов .NET в памяти;</span><span class="sxs-lookup"><span data-stu-id="8aeeb-118">In-memory .NET objects</span></span>
* <span data-ttu-id="8aeeb-119">файлов параметров;</span><span class="sxs-lookup"><span data-stu-id="8aeeb-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="8aeeb-120">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="8aeeb-120">Command-line arguments</span></span>
* <span data-ttu-id="8aeeb-121">а также пользовательские поставщики (устанавливаемые или создаваемые).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="8aeeb-122">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="8aeeb-122">Environment variables</span></span>
* <span data-ttu-id="8aeeb-123">объектов .NET в памяти;</span><span class="sxs-lookup"><span data-stu-id="8aeeb-123">In-memory .NET objects</span></span>
* <span data-ttu-id="8aeeb-124">файлов параметров;</span><span class="sxs-lookup"><span data-stu-id="8aeeb-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="8aeeb-125">*Шаблон параметров* является расширением конфигурации основных понятий, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="8aeeb-126">Параметры используют классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="8aeeb-127">Дополнительные сведения об использовании шаблона параметров см. в разделе <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="8aeeb-128">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8aeeb-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8aeeb-129">Примеры, приведенные в этом разделе, основаны на следующем.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-129">The examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="8aeeb-130">Указание базового пути приложения с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-130">Setting the base path of the app with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="8aeeb-131">`SetBasePath` доступно приложению при использовании ссылки на пакет [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-131">`SetBasePath` is made available to an app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="8aeeb-132">Разрешение разделов файлов конфигурации с помощью <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-132">Resolving sections of configuration files with <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span></span> <span data-ttu-id="8aeeb-133">`GetSection` доступно приложению при использовании ссылки на пакет [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-133">`GetSection` is made available to an app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="8aeeb-134">Связывание конфигурации с .NET-классами с <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> и [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-134">Binding configuration to .NET classes with <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> and [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span></span> <span data-ttu-id="8aeeb-135">`Bind` и `Get<T>` доступны для приложения при использовании ссылки на пакет [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-135">`Bind` and `Get<T>` are made available to an app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span> <span data-ttu-id="8aeeb-136">`Get<T>` доступно в ASP.NET Core 1.1 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-136">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8aeeb-137">Эти три пакета включены в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-137">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8aeeb-138">Эти три пакета включены в [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-138">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="8aeeb-139">Конфигурация узла и приложения</span><span class="sxs-lookup"><span data-stu-id="8aeeb-139">Host vs. app configuration</span></span>

<span data-ttu-id="8aeeb-140">Перед настройкой и запуском приложения настройте и запустите *узел*.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-140">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="8aeeb-141">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-141">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="8aeeb-142">Как приложение, так и узел настраиваются с использованием поставщиков конфигурации, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-142">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="8aeeb-143">Пары "ключ — значение" конфигурации узлов становятся частью глобальной конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-143">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="8aeeb-144">Дополнительные сведения о том, как используются поставщики конфигурации при создании узла и как влияют источники конфигурации на узел, см. в разделе <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-144">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="security"></a><span data-ttu-id="8aeeb-145">Безопасность</span><span class="sxs-lookup"><span data-stu-id="8aeeb-145">Security</span></span>

<span data-ttu-id="8aeeb-146">Придерживайтесь следующих рекомендаций.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-146">Adopt the following best practices:</span></span>

* <span data-ttu-id="8aeeb-147">Никогда не храните пароли или другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации обычного текста.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-147">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="8aeeb-148">Не используйте секреты рабочей среды в средах разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-148">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="8aeeb-149">Указывайте секреты вне проекта, чтобы их нельзя было случайно зафиксировать в репозитории с исходным кодом.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-149">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="8aeeb-150">Дополнительные сведения см. в статье [Использование нескольких сред в ASP.NET Core](xref:fundamentals/environments) и руководствуйтесь статьей [Безопасное хранение секретов приложения во время разработки в ASP.NET Core](xref:security/app-secrets) (включает рекомендации по использованию переменной среды для хранения конфиденциальных данных).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-150">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="8aeeb-151">Менеджер секретов использует поставщик конфигурации файла для хранения конфиденциальных данных пользователя в файле JSON в локальной системе.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-151">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="8aeeb-152">Поставщик конфигурации файлов описан ниже в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-152">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="8aeeb-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) — один из вариантов для безопасного хранения секретов приложения.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="8aeeb-154">Дополнительные сведения см. в разделе <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-154">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="8aeeb-155">Иерархическая модель конфигурации</span><span class="sxs-lookup"><span data-stu-id="8aeeb-155">Hierarchical configuration data</span></span>

<span data-ttu-id="8aeeb-156">API конфигурации способен поддерживать иерархические данные конфигурации, выполняя преобразование в плоскую структуру иерархических данных с использованием разделителя в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-156">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="8aeeb-157">В следующем файле JSON существуют четыре ключа в структурированной иерархии двух разделов.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-157">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="8aeeb-158">При считывании файла в конфигурацию для сохранения исходной иерархической структуры данных источника конфигурации создаются уникальные ключи.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-158">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="8aeeb-159">Разделы и ключи преобразовываются в плоскую структуру с использованием двоеточия (`:`) для сохранения исходной структуры.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-159">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="8aeeb-160">section0:key0</span><span class="sxs-lookup"><span data-stu-id="8aeeb-160">section0:key0</span></span>
* <span data-ttu-id="8aeeb-161">section0:key1</span><span class="sxs-lookup"><span data-stu-id="8aeeb-161">section0:key1</span></span>
* <span data-ttu-id="8aeeb-162">section1:key0</span><span class="sxs-lookup"><span data-stu-id="8aeeb-162">section1:key0</span></span>
* <span data-ttu-id="8aeeb-163">section1:key1</span><span class="sxs-lookup"><span data-stu-id="8aeeb-163">section1:key1</span></span>

<span data-ttu-id="8aeeb-164">Методы <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> и <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> доступны для изолирования разделов и дочерних элементов раздела в данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="8aeeb-165">Эти методы описаны далее в разделе [GetSection, GetChildren и Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-165">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="8aeeb-166">Соглашения</span><span class="sxs-lookup"><span data-stu-id="8aeeb-166">Conventions</span></span>

<span data-ttu-id="8aeeb-167">При запуске приложения источники конфигурации считываются в порядке, в котором были указаны их поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-167">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="8aeeb-168">Поставщики файлов конфигурации имеют возможность перезагрузить конфигурацию при изменении базового файла параметров после запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-168">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="8aeeb-169">Поставщик конфигурации файлов описан ниже в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-169">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="8aeeb-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> доступен в контейнере [Dependency Injection (DI)](xref:fundamentals/dependency-injection) приложения.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="8aeeb-171">Поставщики конфигурации не могут использовать контейнер DI, так как он недоступен при настройке узла.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="8aeeb-172">В ключах конфигурации приняты следующие соглашения.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="8aeeb-173">В ключах не учитывается регистр символов.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-173">Keys are case-insensitive.</span></span> <span data-ttu-id="8aeeb-174">Например `ConnectionString` и `connectionstring` обрабатываются как эквивалентные ключи.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="8aeeb-175">Если значение для одного и того же ключа установлено одним и тем же или разными поставщиками конфигурации, последним значением, установленным на ключе, является используемое значение.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="8aeeb-176">Иерархические ключи</span><span class="sxs-lookup"><span data-stu-id="8aeeb-176">Hierarchical keys</span></span>
  * <span data-ttu-id="8aeeb-177">При взаимодействии с API конфигурации разделитель-двоеточие (`:`) поддерживается на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="8aeeb-178">В переменных среды разделитель-двоеточие может не работать на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="8aeeb-179">Двойной знак подчеркивания (`__`) поддерживается на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="8aeeb-180">В хранилище ключей Azure иерархические ключи используют `--` (два дефиса) в качестве разделителя.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="8aeeb-181">Чтобы заменить дефисы двоеточием, при загрузке секретов в конфигурацию приложения необходимо указать код.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="8aeeb-182"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="8aeeb-183">Привязка массива описана в разделе [Привязка массива к классу](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="8aeeb-184">В значениях конфигурации учитываются следующие соглашения.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="8aeeb-185">Значения являются строками.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-185">Values are strings.</span></span>
* <span data-ttu-id="8aeeb-186">Значение NULL не может храниться в конфигурации или быть привязанным к объектам.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="8aeeb-187">Поставщики</span><span class="sxs-lookup"><span data-stu-id="8aeeb-187">Providers</span></span>

<span data-ttu-id="8aeeb-188">В следующей таблице показаны поставщики конфигурации, доступные для приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="8aeeb-189">Поставщик</span><span class="sxs-lookup"><span data-stu-id="8aeeb-189">Provider</span></span> | <span data-ttu-id="8aeeb-190">Предоставляет конфигурацию из &hellip;</span><span class="sxs-lookup"><span data-stu-id="8aeeb-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="8aeeb-191">[Поставщик конфигурации хранилища ключей Azure](xref:security/key-vault-configuration) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="8aeeb-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="8aeeb-192">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="8aeeb-192">Azure Key Vault</span></span> |
| [<span data-ttu-id="8aeeb-193">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="8aeeb-193">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="8aeeb-194">Параметры командной строки</span><span class="sxs-lookup"><span data-stu-id="8aeeb-194">Command-line parameters</span></span> |
| [<span data-ttu-id="8aeeb-195">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="8aeeb-195">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="8aeeb-196">Источник пользователя</span><span class="sxs-lookup"><span data-stu-id="8aeeb-196">Custom source</span></span> |
| [<span data-ttu-id="8aeeb-197">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="8aeeb-197">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="8aeeb-198">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="8aeeb-198">Environment variables</span></span> |
| [<span data-ttu-id="8aeeb-199">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="8aeeb-199">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="8aeeb-200">Файлы (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="8aeeb-200">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="8aeeb-201">Поставщик конфигурации ключа для каждого файла</span><span class="sxs-lookup"><span data-stu-id="8aeeb-201">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="8aeeb-202">Справочные файлы</span><span class="sxs-lookup"><span data-stu-id="8aeeb-202">Directory files</span></span> |
| [<span data-ttu-id="8aeeb-203">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="8aeeb-203">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="8aeeb-204">Коллекции оперативной памяти</span><span class="sxs-lookup"><span data-stu-id="8aeeb-204">In-memory collections</span></span> |
| <span data-ttu-id="8aeeb-205">[Секреты пользователей (Менеджер секретов)](xref:security/app-secrets) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="8aeeb-205">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="8aeeb-206">Файл в каталоге профиля пользователя</span><span class="sxs-lookup"><span data-stu-id="8aeeb-206">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="8aeeb-207">Поставщик</span><span class="sxs-lookup"><span data-stu-id="8aeeb-207">Provider</span></span> | <span data-ttu-id="8aeeb-208">Предоставляет конфигурацию из &hellip;</span><span class="sxs-lookup"><span data-stu-id="8aeeb-208">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="8aeeb-209">[Поставщик конфигурации хранилища ключей Azure](xref:security/key-vault-configuration) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="8aeeb-209">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="8aeeb-210">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="8aeeb-210">Azure Key Vault</span></span> |
| [<span data-ttu-id="8aeeb-211">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="8aeeb-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="8aeeb-212">Параметры командной строки</span><span class="sxs-lookup"><span data-stu-id="8aeeb-212">Command-line parameters</span></span> |
| [<span data-ttu-id="8aeeb-213">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="8aeeb-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="8aeeb-214">Источник пользователя</span><span class="sxs-lookup"><span data-stu-id="8aeeb-214">Custom source</span></span> |
| [<span data-ttu-id="8aeeb-215">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="8aeeb-215">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="8aeeb-216">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="8aeeb-216">Environment variables</span></span> |
| [<span data-ttu-id="8aeeb-217">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="8aeeb-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="8aeeb-218">Файлы (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="8aeeb-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="8aeeb-219">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="8aeeb-219">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="8aeeb-220">Коллекции оперативной памяти</span><span class="sxs-lookup"><span data-stu-id="8aeeb-220">In-memory collections</span></span> |
| <span data-ttu-id="8aeeb-221">[Секреты пользователей (Менеджер секретов)](xref:security/app-secrets) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="8aeeb-221">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="8aeeb-222">Файл в каталоге профиля пользователя</span><span class="sxs-lookup"><span data-stu-id="8aeeb-222">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="8aeeb-223">Поставщик</span><span class="sxs-lookup"><span data-stu-id="8aeeb-223">Provider</span></span> | <span data-ttu-id="8aeeb-224">Предоставляет конфигурацию из &hellip;</span><span class="sxs-lookup"><span data-stu-id="8aeeb-224">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="8aeeb-225">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="8aeeb-225">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="8aeeb-226">Параметры командной строки</span><span class="sxs-lookup"><span data-stu-id="8aeeb-226">Command-line parameters</span></span> |
| [<span data-ttu-id="8aeeb-227">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="8aeeb-227">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="8aeeb-228">Источник пользователя</span><span class="sxs-lookup"><span data-stu-id="8aeeb-228">Custom source</span></span> |
| [<span data-ttu-id="8aeeb-229">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="8aeeb-229">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="8aeeb-230">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="8aeeb-230">Environment variables</span></span> |
| [<span data-ttu-id="8aeeb-231">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="8aeeb-231">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="8aeeb-232">Файлы (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="8aeeb-232">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="8aeeb-233">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="8aeeb-233">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="8aeeb-234">Коллекции оперативной памяти</span><span class="sxs-lookup"><span data-stu-id="8aeeb-234">In-memory collections</span></span> |
| <span data-ttu-id="8aeeb-235">[Секреты пользователей (Менеджер секретов)](xref:security/app-secrets) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="8aeeb-235">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="8aeeb-236">Файл в каталоге профиля пользователя</span><span class="sxs-lookup"><span data-stu-id="8aeeb-236">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="8aeeb-237">Источники конфигурации считываются в том порядке, в котором при запуске указываются их поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-237">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="8aeeb-238">Поставщики конфигурации в этом разделе описаны в алфавитном порядке, а не в необходимом вам порядке в коде.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-238">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="8aeeb-239">Порядок поставщиков конфигурации в коде соответствует приоритетам ваших основных источников конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-239">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="8aeeb-240">Типичная последовательность поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-240">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="8aeeb-241">Файлы (*appsettings.json*, *appsettings.&lt;Environment&gt;.json*, где `<Environment>` — это текущая среда размещения приложения)</span><span class="sxs-lookup"><span data-stu-id="8aeeb-241">Files (*appsettings.json*, *appsettings.&lt;Environment&gt;.json*, where `<Environment>` is the app's current hosting environment)</span></span>
1. <span data-ttu-id="8aeeb-242">[Секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки)</span><span class="sxs-lookup"><span data-stu-id="8aeeb-242">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="8aeeb-243">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="8aeeb-243">Environment variables</span></span>
1. <span data-ttu-id="8aeeb-244">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="8aeeb-244">Command-line arguments</span></span>

<span data-ttu-id="8aeeb-245">Общепринятой практикой является размещение поставщика конфигурации командной строки последним в ряду поставщиков, чтобы аргументы командной строки могли переопределять конфигурацию, установленную другими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-245">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8aeeb-246">Эта последовательность поставщиков помещается в месте, где происходит инициализация нового класса <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-246">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="8aeeb-247">Дополнительные сведения см. в статье [Веб-узел ASP.NET Core. Создание узла](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-247">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="8aeeb-248">Чтобы указать поставщиков конфигурации приложения, при создании веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-248">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the Web Host to specify the app's configuration providers:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

<span data-ttu-id="8aeeb-249">`ConfigureAppConfiguration` *доступен в ASP.NET Core 2.1 или в более поздних версиях.*</span><span class="sxs-lookup"><span data-stu-id="8aeeb-249">`ConfigureAppConfiguration` *is available in ASP.NET Core 2.1 or later.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8aeeb-250">Эта последовательность поставщиков может быть создана для приложения (а не узла) с помощью <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> и вызова метода <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> в `Startup`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-250">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

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

<span data-ttu-id="8aeeb-251">В приведенном выше примере имя среды (`env.EnvironmentName`) и имя сборки приложения (`env.ApplicationName`) предоставляются <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-251">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="8aeeb-252">Дополнительные сведения см. в разделе <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-252">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="8aeeb-253">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="8aeeb-253">Command-line Configuration Provider</span></span>

<span data-ttu-id="8aeeb-254"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> загружает конфигурацию из пары "ключ — значение" аргумента командной строки в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-254">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8aeeb-255">Чтобы активировать конфигурацию командной строки, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-255">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="8aeeb-256">`AddCommandLine` автоматически вызывается при инициализации нового <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-256">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="8aeeb-257">Дополнительные сведения см. в статье [Веб-узел ASP.NET Core. Создание узла](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-257">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="8aeeb-258">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-258">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="8aeeb-259">дополнительную конфигурацию из *appsettings.json* и *appsettings.&lt; Environment&gt;.json*;</span><span class="sxs-lookup"><span data-stu-id="8aeeb-259">Optional configuration from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>
* <span data-ttu-id="8aeeb-260">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки);</span><span class="sxs-lookup"><span data-stu-id="8aeeb-260">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="8aeeb-261">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-261">Environment variables.</span></span>

<span data-ttu-id="8aeeb-262">`CreateDefaultBuilder` добавляет последним поставщика конфигурации командной строки.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-262">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="8aeeb-263">Аргументы командной строки передаются в набор конфигурации переопределения среды выполнения, установленной другими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-263">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="8aeeb-264">`CreateDefaultBuilder` действует, когда создается узел.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-264">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="8aeeb-265">Поэтому конфигурация командной строки, активированная с помощью `CreateDefaultBuilder`, может повлиять на настройку узла.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-265">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="8aeeb-266">Для создания узла вручную без вызова `CreateDefaultBuilder` вызовите метод расширения `AddCommandLine` для экземпляра <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-266">When building the host manually and not calling `CreateDefaultBuilder`, call the `AddCommandLine` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8aeeb-267">Чтобы активировать конфигурацию командной строки, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-267">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="8aeeb-268">В этом случае аргументы командной строки, передаваемые во время выполнения, переопределяют конфигурацию, заданную другими вызванными ранее поставщиками конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-268">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="8aeeb-269">Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-269">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="8aeeb-270">**Пример**</span><span class="sxs-lookup"><span data-stu-id="8aeeb-270">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8aeeb-271">Пример приложения 2.x использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-271">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8aeeb-272">Пример приложения 1.x вызывает образец <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> в <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-272">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="8aeeb-273">Откройте командную строку в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-273">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="8aeeb-274">Поставьте аргумент командной строки в команду `dotnet run`: `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-274">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="8aeeb-275">После запуска приложения откройте браузер в приложении по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-275">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="8aeeb-276">Обратите внимание, что вывод содержит пару "ключ — значение" для аргумента командной строки конфигурации, предоставленного для `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-276">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="8aeeb-277">Аргументы</span><span class="sxs-lookup"><span data-stu-id="8aeeb-277">Arguments</span></span>

<span data-ttu-id="8aeeb-278">Значение должно соответствовать знаку равенства (`=`), или ключ должен иметь префикс (`--` или `/`), когда значение следует за пробелом.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-278">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="8aeeb-279">Значение может соответствовать NULL, если используется знак равенства (например, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-279">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="8aeeb-280">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="8aeeb-280">Key prefix</span></span>               | <span data-ttu-id="8aeeb-281">Пример</span><span class="sxs-lookup"><span data-stu-id="8aeeb-281">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="8aeeb-282">Без префикса</span><span class="sxs-lookup"><span data-stu-id="8aeeb-282">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="8aeeb-283">Два дефиса (`--`)</span><span class="sxs-lookup"><span data-stu-id="8aeeb-283">Two dashes (`--`)</span></span>        | <span data-ttu-id="8aeeb-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="8aeeb-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="8aeeb-285">Прямая косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="8aeeb-285">Forward slash (`/`)</span></span>      | <span data-ttu-id="8aeeb-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="8aeeb-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="8aeeb-287">В рамках одной и той же команды не смешивайте пары "ключ — значение" аргумента командной строки, которые используют знак равенства, с парами "ключ — значение", которые используют пробел.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-287">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="8aeeb-288">Примеры команд.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-288">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value --CommandLineKey2=value /CommandLineKey2=value
dotnet run --CommandLineKey1 value /CommandLineKey2 value
dotnet run CommandLineKey1= CommandLineKey2=value
```

### <a name="switch-mappings"></a><span data-ttu-id="8aeeb-289">Сопоставления переключений</span><span class="sxs-lookup"><span data-stu-id="8aeeb-289">Switch mappings</span></span>

<span data-ttu-id="8aeeb-290">Сопоставление параметров позволяет указать логику замены имен ключей.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-290">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="8aeeb-291">Когда вручную создается конфигурация с помощью <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, методу <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> можно предоставить словарь сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-291">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="8aeeb-292">В словаре сопоставлений переключений выполняется поиск ключа, который совпадает с ключом, предоставляемым аргументом командной строки.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-292">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="8aeeb-293">Если ключ в командной строке находится в словаре, значение словаря (замена ключа) передается обратно, чтобы установить пару "ключ — значение" в конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-293">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="8aeeb-294">Сопоставление переключений необходимо для любого ключа командной строки с префиксом из одного дефиса (`-`).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-294">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="8aeeb-295">Правила ключей из словаря сопоставления переключений:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-295">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="8aeeb-296">Переключения должны начинаться с дефиса (`-`) или двойного дефиса (`--`).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-296">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="8aeeb-297">Словарь сопоставлений переключений не должен содержать повторяющиеся ключи.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-297">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.0"

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

        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="8aeeb-298">Как показано в предыдущем примере, вызов `CreateDefaultBuilder` не должен передавать аргументы при использовании сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-298">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="8aeeb-299">Вызов команды `AddCommandLine` метода `CreateDefaultBuilder` не включает сопоставленные коммутаторы, и нет возможности передать словарь сопоставления переключений в `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-299">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="8aeeb-300">Если аргументы включают сопоставления переключений и передаются в `CreateDefaultBuilder`, его поставщик `AddCommandLine` не может инициализироваться с помощью <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-300">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="8aeeb-301">Решение заключается не в передаче аргументов команде `CreateDefaultBuilder`, а в том, чтобы вместо этого позволить методу `AddCommandLine` метода `ConfigurationBuilder` обрабатывать как аргументы, так и словари сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-301">The solution is not to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="8aeeb-302">Созданный словарь сопоставлений переключений содержит данные, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-302">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="8aeeb-303">Ключ</span><span class="sxs-lookup"><span data-stu-id="8aeeb-303">Key</span></span>       | <span data-ttu-id="8aeeb-304">Значение</span><span class="sxs-lookup"><span data-stu-id="8aeeb-304">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="8aeeb-305">Если ключи сопоставления переключений используются при запуске приложения, конфигурация принимает значение конфигурации в ключе, предоставленном в словаре.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-305">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="8aeeb-306">После выполнения предыдущей команды конфигурация содержит значения, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-306">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="8aeeb-307">Ключ</span><span class="sxs-lookup"><span data-stu-id="8aeeb-307">Key</span></span>               | <span data-ttu-id="8aeeb-308">Значение</span><span class="sxs-lookup"><span data-stu-id="8aeeb-308">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="8aeeb-309">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="8aeeb-309">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="8aeeb-310"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> загружает конфигурацию из пары "ключ — значение" переменной среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-310">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="8aeeb-311">Чтобы активировать конфигурацию переменных среды, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-311">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="8aeeb-312">В переменных средах разделитель-двоеточие (`:`) может не работать на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-312">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="8aeeb-313">Двойной знак подчеркивания (`__`) поддерживается на всех платформах и его можно заменить двоеточием.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-313">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="8aeeb-314">[Служба приложений Azure](https://azure.microsoft.com/services/app-service/) позволяет задать переменные среды на портале Azure, который может переопределить конфигурацию приложения, используя поставщик конфигурации переменных среды.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="8aeeb-315">Дополнительные сведения см. в разделе [Приложения Azure. Переопределение конфигурации приложения с помощью портала Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-315">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8aeeb-316">`AddEnvironmentVariables` автоматически вызывается при инициализации нового <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-316">`AddEnvironmentVariables` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="8aeeb-317">Дополнительные сведения см. в статье [Веб-узел ASP.NET Core. Создание узла](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-317">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="8aeeb-318">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-318">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="8aeeb-319">дополнительную конфигурацию из *appsettings.json* и *appsettings.&lt; Environment&gt;.json*;</span><span class="sxs-lookup"><span data-stu-id="8aeeb-319">Optional configuration from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>
* <span data-ttu-id="8aeeb-320">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки);</span><span class="sxs-lookup"><span data-stu-id="8aeeb-320">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="8aeeb-321">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-321">Command-line arguments.</span></span>

<span data-ttu-id="8aeeb-322">Поставщик конфигурации переменных среды вызывается после того, как настройка была создана из секретов пользователя и файлов *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-322">The Environment Variable Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="8aeeb-323">Вызов поставщика в этой позиции разрешает чтение переменных среды выполнения, чтобы переопределить конфигурацию, заданную секретом пользователя и файлом *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-323">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="8aeeb-324">Можно также напрямую вызвать метод расширения `AddEnvironmentVariables` в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-324">You can also directly call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8aeeb-325">Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-325">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="8aeeb-326">**Пример**</span><span class="sxs-lookup"><span data-stu-id="8aeeb-326">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8aeeb-327">Пример приложения 2.x использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-327">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8aeeb-328">Пример приложения 1.x вызывает образец `AddEnvironmentVariables` в `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-328">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="8aeeb-329">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-329">Run the sample app.</span></span> <span data-ttu-id="8aeeb-330">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-330">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="8aeeb-331">Обратите внимание, что выходные данные содержат пару "ключ — значение" для переменной среды `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-331">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="8aeeb-332">Значение отражает среду, в которой выполняется приложение, обычно при локальном запуске это `Development`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-332">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="8aeeb-333">Чтобы список переменных сред, отображаемый приложением, был коротким, приложение фильтрует переменные среды по следующим пунктам:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-333">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="8aeeb-334">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="8aeeb-334">ASPNETCORE_</span></span>
* <span data-ttu-id="8aeeb-335">urls</span><span class="sxs-lookup"><span data-stu-id="8aeeb-335">urls</span></span>
* <span data-ttu-id="8aeeb-336">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="8aeeb-336">Logging</span></span>
* <span data-ttu-id="8aeeb-337">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="8aeeb-337">ENVIRONMENT</span></span>
* <span data-ttu-id="8aeeb-338">contentRoot</span><span class="sxs-lookup"><span data-stu-id="8aeeb-338">contentRoot</span></span>
* <span data-ttu-id="8aeeb-339">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="8aeeb-339">AllowedHosts</span></span>
* <span data-ttu-id="8aeeb-340">applicationName</span><span class="sxs-lookup"><span data-stu-id="8aeeb-340">applicationName</span></span>
* <span data-ttu-id="8aeeb-341">CommandLine</span><span class="sxs-lookup"><span data-stu-id="8aeeb-341">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8aeeb-342">Если хотите просмотреть все переменные среды, доступные для приложения, измените значение `FilteredConfiguration` в *Pages/Index.cshtml.cs* на следующее:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-342">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8aeeb-343">Если хотите просмотреть все переменные среды, доступные для приложения, измените значение `FilteredConfiguration` в *Controllers/HomeController.cs* на следующее:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-343">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="8aeeb-344">Префиксы</span><span class="sxs-lookup"><span data-stu-id="8aeeb-344">Prefixes</span></span>

<span data-ttu-id="8aeeb-345">Переменные среды, которые загружаются в конфигурацию приложения, фильтруются при добавлении префикса к методу `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-345">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="8aeeb-346">Например, чтобы отфильтровать переменные среды по префиксу `CUSTOM_`, введите префикс поставщику конфигурации:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-346">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="8aeeb-347">Префикс отделяется при создании пары конфигурации "ключ — значение".</span><span class="sxs-lookup"><span data-stu-id="8aeeb-347">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8aeeb-348">Статический удобный метод `CreateDefaultBuilder` создает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> для установления размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-348">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="8aeeb-349">Когда значение `WebHostBuilder` будет создано, оно найдет конфигурацию узла в переменных среды, которые начинаются с префикса `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-349">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="8aeeb-350">**Префиксы строк подключения**</span><span class="sxs-lookup"><span data-stu-id="8aeeb-350">**Connection string prefixes**</span></span>

<span data-ttu-id="8aeeb-351">API конфигурации имеет специальные правила обработки для четырех строк подключения переменных среды, связанных с настройкой строк подключения Azure для среды приложения.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-351">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="8aeeb-352">Если префикс не указан в `AddEnvironmentVariables`, переменные среды с префиксами, указанными в таблице, загружаются в приложение.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-352">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="8aeeb-353">Префикс строки подключения</span><span class="sxs-lookup"><span data-stu-id="8aeeb-353">Connection string prefix</span></span> | <span data-ttu-id="8aeeb-354">Поставщик</span><span class="sxs-lookup"><span data-stu-id="8aeeb-354">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="8aeeb-355">Поставщик пользователя</span><span class="sxs-lookup"><span data-stu-id="8aeeb-355">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="8aeeb-356">MySQL</span><span class="sxs-lookup"><span data-stu-id="8aeeb-356">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="8aeeb-357">База данных SQL Azure</span><span class="sxs-lookup"><span data-stu-id="8aeeb-357">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="8aeeb-358">SQL Server</span><span class="sxs-lookup"><span data-stu-id="8aeeb-358">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="8aeeb-359">Когда переменная среды обнаруживается и загружается в конфигурацию с одним из четырех префиксов, приведенных в таблице, происходит следующее.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-359">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="8aeeb-360">Ключ конфигурации создается путем удаления префикса переменных среды и добавления ключа раздела конфигурации (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-360">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="8aeeb-361">Создается новая пара "ключ — значение" конфигурации, которая представляет поставщика подключения базы данных (за исключением `CUSTOMCONNSTR_`, который не имеет указанного поставщика).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-361">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="8aeeb-362">Ключ переменной среды</span><span class="sxs-lookup"><span data-stu-id="8aeeb-362">Environment variable key</span></span> | <span data-ttu-id="8aeeb-363">Преобразованный ключ конфигурации</span><span class="sxs-lookup"><span data-stu-id="8aeeb-363">Converted configuration key</span></span> | <span data-ttu-id="8aeeb-364">Запись конфигурации поставщика</span><span class="sxs-lookup"><span data-stu-id="8aeeb-364">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="8aeeb-365">Запись конфигурации не создана.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-365">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="8aeeb-366">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-366">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="8aeeb-367">Значение: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="8aeeb-367">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="8aeeb-368">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-368">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="8aeeb-369">Значение: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="8aeeb-369">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="8aeeb-370">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-370">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="8aeeb-371">Значение: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="8aeeb-371">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="8aeeb-372">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="8aeeb-372">File Configuration Provider</span></span>

<span data-ttu-id="8aeeb-373"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> является базовым классом для загрузки конфигурации из файловой системы.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-373"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="8aeeb-374">Следующие поставщики конфигурации предназначены для определенных типов файлов:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-374">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="8aeeb-375">Поставщик конфигурации INI</span><span class="sxs-lookup"><span data-stu-id="8aeeb-375">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="8aeeb-376">Поставщик конфигурации JSON</span><span class="sxs-lookup"><span data-stu-id="8aeeb-376">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="8aeeb-377">Поставщик конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="8aeeb-377">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="8aeeb-378">Поставщик конфигурации INI</span><span class="sxs-lookup"><span data-stu-id="8aeeb-378">INI Configuration Provider</span></span>

<span data-ttu-id="8aeeb-379"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> загружает конфигурацию из пары "ключ — значение" INI-файла во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-379">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="8aeeb-380">Чтобы активировать конфигурацию INI-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-380">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="8aeeb-381">Двоеточие можно использовать как разделитель раздела в конфигурации файла INI.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-381">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="8aeeb-382">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-382">Overloads permit specifying:</span></span>

* <span data-ttu-id="8aeeb-383">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-383">Whether the file is optional.</span></span>
* <span data-ttu-id="8aeeb-384">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-384">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="8aeeb-385"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-385">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8aeeb-386">Вызывая `CreateDefaultBuilder`, вызовите `UseConfiguration` со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-386">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="8aeeb-387">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите `UseConfiguration` со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-387">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8aeeb-388">Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-388">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="8aeeb-389">Общий пример конфигурации INI-файла.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-389">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="8aeeb-390">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-390">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="8aeeb-391">section0:key0</span><span class="sxs-lookup"><span data-stu-id="8aeeb-391">section0:key0</span></span>
* <span data-ttu-id="8aeeb-392">section0:key1</span><span class="sxs-lookup"><span data-stu-id="8aeeb-392">section0:key1</span></span>
* <span data-ttu-id="8aeeb-393">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="8aeeb-393">section1:subsection:key</span></span>
* <span data-ttu-id="8aeeb-394">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="8aeeb-394">section2:subsection0:key</span></span>
* <span data-ttu-id="8aeeb-395">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="8aeeb-395">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="8aeeb-396">Поставщик конфигурации JSON</span><span class="sxs-lookup"><span data-stu-id="8aeeb-396">JSON Configuration Provider</span></span>

<span data-ttu-id="8aeeb-397"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> загружает конфигурацию из пары "ключ — значение" JSON-файла в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-397">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="8aeeb-398">Чтобы активировать конфигурацию JSON-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-398">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="8aeeb-399">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-399">Overloads permit specifying:</span></span>

* <span data-ttu-id="8aeeb-400">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-400">Whether the file is optional.</span></span>
* <span data-ttu-id="8aeeb-401">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-401">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="8aeeb-402"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-402">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8aeeb-403">`AddJsonFile` автоматически вызывается дважды при инициализации нового <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-403">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="8aeeb-404">Метод вызывается для загрузки конфигурации из:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-404">The method is called to load configuration from:</span></span>

* <span data-ttu-id="8aeeb-405">*appsettings.json* &ndash; первым читается этот файл.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-405">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="8aeeb-406">Версия файла среды может переопределить значения, предоставленные *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-406">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="8aeeb-407">*appsettings.&lt;Environment&gt;.json* &ndash; версия среды файла загружается на основе [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-407">*appsettings.&lt;Environment&gt;.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="8aeeb-408">Дополнительные сведения см. в статье [Веб-узел ASP.NET Core. Создание узла](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-408">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="8aeeb-409">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-409">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="8aeeb-410">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-410">Environment variables.</span></span>
* <span data-ttu-id="8aeeb-411">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки);</span><span class="sxs-lookup"><span data-stu-id="8aeeb-411">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="8aeeb-412">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-412">Command-line arguments.</span></span>

<span data-ttu-id="8aeeb-413">Сначала устанавливается поставщик конфигурации JSON-файлов.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-413">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="8aeeb-414">Таким образом, секреты пользователя, переменные среды и аргументы командной строки переопределят конфигурацию, заданную файлами *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-414">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="8aeeb-415">Вы можете также напрямую вызвать метод расширения `AddJsonFile` в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-415">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="8aeeb-416">Вызывая `CreateDefaultBuilder`, вызовите `UseConfiguration` со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-416">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="8aeeb-417">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите `UseConfiguration` со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-417">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8aeeb-418">Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-418">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="8aeeb-419">**Пример**</span><span class="sxs-lookup"><span data-stu-id="8aeeb-419">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8aeeb-420">В примере приложения 2.x используется преимущество статически удобного метода `CreateDefaultBuilder` для создания узла, который включает два вызова в `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-420">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="8aeeb-421">Конфигурация загружена из *appsettings.json* и *appsettings.&lt; Environment&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-421">Configuration is loaded from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8aeeb-422">Пример приложения 1.x вызывает образец `AddJsonFile` дважды в `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-422">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="8aeeb-423">Конфигурация загружена из *appsettings.json* и *appsettings.&lt; Environment&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-423">Configuration is loaded from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="8aeeb-424">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-424">Run the sample app.</span></span> <span data-ttu-id="8aeeb-425">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-425">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="8aeeb-426">Обратите внимание, что выходные данные содержат пары "ключ — значение" для конфигурации, представленной в таблице в зависимости от среды.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-426">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="8aeeb-427">Ключи конфигурации ведения журнала используют двоеточие (`:`) как иерархический разделитель.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-427">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="8aeeb-428">Ключ</span><span class="sxs-lookup"><span data-stu-id="8aeeb-428">Key</span></span>                        | <span data-ttu-id="8aeeb-429">Значение разработки</span><span class="sxs-lookup"><span data-stu-id="8aeeb-429">Development Value</span></span> | <span data-ttu-id="8aeeb-430">Рабочее значение</span><span class="sxs-lookup"><span data-stu-id="8aeeb-430">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="8aeeb-431">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="8aeeb-431">Logging:LogLevel:System</span></span>    | <span data-ttu-id="8aeeb-432">Сведения</span><span class="sxs-lookup"><span data-stu-id="8aeeb-432">Information</span></span>       | <span data-ttu-id="8aeeb-433">Сведения</span><span class="sxs-lookup"><span data-stu-id="8aeeb-433">Information</span></span>      |
| <span data-ttu-id="8aeeb-434">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="8aeeb-434">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="8aeeb-435">Сведения</span><span class="sxs-lookup"><span data-stu-id="8aeeb-435">Information</span></span>       | <span data-ttu-id="8aeeb-436">Сведения</span><span class="sxs-lookup"><span data-stu-id="8aeeb-436">Information</span></span>      |
| <span data-ttu-id="8aeeb-437">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="8aeeb-437">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="8aeeb-438">Отладка</span><span class="sxs-lookup"><span data-stu-id="8aeeb-438">Debug</span></span>             | <span data-ttu-id="8aeeb-439">Error</span><span class="sxs-lookup"><span data-stu-id="8aeeb-439">Error</span></span>            |
| <span data-ttu-id="8aeeb-440">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="8aeeb-440">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="8aeeb-441">Поставщик конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="8aeeb-441">XML Configuration Provider</span></span>

<span data-ttu-id="8aeeb-442"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> загружает конфигурацию из пары "ключ — значение" XML-файла в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-442">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="8aeeb-443">Чтобы активировать конфигурацию XML-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-443">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="8aeeb-444">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-444">Overloads permit specifying:</span></span>

* <span data-ttu-id="8aeeb-445">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-445">Whether the file is optional.</span></span>
* <span data-ttu-id="8aeeb-446">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-446">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="8aeeb-447"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-447">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="8aeeb-448">Корневой узел файла конфигурации не учитывается при создании пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-448">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="8aeeb-449">Не указывайте в файле Document Type Definition (определение типа документа, DTD) или пространство имен.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-449">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8aeeb-450">Вызывая `CreateDefaultBuilder`, вызовите `UseConfiguration` со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-450">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="8aeeb-451">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите `UseConfiguration` со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-451">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8aeeb-452">Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-452">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="8aeeb-453">XML-файлы конфигурации могут использовать имена отдельных элементов для повторяющихся разделов.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-453">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="8aeeb-454">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-454">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="8aeeb-455">section0:key0</span><span class="sxs-lookup"><span data-stu-id="8aeeb-455">section0:key0</span></span>
* <span data-ttu-id="8aeeb-456">section0:key1</span><span class="sxs-lookup"><span data-stu-id="8aeeb-456">section0:key1</span></span>
* <span data-ttu-id="8aeeb-457">section1:key0</span><span class="sxs-lookup"><span data-stu-id="8aeeb-457">section1:key0</span></span>
* <span data-ttu-id="8aeeb-458">section1:key1</span><span class="sxs-lookup"><span data-stu-id="8aeeb-458">section1:key1</span></span>

<span data-ttu-id="8aeeb-459">Повторяющиеся элементы, использующие то же имя элемента, работают, если атрибут `name` используется для различения элементов.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-459">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="8aeeb-460">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-460">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="8aeeb-461">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="8aeeb-461">section:section0:key:key0</span></span>
* <span data-ttu-id="8aeeb-462">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="8aeeb-462">section:section0:key:key1</span></span>
* <span data-ttu-id="8aeeb-463">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="8aeeb-463">section:section1:key:key0</span></span>
* <span data-ttu-id="8aeeb-464">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="8aeeb-464">section:section1:key:key1</span></span>

<span data-ttu-id="8aeeb-465">Атрибуты можно использовать для предоставления значений.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-465">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="8aeeb-466">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-466">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="8aeeb-467">key:attribute</span><span class="sxs-lookup"><span data-stu-id="8aeeb-467">key:attribute</span></span>
* <span data-ttu-id="8aeeb-468">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="8aeeb-468">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="8aeeb-469">Поставщик конфигурации ключа для каждого файла</span><span class="sxs-lookup"><span data-stu-id="8aeeb-469">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="8aeeb-470"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> использует файлы каталога как пары "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-470">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="8aeeb-471">Ключ является именем файла.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-471">The key is the file name.</span></span> <span data-ttu-id="8aeeb-472">Значение содержит содержимое файла.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-472">The value contains the file's contents.</span></span> <span data-ttu-id="8aeeb-473">Поставщик конфигурации ключа для каждого файла используется в сценариях размещения Docker.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-473">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="8aeeb-474">Чтобы активировать конфигурацию ключа для каждого файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-474">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="8aeeb-475">Значение параметра `directoryPath` должно быть абсолютным путем к файлам.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-475">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="8aeeb-476">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-476">Overloads permit specifying:</span></span>

* <span data-ttu-id="8aeeb-477">`Action<KeyPerFileConfigurationSource>` — делегат, который настраивает источник.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-477">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="8aeeb-478">Обязательно ли указывать каталог и путь к каталогу.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-478">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="8aeeb-479">Вызывая `CreateDefaultBuilder`, вызовите `UseConfiguration` со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-479">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
        var config = new ConfigurationBuilder()
            .AddKeyPerFile(directoryPath: path, optional: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="8aeeb-480">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите `UseConfiguration` со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-480">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="8aeeb-481">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="8aeeb-481">Memory Configuration Provider</span></span>

<span data-ttu-id="8aeeb-482"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> использует коллекцию памяти в качестве пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-482">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="8aeeb-483">Чтобы активировать конфигурацию коллекции в памяти, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-483">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="8aeeb-484">Поставщик конфигурации может инициализироваться значением `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-484">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8aeeb-485">Вызывая `CreateDefaultBuilder`, вызовите `UseConfiguration` со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-485">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="8aeeb-486">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите `UseConfiguration` со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-486">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8aeeb-487">Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-487">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="8aeeb-488">GetValue</span><span class="sxs-lookup"><span data-stu-id="8aeeb-488">GetValue</span></span>

<span data-ttu-id="8aeeb-489">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) извлекает значение из конфигурации с указанным ключом и преобразует его в указанный тип.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-489">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="8aeeb-490">Если ключ не найден, перегрузка позволяет предоставлять значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-490">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="8aeeb-491">В следующем примере извлекается строковое значение из конфигурации с ключом `NumberKey`, вводится значение `int` и сохраняется значение в переменной `intValue`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-491">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="8aeeb-492">Если `NumberKey` не обнаруживается в ключах конфигурации, `intValue` получает значение `99` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-492">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="8aeeb-493">GetSection, GetChildren и Exists</span><span class="sxs-lookup"><span data-stu-id="8aeeb-493">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="8aeeb-494">В следующих примерах рассмотрим файл JSON.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-494">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="8aeeb-495">Четыре ключа находятся в двух разделах, один из которых содержит пару из подразделов.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-495">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="8aeeb-496">Когда файл считывается в конфигурацию, для сохранения значений конфигурации создаются следующие уникальные иерархические ключи.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-496">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="8aeeb-497">section0:key0</span><span class="sxs-lookup"><span data-stu-id="8aeeb-497">section0:key0</span></span>
* <span data-ttu-id="8aeeb-498">section0:key1</span><span class="sxs-lookup"><span data-stu-id="8aeeb-498">section0:key1</span></span>
* <span data-ttu-id="8aeeb-499">section1:key0</span><span class="sxs-lookup"><span data-stu-id="8aeeb-499">section1:key0</span></span>
* <span data-ttu-id="8aeeb-500">section1:key1</span><span class="sxs-lookup"><span data-stu-id="8aeeb-500">section1:key1</span></span>
* <span data-ttu-id="8aeeb-501">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="8aeeb-501">section2:subsection0:key0</span></span>
* <span data-ttu-id="8aeeb-502">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="8aeeb-502">section2:subsection0:key1</span></span>
* <span data-ttu-id="8aeeb-503">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="8aeeb-503">section2:subsection1:key0</span></span>
* <span data-ttu-id="8aeeb-504">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="8aeeb-504">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="8aeeb-505">GetSection</span><span class="sxs-lookup"><span data-stu-id="8aeeb-505">GetSection</span></span>

<span data-ttu-id="8aeeb-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) извлекает подраздел конфигурации с указанным ключом подраздела.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="8aeeb-507">Чтобы вернуть <xref:Microsoft.Extensions.Configuration.IConfigurationSection>, содержащий только пары "ключ — значение" в `section1`, вызовите `GetSection` и укажите имя раздела.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-507">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="8aeeb-508">Аналогично, чтобы получить значения для ключей в `section2:subsection0`, вызовите `GetSection` и укажите путь к разделу.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-508">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="8aeeb-509">Значение `GetSection` никогда не возвращает значение `null`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-509">`GetSection` never returns `null`.</span></span> <span data-ttu-id="8aeeb-510">Если соответствующий раздел не найден, возвращается пустой параметр `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-510">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

### <a name="getchildren"></a><span data-ttu-id="8aeeb-511">GetChildren</span><span class="sxs-lookup"><span data-stu-id="8aeeb-511">GetChildren</span></span>

<span data-ttu-id="8aeeb-512">Вызов [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) в `section2` получает значение `IEnumerable<IConfigurationSection>`, которое включает:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-512">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="8aeeb-513">Exists</span><span class="sxs-lookup"><span data-stu-id="8aeeb-513">Exists</span></span>

<span data-ttu-id="8aeeb-514">Используйте [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*), чтобы определить, существует ли раздел конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-514">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="8aeeb-515">Учитывая данные примера, `sectionExists` является `false`, потому что в данных конфигурации нет `section2:subsection2`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-515">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="8aeeb-516">Привязка к классу</span><span class="sxs-lookup"><span data-stu-id="8aeeb-516">Bind to a class</span></span>

<span data-ttu-id="8aeeb-517">Конфигурация может быть привязана к классам, которые представляют группы связанных параметров, используя шаблон *вариантов*.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-517">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="8aeeb-518">Дополнительные сведения см. в разделе <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-518">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="8aeeb-519">Значения конфигурации возвращаются как строки, но вызов <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> позволяет построить объекты [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-519">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="8aeeb-520">Пример приложения содержит модель `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="8aeeb-520">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="8aeeb-521">Раздел `starship` файла *starship.json* создает конфигурацию, когда образец приложения использует поставщик конфигурации JSON для загрузки конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-521">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="8aeeb-522">Создаются следующие пары "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-522">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="8aeeb-523">Ключ</span><span class="sxs-lookup"><span data-stu-id="8aeeb-523">Key</span></span>                   | <span data-ttu-id="8aeeb-524">Значение</span><span class="sxs-lookup"><span data-stu-id="8aeeb-524">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="8aeeb-525">starship:name</span><span class="sxs-lookup"><span data-stu-id="8aeeb-525">starship:name</span></span>         | <span data-ttu-id="8aeeb-526">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="8aeeb-526">USS Enterprise</span></span>                                    |
| <span data-ttu-id="8aeeb-527">starship:registry</span><span class="sxs-lookup"><span data-stu-id="8aeeb-527">starship:registry</span></span>     | <span data-ttu-id="8aeeb-528">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="8aeeb-528">NCC-1701</span></span>                                          |
| <span data-ttu-id="8aeeb-529">starship:class</span><span class="sxs-lookup"><span data-stu-id="8aeeb-529">starship:class</span></span>        | <span data-ttu-id="8aeeb-530">Constitution</span><span class="sxs-lookup"><span data-stu-id="8aeeb-530">Constitution</span></span>                                      |
| <span data-ttu-id="8aeeb-531">starship:length</span><span class="sxs-lookup"><span data-stu-id="8aeeb-531">starship:length</span></span>       | <span data-ttu-id="8aeeb-532">304.8</span><span class="sxs-lookup"><span data-stu-id="8aeeb-532">304.8</span></span>                                             |
| <span data-ttu-id="8aeeb-533">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="8aeeb-533">starship:commissioned</span></span> | <span data-ttu-id="8aeeb-534">False</span><span class="sxs-lookup"><span data-stu-id="8aeeb-534">False</span></span>                                             |
| <span data-ttu-id="8aeeb-535">trademark</span><span class="sxs-lookup"><span data-stu-id="8aeeb-535">trademark</span></span>             | <span data-ttu-id="8aeeb-536">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="8aeeb-536">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="8aeeb-537">Пример приложения вызывает `GetSection` с помощью ключа `starship`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-537">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="8aeeb-538">Пары "ключ — значение" `starship` изолированы.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-538">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="8aeeb-539">Метод `Bind` вызывается в подразделе при прохождении в экземпляр класса `Starship`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-539">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="8aeeb-540">После привязки значения экземпляра экземпляру присваивается свойство для преобразования для просмотра.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-540">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="8aeeb-541">Привязка к графу объектов</span><span class="sxs-lookup"><span data-stu-id="8aeeb-541">Bind to an object graph</span></span>

<span data-ttu-id="8aeeb-542"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> способна связывать весь граф объекта POCO.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-542"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="8aeeb-543">Образец содержит модель `TvShow`, в графе объектов которого находятся классы `Metadata` и `Actors` (*Модели/TvShow.cs*).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-543">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="8aeeb-544">В примере приложения есть файл *tvshow.xml*, содержащий данные конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-544">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="8aeeb-545">Конфигурация привязана ко всему ​​графу объектов `TvShow` с помощью метода `Bind`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-545">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="8aeeb-546">Привязанный экземпляр присваивается свойству для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-546">The bound instance is assigned to a property for rendering:</span></span>

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

<span data-ttu-id="8aeeb-547">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) привязывает и возвращает указанный тип.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-547">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="8aeeb-548">Метод `Get<T>` может быть более удобным, чем использование `Bind`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-548">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="8aeeb-549">В следующем коде показано, как использовать `Get<T>` с предыдущим примером, который позволяет привязать связанный экземпляр непосредственно к свойству, используемому для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-549">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="8aeeb-550">Привязка массива к классу</span><span class="sxs-lookup"><span data-stu-id="8aeeb-550">Bind an array to a class</span></span>

<span data-ttu-id="8aeeb-551">*Пример приложения демонстрирует концепции, описанные в этом разделе.*</span><span class="sxs-lookup"><span data-stu-id="8aeeb-551">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="8aeeb-552"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-552">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="8aeeb-553">Любой формат массива, который предоставляет сегмент числового ключа (`:0:`, `:1:`, &hellip; `:{n}:`), способен привязать массив к массиву класса POCO.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-553">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="8aeeb-554">Привязка предоставляется соглашением.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-554">Binding is provided by convention.</span></span> <span data-ttu-id="8aeeb-555">Пользовательские поставщики конфигурации не обязаны реализовывать привязку массива.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-555">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="8aeeb-556">**Обработка массива в оперативной памяти**</span><span class="sxs-lookup"><span data-stu-id="8aeeb-556">**In-memory array processing**</span></span>

<span data-ttu-id="8aeeb-557">Рассмотрите ключи и значения конфигурации, приведенные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-557">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="8aeeb-558">Ключ</span><span class="sxs-lookup"><span data-stu-id="8aeeb-558">Key</span></span>     | <span data-ttu-id="8aeeb-559">Значение</span><span class="sxs-lookup"><span data-stu-id="8aeeb-559">Value</span></span>  |
| :-----: | :----: |
| <span data-ttu-id="8aeeb-560">array:0</span><span class="sxs-lookup"><span data-stu-id="8aeeb-560">array:0</span></span> | <span data-ttu-id="8aeeb-561">value0</span><span class="sxs-lookup"><span data-stu-id="8aeeb-561">value0</span></span> |
| <span data-ttu-id="8aeeb-562">array:1</span><span class="sxs-lookup"><span data-stu-id="8aeeb-562">array:1</span></span> | <span data-ttu-id="8aeeb-563">value1</span><span class="sxs-lookup"><span data-stu-id="8aeeb-563">value1</span></span> |
| <span data-ttu-id="8aeeb-564">array:2</span><span class="sxs-lookup"><span data-stu-id="8aeeb-564">array:2</span></span> | <span data-ttu-id="8aeeb-565">value2</span><span class="sxs-lookup"><span data-stu-id="8aeeb-565">value2</span></span> |
| <span data-ttu-id="8aeeb-566">array:4</span><span class="sxs-lookup"><span data-stu-id="8aeeb-566">array:4</span></span> | <span data-ttu-id="8aeeb-567">value4</span><span class="sxs-lookup"><span data-stu-id="8aeeb-567">value4</span></span> |
| <span data-ttu-id="8aeeb-568">array:5</span><span class="sxs-lookup"><span data-stu-id="8aeeb-568">array:5</span></span> | <span data-ttu-id="8aeeb-569">value5</span><span class="sxs-lookup"><span data-stu-id="8aeeb-569">value5</span></span> |

<span data-ttu-id="8aeeb-570">Эти ключи и значения загружаются в пример приложения с помощью поставщика конфигурации памяти.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-570">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="8aeeb-571">Массив пропускает значения для индекса &num;3.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-571">The array skips a value for index &num;3.</span></span> <span data-ttu-id="8aeeb-572">Связующее свойство конфигурации не способно связывать нулевые значения или создавать нулевые записи в связанных объектах, что становится ясным в тот момент, когда демонстрируется результат привязки этого массива к объекту.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-572">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="8aeeb-573">В примере приложения класс POCO доступен для хранения привязанных данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-573">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="8aeeb-574">Данные конфигурации, связанные с объектом.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-574">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="8aeeb-575">Синтаксис [ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) также может использоваться для получения более компактного кода.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-575">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="8aeeb-576">Связанный объект, экземпляр `ArrayExample`, получает данные массива из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-576">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="8aeeb-577">Индекс `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="8aeeb-577">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="8aeeb-578">Значение `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="8aeeb-578">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="8aeeb-579">0</span><span class="sxs-lookup"><span data-stu-id="8aeeb-579">0</span></span>                             | <span data-ttu-id="8aeeb-580">value0</span><span class="sxs-lookup"><span data-stu-id="8aeeb-580">value0</span></span>                        |
| <span data-ttu-id="8aeeb-581">1</span><span class="sxs-lookup"><span data-stu-id="8aeeb-581">1</span></span>                             | <span data-ttu-id="8aeeb-582">value1</span><span class="sxs-lookup"><span data-stu-id="8aeeb-582">value1</span></span>                        |
| <span data-ttu-id="8aeeb-583">2</span><span class="sxs-lookup"><span data-stu-id="8aeeb-583">2</span></span>                             | <span data-ttu-id="8aeeb-584">value2</span><span class="sxs-lookup"><span data-stu-id="8aeeb-584">value2</span></span>                        |
| <span data-ttu-id="8aeeb-585">3</span><span class="sxs-lookup"><span data-stu-id="8aeeb-585">3</span></span>                             | <span data-ttu-id="8aeeb-586">value4</span><span class="sxs-lookup"><span data-stu-id="8aeeb-586">value4</span></span>                        |
| <span data-ttu-id="8aeeb-587">4</span><span class="sxs-lookup"><span data-stu-id="8aeeb-587">4</span></span>                             | <span data-ttu-id="8aeeb-588">value5</span><span class="sxs-lookup"><span data-stu-id="8aeeb-588">value5</span></span>                        |

<span data-ttu-id="8aeeb-589">Индекс &num;3 в связанном объекте содержит данные конфигурации для конфигурационного ключа `array:4` и его значения `value4`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-589">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="8aeeb-590">При привязке данных конфигурации, содержащих массив индексов, в ключах конфигурации эти индексы просто используются для выполнения итерации данных конфигурации при создании объекта.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-590">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="8aeeb-591">Когда массив ключей конфигурации пропускает один или несколько индексов, в данных конфигурации не может быть сохранено нулевое значение и в связанном объекте не создается нулевая запись.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-591">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="8aeeb-592">Отсутствующий элемент конфигурации для индекса &num;3 может быть предоставлен перед привязкой к экземпляру `ArrayExamples` любым поставщиком конфигурации, который создает правильную пару "ключ — значение" в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-592">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExamples` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="8aeeb-593">Если в образец включен дополнительный поставщик конфигурации JSON с отсутствующей парой "ключ — значение", то `ArrayExamples.Entries` подойдет полному массиву конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-593">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExamples.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="8aeeb-594">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-594">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8aeeb-595">В `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-595">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8aeeb-596">В конструкторе `Startup`:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-596">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="8aeeb-597">Пары "ключ — значение", показанные в таблице, загружаются в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-597">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="8aeeb-598">Ключ</span><span class="sxs-lookup"><span data-stu-id="8aeeb-598">Key</span></span>             | <span data-ttu-id="8aeeb-599">Значение</span><span class="sxs-lookup"><span data-stu-id="8aeeb-599">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="8aeeb-600">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="8aeeb-600">array:entries:3</span></span> | <span data-ttu-id="8aeeb-601">value3</span><span class="sxs-lookup"><span data-stu-id="8aeeb-601">value3</span></span> |

<span data-ttu-id="8aeeb-602">Если экземпляр класса `ArrayExamples` связан после того, как поставщик конфигурации JSON включает запись для индекса &num;3, то массив `ArrayExamples.Entries` включит значение.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-602">If the `ArrayExamples` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExamples.Entries` array includes the value.</span></span>

| <span data-ttu-id="8aeeb-603">Индекс `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="8aeeb-603">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="8aeeb-604">Значение `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="8aeeb-604">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="8aeeb-605">0</span><span class="sxs-lookup"><span data-stu-id="8aeeb-605">0</span></span>                             | <span data-ttu-id="8aeeb-606">value0</span><span class="sxs-lookup"><span data-stu-id="8aeeb-606">value0</span></span>                        |
| <span data-ttu-id="8aeeb-607">1</span><span class="sxs-lookup"><span data-stu-id="8aeeb-607">1</span></span>                             | <span data-ttu-id="8aeeb-608">value1</span><span class="sxs-lookup"><span data-stu-id="8aeeb-608">value1</span></span>                        |
| <span data-ttu-id="8aeeb-609">2</span><span class="sxs-lookup"><span data-stu-id="8aeeb-609">2</span></span>                             | <span data-ttu-id="8aeeb-610">value2</span><span class="sxs-lookup"><span data-stu-id="8aeeb-610">value2</span></span>                        |
| <span data-ttu-id="8aeeb-611">3</span><span class="sxs-lookup"><span data-stu-id="8aeeb-611">3</span></span>                             | <span data-ttu-id="8aeeb-612">value3</span><span class="sxs-lookup"><span data-stu-id="8aeeb-612">value3</span></span>                        |
| <span data-ttu-id="8aeeb-613">4</span><span class="sxs-lookup"><span data-stu-id="8aeeb-613">4</span></span>                             | <span data-ttu-id="8aeeb-614">value4</span><span class="sxs-lookup"><span data-stu-id="8aeeb-614">value4</span></span>                        |
| <span data-ttu-id="8aeeb-615">5</span><span class="sxs-lookup"><span data-stu-id="8aeeb-615">5</span></span>                             | <span data-ttu-id="8aeeb-616">value5</span><span class="sxs-lookup"><span data-stu-id="8aeeb-616">value5</span></span>                        |

<span data-ttu-id="8aeeb-617">**Обработка массива JSON**</span><span class="sxs-lookup"><span data-stu-id="8aeeb-617">**JSON array processing**</span></span>

<span data-ttu-id="8aeeb-618">Если JSON-файл содержит массив, ключи конфигурации будут созданы для элементов массива с индексом раздела, начиная с нуля.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-618">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="8aeeb-619">В следующем файле конфигурации `subsection` — это массив.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-619">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="8aeeb-620">Поставщик конфигурации JSON считывает данные конфигурации в следующие пары "ключ — значение".</span><span class="sxs-lookup"><span data-stu-id="8aeeb-620">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="8aeeb-621">Ключ</span><span class="sxs-lookup"><span data-stu-id="8aeeb-621">Key</span></span>                     | <span data-ttu-id="8aeeb-622">Значение</span><span class="sxs-lookup"><span data-stu-id="8aeeb-622">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="8aeeb-623">json_array:key</span><span class="sxs-lookup"><span data-stu-id="8aeeb-623">json_array:key</span></span>          | <span data-ttu-id="8aeeb-624">valueA</span><span class="sxs-lookup"><span data-stu-id="8aeeb-624">valueA</span></span> |
| <span data-ttu-id="8aeeb-625">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="8aeeb-625">json_array:subsection:0</span></span> | <span data-ttu-id="8aeeb-626">valueB</span><span class="sxs-lookup"><span data-stu-id="8aeeb-626">valueB</span></span> |
| <span data-ttu-id="8aeeb-627">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="8aeeb-627">json_array:subsection:1</span></span> | <span data-ttu-id="8aeeb-628">valueC</span><span class="sxs-lookup"><span data-stu-id="8aeeb-628">valueC</span></span> |
| <span data-ttu-id="8aeeb-629">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="8aeeb-629">json_array:subsection:2</span></span> | <span data-ttu-id="8aeeb-630">valueD</span><span class="sxs-lookup"><span data-stu-id="8aeeb-630">valueD</span></span> |

<span data-ttu-id="8aeeb-631">В примере приложения для привязки пар "ключ — значение" конфигурации доступен следующий класс POCO.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-631">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="8aeeb-632">После выполнения привязки `JsonArrayExample.Key` содержит значение `valueA`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-632">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="8aeeb-633">Подраздел значения хранится в свойстве массива POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-633">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="8aeeb-634">Индекс `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="8aeeb-634">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="8aeeb-635">Значение `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="8aeeb-635">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="8aeeb-636">0</span><span class="sxs-lookup"><span data-stu-id="8aeeb-636">0</span></span>                                   | <span data-ttu-id="8aeeb-637">valueB</span><span class="sxs-lookup"><span data-stu-id="8aeeb-637">valueB</span></span>                              |
| <span data-ttu-id="8aeeb-638">1</span><span class="sxs-lookup"><span data-stu-id="8aeeb-638">1</span></span>                                   | <span data-ttu-id="8aeeb-639">valueC</span><span class="sxs-lookup"><span data-stu-id="8aeeb-639">valueC</span></span>                              |
| <span data-ttu-id="8aeeb-640">2</span><span class="sxs-lookup"><span data-stu-id="8aeeb-640">2</span></span>                                   | <span data-ttu-id="8aeeb-641">valueD</span><span class="sxs-lookup"><span data-stu-id="8aeeb-641">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="8aeeb-642">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="8aeeb-642">Custom configuration provider</span></span>

<span data-ttu-id="8aeeb-643">Пример приложения демонстрирует, как создать базовый поставщик конфигурации, который считывает пары "ключ — значение" конфигурации из базы данных, используя [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-643">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="8aeeb-644">Поставщик имеет следующие характеристики.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-644">The provider has the following characteristics:</span></span>

* <span data-ttu-id="8aeeb-645">База данных в памяти EF используется для демонстрационных целей.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-645">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="8aeeb-646">Чтобы использовать базу данных, для которой требуется строка подключения, выполните вторичный `ConfigurationBuilder`, чтобы предоставить строку подключения от другого поставщика конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-646">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="8aeeb-647">Поставщик считывает таблицу базы данных в конфигурации при запуске.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-647">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="8aeeb-648">Поставщик не запрашивает базу данных для каждого ключа.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-648">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="8aeeb-649">Функция перезагрузки на изменение не реализована, поэтому обновление базы данных после запуска приложения не влияет на конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-649">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="8aeeb-650">Определите сущность `EFConfigurationValue` для хранения значений конфигурации в базе данных.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-650">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="8aeeb-651">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-651">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="8aeeb-652">Добавьте `EFConfigurationContext` в хранилище и обратитесь к настроенным значениям.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-652">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="8aeeb-653">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-653">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="8aeeb-654">Создайте класс, реализующий <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-654">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="8aeeb-655">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-655">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="8aeeb-656">Создайте пользовательский поставщик конфигурации путем наследования от <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-656">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="8aeeb-657">Поставщик конфигурации инициализирует пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-657">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="8aeeb-658">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-658">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="8aeeb-659">Метод расширения `AddEFConfiguration` позволяет добавить источник конфигурации к `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-659">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="8aeeb-660">*EFConfigurationProvider/EFConfigurationExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="8aeeb-660">*EFConfigurationProvider/EFConfigurationExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="8aeeb-661">В следующем коде показано, как использовать пользовательский `EFConfigurationProvider` в *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-661">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="8aeeb-662">Доступ к конфигурации во время запуска</span><span class="sxs-lookup"><span data-stu-id="8aeeb-662">Access configuration during startup</span></span>

<span data-ttu-id="8aeeb-663">Внесите значение `IConfiguration` в конструктор `Startup` для доступа к значениям конфигурации в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-663">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8aeeb-664">Чтобы получить доступ к конфигурации в `Startup.Configure`, либо добавьте значение `IConfiguration` непосредственно в метод, либо используйте экземпляр из конструктора.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-664">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="8aeeb-665">Пример доступа к конфигурации с использованием удобных методов запуска см. в разделе [Запуск приложения. Удобные методы](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="8aeeb-665">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="8aeeb-666">Настройте доступ на странице Razor Pages или в представлении MVC</span><span class="sxs-lookup"><span data-stu-id="8aeeb-666">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="8aeeb-667">Для доступа к параметрам конфигурации на странице Razor Pages или в представлении MVC добавьте [использование директивы](xref:mvc/views/razor#using) ([Справочник по C#: использование директивы](/dotnet/csharp/language-reference/keywords/using-directive)) для [пространства имен Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) и вставьте <xref:Microsoft.Extensions.Configuration.IConfiguration> на страницу или в представление.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-667">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="8aeeb-668">На странице Razor Pages</span><span class="sxs-lookup"><span data-stu-id="8aeeb-668">In a Razor Pages page:</span></span>

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

<span data-ttu-id="8aeeb-669">В представлении MVC</span><span class="sxs-lookup"><span data-stu-id="8aeeb-669">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="8aeeb-670">Добавление конфигурации из внешней сборки</span><span class="sxs-lookup"><span data-stu-id="8aeeb-670">Add configuration from an external assembly</span></span>

<span data-ttu-id="8aeeb-671">Реализация <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> позволяет при запуске добавлять в приложение улучшения из внешней сборки вне приложения класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-671">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="8aeeb-672">Дополнительные сведения см. в разделе <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="8aeeb-672">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8aeeb-673">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="8aeeb-673">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* <span data-ttu-id="8aeeb-674">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/) (Подробные сведения о конфигурации Microsoft)</span><span class="sxs-lookup"><span data-stu-id="8aeeb-674">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)</span></span>
