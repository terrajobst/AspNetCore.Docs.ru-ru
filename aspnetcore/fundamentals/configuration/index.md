---
title: Конфигурация в .NET Core
author: guardrex
description: Узнайте, как использовать API конфигурации для настройки приложения ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/09/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 35f283becd156da22a4d9d2034055ee79b75ffda
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49326177"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="f21b8-103">Конфигурация в .NET Core</span><span class="sxs-lookup"><span data-stu-id="f21b8-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="f21b8-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="f21b8-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f21b8-105">Конфигурация приложения в ASP.NET Core основана на парах "ключ — значение", установленных *поставщиками конфигурации*.</span><span class="sxs-lookup"><span data-stu-id="f21b8-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="f21b8-106">Поставщики конфигурации получают данные конфигурации в парах "ключ — значение" из различных источников:</span><span class="sxs-lookup"><span data-stu-id="f21b8-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="f21b8-107">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="f21b8-107">Azure Key Vault</span></span>
* <span data-ttu-id="f21b8-108">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="f21b8-108">Command-line arguments</span></span>
* <span data-ttu-id="f21b8-109">а также пользовательские поставщики (устанавливаемые или создаваемые).</span><span class="sxs-lookup"><span data-stu-id="f21b8-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="f21b8-110">Справочные файлы</span><span class="sxs-lookup"><span data-stu-id="f21b8-110">Directory files</span></span>
* <span data-ttu-id="f21b8-111">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="f21b8-111">Environment variables</span></span>
* <span data-ttu-id="f21b8-112">объектов .NET в памяти;</span><span class="sxs-lookup"><span data-stu-id="f21b8-112">In-memory .NET objects</span></span>
* <span data-ttu-id="f21b8-113">файлов параметров;</span><span class="sxs-lookup"><span data-stu-id="f21b8-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="f21b8-114">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="f21b8-114">Azure Key Vault</span></span>
* <span data-ttu-id="f21b8-115">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="f21b8-115">Command-line arguments</span></span>
* <span data-ttu-id="f21b8-116">а также пользовательские поставщики (устанавливаемые или создаваемые).</span><span class="sxs-lookup"><span data-stu-id="f21b8-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="f21b8-117">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="f21b8-117">Environment variables</span></span>
* <span data-ttu-id="f21b8-118">объектов .NET в памяти;</span><span class="sxs-lookup"><span data-stu-id="f21b8-118">In-memory .NET objects</span></span>
* <span data-ttu-id="f21b8-119">файлов параметров;</span><span class="sxs-lookup"><span data-stu-id="f21b8-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="f21b8-120">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="f21b8-120">Command-line arguments</span></span>
* <span data-ttu-id="f21b8-121">а также пользовательские поставщики (устанавливаемые или создаваемые).</span><span class="sxs-lookup"><span data-stu-id="f21b8-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="f21b8-122">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="f21b8-122">Environment variables</span></span>
* <span data-ttu-id="f21b8-123">объектов .NET в памяти;</span><span class="sxs-lookup"><span data-stu-id="f21b8-123">In-memory .NET objects</span></span>
* <span data-ttu-id="f21b8-124">файлов параметров;</span><span class="sxs-lookup"><span data-stu-id="f21b8-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="f21b8-125">*Шаблон параметров* является расширением конфигурации основных понятий, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="f21b8-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="f21b8-126">Параметры используют классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="f21b8-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="f21b8-127">Дополнительные сведения об использовании шаблона параметров см. в разделе <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="f21b8-128">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f21b8-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f21b8-129">Примеры, приведенные в этом разделе, основаны на следующем.</span><span class="sxs-lookup"><span data-stu-id="f21b8-129">The examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="f21b8-130">Указание базового пути приложения с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-130">Setting the base path of the app with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="f21b8-131">`SetBasePath` доступно приложению при использовании ссылки на пакет [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/).</span><span class="sxs-lookup"><span data-stu-id="f21b8-131">`SetBasePath` is made available to an app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="f21b8-132">Разрешение разделов файлов конфигурации с помощью <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-132">Resolving sections of configuration files with <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span></span> <span data-ttu-id="f21b8-133">`GetSection` доступно приложению при использовании ссылки на пакет [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/).</span><span class="sxs-lookup"><span data-stu-id="f21b8-133">`GetSection` is made available to an app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="f21b8-134">Связывание конфигурации с .NET-классами с <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> и [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span><span class="sxs-lookup"><span data-stu-id="f21b8-134">Binding configuration to .NET classes with <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> and [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span></span> <span data-ttu-id="f21b8-135">`Bind` и `Get<T>` доступны для приложения при использовании ссылки на пакет [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/).</span><span class="sxs-lookup"><span data-stu-id="f21b8-135">`Bind` and `Get<T>` are made available to an app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span> <span data-ttu-id="f21b8-136">`Get<T>` доступно в ASP.NET Core 1.1 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="f21b8-136">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f21b8-137">Эти три пакета включены в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="f21b8-137">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f21b8-138">Эти три пакета включены в [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="f21b8-138">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="f21b8-139">Конфигурация узла и приложения</span><span class="sxs-lookup"><span data-stu-id="f21b8-139">Host vs. app configuration</span></span>

<span data-ttu-id="f21b8-140">Перед настройкой и запуском приложения настройте и запустите *узел*.</span><span class="sxs-lookup"><span data-stu-id="f21b8-140">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="f21b8-141">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="f21b8-141">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="f21b8-142">Как приложение, так и узел настраиваются с использованием поставщиков конфигурации, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="f21b8-142">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="f21b8-143">Пары "ключ — значение" конфигурации узлов становятся частью глобальной конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="f21b8-143">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="f21b8-144">Дополнительные сведения о том, как используются поставщики конфигурации при создании узла и как влияют источники конфигурации на узел, см. в разделе <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-144">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="security"></a><span data-ttu-id="f21b8-145">Безопасность</span><span class="sxs-lookup"><span data-stu-id="f21b8-145">Security</span></span>

<span data-ttu-id="f21b8-146">Придерживайтесь следующих рекомендаций.</span><span class="sxs-lookup"><span data-stu-id="f21b8-146">Adopt the following best practices:</span></span>

* <span data-ttu-id="f21b8-147">Никогда не храните пароли или другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации обычного текста.</span><span class="sxs-lookup"><span data-stu-id="f21b8-147">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="f21b8-148">Не используйте секреты рабочей среды в средах разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="f21b8-148">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="f21b8-149">Указывайте секреты вне проекта, чтобы их нельзя было случайно зафиксировать в репозитории с исходным кодом.</span><span class="sxs-lookup"><span data-stu-id="f21b8-149">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="f21b8-150">Дополнительные сведения см. в статье [Использование нескольких сред в ASP.NET Core](xref:fundamentals/environments) и руководствуйтесь статьей [Безопасное хранение секретов приложения во время разработки в ASP.NET Core](xref:security/app-secrets) (включает рекомендации по использованию переменной среды для хранения конфиденциальных данных).</span><span class="sxs-lookup"><span data-stu-id="f21b8-150">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="f21b8-151">Менеджер секретов использует поставщик конфигурации файла для хранения конфиденциальных данных пользователя в файле JSON в локальной системе.</span><span class="sxs-lookup"><span data-stu-id="f21b8-151">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="f21b8-152">Поставщик конфигурации файлов описан ниже в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="f21b8-152">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="f21b8-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) — один из вариантов для безопасного хранения секретов приложения.</span><span class="sxs-lookup"><span data-stu-id="f21b8-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="f21b8-154">Для получения дополнительной информации см. <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-154">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="f21b8-155">Иерархическая модель конфигурации</span><span class="sxs-lookup"><span data-stu-id="f21b8-155">Hierarchical configuration data</span></span>

<span data-ttu-id="f21b8-156">API конфигурации способен поддерживать иерархические данные конфигурации, выполняя преобразование в плоскую структуру иерархических данных с использованием разделителя в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f21b8-156">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="f21b8-157">В следующем файле JSON существуют четыре ключа в структурированной иерархии двух разделов.</span><span class="sxs-lookup"><span data-stu-id="f21b8-157">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="f21b8-158">При считывании файла в конфигурацию для сохранения исходной иерархической структуры данных источника конфигурации создаются уникальные ключи.</span><span class="sxs-lookup"><span data-stu-id="f21b8-158">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="f21b8-159">Разделы и ключи преобразовываются в плоскую структуру с использованием двоеточия (`:`) для сохранения исходной структуры.</span><span class="sxs-lookup"><span data-stu-id="f21b8-159">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="f21b8-160">section0:key0</span><span class="sxs-lookup"><span data-stu-id="f21b8-160">section0:key0</span></span>
* <span data-ttu-id="f21b8-161">section0:key1</span><span class="sxs-lookup"><span data-stu-id="f21b8-161">section0:key1</span></span>
* <span data-ttu-id="f21b8-162">section1:key0</span><span class="sxs-lookup"><span data-stu-id="f21b8-162">section1:key0</span></span>
* <span data-ttu-id="f21b8-163">section1:key1</span><span class="sxs-lookup"><span data-stu-id="f21b8-163">section1:key1</span></span>

<span data-ttu-id="f21b8-164">Методы <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> и <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> доступны для изолирования разделов и дочерних элементов раздела в данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f21b8-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="f21b8-165">Эти методы описаны далее в разделе [GetSection, GetChildren и Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="f21b8-165">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="f21b8-166">Соглашения</span><span class="sxs-lookup"><span data-stu-id="f21b8-166">Conventions</span></span>

<span data-ttu-id="f21b8-167">При запуске приложения источники конфигурации считываются в порядке, в котором были указаны их поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f21b8-167">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="f21b8-168">Поставщики файлов конфигурации имеют возможность перезагрузить конфигурацию при изменении базового файла параметров после запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="f21b8-168">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="f21b8-169">Поставщик конфигурации файлов описан ниже в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="f21b8-169">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="f21b8-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> доступен в контейнере [Dependency Injection (DI)](xref:fundamentals/dependency-injection) приложения.</span><span class="sxs-lookup"><span data-stu-id="f21b8-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="f21b8-171">Поставщики конфигурации не могут использовать контейнер DI, так как он недоступен при настройке узла.</span><span class="sxs-lookup"><span data-stu-id="f21b8-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="f21b8-172">В ключах конфигурации приняты следующие соглашения.</span><span class="sxs-lookup"><span data-stu-id="f21b8-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="f21b8-173">В ключах не учитывается регистр символов.</span><span class="sxs-lookup"><span data-stu-id="f21b8-173">Keys are case-insensitive.</span></span> <span data-ttu-id="f21b8-174">Например `ConnectionString` и `connectionstring` обрабатываются как эквивалентные ключи.</span><span class="sxs-lookup"><span data-stu-id="f21b8-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="f21b8-175">Если значение для одного и того же ключа установлено одним и тем же или разными поставщиками конфигурации, последним значением, установленным на ключе, является используемое значение.</span><span class="sxs-lookup"><span data-stu-id="f21b8-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="f21b8-176">Иерархические ключи</span><span class="sxs-lookup"><span data-stu-id="f21b8-176">Hierarchical keys</span></span>
  * <span data-ttu-id="f21b8-177">При взаимодействии с API конфигурации разделитель-двоеточие (`:`) поддерживается на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="f21b8-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="f21b8-178">В переменных среды разделитель-двоеточие может не работать на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="f21b8-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="f21b8-179">Двойной знак подчеркивания (`__`) поддерживается на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="f21b8-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="f21b8-180">В хранилище ключей Azure иерархические ключи используют `--` (два дефиса) в качестве разделителя.</span><span class="sxs-lookup"><span data-stu-id="f21b8-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="f21b8-181">Чтобы заменить дефисы двоеточием, при загрузке секретов в конфигурацию приложения необходимо указать код.</span><span class="sxs-lookup"><span data-stu-id="f21b8-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="f21b8-182"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f21b8-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="f21b8-183">Привязка массива описана в разделе [Привязка массива к классу](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="f21b8-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="f21b8-184">В значениях конфигурации учитываются следующие соглашения.</span><span class="sxs-lookup"><span data-stu-id="f21b8-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="f21b8-185">Значения являются строками.</span><span class="sxs-lookup"><span data-stu-id="f21b8-185">Values are strings.</span></span>
* <span data-ttu-id="f21b8-186">Значение NULL не может храниться в конфигурации или быть привязанным к объектам.</span><span class="sxs-lookup"><span data-stu-id="f21b8-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="f21b8-187">Поставщики</span><span class="sxs-lookup"><span data-stu-id="f21b8-187">Providers</span></span>

<span data-ttu-id="f21b8-188">В следующей таблице показаны поставщики конфигурации, доступные для приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f21b8-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="f21b8-189">Поставщик</span><span class="sxs-lookup"><span data-stu-id="f21b8-189">Provider</span></span> | <span data-ttu-id="f21b8-190">Предоставляет конфигурацию из &hellip;</span><span class="sxs-lookup"><span data-stu-id="f21b8-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="f21b8-191">[Поставщик конфигурации хранилища ключей Azure](xref:security/key-vault-configuration) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="f21b8-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="f21b8-192">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="f21b8-192">Azure Key Vault</span></span> |
| [<span data-ttu-id="f21b8-193">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="f21b8-193">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="f21b8-194">Параметры командной строки</span><span class="sxs-lookup"><span data-stu-id="f21b8-194">Command-line parameters</span></span> |
| [<span data-ttu-id="f21b8-195">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="f21b8-195">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="f21b8-196">Источник пользователя</span><span class="sxs-lookup"><span data-stu-id="f21b8-196">Custom source</span></span> |
| [<span data-ttu-id="f21b8-197">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="f21b8-197">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="f21b8-198">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="f21b8-198">Environment variables</span></span> |
| [<span data-ttu-id="f21b8-199">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="f21b8-199">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="f21b8-200">Файлы (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="f21b8-200">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="f21b8-201">Поставщик конфигурации ключа для каждого файла</span><span class="sxs-lookup"><span data-stu-id="f21b8-201">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="f21b8-202">Справочные файлы</span><span class="sxs-lookup"><span data-stu-id="f21b8-202">Directory files</span></span> |
| [<span data-ttu-id="f21b8-203">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="f21b8-203">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="f21b8-204">Коллекции оперативной памяти</span><span class="sxs-lookup"><span data-stu-id="f21b8-204">In-memory collections</span></span> |
| <span data-ttu-id="f21b8-205">[Секреты пользователей (Менеджер секретов)](xref:security/app-secrets) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="f21b8-205">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="f21b8-206">Файл в каталоге профиля пользователя</span><span class="sxs-lookup"><span data-stu-id="f21b8-206">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="f21b8-207">Поставщик</span><span class="sxs-lookup"><span data-stu-id="f21b8-207">Provider</span></span> | <span data-ttu-id="f21b8-208">Предоставляет конфигурацию из &hellip;</span><span class="sxs-lookup"><span data-stu-id="f21b8-208">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="f21b8-209">[Поставщик конфигурации хранилища ключей Azure](xref:security/key-vault-configuration) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="f21b8-209">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="f21b8-210">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="f21b8-210">Azure Key Vault</span></span> |
| [<span data-ttu-id="f21b8-211">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="f21b8-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="f21b8-212">Параметры командной строки</span><span class="sxs-lookup"><span data-stu-id="f21b8-212">Command-line parameters</span></span> |
| [<span data-ttu-id="f21b8-213">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="f21b8-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="f21b8-214">Источник пользователя</span><span class="sxs-lookup"><span data-stu-id="f21b8-214">Custom source</span></span> |
| [<span data-ttu-id="f21b8-215">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="f21b8-215">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="f21b8-216">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="f21b8-216">Environment variables</span></span> |
| [<span data-ttu-id="f21b8-217">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="f21b8-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="f21b8-218">Файлы (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="f21b8-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="f21b8-219">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="f21b8-219">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="f21b8-220">Коллекции оперативной памяти</span><span class="sxs-lookup"><span data-stu-id="f21b8-220">In-memory collections</span></span> |
| <span data-ttu-id="f21b8-221">[Секреты пользователей (Менеджер секретов)](xref:security/app-secrets) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="f21b8-221">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="f21b8-222">Файл в каталоге профиля пользователя</span><span class="sxs-lookup"><span data-stu-id="f21b8-222">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="f21b8-223">Поставщик</span><span class="sxs-lookup"><span data-stu-id="f21b8-223">Provider</span></span> | <span data-ttu-id="f21b8-224">Предоставляет конфигурацию из &hellip;</span><span class="sxs-lookup"><span data-stu-id="f21b8-224">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="f21b8-225">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="f21b8-225">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="f21b8-226">Параметры командной строки</span><span class="sxs-lookup"><span data-stu-id="f21b8-226">Command-line parameters</span></span> |
| [<span data-ttu-id="f21b8-227">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="f21b8-227">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="f21b8-228">Источник пользователя</span><span class="sxs-lookup"><span data-stu-id="f21b8-228">Custom source</span></span> |
| [<span data-ttu-id="f21b8-229">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="f21b8-229">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="f21b8-230">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="f21b8-230">Environment variables</span></span> |
| [<span data-ttu-id="f21b8-231">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="f21b8-231">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="f21b8-232">Файлы (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="f21b8-232">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="f21b8-233">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="f21b8-233">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="f21b8-234">Коллекции оперативной памяти</span><span class="sxs-lookup"><span data-stu-id="f21b8-234">In-memory collections</span></span> |
| <span data-ttu-id="f21b8-235">[Секреты пользователей (Менеджер секретов)](xref:security/app-secrets) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="f21b8-235">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="f21b8-236">Файл в каталоге профиля пользователя</span><span class="sxs-lookup"><span data-stu-id="f21b8-236">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="f21b8-237">Источники конфигурации считываются в том порядке, в котором при запуске указываются их поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f21b8-237">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="f21b8-238">Поставщики конфигурации в этом разделе описаны в алфавитном порядке, а не в необходимом вам порядке в коде.</span><span class="sxs-lookup"><span data-stu-id="f21b8-238">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="f21b8-239">Порядок поставщиков конфигурации в коде соответствует приоритетам ваших основных источников конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f21b8-239">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="f21b8-240">Типичная последовательность поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f21b8-240">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="f21b8-241">Файлы (*appsettings.json*, *appsettings.{Environment}.json*, где `{Environment}` — это текущая среда размещения приложения)</span><span class="sxs-lookup"><span data-stu-id="f21b8-241">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. <span data-ttu-id="f21b8-242">[Azure Key Vault](xref:security/key-vault-configuration);</span><span class="sxs-lookup"><span data-stu-id="f21b8-242">[Azure Key Vault](xref:security/key-vault-configuration)</span></span>
1. <span data-ttu-id="f21b8-243">[Секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки)</span><span class="sxs-lookup"><span data-stu-id="f21b8-243">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="f21b8-244">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="f21b8-244">Environment variables</span></span>
1. <span data-ttu-id="f21b8-245">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="f21b8-245">Command-line arguments</span></span>

<span data-ttu-id="f21b8-246">Общепринятой практикой является размещение поставщика конфигурации командной строки последним в ряду поставщиков, чтобы аргументы командной строки могли переопределять конфигурацию, установленную другими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="f21b8-246">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f21b8-247">Эта последовательность поставщиков помещается в месте, где происходит инициализация нового класса <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-247">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="f21b8-248">Дополнительные сведения см. в статье [Веб-узел ASP.NET Core. Создание узла](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="f21b8-248">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f21b8-249">Эта последовательность поставщиков может быть создана для приложения (а не узла) с помощью <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> и вызова метода <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> в `Startup`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-249">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

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

<span data-ttu-id="f21b8-250">В приведенном выше примере имя среды (`env.EnvironmentName`) и имя сборки приложения (`env.ApplicationName`) предоставляются <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-250">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="f21b8-251">Для получения дополнительной информации см. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-251">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a><span data-ttu-id="f21b8-252">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="f21b8-252">ConfigureAppConfiguration</span></span>

<span data-ttu-id="f21b8-253">Вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> при сборке веб-узла, чтобы указать поставщиков конфигурации приложения в дополнение к тем, которые автоматически добавлены <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-253">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the Web Host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="f21b8-254">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="f21b8-254">Command-line Configuration Provider</span></span>

<span data-ttu-id="f21b8-255"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> загружает конфигурацию из пары "ключ — значение" аргумента командной строки в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="f21b8-255">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="f21b8-256">Чтобы активировать конфигурацию командной строки, вызывается метод расширения <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> для экземпляра <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-256">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f21b8-257">`AddCommandLine` автоматически вызывается при инициализации нового <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-257">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="f21b8-258">Дополнительные сведения см. в статье [Веб-узел ASP.NET Core. Создание узла](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="f21b8-258">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="f21b8-259">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="f21b8-259">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="f21b8-260">дополнительную конфигурацию из *appsettings.json* и *appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="f21b8-260">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="f21b8-261">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки);</span><span class="sxs-lookup"><span data-stu-id="f21b8-261">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="f21b8-262">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="f21b8-262">Environment variables.</span></span>

<span data-ttu-id="f21b8-263">`CreateDefaultBuilder` добавляет последним поставщика конфигурации командной строки.</span><span class="sxs-lookup"><span data-stu-id="f21b8-263">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="f21b8-264">Аргументы командной строки передаются в набор конфигурации переопределения среды выполнения, установленной другими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="f21b8-264">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="f21b8-265">`CreateDefaultBuilder` действует, когда создается узел.</span><span class="sxs-lookup"><span data-stu-id="f21b8-265">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="f21b8-266">Поэтому конфигурация командной строки, активированная с помощью `CreateDefaultBuilder`, может повлиять на настройку узла.</span><span class="sxs-lookup"><span data-stu-id="f21b8-266">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f21b8-267">Чтобы указать конфигурацию приложения, при сборке узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-267">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="f21b8-268">Объект `AddCommandLine` уже был вызван методом `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-268">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="f21b8-269">Если нужно предоставить конфигурацию приложения, сохранив возможность переопределить ее через аргументы командной строки, вызовите в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>дополнительные поставщики приложения, в завершение вызвав `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-269">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

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
                config.AddCommandLine(args)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="f21b8-270">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="f21b8-270">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f21b8-271">Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-271">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="f21b8-272">Объект `AddCommandLine` уже был вызван методом `CreateDefaultBuilder` при вызове `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-272">`AddCommandLine` has already been called by `CreateDefaultBuilder` when `UseConfiguration` is called.</span></span> <span data-ttu-id="f21b8-273">Если нужно предоставить конфигурацию приложения, сохранив возможность переопределить ее через аргументы командной строки, вызовите в `ConfigurationBuilder`дополнительные поставщики приложения, в завершение вызвав `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-273">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers on a `ConfigurationBuilder` and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="f21b8-274">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="f21b8-274">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f21b8-275">Чтобы активировать конфигурацию командной строки, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-275">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f21b8-276">В этом случае аргументы командной строки, передаваемые во время выполнения, переопределяют конфигурацию, заданную другими вызванными ранее поставщиками конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f21b8-276">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="f21b8-277">Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="f21b8-277">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="f21b8-278">**Пример**</span><span class="sxs-lookup"><span data-stu-id="f21b8-278">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f21b8-279">Пример приложения 2.x использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-279">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f21b8-280">Пример приложения 1.x вызывает образец <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> в <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-280">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="f21b8-281">Откройте командную строку в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="f21b8-281">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="f21b8-282">Поставьте аргумент командной строки в команду `dotnet run`: `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-282">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="f21b8-283">После запуска приложения откройте браузер в приложении по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-283">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="f21b8-284">Обратите внимание, что вывод содержит пару "ключ — значение" для аргумента командной строки конфигурации, предоставленного для `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-284">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="f21b8-285">Аргументы</span><span class="sxs-lookup"><span data-stu-id="f21b8-285">Arguments</span></span>

<span data-ttu-id="f21b8-286">Значение должно соответствовать знаку равенства (`=`), или ключ должен иметь префикс (`--` или `/`), когда значение следует за пробелом.</span><span class="sxs-lookup"><span data-stu-id="f21b8-286">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="f21b8-287">Значение может соответствовать NULL, если используется знак равенства (например, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="f21b8-287">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="f21b8-288">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="f21b8-288">Key prefix</span></span>               | <span data-ttu-id="f21b8-289">Пример</span><span class="sxs-lookup"><span data-stu-id="f21b8-289">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="f21b8-290">Без префикса</span><span class="sxs-lookup"><span data-stu-id="f21b8-290">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="f21b8-291">Два дефиса (`--`)</span><span class="sxs-lookup"><span data-stu-id="f21b8-291">Two dashes (`--`)</span></span>        | <span data-ttu-id="f21b8-292">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="f21b8-292">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="f21b8-293">Прямая косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="f21b8-293">Forward slash (`/`)</span></span>      | <span data-ttu-id="f21b8-294">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="f21b8-294">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="f21b8-295">В рамках одной и той же команды не смешивайте пары "ключ — значение" аргумента командной строки, которые используют знак равенства, с парами "ключ — значение", которые используют пробел.</span><span class="sxs-lookup"><span data-stu-id="f21b8-295">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="f21b8-296">Примеры команд.</span><span class="sxs-lookup"><span data-stu-id="f21b8-296">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value --CommandLineKey2=value /CommandLineKey2=value
dotnet run --CommandLineKey1 value /CommandLineKey2 value
dotnet run CommandLineKey1= CommandLineKey2=value
```

### <a name="switch-mappings"></a><span data-ttu-id="f21b8-297">Сопоставления переключений</span><span class="sxs-lookup"><span data-stu-id="f21b8-297">Switch mappings</span></span>

<span data-ttu-id="f21b8-298">Сопоставление параметров позволяет указать логику замены имен ключей.</span><span class="sxs-lookup"><span data-stu-id="f21b8-298">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="f21b8-299">Когда вручную создается конфигурация с помощью <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, методу <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> можно предоставить словарь сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="f21b8-299">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="f21b8-300">В словаре сопоставлений переключений выполняется поиск ключа, который совпадает с ключом, предоставляемым аргументом командной строки.</span><span class="sxs-lookup"><span data-stu-id="f21b8-300">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="f21b8-301">Если ключ в командной строке находится в словаре, значение словаря (замена ключа) передается обратно, чтобы установить пару "ключ — значение" в конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="f21b8-301">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="f21b8-302">Сопоставление переключений необходимо для любого ключа командной строки с префиксом из одного дефиса (`-`).</span><span class="sxs-lookup"><span data-stu-id="f21b8-302">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="f21b8-303">Правила ключей из словаря сопоставления переключений:</span><span class="sxs-lookup"><span data-stu-id="f21b8-303">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="f21b8-304">Переключения должны начинаться с дефиса (`-`) или двойного дефиса (`--`).</span><span class="sxs-lookup"><span data-stu-id="f21b8-304">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="f21b8-305">Словарь сопоставлений переключений не должен содержать повторяющиеся ключи.</span><span class="sxs-lookup"><span data-stu-id="f21b8-305">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f21b8-306">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-306">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddCommandLine(args, _switchMappings)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="f21b8-307">Как показано в предыдущем примере, вызов `CreateDefaultBuilder` не должен передавать аргументы при использовании сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="f21b8-307">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="f21b8-308">Вызов команды `AddCommandLine` метода `CreateDefaultBuilder` не включает сопоставленные коммутаторы, и нет возможности передать словарь сопоставления переключений в `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-308">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="f21b8-309">Если аргументы включают сопоставления переключений и передаются в `CreateDefaultBuilder`, его поставщик `AddCommandLine` не может инициализироваться с помощью <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-309">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="f21b8-310">Чтобы решить эту проблему, нужно не передавать аргументы команде `CreateDefaultBuilder`, а позволить методу `AddCommandLine` метода `ConfigurationBuilder` обрабатывать как аргументы, так и словарь сопоставления параметров.</span><span class="sxs-lookup"><span data-stu-id="f21b8-310">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="f21b8-311">Как показано в предыдущем примере, вызов `CreateDefaultBuilder` не должен передавать аргументы при использовании сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="f21b8-311">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="f21b8-312">Вызов команды `AddCommandLine` метода `CreateDefaultBuilder` не включает сопоставленные коммутаторы, и нет возможности передать словарь сопоставления переключений в `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-312">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="f21b8-313">Если аргументы включают сопоставления переключений и передаются в `CreateDefaultBuilder`, его поставщик `AddCommandLine` не может инициализироваться с помощью <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-313">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="f21b8-314">Чтобы решить эту проблему, нужно не передавать аргументы команде `CreateDefaultBuilder`, а позволить методу `AddCommandLine` метода `ConfigurationBuilder` обрабатывать как аргументы, так и словарь сопоставления параметров.</span><span class="sxs-lookup"><span data-stu-id="f21b8-314">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="f21b8-315">Созданный словарь сопоставлений переключений содержит данные, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="f21b8-315">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="f21b8-316">Ключ</span><span class="sxs-lookup"><span data-stu-id="f21b8-316">Key</span></span>       | <span data-ttu-id="f21b8-317">Значение</span><span class="sxs-lookup"><span data-stu-id="f21b8-317">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="f21b8-318">Если ключи сопоставления переключений используются при запуске приложения, конфигурация принимает значение конфигурации в ключе, предоставленном в словаре.</span><span class="sxs-lookup"><span data-stu-id="f21b8-318">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="f21b8-319">После выполнения предыдущей команды конфигурация содержит значения, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="f21b8-319">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="f21b8-320">Ключ</span><span class="sxs-lookup"><span data-stu-id="f21b8-320">Key</span></span>               | <span data-ttu-id="f21b8-321">Значение</span><span class="sxs-lookup"><span data-stu-id="f21b8-321">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="f21b8-322">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="f21b8-322">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="f21b8-323"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> загружает конфигурацию из пары "ключ — значение" переменной среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="f21b8-323">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="f21b8-324">Чтобы активировать конфигурацию переменных среды, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-324">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f21b8-325">В переменных средах разделитель-двоеточие (`:`) может не работать на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="f21b8-325">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="f21b8-326">Двойной знак подчеркивания (`__`) поддерживается на всех платформах и его можно заменить двоеточием.</span><span class="sxs-lookup"><span data-stu-id="f21b8-326">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="f21b8-327">[Служба приложений Azure](https://azure.microsoft.com/services/app-service/) позволяет задать переменные среды на портале Azure, который может переопределить конфигурацию приложения, используя поставщик конфигурации переменных среды.</span><span class="sxs-lookup"><span data-stu-id="f21b8-327">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="f21b8-328">Дополнительные сведения см. в разделе [Приложения Azure. Переопределение конфигурации приложения с помощью портала Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="f21b8-328">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f21b8-329">`AddEnvironmentVariables` автоматически вызывается при инициализации нового <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-329">`AddEnvironmentVariables` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="f21b8-330">Дополнительные сведения см. в статье [Веб-узел ASP.NET Core. Создание узла](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="f21b8-330">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="f21b8-331">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="f21b8-331">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="f21b8-332">дополнительную конфигурацию из *appsettings.json* и *appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="f21b8-332">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="f21b8-333">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки);</span><span class="sxs-lookup"><span data-stu-id="f21b8-333">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="f21b8-334">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="f21b8-334">Command-line arguments.</span></span>

<span data-ttu-id="f21b8-335">Поставщик конфигурации переменных среды вызывается после того, как настройка была создана из секретов пользователя и файлов *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="f21b8-335">The Environment Variable Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="f21b8-336">Вызов поставщика в этой позиции разрешает чтение переменных среды выполнения, чтобы переопределить конфигурацию, заданную секретом пользователя и файлом *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="f21b8-336">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f21b8-337">Чтобы указать конфигурацию приложения, при сборке узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-337">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="f21b8-338">Метод `AddEnvironmentVariables` для переменных среды с префиксом `ASPNETCORE_` уже был вызван из `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-338">`AddEnvironmentVariables` for environment variables prefixed with `ASPNETCORE_` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="f21b8-339">Если вам нужно добавить конфигурацию приложения из дополнительных переменных среды, вызовите дополнительные поставщики приложения в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, а затем вызовите `AddEnvironmentVariables` с префиксом.</span><span class="sxs-lookup"><span data-stu-id="f21b8-339">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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
                config.AddEnvironmentVariables(prefix: "PREFIX_")
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="f21b8-340">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="f21b8-340">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f21b8-341">Вызовите метод расширения `AddEnvironmentVariables` для экземпляра <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-341">Call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="f21b8-342">Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-342">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="f21b8-343">Метод `AddEnvironmentVariables` для переменных среды с префиксом `ASPNETCORE_` уже был вызван из `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-343">`AddEnvironmentVariables` for environment variables prefixed with `ASPNETCORE_` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="f21b8-344">Если вам нужно добавить конфигурацию приложения из дополнительных переменных среды, вызовите дополнительные поставщики приложения в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, а затем вызовите `AddEnvironmentVariables` с префиксом.</span><span class="sxs-lookup"><span data-stu-id="f21b8-344">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="f21b8-345">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="f21b8-345">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f21b8-346">Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="f21b8-346">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="f21b8-347">**Пример**</span><span class="sxs-lookup"><span data-stu-id="f21b8-347">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f21b8-348">Пример приложения 2.x использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-348">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f21b8-349">Пример приложения 1.x вызывает образец `AddEnvironmentVariables` в `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-349">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="f21b8-350">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="f21b8-350">Run the sample app.</span></span> <span data-ttu-id="f21b8-351">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-351">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="f21b8-352">Обратите внимание, что выходные данные содержат пару "ключ — значение" для переменной среды `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-352">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="f21b8-353">Значение отражает среду, в которой выполняется приложение, обычно при локальном запуске это `Development`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-353">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="f21b8-354">Чтобы список переменных сред, отображаемый приложением, был коротким, приложение фильтрует переменные среды по следующим пунктам:</span><span class="sxs-lookup"><span data-stu-id="f21b8-354">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="f21b8-355">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="f21b8-355">ASPNETCORE_</span></span>
* <span data-ttu-id="f21b8-356">urls</span><span class="sxs-lookup"><span data-stu-id="f21b8-356">urls</span></span>
* <span data-ttu-id="f21b8-357">Ведение журналов</span><span class="sxs-lookup"><span data-stu-id="f21b8-357">Logging</span></span>
* <span data-ttu-id="f21b8-358">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="f21b8-358">ENVIRONMENT</span></span>
* <span data-ttu-id="f21b8-359">contentRoot</span><span class="sxs-lookup"><span data-stu-id="f21b8-359">contentRoot</span></span>
* <span data-ttu-id="f21b8-360">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="f21b8-360">AllowedHosts</span></span>
* <span data-ttu-id="f21b8-361">applicationName</span><span class="sxs-lookup"><span data-stu-id="f21b8-361">applicationName</span></span>
* <span data-ttu-id="f21b8-362">CommandLine</span><span class="sxs-lookup"><span data-stu-id="f21b8-362">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f21b8-363">Если хотите просмотреть все переменные среды, доступные для приложения, измените значение `FilteredConfiguration` в *Pages/Index.cshtml.cs* на следующее:</span><span class="sxs-lookup"><span data-stu-id="f21b8-363">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f21b8-364">Если хотите просмотреть все переменные среды, доступные для приложения, измените значение `FilteredConfiguration` в *Controllers/HomeController.cs* на следующее:</span><span class="sxs-lookup"><span data-stu-id="f21b8-364">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="f21b8-365">Префиксы</span><span class="sxs-lookup"><span data-stu-id="f21b8-365">Prefixes</span></span>

<span data-ttu-id="f21b8-366">Переменные среды, которые загружаются в конфигурацию приложения, фильтруются при добавлении префикса к методу `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-366">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="f21b8-367">Например, чтобы отфильтровать переменные среды по префиксу `CUSTOM_`, введите префикс поставщику конфигурации:</span><span class="sxs-lookup"><span data-stu-id="f21b8-367">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="f21b8-368">Префикс отделяется при создании пары конфигурации "ключ — значение".</span><span class="sxs-lookup"><span data-stu-id="f21b8-368">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f21b8-369">Статический удобный метод `CreateDefaultBuilder` создает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> для установления размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="f21b8-369">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="f21b8-370">Когда значение `WebHostBuilder` будет создано, оно найдет конфигурацию узла в переменных среды, которые начинаются с префикса `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-370">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="f21b8-371">**Префиксы строк подключения**</span><span class="sxs-lookup"><span data-stu-id="f21b8-371">**Connection string prefixes**</span></span>

<span data-ttu-id="f21b8-372">API конфигурации имеет специальные правила обработки для четырех строк подключения переменных среды, связанных с настройкой строк подключения Azure для среды приложения.</span><span class="sxs-lookup"><span data-stu-id="f21b8-372">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="f21b8-373">Если префикс не указан в `AddEnvironmentVariables`, переменные среды с префиксами, указанными в таблице, загружаются в приложение.</span><span class="sxs-lookup"><span data-stu-id="f21b8-373">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="f21b8-374">Префикс строки подключения</span><span class="sxs-lookup"><span data-stu-id="f21b8-374">Connection string prefix</span></span> | <span data-ttu-id="f21b8-375">Поставщик</span><span class="sxs-lookup"><span data-stu-id="f21b8-375">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="f21b8-376">Поставщик пользователя</span><span class="sxs-lookup"><span data-stu-id="f21b8-376">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="f21b8-377">MySQL</span><span class="sxs-lookup"><span data-stu-id="f21b8-377">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="f21b8-378">База данных SQL Azure</span><span class="sxs-lookup"><span data-stu-id="f21b8-378">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="f21b8-379">SQL Server</span><span class="sxs-lookup"><span data-stu-id="f21b8-379">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="f21b8-380">Когда переменная среды обнаруживается и загружается в конфигурацию с одним из четырех префиксов, приведенных в таблице, происходит следующее.</span><span class="sxs-lookup"><span data-stu-id="f21b8-380">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="f21b8-381">Ключ конфигурации создается путем удаления префикса переменных среды и добавления ключа раздела конфигурации (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="f21b8-381">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="f21b8-382">Создается новая пара "ключ — значение" конфигурации, которая представляет поставщика подключения базы данных (за исключением `CUSTOMCONNSTR_`, который не имеет указанного поставщика).</span><span class="sxs-lookup"><span data-stu-id="f21b8-382">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="f21b8-383">Ключ переменной среды</span><span class="sxs-lookup"><span data-stu-id="f21b8-383">Environment variable key</span></span> | <span data-ttu-id="f21b8-384">Преобразованный ключ конфигурации</span><span class="sxs-lookup"><span data-stu-id="f21b8-384">Converted configuration key</span></span> | <span data-ttu-id="f21b8-385">Запись конфигурации поставщика</span><span class="sxs-lookup"><span data-stu-id="f21b8-385">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="f21b8-386">Запись конфигурации не создана.</span><span class="sxs-lookup"><span data-stu-id="f21b8-386">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="f21b8-387">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="f21b8-387">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="f21b8-388">Значение: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="f21b8-388">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="f21b8-389">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="f21b8-389">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="f21b8-390">Значение: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="f21b8-390">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="f21b8-391">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="f21b8-391">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="f21b8-392">Значение: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="f21b8-392">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="f21b8-393">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="f21b8-393">File Configuration Provider</span></span>

<span data-ttu-id="f21b8-394"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> является базовым классом для загрузки конфигурации из файловой системы.</span><span class="sxs-lookup"><span data-stu-id="f21b8-394"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="f21b8-395">Следующие поставщики конфигурации предназначены для определенных типов файлов:</span><span class="sxs-lookup"><span data-stu-id="f21b8-395">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="f21b8-396">Поставщик конфигурации INI</span><span class="sxs-lookup"><span data-stu-id="f21b8-396">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="f21b8-397">Поставщик конфигурации JSON</span><span class="sxs-lookup"><span data-stu-id="f21b8-397">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="f21b8-398">Поставщик конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="f21b8-398">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="f21b8-399">Поставщик конфигурации INI</span><span class="sxs-lookup"><span data-stu-id="f21b8-399">INI Configuration Provider</span></span>

<span data-ttu-id="f21b8-400"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> загружает конфигурацию из пары "ключ — значение" INI-файла во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="f21b8-400">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="f21b8-401">Чтобы активировать конфигурацию INI-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-401">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f21b8-402">Двоеточие можно использовать как разделитель раздела в конфигурации файла INI.</span><span class="sxs-lookup"><span data-stu-id="f21b8-402">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="f21b8-403">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="f21b8-403">Overloads permit specifying:</span></span>

* <span data-ttu-id="f21b8-404">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="f21b8-404">Whether the file is optional.</span></span>
* <span data-ttu-id="f21b8-405">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="f21b8-405">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="f21b8-406"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="f21b8-406">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f21b8-407">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-407">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddIniFile("config.ini", optional: true, reloadOnChange: true)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="f21b8-408">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="f21b8-408">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f21b8-409">Вызывая `CreateDefaultBuilder`, вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="f21b8-409">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="f21b8-410">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="f21b8-410">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f21b8-411">Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="f21b8-411">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="f21b8-412">Общий пример конфигурации INI-файла.</span><span class="sxs-lookup"><span data-stu-id="f21b8-412">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="f21b8-413">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-413">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="f21b8-414">section0:key0</span><span class="sxs-lookup"><span data-stu-id="f21b8-414">section0:key0</span></span>
* <span data-ttu-id="f21b8-415">section0:key1</span><span class="sxs-lookup"><span data-stu-id="f21b8-415">section0:key1</span></span>
* <span data-ttu-id="f21b8-416">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="f21b8-416">section1:subsection:key</span></span>
* <span data-ttu-id="f21b8-417">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="f21b8-417">section2:subsection0:key</span></span>
* <span data-ttu-id="f21b8-418">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="f21b8-418">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="f21b8-419">Поставщик конфигурации JSON</span><span class="sxs-lookup"><span data-stu-id="f21b8-419">JSON Configuration Provider</span></span>

<span data-ttu-id="f21b8-420"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> загружает конфигурацию из пары "ключ — значение" JSON-файла в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="f21b8-420">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="f21b8-421">Чтобы активировать конфигурацию JSON-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-421">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f21b8-422">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="f21b8-422">Overloads permit specifying:</span></span>

* <span data-ttu-id="f21b8-423">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="f21b8-423">Whether the file is optional.</span></span>
* <span data-ttu-id="f21b8-424">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="f21b8-424">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="f21b8-425"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="f21b8-425">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f21b8-426">`AddJsonFile` автоматически вызывается дважды при инициализации нового <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-426">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="f21b8-427">Метод вызывается для загрузки конфигурации из:</span><span class="sxs-lookup"><span data-stu-id="f21b8-427">The method is called to load configuration from:</span></span>

* <span data-ttu-id="f21b8-428">*appsettings.json* &ndash; первым читается этот файл.</span><span class="sxs-lookup"><span data-stu-id="f21b8-428">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="f21b8-429">Версия файла среды может переопределить значения, предоставленные *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f21b8-429">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="f21b8-430">*appsettings.{Environment}.json* — версия среды файла загружается на основе [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="f21b8-430">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="f21b8-431">Дополнительные сведения см. в статье [Веб-узел ASP.NET Core. Создание узла](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="f21b8-431">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="f21b8-432">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="f21b8-432">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="f21b8-433">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="f21b8-433">Environment variables.</span></span>
* <span data-ttu-id="f21b8-434">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки);</span><span class="sxs-lookup"><span data-stu-id="f21b8-434">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="f21b8-435">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="f21b8-435">Command-line arguments.</span></span>

<span data-ttu-id="f21b8-436">Сначала устанавливается поставщик конфигурации JSON-файлов.</span><span class="sxs-lookup"><span data-stu-id="f21b8-436">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="f21b8-437">Таким образом, секреты пользователя, переменные среды и аргументы командной строки переопределят конфигурацию, заданную файлами *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="f21b8-437">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f21b8-438">Вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> при сборке узла, чтобы указать конфигурацию приложения для файлов, отличных от *appsettings.json* и *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="f21b8-438">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

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
                config.AddJsonFile("config.json", optional: true, reloadOnChange: true)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="f21b8-439">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="f21b8-439">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f21b8-440">Вы можете также напрямую вызвать метод расширения `AddJsonFile` в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-440">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f21b8-441">Вызывая `CreateDefaultBuilder`, вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="f21b8-441">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="f21b8-442">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="f21b8-442">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f21b8-443">Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="f21b8-443">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="f21b8-444">**Пример**</span><span class="sxs-lookup"><span data-stu-id="f21b8-444">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f21b8-445">В примере приложения 2.x используется преимущество статически удобного метода `CreateDefaultBuilder` для создания узла, который включает два вызова в `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-445">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="f21b8-446">Конфигурация загружена из *appsettings.json* и *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="f21b8-446">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f21b8-447">Пример приложения 1.x вызывает образец `AddJsonFile` дважды в `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-447">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="f21b8-448">Конфигурация загружена из *appsettings.json* и *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="f21b8-448">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="f21b8-449">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="f21b8-449">Run the sample app.</span></span> <span data-ttu-id="f21b8-450">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-450">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="f21b8-451">Обратите внимание, что выходные данные содержат пары "ключ — значение" для конфигурации, представленной в таблице в зависимости от среды.</span><span class="sxs-lookup"><span data-stu-id="f21b8-451">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="f21b8-452">Ключи конфигурации ведения журнала используют двоеточие (`:`) как иерархический разделитель.</span><span class="sxs-lookup"><span data-stu-id="f21b8-452">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="f21b8-453">Ключ</span><span class="sxs-lookup"><span data-stu-id="f21b8-453">Key</span></span>                        | <span data-ttu-id="f21b8-454">Значение разработки</span><span class="sxs-lookup"><span data-stu-id="f21b8-454">Development Value</span></span> | <span data-ttu-id="f21b8-455">Рабочее значение</span><span class="sxs-lookup"><span data-stu-id="f21b8-455">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="f21b8-456">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="f21b8-456">Logging:LogLevel:System</span></span>    | <span data-ttu-id="f21b8-457">Сведения</span><span class="sxs-lookup"><span data-stu-id="f21b8-457">Information</span></span>       | <span data-ttu-id="f21b8-458">Сведения</span><span class="sxs-lookup"><span data-stu-id="f21b8-458">Information</span></span>      |
| <span data-ttu-id="f21b8-459">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="f21b8-459">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="f21b8-460">Сведения</span><span class="sxs-lookup"><span data-stu-id="f21b8-460">Information</span></span>       | <span data-ttu-id="f21b8-461">Сведения</span><span class="sxs-lookup"><span data-stu-id="f21b8-461">Information</span></span>      |
| <span data-ttu-id="f21b8-462">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="f21b8-462">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="f21b8-463">Отладка</span><span class="sxs-lookup"><span data-stu-id="f21b8-463">Debug</span></span>             | <span data-ttu-id="f21b8-464">Ошибка</span><span class="sxs-lookup"><span data-stu-id="f21b8-464">Error</span></span>            |
| <span data-ttu-id="f21b8-465">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="f21b8-465">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="f21b8-466">Поставщик конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="f21b8-466">XML Configuration Provider</span></span>

<span data-ttu-id="f21b8-467"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> загружает конфигурацию из пары "ключ — значение" XML-файла в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="f21b8-467">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="f21b8-468">Чтобы активировать конфигурацию XML-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-468">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f21b8-469">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="f21b8-469">Overloads permit specifying:</span></span>

* <span data-ttu-id="f21b8-470">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="f21b8-470">Whether the file is optional.</span></span>
* <span data-ttu-id="f21b8-471">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="f21b8-471">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="f21b8-472"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="f21b8-472">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="f21b8-473">Корневой узел файла конфигурации не учитывается при создании пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f21b8-473">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="f21b8-474">Не указывайте в файле Document Type Definition (определение типа документа, DTD) или пространство имен.</span><span class="sxs-lookup"><span data-stu-id="f21b8-474">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f21b8-475">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-475">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddXmlFile("config.xml", optional: true, reloadOnChange: true)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="f21b8-476">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="f21b8-476">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f21b8-477">Вызывая `CreateDefaultBuilder`, вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="f21b8-477">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="f21b8-478">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="f21b8-478">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f21b8-479">Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="f21b8-479">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="f21b8-480">XML-файлы конфигурации могут использовать имена отдельных элементов для повторяющихся разделов.</span><span class="sxs-lookup"><span data-stu-id="f21b8-480">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="f21b8-481">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-481">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="f21b8-482">section0:key0</span><span class="sxs-lookup"><span data-stu-id="f21b8-482">section0:key0</span></span>
* <span data-ttu-id="f21b8-483">section0:key1</span><span class="sxs-lookup"><span data-stu-id="f21b8-483">section0:key1</span></span>
* <span data-ttu-id="f21b8-484">section1:key0</span><span class="sxs-lookup"><span data-stu-id="f21b8-484">section1:key0</span></span>
* <span data-ttu-id="f21b8-485">section1:key1</span><span class="sxs-lookup"><span data-stu-id="f21b8-485">section1:key1</span></span>

<span data-ttu-id="f21b8-486">Повторяющиеся элементы, использующие то же имя элемента, работают, если атрибут `name` используется для различения элементов.</span><span class="sxs-lookup"><span data-stu-id="f21b8-486">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="f21b8-487">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-487">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="f21b8-488">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="f21b8-488">section:section0:key:key0</span></span>
* <span data-ttu-id="f21b8-489">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="f21b8-489">section:section0:key:key1</span></span>
* <span data-ttu-id="f21b8-490">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="f21b8-490">section:section1:key:key0</span></span>
* <span data-ttu-id="f21b8-491">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="f21b8-491">section:section1:key:key1</span></span>

<span data-ttu-id="f21b8-492">Атрибуты можно использовать для предоставления значений.</span><span class="sxs-lookup"><span data-stu-id="f21b8-492">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="f21b8-493">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-493">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="f21b8-494">key:attribute</span><span class="sxs-lookup"><span data-stu-id="f21b8-494">key:attribute</span></span>
* <span data-ttu-id="f21b8-495">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="f21b8-495">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="f21b8-496">Поставщик конфигурации ключа для каждого файла</span><span class="sxs-lookup"><span data-stu-id="f21b8-496">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="f21b8-497"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> использует файлы каталога как пары "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f21b8-497">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="f21b8-498">Ключ является именем файла.</span><span class="sxs-lookup"><span data-stu-id="f21b8-498">The key is the file name.</span></span> <span data-ttu-id="f21b8-499">Значение содержит содержимое файла.</span><span class="sxs-lookup"><span data-stu-id="f21b8-499">The value contains the file's contents.</span></span> <span data-ttu-id="f21b8-500">Поставщик конфигурации ключа для каждого файла используется в сценариях размещения Docker.</span><span class="sxs-lookup"><span data-stu-id="f21b8-500">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="f21b8-501">Чтобы активировать конфигурацию ключа для каждого файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-501">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="f21b8-502">Значение параметра `directoryPath` должно быть абсолютным путем к файлам.</span><span class="sxs-lookup"><span data-stu-id="f21b8-502">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="f21b8-503">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="f21b8-503">Overloads permit specifying:</span></span>

* <span data-ttu-id="f21b8-504">`Action<KeyPerFileConfigurationSource>` — делегат, который настраивает источник.</span><span class="sxs-lookup"><span data-stu-id="f21b8-504">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="f21b8-505">Обязательно ли указывать каталог и путь к каталогу.</span><span class="sxs-lookup"><span data-stu-id="f21b8-505">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="f21b8-506">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-506">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddKeyPerFile(directoryPath: path, optional: true)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="f21b8-507">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="f21b8-507">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="f21b8-508">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="f21b8-508">Memory Configuration Provider</span></span>

<span data-ttu-id="f21b8-509"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> использует коллекцию памяти в качестве пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f21b8-509">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="f21b8-510">Чтобы активировать конфигурацию коллекции в памяти, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-510">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f21b8-511">Поставщик конфигурации может инициализироваться значением `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-511">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f21b8-512">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-512">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddInMemoryCollection(_dict)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="f21b8-513">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="f21b8-513">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f21b8-514">Вызывая `CreateDefaultBuilder`, вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="f21b8-514">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="f21b8-515">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="f21b8-515">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f21b8-516">Примените конфигурацию к <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="f21b8-516">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="f21b8-517">GetValue</span><span class="sxs-lookup"><span data-stu-id="f21b8-517">GetValue</span></span>

<span data-ttu-id="f21b8-518">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) извлекает значение из конфигурации с указанным ключом и преобразует его в указанный тип.</span><span class="sxs-lookup"><span data-stu-id="f21b8-518">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="f21b8-519">Если ключ не найден, перегрузка позволяет предоставлять значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f21b8-519">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="f21b8-520">В следующем примере извлекается строковое значение из конфигурации с ключом `NumberKey`, вводится значение `int` и сохраняется значение в переменной `intValue`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-520">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="f21b8-521">Если `NumberKey` не обнаруживается в ключах конфигурации, `intValue` получает значение `99` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f21b8-521">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="f21b8-522">GetSection, GetChildren и Exists</span><span class="sxs-lookup"><span data-stu-id="f21b8-522">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="f21b8-523">В следующих примерах рассмотрим файл JSON.</span><span class="sxs-lookup"><span data-stu-id="f21b8-523">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="f21b8-524">Четыре ключа находятся в двух разделах, один из которых содержит пару из подразделов.</span><span class="sxs-lookup"><span data-stu-id="f21b8-524">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="f21b8-525">Когда файл считывается в конфигурацию, для сохранения значений конфигурации создаются следующие уникальные иерархические ключи.</span><span class="sxs-lookup"><span data-stu-id="f21b8-525">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="f21b8-526">section0:key0</span><span class="sxs-lookup"><span data-stu-id="f21b8-526">section0:key0</span></span>
* <span data-ttu-id="f21b8-527">section0:key1</span><span class="sxs-lookup"><span data-stu-id="f21b8-527">section0:key1</span></span>
* <span data-ttu-id="f21b8-528">section1:key0</span><span class="sxs-lookup"><span data-stu-id="f21b8-528">section1:key0</span></span>
* <span data-ttu-id="f21b8-529">section1:key1</span><span class="sxs-lookup"><span data-stu-id="f21b8-529">section1:key1</span></span>
* <span data-ttu-id="f21b8-530">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="f21b8-530">section2:subsection0:key0</span></span>
* <span data-ttu-id="f21b8-531">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="f21b8-531">section2:subsection0:key1</span></span>
* <span data-ttu-id="f21b8-532">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="f21b8-532">section2:subsection1:key0</span></span>
* <span data-ttu-id="f21b8-533">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="f21b8-533">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="f21b8-534">GetSection</span><span class="sxs-lookup"><span data-stu-id="f21b8-534">GetSection</span></span>

<span data-ttu-id="f21b8-535">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) извлекает подраздел конфигурации с указанным ключом подраздела.</span><span class="sxs-lookup"><span data-stu-id="f21b8-535">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="f21b8-536">Чтобы вернуть <xref:Microsoft.Extensions.Configuration.IConfigurationSection>, содержащий только пары "ключ — значение" в `section1`, вызовите `GetSection` и укажите имя раздела.</span><span class="sxs-lookup"><span data-stu-id="f21b8-536">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="f21b8-537">Аналогично, чтобы получить значения для ключей в `section2:subsection0`, вызовите `GetSection` и укажите путь к разделу.</span><span class="sxs-lookup"><span data-stu-id="f21b8-537">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="f21b8-538">Значение `GetSection` никогда не возвращает значение `null`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-538">`GetSection` never returns `null`.</span></span> <span data-ttu-id="f21b8-539">Если соответствующий раздел не найден, возвращается пустой параметр `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-539">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

### <a name="getchildren"></a><span data-ttu-id="f21b8-540">GetChildren</span><span class="sxs-lookup"><span data-stu-id="f21b8-540">GetChildren</span></span>

<span data-ttu-id="f21b8-541">Вызов [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) в `section2` получает значение `IEnumerable<IConfigurationSection>`, которое включает:</span><span class="sxs-lookup"><span data-stu-id="f21b8-541">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="f21b8-542">Exists</span><span class="sxs-lookup"><span data-stu-id="f21b8-542">Exists</span></span>

<span data-ttu-id="f21b8-543">Используйте [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*), чтобы определить, существует ли раздел конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f21b8-543">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="f21b8-544">Учитывая данные примера, `sectionExists` является `false`, потому что в данных конфигурации нет `section2:subsection2`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-544">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="f21b8-545">Привязка к классу</span><span class="sxs-lookup"><span data-stu-id="f21b8-545">Bind to a class</span></span>

<span data-ttu-id="f21b8-546">Конфигурация может быть привязана к классам, которые представляют группы связанных параметров, используя шаблон *вариантов*.</span><span class="sxs-lookup"><span data-stu-id="f21b8-546">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="f21b8-547">Для получения дополнительной информации см. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-547">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="f21b8-548">Значения конфигурации возвращаются как строки, но вызов <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> позволяет построить объекты [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="f21b8-548">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="f21b8-549">Пример приложения содержит модель `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="f21b8-549">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f21b8-550">Раздел `starship` файла *starship.json* создает конфигурацию, когда образец приложения использует поставщик конфигурации JSON для загрузки конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f21b8-550">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="f21b8-551">Создаются следующие пары "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f21b8-551">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="f21b8-552">Ключ</span><span class="sxs-lookup"><span data-stu-id="f21b8-552">Key</span></span>                   | <span data-ttu-id="f21b8-553">Значение</span><span class="sxs-lookup"><span data-stu-id="f21b8-553">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="f21b8-554">starship:name</span><span class="sxs-lookup"><span data-stu-id="f21b8-554">starship:name</span></span>         | <span data-ttu-id="f21b8-555">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="f21b8-555">USS Enterprise</span></span>                                    |
| <span data-ttu-id="f21b8-556">starship:registry</span><span class="sxs-lookup"><span data-stu-id="f21b8-556">starship:registry</span></span>     | <span data-ttu-id="f21b8-557">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="f21b8-557">NCC-1701</span></span>                                          |
| <span data-ttu-id="f21b8-558">starship:class</span><span class="sxs-lookup"><span data-stu-id="f21b8-558">starship:class</span></span>        | <span data-ttu-id="f21b8-559">Constitution</span><span class="sxs-lookup"><span data-stu-id="f21b8-559">Constitution</span></span>                                      |
| <span data-ttu-id="f21b8-560">starship:length</span><span class="sxs-lookup"><span data-stu-id="f21b8-560">starship:length</span></span>       | <span data-ttu-id="f21b8-561">304.8</span><span class="sxs-lookup"><span data-stu-id="f21b8-561">304.8</span></span>                                             |
| <span data-ttu-id="f21b8-562">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="f21b8-562">starship:commissioned</span></span> | <span data-ttu-id="f21b8-563">False</span><span class="sxs-lookup"><span data-stu-id="f21b8-563">False</span></span>                                             |
| <span data-ttu-id="f21b8-564">trademark</span><span class="sxs-lookup"><span data-stu-id="f21b8-564">trademark</span></span>             | <span data-ttu-id="f21b8-565">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="f21b8-565">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="f21b8-566">Пример приложения вызывает `GetSection` с помощью ключа `starship`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-566">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="f21b8-567">Пары "ключ — значение" `starship` изолированы.</span><span class="sxs-lookup"><span data-stu-id="f21b8-567">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="f21b8-568">Метод `Bind` вызывается в подразделе при прохождении в экземпляр класса `Starship`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-568">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="f21b8-569">После привязки значения экземпляра экземпляру присваивается свойство для преобразования для просмотра.</span><span class="sxs-lookup"><span data-stu-id="f21b8-569">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="f21b8-570">Привязка к графу объектов</span><span class="sxs-lookup"><span data-stu-id="f21b8-570">Bind to an object graph</span></span>

<span data-ttu-id="f21b8-571"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> способна связывать весь граф объекта POCO.</span><span class="sxs-lookup"><span data-stu-id="f21b8-571"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="f21b8-572">Образец содержит модель `TvShow`, в графе объектов которого находятся классы `Metadata` и `Actors` (*Модели/TvShow.cs*).</span><span class="sxs-lookup"><span data-stu-id="f21b8-572">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f21b8-573">В примере приложения есть файл *tvshow.xml*, содержащий данные конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f21b8-573">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="f21b8-574">Конфигурация привязана ко всему ​​графу объектов `TvShow` с помощью метода `Bind`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-574">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="f21b8-575">Привязанный экземпляр присваивается свойству для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="f21b8-575">The bound instance is assigned to a property for rendering:</span></span>

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

<span data-ttu-id="f21b8-576">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) привязывает и возвращает указанный тип.</span><span class="sxs-lookup"><span data-stu-id="f21b8-576">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="f21b8-577">Метод `Get<T>` может быть более удобным, чем использование `Bind`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-577">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="f21b8-578">В следующем коде показано, как использовать `Get<T>` с предыдущим примером, который позволяет привязать связанный экземпляр непосредственно к свойству, используемому для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="f21b8-578">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="f21b8-579">Привязка массива к классу</span><span class="sxs-lookup"><span data-stu-id="f21b8-579">Bind an array to a class</span></span>

<span data-ttu-id="f21b8-580">*Пример приложения демонстрирует концепции, описанные в этом разделе.*</span><span class="sxs-lookup"><span data-stu-id="f21b8-580">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="f21b8-581"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f21b8-581">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="f21b8-582">Любой формат массива, который предоставляет сегмент числового ключа (`:0:`, `:1:`, &hellip; `:{n}:`), способен привязать массив к массиву класса POCO.</span><span class="sxs-lookup"><span data-stu-id="f21b8-582">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="f21b8-583">Привязка предоставляется соглашением.</span><span class="sxs-lookup"><span data-stu-id="f21b8-583">Binding is provided by convention.</span></span> <span data-ttu-id="f21b8-584">Пользовательские поставщики конфигурации не обязаны реализовывать привязку массива.</span><span class="sxs-lookup"><span data-stu-id="f21b8-584">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="f21b8-585">**Обработка массива в оперативной памяти**</span><span class="sxs-lookup"><span data-stu-id="f21b8-585">**In-memory array processing**</span></span>

<span data-ttu-id="f21b8-586">Рассмотрите ключи и значения конфигурации, приведенные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="f21b8-586">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="f21b8-587">Ключ</span><span class="sxs-lookup"><span data-stu-id="f21b8-587">Key</span></span>     | <span data-ttu-id="f21b8-588">Значение</span><span class="sxs-lookup"><span data-stu-id="f21b8-588">Value</span></span>  |
| :-----: | :----: |
| <span data-ttu-id="f21b8-589">array:0</span><span class="sxs-lookup"><span data-stu-id="f21b8-589">array:0</span></span> | <span data-ttu-id="f21b8-590">value0</span><span class="sxs-lookup"><span data-stu-id="f21b8-590">value0</span></span> |
| <span data-ttu-id="f21b8-591">array:1</span><span class="sxs-lookup"><span data-stu-id="f21b8-591">array:1</span></span> | <span data-ttu-id="f21b8-592">value1</span><span class="sxs-lookup"><span data-stu-id="f21b8-592">value1</span></span> |
| <span data-ttu-id="f21b8-593">array:2</span><span class="sxs-lookup"><span data-stu-id="f21b8-593">array:2</span></span> | <span data-ttu-id="f21b8-594">value2</span><span class="sxs-lookup"><span data-stu-id="f21b8-594">value2</span></span> |
| <span data-ttu-id="f21b8-595">array:4</span><span class="sxs-lookup"><span data-stu-id="f21b8-595">array:4</span></span> | <span data-ttu-id="f21b8-596">value4</span><span class="sxs-lookup"><span data-stu-id="f21b8-596">value4</span></span> |
| <span data-ttu-id="f21b8-597">array:5</span><span class="sxs-lookup"><span data-stu-id="f21b8-597">array:5</span></span> | <span data-ttu-id="f21b8-598">value5</span><span class="sxs-lookup"><span data-stu-id="f21b8-598">value5</span></span> |

<span data-ttu-id="f21b8-599">Эти ключи и значения загружаются в пример приложения с помощью поставщика конфигурации памяти.</span><span class="sxs-lookup"><span data-stu-id="f21b8-599">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="f21b8-600">Массив пропускает значения для индекса &num;3.</span><span class="sxs-lookup"><span data-stu-id="f21b8-600">The array skips a value for index &num;3.</span></span> <span data-ttu-id="f21b8-601">Связующее свойство конфигурации не способно связывать нулевые значения или создавать нулевые записи в связанных объектах, что становится ясным в тот момент, когда демонстрируется результат привязки этого массива к объекту.</span><span class="sxs-lookup"><span data-stu-id="f21b8-601">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="f21b8-602">В примере приложения класс POCO доступен для хранения привязанных данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f21b8-602">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f21b8-603">Данные конфигурации, связанные с объектом.</span><span class="sxs-lookup"><span data-stu-id="f21b8-603">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="f21b8-604">Синтаксис [ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) также может использоваться для получения более компактного кода.</span><span class="sxs-lookup"><span data-stu-id="f21b8-604">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="f21b8-605">Связанный объект, экземпляр `ArrayExample`, получает данные массива из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f21b8-605">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="f21b8-606">Индекс `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="f21b8-606">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="f21b8-607">Значение `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="f21b8-607">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="f21b8-608">0</span><span class="sxs-lookup"><span data-stu-id="f21b8-608">0</span></span>                             | <span data-ttu-id="f21b8-609">value0</span><span class="sxs-lookup"><span data-stu-id="f21b8-609">value0</span></span>                        |
| <span data-ttu-id="f21b8-610">1</span><span class="sxs-lookup"><span data-stu-id="f21b8-610">1</span></span>                             | <span data-ttu-id="f21b8-611">value1</span><span class="sxs-lookup"><span data-stu-id="f21b8-611">value1</span></span>                        |
| <span data-ttu-id="f21b8-612">2</span><span class="sxs-lookup"><span data-stu-id="f21b8-612">2</span></span>                             | <span data-ttu-id="f21b8-613">value2</span><span class="sxs-lookup"><span data-stu-id="f21b8-613">value2</span></span>                        |
| <span data-ttu-id="f21b8-614">3</span><span class="sxs-lookup"><span data-stu-id="f21b8-614">3</span></span>                             | <span data-ttu-id="f21b8-615">value4</span><span class="sxs-lookup"><span data-stu-id="f21b8-615">value4</span></span>                        |
| <span data-ttu-id="f21b8-616">4</span><span class="sxs-lookup"><span data-stu-id="f21b8-616">4</span></span>                             | <span data-ttu-id="f21b8-617">value5</span><span class="sxs-lookup"><span data-stu-id="f21b8-617">value5</span></span>                        |

<span data-ttu-id="f21b8-618">Индекс &num;3 в связанном объекте содержит данные конфигурации для конфигурационного ключа `array:4` и его значения `value4`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-618">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="f21b8-619">При привязке данных конфигурации, содержащих массив индексов, в ключах конфигурации эти индексы просто используются для выполнения итерации данных конфигурации при создании объекта.</span><span class="sxs-lookup"><span data-stu-id="f21b8-619">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="f21b8-620">Когда массив ключей конфигурации пропускает один или несколько индексов, в данных конфигурации не может быть сохранено нулевое значение и в связанном объекте не создается нулевая запись.</span><span class="sxs-lookup"><span data-stu-id="f21b8-620">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="f21b8-621">Отсутствующий элемент конфигурации для индекса &num;3 может быть предоставлен перед привязкой к экземпляру `ArrayExamples` любым поставщиком конфигурации, который создает правильную пару "ключ — значение" в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f21b8-621">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExamples` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="f21b8-622">Если в образец включен дополнительный поставщик конфигурации JSON с отсутствующей парой "ключ — значение", то `ArrayExamples.Entries` подойдет полному массиву конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f21b8-622">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExamples.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="f21b8-623">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="f21b8-623">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f21b8-624">В <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="f21b8-624">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f21b8-625">В конструкторе `Startup`:</span><span class="sxs-lookup"><span data-stu-id="f21b8-625">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="f21b8-626">Пары "ключ — значение", показанные в таблице, загружаются в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f21b8-626">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="f21b8-627">Ключ</span><span class="sxs-lookup"><span data-stu-id="f21b8-627">Key</span></span>             | <span data-ttu-id="f21b8-628">Значение</span><span class="sxs-lookup"><span data-stu-id="f21b8-628">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="f21b8-629">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="f21b8-629">array:entries:3</span></span> | <span data-ttu-id="f21b8-630">value3</span><span class="sxs-lookup"><span data-stu-id="f21b8-630">value3</span></span> |

<span data-ttu-id="f21b8-631">Если экземпляр класса `ArrayExamples` связан после того, как поставщик конфигурации JSON включает запись для индекса &num;3, то массив `ArrayExamples.Entries` включит значение.</span><span class="sxs-lookup"><span data-stu-id="f21b8-631">If the `ArrayExamples` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExamples.Entries` array includes the value.</span></span>

| <span data-ttu-id="f21b8-632">Индекс `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="f21b8-632">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="f21b8-633">Значение `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="f21b8-633">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="f21b8-634">0</span><span class="sxs-lookup"><span data-stu-id="f21b8-634">0</span></span>                             | <span data-ttu-id="f21b8-635">value0</span><span class="sxs-lookup"><span data-stu-id="f21b8-635">value0</span></span>                        |
| <span data-ttu-id="f21b8-636">1</span><span class="sxs-lookup"><span data-stu-id="f21b8-636">1</span></span>                             | <span data-ttu-id="f21b8-637">value1</span><span class="sxs-lookup"><span data-stu-id="f21b8-637">value1</span></span>                        |
| <span data-ttu-id="f21b8-638">2</span><span class="sxs-lookup"><span data-stu-id="f21b8-638">2</span></span>                             | <span data-ttu-id="f21b8-639">value2</span><span class="sxs-lookup"><span data-stu-id="f21b8-639">value2</span></span>                        |
| <span data-ttu-id="f21b8-640">3</span><span class="sxs-lookup"><span data-stu-id="f21b8-640">3</span></span>                             | <span data-ttu-id="f21b8-641">value3</span><span class="sxs-lookup"><span data-stu-id="f21b8-641">value3</span></span>                        |
| <span data-ttu-id="f21b8-642">4</span><span class="sxs-lookup"><span data-stu-id="f21b8-642">4</span></span>                             | <span data-ttu-id="f21b8-643">value4</span><span class="sxs-lookup"><span data-stu-id="f21b8-643">value4</span></span>                        |
| <span data-ttu-id="f21b8-644">5</span><span class="sxs-lookup"><span data-stu-id="f21b8-644">5</span></span>                             | <span data-ttu-id="f21b8-645">value5</span><span class="sxs-lookup"><span data-stu-id="f21b8-645">value5</span></span>                        |

<span data-ttu-id="f21b8-646">**Обработка массива JSON**</span><span class="sxs-lookup"><span data-stu-id="f21b8-646">**JSON array processing**</span></span>

<span data-ttu-id="f21b8-647">Если JSON-файл содержит массив, ключи конфигурации будут созданы для элементов массива с индексом раздела, начиная с нуля.</span><span class="sxs-lookup"><span data-stu-id="f21b8-647">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="f21b8-648">В следующем файле конфигурации `subsection` — это массив.</span><span class="sxs-lookup"><span data-stu-id="f21b8-648">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="f21b8-649">Поставщик конфигурации JSON считывает данные конфигурации в следующие пары "ключ — значение".</span><span class="sxs-lookup"><span data-stu-id="f21b8-649">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="f21b8-650">Ключ</span><span class="sxs-lookup"><span data-stu-id="f21b8-650">Key</span></span>                     | <span data-ttu-id="f21b8-651">Значение</span><span class="sxs-lookup"><span data-stu-id="f21b8-651">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="f21b8-652">json_array:key</span><span class="sxs-lookup"><span data-stu-id="f21b8-652">json_array:key</span></span>          | <span data-ttu-id="f21b8-653">valueA</span><span class="sxs-lookup"><span data-stu-id="f21b8-653">valueA</span></span> |
| <span data-ttu-id="f21b8-654">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="f21b8-654">json_array:subsection:0</span></span> | <span data-ttu-id="f21b8-655">valueB</span><span class="sxs-lookup"><span data-stu-id="f21b8-655">valueB</span></span> |
| <span data-ttu-id="f21b8-656">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="f21b8-656">json_array:subsection:1</span></span> | <span data-ttu-id="f21b8-657">valueC</span><span class="sxs-lookup"><span data-stu-id="f21b8-657">valueC</span></span> |
| <span data-ttu-id="f21b8-658">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="f21b8-658">json_array:subsection:2</span></span> | <span data-ttu-id="f21b8-659">valueD</span><span class="sxs-lookup"><span data-stu-id="f21b8-659">valueD</span></span> |

<span data-ttu-id="f21b8-660">В примере приложения для привязки пар "ключ — значение" конфигурации доступен следующий класс POCO.</span><span class="sxs-lookup"><span data-stu-id="f21b8-660">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f21b8-661">После выполнения привязки `JsonArrayExample.Key` содержит значение `valueA`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-661">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="f21b8-662">Подраздел значения хранится в свойстве массива POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-662">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="f21b8-663">Индекс `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="f21b8-663">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="f21b8-664">Значение `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="f21b8-664">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="f21b8-665">0</span><span class="sxs-lookup"><span data-stu-id="f21b8-665">0</span></span>                                   | <span data-ttu-id="f21b8-666">valueB</span><span class="sxs-lookup"><span data-stu-id="f21b8-666">valueB</span></span>                              |
| <span data-ttu-id="f21b8-667">1</span><span class="sxs-lookup"><span data-stu-id="f21b8-667">1</span></span>                                   | <span data-ttu-id="f21b8-668">valueC</span><span class="sxs-lookup"><span data-stu-id="f21b8-668">valueC</span></span>                              |
| <span data-ttu-id="f21b8-669">2</span><span class="sxs-lookup"><span data-stu-id="f21b8-669">2</span></span>                                   | <span data-ttu-id="f21b8-670">valueD</span><span class="sxs-lookup"><span data-stu-id="f21b8-670">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="f21b8-671">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="f21b8-671">Custom configuration provider</span></span>

<span data-ttu-id="f21b8-672">Пример приложения демонстрирует, как создать базовый поставщик конфигурации, который считывает пары "ключ — значение" конфигурации из базы данных, используя [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="f21b8-672">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="f21b8-673">Поставщик имеет следующие характеристики.</span><span class="sxs-lookup"><span data-stu-id="f21b8-673">The provider has the following characteristics:</span></span>

* <span data-ttu-id="f21b8-674">База данных в памяти EF используется для демонстрационных целей.</span><span class="sxs-lookup"><span data-stu-id="f21b8-674">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="f21b8-675">Чтобы использовать базу данных, для которой требуется строка подключения, выполните вторичный `ConfigurationBuilder`, чтобы предоставить строку подключения от другого поставщика конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f21b8-675">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="f21b8-676">Поставщик считывает таблицу базы данных в конфигурации при запуске.</span><span class="sxs-lookup"><span data-stu-id="f21b8-676">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="f21b8-677">Поставщик не запрашивает базу данных для каждого ключа.</span><span class="sxs-lookup"><span data-stu-id="f21b8-677">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="f21b8-678">Функция перезагрузки на изменение не реализована, поэтому обновление базы данных после запуска приложения не влияет на конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="f21b8-678">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="f21b8-679">Определите сущность `EFConfigurationValue` для хранения значений конфигурации в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f21b8-679">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="f21b8-680">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="f21b8-680">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f21b8-681">Добавьте `EFConfigurationContext` в хранилище и обратитесь к настроенным значениям.</span><span class="sxs-lookup"><span data-stu-id="f21b8-681">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="f21b8-682">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="f21b8-682">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f21b8-683">Создайте класс, реализующий <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-683">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="f21b8-684">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="f21b8-684">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f21b8-685">Создайте пользовательский поставщик конфигурации путем наследования от <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-685">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="f21b8-686">Поставщик конфигурации инициализирует пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="f21b8-686">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="f21b8-687">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="f21b8-687">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f21b8-688">Метод расширения `AddEFConfiguration` позволяет добавить источник конфигурации к `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-688">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="f21b8-689">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="f21b8-689">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f21b8-690">В следующем коде показано, как использовать пользовательский `EFConfigurationProvider` в *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="f21b8-690">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="f21b8-691">Доступ к конфигурации во время запуска</span><span class="sxs-lookup"><span data-stu-id="f21b8-691">Access configuration during startup</span></span>

<span data-ttu-id="f21b8-692">Внесите значение `IConfiguration` в конструктор `Startup` для доступа к значениям конфигурации в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-692">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="f21b8-693">Чтобы получить доступ к конфигурации в `Startup.Configure`, либо добавьте значение `IConfiguration` непосредственно в метод, либо используйте экземпляр из конструктора.</span><span class="sxs-lookup"><span data-stu-id="f21b8-693">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="f21b8-694">Пример доступа к конфигурации с использованием удобных методов запуска см. в разделе [Запуск приложения. Удобные методы](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="f21b8-694">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="f21b8-695">Настройте доступ на странице Razor Pages или в представлении MVC</span><span class="sxs-lookup"><span data-stu-id="f21b8-695">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="f21b8-696">Для доступа к параметрам конфигурации на странице Razor Pages или в представлении MVC добавьте [использование директивы](xref:mvc/views/razor#using) ([Справочник по C#: использование директивы](/dotnet/csharp/language-reference/keywords/using-directive)) для [пространства имен Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) и вставьте <xref:Microsoft.Extensions.Configuration.IConfiguration> на страницу или в представление.</span><span class="sxs-lookup"><span data-stu-id="f21b8-696">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="f21b8-697">На странице Razor Pages</span><span class="sxs-lookup"><span data-stu-id="f21b8-697">In a Razor Pages page:</span></span>

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

<span data-ttu-id="f21b8-698">В представлении MVC</span><span class="sxs-lookup"><span data-stu-id="f21b8-698">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="f21b8-699">Добавление конфигурации из внешней сборки</span><span class="sxs-lookup"><span data-stu-id="f21b8-699">Add configuration from an external assembly</span></span>

<span data-ttu-id="f21b8-700">Реализация <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> позволяет при запуске добавлять в приложение улучшения из внешней сборки вне приложения класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="f21b8-700">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="f21b8-701">Для получения дополнительной информации см. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="f21b8-701">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f21b8-702">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f21b8-702">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* <span data-ttu-id="f21b8-703">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/) (Подробные сведения о конфигурации Microsoft)</span><span class="sxs-lookup"><span data-stu-id="f21b8-703">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)</span></span>
