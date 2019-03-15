---
title: Конфигурация в .NET Core
author: guardrex
description: "Узнайте, как использовать API конфигурации для настройки приложения ASP.NET\_Core."
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/11/2019
uid: fundamentals/configuration/index
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="6ae71-103">Конфигурация в .NET Core</span><span class="sxs-lookup"><span data-stu-id="6ae71-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="6ae71-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="6ae71-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6ae71-105">Конфигурация приложения в ASP.NET Core основана на парах "ключ — значение", установленных *поставщиками конфигурации*.</span><span class="sxs-lookup"><span data-stu-id="6ae71-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="6ae71-106">Поставщики конфигурации получают данные конфигурации в парах "ключ — значение" из различных источников:</span><span class="sxs-lookup"><span data-stu-id="6ae71-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="6ae71-107">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="6ae71-107">Azure Key Vault</span></span>
* <span data-ttu-id="6ae71-108">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="6ae71-108">Command-line arguments</span></span>
* <span data-ttu-id="6ae71-109">а также пользовательские поставщики (устанавливаемые или создаваемые).</span><span class="sxs-lookup"><span data-stu-id="6ae71-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="6ae71-110">Справочные файлы</span><span class="sxs-lookup"><span data-stu-id="6ae71-110">Directory files</span></span>
* <span data-ttu-id="6ae71-111">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="6ae71-111">Environment variables</span></span>
* <span data-ttu-id="6ae71-112">объектов .NET в памяти;</span><span class="sxs-lookup"><span data-stu-id="6ae71-112">In-memory .NET objects</span></span>
* <span data-ttu-id="6ae71-113">файлов параметров;</span><span class="sxs-lookup"><span data-stu-id="6ae71-113">Settings files</span></span>

<span data-ttu-id="6ae71-114">*Шаблон параметров* является расширением конфигурации основных понятий, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="6ae71-114">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="6ae71-115">Параметры используют классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="6ae71-115">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="6ae71-116">Дополнительные сведения об использовании шаблона параметров см. в разделе <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-116">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="6ae71-117">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6ae71-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6ae71-118">Эти три пакета включены в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6ae71-118">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="host-vs-app-configuration"></a><span data-ttu-id="6ae71-119">Конфигурация узла и приложения</span><span class="sxs-lookup"><span data-stu-id="6ae71-119">Host vs. app configuration</span></span>

<span data-ttu-id="6ae71-120">Перед настройкой и запуском приложения настройте и запустите *узел*.</span><span class="sxs-lookup"><span data-stu-id="6ae71-120">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="6ae71-121">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="6ae71-121">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="6ae71-122">Как приложение, так и узел настраиваются с использованием поставщиков конфигурации, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="6ae71-122">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="6ae71-123">Пары "ключ — значение" конфигурации узлов становятся частью глобальной конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="6ae71-123">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="6ae71-124">Дополнительные сведения о том, как используются поставщики конфигурации при создании узла и как источники конфигурации влияют на его настройку, см. в разделе [Узел](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="6ae71-124">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see [The host](xref:fundamentals/index#host).</span></span>

## <a name="default-configuration"></a><span data-ttu-id="6ae71-125">Конфигурация по умолчанию</span><span class="sxs-lookup"><span data-stu-id="6ae71-125">Default configuration</span></span>

<span data-ttu-id="6ae71-126">Веб-приложения на основе шаблонов [dotnet new](/dotnet/core/tools/dotnet-new) ASP.NET Core вызывают <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> при создании узла.</span><span class="sxs-lookup"><span data-stu-id="6ae71-126">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="6ae71-127"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> предоставляет конфигурацию по умолчанию для приложения в следующем порядке:</span><span class="sxs-lookup"><span data-stu-id="6ae71-127"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> provides default configuration for the app in the following order:</span></span>

* <span data-ttu-id="6ae71-128">Существуют следующие способы предоставления конфигурации узла.</span><span class="sxs-lookup"><span data-stu-id="6ae71-128">Host configuration is provided from:</span></span>
  * <span data-ttu-id="6ae71-129">Переменные среды с префиксом `ASPNETCORE_` (например, `ASPNETCORE_ENVIRONMENT`), использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="6ae71-129">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="6ae71-130">Префикс (`ASPNETCORE_`) исключается при загрузке пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-130">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="6ae71-131">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="6ae71-131">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="6ae71-132">Конфигурация приложения предоставляется из следующих ресурсов:</span><span class="sxs-lookup"><span data-stu-id="6ae71-132">App configuration is provided from:</span></span>
  * <span data-ttu-id="6ae71-133">Файл *appsettings.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="6ae71-133">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="6ae71-134">Файл *appsettings.{Environment}.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="6ae71-134">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="6ae71-135">[Менеджера секретов](xref:security/app-secrets), когда приложение выполняется в среде `Development` с использованием начальных сборок.</span><span class="sxs-lookup"><span data-stu-id="6ae71-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="6ae71-136">Переменные среды, использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="6ae71-136">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="6ae71-137">Если используется пользовательский префикс (например, `PREFIX_` с `.AddEnvironmentVariables(prefix: "PREFIX_")`), такой префикс исключается при загрузке пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-137">If a custom prefix is used (for example, `PREFIX_` with `.AddEnvironmentVariables(prefix: "PREFIX_")`), the prefix is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="6ae71-138">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="6ae71-138">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="6ae71-139">Поставщики конфигурации описаны ниже в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="6ae71-139">The configuration providers are explained later in this topic.</span></span> <span data-ttu-id="6ae71-140">Дополнительные сведения об узле и <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>: <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-140">For more information on the host and <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="security"></a><span data-ttu-id="6ae71-141">Безопасность</span><span class="sxs-lookup"><span data-stu-id="6ae71-141">Security</span></span>

<span data-ttu-id="6ae71-142">Придерживайтесь следующих рекомендаций.</span><span class="sxs-lookup"><span data-stu-id="6ae71-142">Adopt the following best practices:</span></span>

* <span data-ttu-id="6ae71-143">Никогда не храните пароли или другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации обычного текста.</span><span class="sxs-lookup"><span data-stu-id="6ae71-143">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="6ae71-144">Не используйте секреты рабочей среды в средах разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="6ae71-144">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="6ae71-145">Указывайте секреты вне проекта, чтобы их нельзя было случайно зафиксировать в репозитории с исходным кодом.</span><span class="sxs-lookup"><span data-stu-id="6ae71-145">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="6ae71-146">Дополнительные сведения см. в статье [Использование нескольких сред в ASP.NET Core](xref:fundamentals/environments) и руководствуйтесь статьей [Безопасное хранение секретов приложения во время разработки в ASP.NET Core](xref:security/app-secrets) (включает рекомендации по использованию переменной среды для хранения конфиденциальных данных).</span><span class="sxs-lookup"><span data-stu-id="6ae71-146">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="6ae71-147">Менеджер секретов использует поставщик конфигурации файла для хранения конфиденциальных данных пользователя в файле JSON в локальной системе.</span><span class="sxs-lookup"><span data-stu-id="6ae71-147">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="6ae71-148">Поставщик конфигурации файлов описан ниже в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="6ae71-148">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="6ae71-149">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) — один из вариантов для безопасного хранения секретов приложения.</span><span class="sxs-lookup"><span data-stu-id="6ae71-149">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="6ae71-150">Для получения дополнительной информации см. <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-150">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="6ae71-151">Иерархическая модель конфигурации</span><span class="sxs-lookup"><span data-stu-id="6ae71-151">Hierarchical configuration data</span></span>

<span data-ttu-id="6ae71-152">API конфигурации способен поддерживать иерархические данные конфигурации, выполняя преобразование в плоскую структуру иерархических данных с использованием разделителя в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-152">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="6ae71-153">В следующем файле JSON существуют четыре ключа в структурированной иерархии двух разделов.</span><span class="sxs-lookup"><span data-stu-id="6ae71-153">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="6ae71-154">При считывании файла в конфигурацию для сохранения исходной иерархической структуры данных источника конфигурации создаются уникальные ключи.</span><span class="sxs-lookup"><span data-stu-id="6ae71-154">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="6ae71-155">Разделы и ключи преобразовываются в плоскую структуру с использованием двоеточия (`:`) для сохранения исходной структуры.</span><span class="sxs-lookup"><span data-stu-id="6ae71-155">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="6ae71-156">section0:key0</span><span class="sxs-lookup"><span data-stu-id="6ae71-156">section0:key0</span></span>
* <span data-ttu-id="6ae71-157">section0:key1</span><span class="sxs-lookup"><span data-stu-id="6ae71-157">section0:key1</span></span>
* <span data-ttu-id="6ae71-158">section1:key0</span><span class="sxs-lookup"><span data-stu-id="6ae71-158">section1:key0</span></span>
* <span data-ttu-id="6ae71-159">section1:key1</span><span class="sxs-lookup"><span data-stu-id="6ae71-159">section1:key1</span></span>

<span data-ttu-id="6ae71-160">Методы <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> и <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> доступны для изолирования разделов и дочерних элементов раздела в данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-160"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="6ae71-161">Эти методы описаны далее в разделе [GetSection, GetChildren и Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="6ae71-161">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span> <span data-ttu-id="6ae71-162">`GetSection` находится в пакете [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6ae71-162">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="conventions"></a><span data-ttu-id="6ae71-163">Соглашения</span><span class="sxs-lookup"><span data-stu-id="6ae71-163">Conventions</span></span>

<span data-ttu-id="6ae71-164">При запуске приложения источники конфигурации считываются в порядке, в котором были указаны их поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-164">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="6ae71-165">Поставщики файлов конфигурации имеют возможность перезагрузить конфигурацию при изменении базового файла параметров после запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="6ae71-165">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="6ae71-166">Поставщик конфигурации файлов описан ниже в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="6ae71-166">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="6ae71-167">Объект <xref:Microsoft.Extensions.Configuration.IConfiguration> доступен в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) приложения.</span><span class="sxs-lookup"><span data-stu-id="6ae71-167"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="6ae71-168"><xref:Microsoft.Extensions.Configuration.IConfiguration> можно внедрить в <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> Razor Pages, чтобы получить конфигурацию для класса:</span><span class="sxs-lookup"><span data-stu-id="6ae71-168"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

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

<span data-ttu-id="6ae71-169">Поставщики конфигурации не могут использовать контейнер DI, так как он недоступен при настройке узла.</span><span class="sxs-lookup"><span data-stu-id="6ae71-169">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="6ae71-170">В ключах конфигурации приняты следующие соглашения.</span><span class="sxs-lookup"><span data-stu-id="6ae71-170">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="6ae71-171">В ключах не учитывается регистр символов.</span><span class="sxs-lookup"><span data-stu-id="6ae71-171">Keys are case-insensitive.</span></span> <span data-ttu-id="6ae71-172">Например `ConnectionString` и `connectionstring` обрабатываются как эквивалентные ключи.</span><span class="sxs-lookup"><span data-stu-id="6ae71-172">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="6ae71-173">Если значение для одного и того же ключа установлено одним и тем же или разными поставщиками конфигурации, последним значением, установленным на ключе, является используемое значение.</span><span class="sxs-lookup"><span data-stu-id="6ae71-173">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="6ae71-174">Иерархические ключи</span><span class="sxs-lookup"><span data-stu-id="6ae71-174">Hierarchical keys</span></span>
  * <span data-ttu-id="6ae71-175">При взаимодействии с API конфигурации разделитель-двоеточие (`:`) поддерживается на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="6ae71-175">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="6ae71-176">В переменных среды разделитель-двоеточие может не работать на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="6ae71-176">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="6ae71-177">Двойной знак подчеркивания (`__`) поддерживается на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="6ae71-177">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="6ae71-178">В хранилище ключей Azure иерархические ключи используют `--` (два дефиса) в качестве разделителя.</span><span class="sxs-lookup"><span data-stu-id="6ae71-178">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="6ae71-179">Чтобы заменить дефисы двоеточием, при загрузке секретов в конфигурацию приложения необходимо указать код.</span><span class="sxs-lookup"><span data-stu-id="6ae71-179">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="6ae71-180"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-180">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="6ae71-181">Привязка массива описана в разделе [Привязка массива к классу](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="6ae71-181">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="6ae71-182">В значениях конфигурации учитываются следующие соглашения.</span><span class="sxs-lookup"><span data-stu-id="6ae71-182">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="6ae71-183">Значения являются строками.</span><span class="sxs-lookup"><span data-stu-id="6ae71-183">Values are strings.</span></span>
* <span data-ttu-id="6ae71-184">Значение NULL не может храниться в конфигурации или быть привязанным к объектам.</span><span class="sxs-lookup"><span data-stu-id="6ae71-184">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="6ae71-185">Поставщики</span><span class="sxs-lookup"><span data-stu-id="6ae71-185">Providers</span></span>

<span data-ttu-id="6ae71-186">В следующей таблице показаны поставщики конфигурации, доступные для приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6ae71-186">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="6ae71-187">Поставщик</span><span class="sxs-lookup"><span data-stu-id="6ae71-187">Provider</span></span> | <span data-ttu-id="6ae71-188">Предоставляет конфигурацию из &hellip;</span><span class="sxs-lookup"><span data-stu-id="6ae71-188">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="6ae71-189">[Поставщик конфигурации хранилища ключей Azure](xref:security/key-vault-configuration) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="6ae71-189">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="6ae71-190">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="6ae71-190">Azure Key Vault</span></span> |
| [<span data-ttu-id="6ae71-191">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="6ae71-191">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="6ae71-192">Параметры командной строки</span><span class="sxs-lookup"><span data-stu-id="6ae71-192">Command-line parameters</span></span> |
| [<span data-ttu-id="6ae71-193">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="6ae71-193">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="6ae71-194">Источник пользователя</span><span class="sxs-lookup"><span data-stu-id="6ae71-194">Custom source</span></span> |
| [<span data-ttu-id="6ae71-195">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="6ae71-195">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="6ae71-196">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="6ae71-196">Environment variables</span></span> |
| [<span data-ttu-id="6ae71-197">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="6ae71-197">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="6ae71-198">Файлы (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="6ae71-198">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="6ae71-199">Поставщик конфигурации ключа для каждого файла</span><span class="sxs-lookup"><span data-stu-id="6ae71-199">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="6ae71-200">Справочные файлы</span><span class="sxs-lookup"><span data-stu-id="6ae71-200">Directory files</span></span> |
| [<span data-ttu-id="6ae71-201">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="6ae71-201">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="6ae71-202">Коллекции оперативной памяти</span><span class="sxs-lookup"><span data-stu-id="6ae71-202">In-memory collections</span></span> |
| <span data-ttu-id="6ae71-203">[Секреты пользователей (Менеджер секретов)](xref:security/app-secrets) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="6ae71-203">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="6ae71-204">Файл в каталоге профиля пользователя</span><span class="sxs-lookup"><span data-stu-id="6ae71-204">File in the user profile directory</span></span> |

<span data-ttu-id="6ae71-205">Источники конфигурации считываются в том порядке, в котором при запуске указываются их поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-205">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="6ae71-206">Поставщики конфигурации в этом разделе описаны в алфавитном порядке, а не в необходимом вам порядке в коде.</span><span class="sxs-lookup"><span data-stu-id="6ae71-206">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="6ae71-207">Порядок поставщиков конфигурации в коде соответствует приоритетам ваших основных источников конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-207">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="6ae71-208">Типичная последовательность поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-208">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="6ae71-209">Файлы (*appsettings.json*, *appsettings.{Environment}.json*, где `{Environment}` — это текущая среда размещения приложения)</span><span class="sxs-lookup"><span data-stu-id="6ae71-209">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. <span data-ttu-id="6ae71-210">[Azure Key Vault](xref:security/key-vault-configuration);</span><span class="sxs-lookup"><span data-stu-id="6ae71-210">[Azure Key Vault](xref:security/key-vault-configuration)</span></span>
1. <span data-ttu-id="6ae71-211">[Секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки)</span><span class="sxs-lookup"><span data-stu-id="6ae71-211">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="6ae71-212">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="6ae71-212">Environment variables</span></span>
1. <span data-ttu-id="6ae71-213">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="6ae71-213">Command-line arguments</span></span>

<span data-ttu-id="6ae71-214">Общепринятой практикой является размещение поставщика конфигурации командной строки последним в ряду поставщиков, чтобы аргументы командной строки могли переопределять конфигурацию, установленную другими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="6ae71-214">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="6ae71-215">Эта последовательность поставщиков помещается в месте, где происходит инициализация нового класса <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с помощью метода <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-215">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="6ae71-216">Дополнительные сведения см. в руководстве по [настройке веб-узла](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="6ae71-216">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

## <a name="configureappconfiguration"></a><span data-ttu-id="6ae71-217">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="6ae71-217">ConfigureAppConfiguration</span></span>

<span data-ttu-id="6ae71-218">Вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> при сборке узла, чтобы указать поставщики конфигурации приложения в дополнение к тем, которые автоматически добавлены <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-218">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

<span data-ttu-id="6ae71-219">Конфигурация, предоставленная приложению в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, доступна во время запуска приложения, включая `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-219">Configuration supplied to the app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="6ae71-220">Дополнительные сведения см. в разделе [Доступ к конфигурации во время запуска](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="6ae71-220">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="6ae71-221">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="6ae71-221">Command-line Configuration Provider</span></span>

<span data-ttu-id="6ae71-222"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> загружает конфигурацию из пары "ключ — значение" аргумента командной строки в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="6ae71-222">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="6ae71-223">Чтобы активировать конфигурацию командной строки, вызывается метод расширения <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> для экземпляра <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-223">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="6ae71-224">`AddCommandLine` автоматически вызывается при инициализации нового <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-224">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="6ae71-225">Дополнительные сведения см. в руководстве по [настройке веб-узла](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="6ae71-225">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="6ae71-226">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="6ae71-226">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="6ae71-227">дополнительную конфигурацию из *appsettings.json* и *appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="6ae71-227">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="6ae71-228">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки);</span><span class="sxs-lookup"><span data-stu-id="6ae71-228">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="6ae71-229">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="6ae71-229">Environment variables.</span></span>

<span data-ttu-id="6ae71-230">`CreateDefaultBuilder` добавляет последним поставщика конфигурации командной строки.</span><span class="sxs-lookup"><span data-stu-id="6ae71-230">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="6ae71-231">Аргументы командной строки передаются в набор конфигурации переопределения среды выполнения, установленной другими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="6ae71-231">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="6ae71-232">`CreateDefaultBuilder` действует, когда создается узел.</span><span class="sxs-lookup"><span data-stu-id="6ae71-232">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="6ae71-233">Поэтому конфигурация командной строки, активированная с помощью `CreateDefaultBuilder`, может повлиять на настройку узла.</span><span class="sxs-lookup"><span data-stu-id="6ae71-233">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="6ae71-234">Чтобы указать конфигурацию приложения, при сборке узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-234">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="6ae71-235">Объект `AddCommandLine` уже был вызван методом `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-235">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="6ae71-236">Если нужно предоставить конфигурацию приложения, сохранив возможность переопределить ее через аргументы командной строки, вызовите в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>дополнительные поставщики приложения, в завершение вызвав `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-236">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="6ae71-237">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="6ae71-237">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="6ae71-238">**Пример**</span><span class="sxs-lookup"><span data-stu-id="6ae71-238">**Example**</span></span>

<span data-ttu-id="6ae71-239">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-239">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="6ae71-240">Откройте командную строку в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="6ae71-240">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="6ae71-241">Поставьте аргумент командной строки в команду `dotnet run`: `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-241">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="6ae71-242">После запуска приложения откройте браузер в приложении по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-242">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="6ae71-243">Обратите внимание, что вывод содержит пару "ключ — значение" для аргумента командной строки конфигурации, предоставленного для `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-243">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="6ae71-244">Аргументы</span><span class="sxs-lookup"><span data-stu-id="6ae71-244">Arguments</span></span>

<span data-ttu-id="6ae71-245">Значение должно соответствовать знаку равенства (`=`), или ключ должен иметь префикс (`--` или `/`), когда значение следует за пробелом.</span><span class="sxs-lookup"><span data-stu-id="6ae71-245">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="6ae71-246">Значение может соответствовать NULL, если используется знак равенства (например, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="6ae71-246">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="6ae71-247">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="6ae71-247">Key prefix</span></span>               | <span data-ttu-id="6ae71-248">Пример</span><span class="sxs-lookup"><span data-stu-id="6ae71-248">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="6ae71-249">Без префикса</span><span class="sxs-lookup"><span data-stu-id="6ae71-249">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="6ae71-250">Два дефиса (`--`)</span><span class="sxs-lookup"><span data-stu-id="6ae71-250">Two dashes (`--`)</span></span>        | <span data-ttu-id="6ae71-251">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="6ae71-251">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="6ae71-252">Прямая косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="6ae71-252">Forward slash (`/`)</span></span>      | <span data-ttu-id="6ae71-253">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="6ae71-253">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="6ae71-254">В рамках одной и той же команды не смешивайте пары "ключ — значение" аргумента командной строки, которые используют знак равенства, с парами "ключ — значение", которые используют пробел.</span><span class="sxs-lookup"><span data-stu-id="6ae71-254">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="6ae71-255">Примеры команд.</span><span class="sxs-lookup"><span data-stu-id="6ae71-255">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="6ae71-256">Сопоставления переключений</span><span class="sxs-lookup"><span data-stu-id="6ae71-256">Switch mappings</span></span>

<span data-ttu-id="6ae71-257">Сопоставление параметров позволяет указать логику замены имен ключей.</span><span class="sxs-lookup"><span data-stu-id="6ae71-257">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="6ae71-258">Когда вручную создается конфигурация с помощью <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, методу <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> можно предоставить словарь сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="6ae71-258">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="6ae71-259">В словаре сопоставлений переключений выполняется поиск ключа, который совпадает с ключом, предоставляемым аргументом командной строки.</span><span class="sxs-lookup"><span data-stu-id="6ae71-259">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="6ae71-260">Если ключ в командной строке находится в словаре, значение словаря (замена ключа) передается обратно, чтобы установить пару "ключ — значение" в конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="6ae71-260">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="6ae71-261">Сопоставление переключений необходимо для любого ключа командной строки с префиксом из одного дефиса (`-`).</span><span class="sxs-lookup"><span data-stu-id="6ae71-261">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="6ae71-262">Правила ключей из словаря сопоставления переключений:</span><span class="sxs-lookup"><span data-stu-id="6ae71-262">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="6ae71-263">Переключения должны начинаться с дефиса (`-`) или двойного дефиса (`--`).</span><span class="sxs-lookup"><span data-stu-id="6ae71-263">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="6ae71-264">Словарь сопоставлений переключений не должен содержать повторяющиеся ключи.</span><span class="sxs-lookup"><span data-stu-id="6ae71-264">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="6ae71-265">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-265">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="6ae71-266">Как показано в предыдущем примере, вызов `CreateDefaultBuilder` не должен передавать аргументы при использовании сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="6ae71-266">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="6ae71-267">Вызов команды `AddCommandLine` метода `CreateDefaultBuilder` не включает сопоставленные коммутаторы, и нет возможности передать словарь сопоставления переключений в `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-267">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="6ae71-268">Если аргументы включают сопоставления переключений и передаются в `CreateDefaultBuilder`, его поставщик `AddCommandLine` не может инициализироваться с помощью <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-268">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="6ae71-269">Чтобы решить эту проблему, нужно не передавать аргументы команде `CreateDefaultBuilder`, а позволить методу `AddCommandLine` метода `ConfigurationBuilder` обрабатывать как аргументы, так и словарь сопоставления параметров.</span><span class="sxs-lookup"><span data-stu-id="6ae71-269">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="6ae71-270">Созданный словарь сопоставлений переключений содержит данные, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="6ae71-270">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="6ae71-271">Ключ</span><span class="sxs-lookup"><span data-stu-id="6ae71-271">Key</span></span>       | <span data-ttu-id="6ae71-272">Значение</span><span class="sxs-lookup"><span data-stu-id="6ae71-272">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="6ae71-273">Если ключи сопоставления переключений используются при запуске приложения, конфигурация принимает значение конфигурации в ключе, предоставленном в словаре.</span><span class="sxs-lookup"><span data-stu-id="6ae71-273">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="6ae71-274">После выполнения предыдущей команды конфигурация содержит значения, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="6ae71-274">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="6ae71-275">Ключ</span><span class="sxs-lookup"><span data-stu-id="6ae71-275">Key</span></span>               | <span data-ttu-id="6ae71-276">Значение</span><span class="sxs-lookup"><span data-stu-id="6ae71-276">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="6ae71-277">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="6ae71-277">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="6ae71-278"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> загружает конфигурацию из пары "ключ — значение" переменной среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="6ae71-278">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="6ae71-279">Чтобы активировать конфигурацию переменных среды, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-279">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="6ae71-280">В переменных средах разделитель-двоеточие (`:`) может не работать на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="6ae71-280">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="6ae71-281">Двойной знак подчеркивания (`__`) поддерживается на всех платформах и его можно заменить двоеточием.</span><span class="sxs-lookup"><span data-stu-id="6ae71-281">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="6ae71-282">[Служба приложений Azure](https://azure.microsoft.com/services/app-service/) позволяет задать переменные среды на портале Azure, который может переопределить конфигурацию приложения, используя поставщик конфигурации переменных среды.</span><span class="sxs-lookup"><span data-stu-id="6ae71-282">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="6ae71-283">Дополнительные сведения см. в руководстве по [переопределению конфигурации приложения Azure с помощью портала Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="6ae71-283">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="6ae71-284">`AddEnvironmentVariables` автоматически вызывается для переменных среды, имеющих префикс `ASPNETCORE_`, при инициализации нового экземпляра <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-284">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="6ae71-285">Дополнительные сведения см. в руководстве по [настройке веб-узла](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="6ae71-285">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="6ae71-286">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="6ae71-286">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="6ae71-287">конфигурация приложения на основе переменных среды без префикса путем вызова `AddEnvironmentVariables` без префикса;</span><span class="sxs-lookup"><span data-stu-id="6ae71-287">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="6ae71-288">дополнительную конфигурацию из *appsettings.json* и *appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="6ae71-288">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="6ae71-289">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки);</span><span class="sxs-lookup"><span data-stu-id="6ae71-289">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="6ae71-290">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="6ae71-290">Command-line arguments.</span></span>

<span data-ttu-id="6ae71-291">Поставщик конфигурации переменных среды вызывается после выполнения настройки с помощью секретов пользователя и файлов *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="6ae71-291">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="6ae71-292">Вызов поставщика в этой позиции разрешает чтение переменных среды выполнения, чтобы переопределить конфигурацию, заданную секретом пользователя и файлом *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="6ae71-292">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="6ae71-293">Чтобы указать конфигурацию приложения, при сборке узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-293">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="6ae71-294">Если вам нужно добавить конфигурацию приложения из дополнительных переменных среды, вызовите дополнительные поставщики приложения в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, а затем вызовите `AddEnvironmentVariables` с префиксом.</span><span class="sxs-lookup"><span data-stu-id="6ae71-294">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="6ae71-295">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="6ae71-295">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="6ae71-296">**Пример**</span><span class="sxs-lookup"><span data-stu-id="6ae71-296">**Example**</span></span>

<span data-ttu-id="6ae71-297">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-297">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="6ae71-298">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="6ae71-298">Run the sample app.</span></span> <span data-ttu-id="6ae71-299">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-299">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="6ae71-300">Обратите внимание, что выходные данные содержат пару "ключ — значение" для переменной среды `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-300">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="6ae71-301">Значение отражает среду, в которой выполняется приложение, обычно при локальном запуске это `Development`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-301">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="6ae71-302">Чтобы список переменных сред, отображаемый приложением, был коротким, приложение фильтрует переменные среды по следующим пунктам:</span><span class="sxs-lookup"><span data-stu-id="6ae71-302">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="6ae71-303">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="6ae71-303">ASPNETCORE_</span></span>
* <span data-ttu-id="6ae71-304">urls</span><span class="sxs-lookup"><span data-stu-id="6ae71-304">urls</span></span>
* <span data-ttu-id="6ae71-305">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="6ae71-305">Logging</span></span>
* <span data-ttu-id="6ae71-306">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="6ae71-306">ENVIRONMENT</span></span>
* <span data-ttu-id="6ae71-307">contentRoot</span><span class="sxs-lookup"><span data-stu-id="6ae71-307">contentRoot</span></span>
* <span data-ttu-id="6ae71-308">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="6ae71-308">AllowedHosts</span></span>
* <span data-ttu-id="6ae71-309">applicationName</span><span class="sxs-lookup"><span data-stu-id="6ae71-309">applicationName</span></span>
* <span data-ttu-id="6ae71-310">CommandLine</span><span class="sxs-lookup"><span data-stu-id="6ae71-310">CommandLine</span></span>

<span data-ttu-id="6ae71-311">Если хотите просмотреть все переменные среды, доступные для приложения, измените значение `FilteredConfiguration` в *Pages/Index.cshtml.cs* на следующее:</span><span class="sxs-lookup"><span data-stu-id="6ae71-311">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="6ae71-312">Префиксы</span><span class="sxs-lookup"><span data-stu-id="6ae71-312">Prefixes</span></span>

<span data-ttu-id="6ae71-313">Переменные среды, которые загружаются в конфигурацию приложения, фильтруются при добавлении префикса к методу `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-313">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="6ae71-314">Например, чтобы отфильтровать переменные среды по префиксу `CUSTOM_`, введите префикс поставщику конфигурации:</span><span class="sxs-lookup"><span data-stu-id="6ae71-314">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="6ae71-315">Префикс отделяется при создании пары конфигурации "ключ — значение".</span><span class="sxs-lookup"><span data-stu-id="6ae71-315">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="6ae71-316">Статический удобный метод `CreateDefaultBuilder` создает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> для установления размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="6ae71-316">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="6ae71-317">Когда значение `WebHostBuilder` будет создано, оно найдет конфигурацию узла в переменных среды, которые начинаются с префикса `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-317">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

<span data-ttu-id="6ae71-318">**Префиксы строк подключения**</span><span class="sxs-lookup"><span data-stu-id="6ae71-318">**Connection string prefixes**</span></span>

<span data-ttu-id="6ae71-319">API конфигурации имеет специальные правила обработки для четырех строк подключения переменных среды, связанных с настройкой строк подключения Azure для среды приложения.</span><span class="sxs-lookup"><span data-stu-id="6ae71-319">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="6ae71-320">Если префикс не указан в `AddEnvironmentVariables`, переменные среды с префиксами, указанными в таблице, загружаются в приложение.</span><span class="sxs-lookup"><span data-stu-id="6ae71-320">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="6ae71-321">Префикс строки подключения</span><span class="sxs-lookup"><span data-stu-id="6ae71-321">Connection string prefix</span></span> | <span data-ttu-id="6ae71-322">Поставщик</span><span class="sxs-lookup"><span data-stu-id="6ae71-322">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="6ae71-323">Поставщик пользователя</span><span class="sxs-lookup"><span data-stu-id="6ae71-323">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="6ae71-324">MySQL</span><span class="sxs-lookup"><span data-stu-id="6ae71-324">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="6ae71-325">База данных SQL Azure</span><span class="sxs-lookup"><span data-stu-id="6ae71-325">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="6ae71-326">SQL Server</span><span class="sxs-lookup"><span data-stu-id="6ae71-326">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="6ae71-327">Когда переменная среды обнаруживается и загружается в конфигурацию с одним из четырех префиксов, приведенных в таблице, происходит следующее.</span><span class="sxs-lookup"><span data-stu-id="6ae71-327">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="6ae71-328">Ключ конфигурации создается путем удаления префикса переменных среды и добавления ключа раздела конфигурации (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="6ae71-328">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="6ae71-329">Создается новая пара "ключ — значение" конфигурации, которая представляет поставщика подключения базы данных (за исключением `CUSTOMCONNSTR_`, который не имеет указанного поставщика).</span><span class="sxs-lookup"><span data-stu-id="6ae71-329">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="6ae71-330">Ключ переменной среды</span><span class="sxs-lookup"><span data-stu-id="6ae71-330">Environment variable key</span></span> | <span data-ttu-id="6ae71-331">Преобразованный ключ конфигурации</span><span class="sxs-lookup"><span data-stu-id="6ae71-331">Converted configuration key</span></span> | <span data-ttu-id="6ae71-332">Запись конфигурации поставщика</span><span class="sxs-lookup"><span data-stu-id="6ae71-332">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="6ae71-333">Запись конфигурации не создана.</span><span class="sxs-lookup"><span data-stu-id="6ae71-333">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="6ae71-334">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="6ae71-334">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="6ae71-335">Значение: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="6ae71-335">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="6ae71-336">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="6ae71-336">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="6ae71-337">Значение: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="6ae71-337">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="6ae71-338">Ключ: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="6ae71-338">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="6ae71-339">Значение: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="6ae71-339">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="6ae71-340">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="6ae71-340">File Configuration Provider</span></span>

<span data-ttu-id="6ae71-341"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> является базовым классом для загрузки конфигурации из файловой системы.</span><span class="sxs-lookup"><span data-stu-id="6ae71-341"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="6ae71-342">Следующие поставщики конфигурации предназначены для определенных типов файлов:</span><span class="sxs-lookup"><span data-stu-id="6ae71-342">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="6ae71-343">Поставщик конфигурации INI</span><span class="sxs-lookup"><span data-stu-id="6ae71-343">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="6ae71-344">Поставщик конфигурации JSON</span><span class="sxs-lookup"><span data-stu-id="6ae71-344">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="6ae71-345">Поставщик конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="6ae71-345">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="6ae71-346">Поставщик конфигурации INI</span><span class="sxs-lookup"><span data-stu-id="6ae71-346">INI Configuration Provider</span></span>

<span data-ttu-id="6ae71-347"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> загружает конфигурацию из пары "ключ — значение" INI-файла во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="6ae71-347">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="6ae71-348">Чтобы активировать конфигурацию INI-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-348">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="6ae71-349">Двоеточие можно использовать как разделитель раздела в конфигурации файла INI.</span><span class="sxs-lookup"><span data-stu-id="6ae71-349">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="6ae71-350">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="6ae71-350">Overloads permit specifying:</span></span>

* <span data-ttu-id="6ae71-351">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="6ae71-351">Whether the file is optional.</span></span>
* <span data-ttu-id="6ae71-352">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="6ae71-352">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="6ae71-353"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="6ae71-353">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="6ae71-354">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-354">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="6ae71-355">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-355">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="6ae71-356">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6ae71-356">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="6ae71-357">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="6ae71-357">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="6ae71-358">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-358">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="6ae71-359">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6ae71-359">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="6ae71-360">Общий пример конфигурации INI-файла.</span><span class="sxs-lookup"><span data-stu-id="6ae71-360">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="6ae71-361">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-361">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="6ae71-362">section0:key0</span><span class="sxs-lookup"><span data-stu-id="6ae71-362">section0:key0</span></span>
* <span data-ttu-id="6ae71-363">section0:key1</span><span class="sxs-lookup"><span data-stu-id="6ae71-363">section0:key1</span></span>
* <span data-ttu-id="6ae71-364">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="6ae71-364">section1:subsection:key</span></span>
* <span data-ttu-id="6ae71-365">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="6ae71-365">section2:subsection0:key</span></span>
* <span data-ttu-id="6ae71-366">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="6ae71-366">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="6ae71-367">Поставщик конфигурации JSON</span><span class="sxs-lookup"><span data-stu-id="6ae71-367">JSON Configuration Provider</span></span>

<span data-ttu-id="6ae71-368"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> загружает конфигурацию из пары "ключ — значение" JSON-файла в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="6ae71-368">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="6ae71-369">Чтобы активировать конфигурацию JSON-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-369">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="6ae71-370">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="6ae71-370">Overloads permit specifying:</span></span>

* <span data-ttu-id="6ae71-371">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="6ae71-371">Whether the file is optional.</span></span>
* <span data-ttu-id="6ae71-372">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="6ae71-372">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="6ae71-373"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="6ae71-373">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>



<span data-ttu-id="6ae71-374">`AddJsonFile` автоматически вызывается дважды при инициализации нового <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> с <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-374">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="6ae71-375">Метод вызывается для загрузки конфигурации из:</span><span class="sxs-lookup"><span data-stu-id="6ae71-375">The method is called to load configuration from:</span></span>

* <span data-ttu-id="6ae71-376">*appsettings.json* &ndash; первым читается этот файл.</span><span class="sxs-lookup"><span data-stu-id="6ae71-376">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="6ae71-377">Версия файла среды может переопределить значения, предоставленные *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="6ae71-377">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="6ae71-378">*appsettings.{Environment}.json* — версия среды файла загружается на основе [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="6ae71-378">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="6ae71-379">Дополнительные сведения см. в руководстве по [настройке веб-узла](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="6ae71-379">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="6ae71-380">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="6ae71-380">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="6ae71-381">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="6ae71-381">Environment variables.</span></span>
* <span data-ttu-id="6ae71-382">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки);</span><span class="sxs-lookup"><span data-stu-id="6ae71-382">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="6ae71-383">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="6ae71-383">Command-line arguments.</span></span>

<span data-ttu-id="6ae71-384">Сначала устанавливается поставщик конфигурации JSON-файлов.</span><span class="sxs-lookup"><span data-stu-id="6ae71-384">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="6ae71-385">Таким образом, секреты пользователя, переменные среды и аргументы командной строки переопределят конфигурацию, заданную файлами *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="6ae71-385">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="6ae71-386">Вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> при сборке узла, чтобы указать конфигурацию приложения для файлов, отличных от *appsettings.json* и *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="6ae71-386">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

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

<span data-ttu-id="6ae71-387">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-387">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="6ae71-388">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6ae71-388">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="6ae71-389">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="6ae71-389">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="6ae71-390">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-390">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="6ae71-391">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6ae71-391">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="6ae71-392">**Пример**</span><span class="sxs-lookup"><span data-stu-id="6ae71-392">**Example**</span></span>

<span data-ttu-id="6ae71-393">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает два вызова `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-393">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="6ae71-394">Конфигурация загружена из *appsettings.json* и *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="6ae71-394">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="6ae71-395">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="6ae71-395">Run the sample app.</span></span> <span data-ttu-id="6ae71-396">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-396">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="6ae71-397">Обратите внимание, что выходные данные содержат пары "ключ — значение" для конфигурации, представленной в таблице в зависимости от среды.</span><span class="sxs-lookup"><span data-stu-id="6ae71-397">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="6ae71-398">Ключи конфигурации ведения журнала используют двоеточие (`:`) как иерархический разделитель.</span><span class="sxs-lookup"><span data-stu-id="6ae71-398">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="6ae71-399">Ключ</span><span class="sxs-lookup"><span data-stu-id="6ae71-399">Key</span></span>                        | <span data-ttu-id="6ae71-400">Значение разработки</span><span class="sxs-lookup"><span data-stu-id="6ae71-400">Development Value</span></span> | <span data-ttu-id="6ae71-401">Рабочее значение</span><span class="sxs-lookup"><span data-stu-id="6ae71-401">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="6ae71-402">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="6ae71-402">Logging:LogLevel:System</span></span>    | <span data-ttu-id="6ae71-403">Сведения</span><span class="sxs-lookup"><span data-stu-id="6ae71-403">Information</span></span>       | <span data-ttu-id="6ae71-404">Сведения</span><span class="sxs-lookup"><span data-stu-id="6ae71-404">Information</span></span>      |
| <span data-ttu-id="6ae71-405">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="6ae71-405">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="6ae71-406">Сведения</span><span class="sxs-lookup"><span data-stu-id="6ae71-406">Information</span></span>       | <span data-ttu-id="6ae71-407">Сведения</span><span class="sxs-lookup"><span data-stu-id="6ae71-407">Information</span></span>      |
| <span data-ttu-id="6ae71-408">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="6ae71-408">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="6ae71-409">Отладка</span><span class="sxs-lookup"><span data-stu-id="6ae71-409">Debug</span></span>             | <span data-ttu-id="6ae71-410">Error</span><span class="sxs-lookup"><span data-stu-id="6ae71-410">Error</span></span>            |
| <span data-ttu-id="6ae71-411">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="6ae71-411">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="6ae71-412">Поставщик конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="6ae71-412">XML Configuration Provider</span></span>

<span data-ttu-id="6ae71-413"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> загружает конфигурацию из пары "ключ — значение" XML-файла в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="6ae71-413">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="6ae71-414">Чтобы активировать конфигурацию XML-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-414">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="6ae71-415">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="6ae71-415">Overloads permit specifying:</span></span>

* <span data-ttu-id="6ae71-416">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="6ae71-416">Whether the file is optional.</span></span>
* <span data-ttu-id="6ae71-417">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="6ae71-417">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="6ae71-418"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="6ae71-418">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="6ae71-419">Корневой узел файла конфигурации не учитывается при создании пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-419">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="6ae71-420">Не указывайте в файле Document Type Definition (определение типа документа, DTD) или пространство имен.</span><span class="sxs-lookup"><span data-stu-id="6ae71-420">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="6ae71-421">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-421">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="6ae71-422">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-422">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="6ae71-423">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6ae71-423">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="6ae71-424">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="6ae71-424">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="6ae71-425">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-425">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="6ae71-426">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6ae71-426">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="6ae71-427">XML-файлы конфигурации могут использовать имена отдельных элементов для повторяющихся разделов.</span><span class="sxs-lookup"><span data-stu-id="6ae71-427">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="6ae71-428">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-428">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="6ae71-429">section0:key0</span><span class="sxs-lookup"><span data-stu-id="6ae71-429">section0:key0</span></span>
* <span data-ttu-id="6ae71-430">section0:key1</span><span class="sxs-lookup"><span data-stu-id="6ae71-430">section0:key1</span></span>
* <span data-ttu-id="6ae71-431">section1:key0</span><span class="sxs-lookup"><span data-stu-id="6ae71-431">section1:key0</span></span>
* <span data-ttu-id="6ae71-432">section1:key1</span><span class="sxs-lookup"><span data-stu-id="6ae71-432">section1:key1</span></span>

<span data-ttu-id="6ae71-433">Повторяющиеся элементы, использующие то же имя элемента, работают, если атрибут `name` используется для различения элементов.</span><span class="sxs-lookup"><span data-stu-id="6ae71-433">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="6ae71-434">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-434">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="6ae71-435">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="6ae71-435">section:section0:key:key0</span></span>
* <span data-ttu-id="6ae71-436">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="6ae71-436">section:section0:key:key1</span></span>
* <span data-ttu-id="6ae71-437">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="6ae71-437">section:section1:key:key0</span></span>
* <span data-ttu-id="6ae71-438">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="6ae71-438">section:section1:key:key1</span></span>

<span data-ttu-id="6ae71-439">Атрибуты можно использовать для предоставления значений.</span><span class="sxs-lookup"><span data-stu-id="6ae71-439">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="6ae71-440">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-440">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="6ae71-441">key:attribute</span><span class="sxs-lookup"><span data-stu-id="6ae71-441">key:attribute</span></span>
* <span data-ttu-id="6ae71-442">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="6ae71-442">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="6ae71-443">Поставщик конфигурации ключа для каждого файла</span><span class="sxs-lookup"><span data-stu-id="6ae71-443">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="6ae71-444"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> использует файлы каталога как пары "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-444">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="6ae71-445">Ключ является именем файла.</span><span class="sxs-lookup"><span data-stu-id="6ae71-445">The key is the file name.</span></span> <span data-ttu-id="6ae71-446">Значение содержит содержимое файла.</span><span class="sxs-lookup"><span data-stu-id="6ae71-446">The value contains the file's contents.</span></span> <span data-ttu-id="6ae71-447">Поставщик конфигурации ключа для каждого файла используется в сценариях размещения Docker.</span><span class="sxs-lookup"><span data-stu-id="6ae71-447">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="6ae71-448">Чтобы активировать конфигурацию ключа для каждого файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-448">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="6ae71-449">Значение параметра `directoryPath` должно быть абсолютным путем к файлам.</span><span class="sxs-lookup"><span data-stu-id="6ae71-449">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="6ae71-450">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="6ae71-450">Overloads permit specifying:</span></span>

* <span data-ttu-id="6ae71-451">`Action<KeyPerFileConfigurationSource>` — делегат, который настраивает источник.</span><span class="sxs-lookup"><span data-stu-id="6ae71-451">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="6ae71-452">Обязательно ли указывать каталог и путь к каталогу.</span><span class="sxs-lookup"><span data-stu-id="6ae71-452">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="6ae71-453">Двойное подчеркивание (`__`) используется в качестве разделителя ключа конфигурации в именах файлов.</span><span class="sxs-lookup"><span data-stu-id="6ae71-453">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="6ae71-454">Например, в имени файла `Logging__LogLevel__System` создается ключ конфигурации `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-454">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="6ae71-455">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-455">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="6ae71-456">Базовый путь задается с помощью <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-456">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="6ae71-457">`SetBasePath` находится в пакете [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6ae71-457">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="6ae71-458">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="6ae71-458">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="6ae71-459">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="6ae71-459">Memory Configuration Provider</span></span>

<span data-ttu-id="6ae71-460"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> использует коллекцию памяти в качестве пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-460">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="6ae71-461">Чтобы активировать конфигурацию коллекции в памяти, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-461">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="6ae71-462">Поставщик конфигурации может инициализироваться значением `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-462">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="6ae71-463">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-463">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="6ae71-464">При создании <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> со следующей конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="6ae71-464">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="6ae71-465">GetValue</span><span class="sxs-lookup"><span data-stu-id="6ae71-465">GetValue</span></span>

<span data-ttu-id="6ae71-466">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) извлекает значение из конфигурации с указанным ключом и преобразует его в указанный тип.</span><span class="sxs-lookup"><span data-stu-id="6ae71-466">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="6ae71-467">Если ключ не найден, перегрузка позволяет предоставлять значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6ae71-467">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="6ae71-468">В следующем примере происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="6ae71-468">The following example:</span></span>

* <span data-ttu-id="6ae71-469">Из конфигурации извлекается строковое значение с ключом `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-469">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="6ae71-470">Если `NumberKey` отсутствует в ключах конфигурации, используется значение по умолчанию `99`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-470">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="6ae71-471">Значение получает тип `int`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-471">Types the value as an `int`.</span></span>
* <span data-ttu-id="6ae71-472">Значение сохраняется в свойстве `NumberConfig` для использования на странице.</span><span class="sxs-lookup"><span data-stu-id="6ae71-472">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="6ae71-473">GetSection, GetChildren и Exists</span><span class="sxs-lookup"><span data-stu-id="6ae71-473">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="6ae71-474">В следующих примерах рассмотрим файл JSON.</span><span class="sxs-lookup"><span data-stu-id="6ae71-474">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="6ae71-475">Четыре ключа находятся в двух разделах, один из которых содержит пару из подразделов.</span><span class="sxs-lookup"><span data-stu-id="6ae71-475">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="6ae71-476">Когда файл считывается в конфигурацию, для сохранения значений конфигурации создаются следующие уникальные иерархические ключи.</span><span class="sxs-lookup"><span data-stu-id="6ae71-476">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="6ae71-477">section0:key0</span><span class="sxs-lookup"><span data-stu-id="6ae71-477">section0:key0</span></span>
* <span data-ttu-id="6ae71-478">section0:key1</span><span class="sxs-lookup"><span data-stu-id="6ae71-478">section0:key1</span></span>
* <span data-ttu-id="6ae71-479">section1:key0</span><span class="sxs-lookup"><span data-stu-id="6ae71-479">section1:key0</span></span>
* <span data-ttu-id="6ae71-480">section1:key1</span><span class="sxs-lookup"><span data-stu-id="6ae71-480">section1:key1</span></span>
* <span data-ttu-id="6ae71-481">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="6ae71-481">section2:subsection0:key0</span></span>
* <span data-ttu-id="6ae71-482">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="6ae71-482">section2:subsection0:key1</span></span>
* <span data-ttu-id="6ae71-483">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="6ae71-483">section2:subsection1:key0</span></span>
* <span data-ttu-id="6ae71-484">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="6ae71-484">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="6ae71-485">GetSection</span><span class="sxs-lookup"><span data-stu-id="6ae71-485">GetSection</span></span>

<span data-ttu-id="6ae71-486">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) извлекает подраздел конфигурации с указанным ключом подраздела.</span><span class="sxs-lookup"><span data-stu-id="6ae71-486">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span> <span data-ttu-id="6ae71-487">`GetSection` находится в пакете [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6ae71-487">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="6ae71-488">Чтобы вернуть <xref:Microsoft.Extensions.Configuration.IConfigurationSection>, содержащий только пары "ключ — значение" в `section1`, вызовите `GetSection` и укажите имя раздела.</span><span class="sxs-lookup"><span data-stu-id="6ae71-488">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="6ae71-489">`configSection` не содержит значение, только ключ и путь.</span><span class="sxs-lookup"><span data-stu-id="6ae71-489">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="6ae71-490">Аналогично, чтобы получить значения для ключей в `section2:subsection0`, вызовите `GetSection` и укажите путь к разделу.</span><span class="sxs-lookup"><span data-stu-id="6ae71-490">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="6ae71-491">Значение `GetSection` никогда не возвращает значение `null`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-491">`GetSection` never returns `null`.</span></span> <span data-ttu-id="6ae71-492">Если соответствующий раздел не найден, возвращается пустой параметр `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-492">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="6ae71-493">Когда `GetSection` возвращает соответствующий раздел, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> не заполняется.</span><span class="sxs-lookup"><span data-stu-id="6ae71-493">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="6ae71-494"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> и <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> возвращаются, если раздел существует.</span><span class="sxs-lookup"><span data-stu-id="6ae71-494">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="6ae71-495">GetChildren</span><span class="sxs-lookup"><span data-stu-id="6ae71-495">GetChildren</span></span>

<span data-ttu-id="6ae71-496">Вызов [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) в `section2` получает значение `IEnumerable<IConfigurationSection>`, которое включает:</span><span class="sxs-lookup"><span data-stu-id="6ae71-496">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="6ae71-497">Exists</span><span class="sxs-lookup"><span data-stu-id="6ae71-497">Exists</span></span>

<span data-ttu-id="6ae71-498">Используйте [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*), чтобы определить, существует ли раздел конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-498">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="6ae71-499">Учитывая данные примера, `sectionExists` является `false`, потому что в данных конфигурации нет `section2:subsection2`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-499">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="6ae71-500">Привязка к классу</span><span class="sxs-lookup"><span data-stu-id="6ae71-500">Bind to a class</span></span>

<span data-ttu-id="6ae71-501">Конфигурация может быть привязана к классам, которые представляют группы связанных параметров, используя шаблон *вариантов*.</span><span class="sxs-lookup"><span data-stu-id="6ae71-501">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="6ae71-502">Для получения дополнительной информации см. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-502">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="6ae71-503">Значения конфигурации возвращаются как строки, но вызов <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> позволяет построить объекты [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="6ae71-503">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="6ae71-504">`Bind` находится в пакете [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6ae71-504">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="6ae71-505">Пример приложения содержит модель `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="6ae71-505">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="6ae71-506">Раздел `starship` файла *starship.json* создает конфигурацию, когда образец приложения использует поставщик конфигурации JSON для загрузки конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-506">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

<span data-ttu-id="6ae71-507">Создаются следующие пары "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-507">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="6ae71-508">Ключ</span><span class="sxs-lookup"><span data-stu-id="6ae71-508">Key</span></span>                   | <span data-ttu-id="6ae71-509">Значение</span><span class="sxs-lookup"><span data-stu-id="6ae71-509">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="6ae71-510">starship:name</span><span class="sxs-lookup"><span data-stu-id="6ae71-510">starship:name</span></span>         | <span data-ttu-id="6ae71-511">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="6ae71-511">USS Enterprise</span></span>                                    |
| <span data-ttu-id="6ae71-512">starship:registry</span><span class="sxs-lookup"><span data-stu-id="6ae71-512">starship:registry</span></span>     | <span data-ttu-id="6ae71-513">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="6ae71-513">NCC-1701</span></span>                                          |
| <span data-ttu-id="6ae71-514">starship:class</span><span class="sxs-lookup"><span data-stu-id="6ae71-514">starship:class</span></span>        | <span data-ttu-id="6ae71-515">Constitution</span><span class="sxs-lookup"><span data-stu-id="6ae71-515">Constitution</span></span>                                      |
| <span data-ttu-id="6ae71-516">starship:length</span><span class="sxs-lookup"><span data-stu-id="6ae71-516">starship:length</span></span>       | <span data-ttu-id="6ae71-517">304.8</span><span class="sxs-lookup"><span data-stu-id="6ae71-517">304.8</span></span>                                             |
| <span data-ttu-id="6ae71-518">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="6ae71-518">starship:commissioned</span></span> | <span data-ttu-id="6ae71-519">False</span><span class="sxs-lookup"><span data-stu-id="6ae71-519">False</span></span>                                             |
| <span data-ttu-id="6ae71-520">trademark</span><span class="sxs-lookup"><span data-stu-id="6ae71-520">trademark</span></span>             | <span data-ttu-id="6ae71-521">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="6ae71-521">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="6ae71-522">Пример приложения вызывает `GetSection` с помощью ключа `starship`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-522">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="6ae71-523">Пары "ключ — значение" `starship` изолированы.</span><span class="sxs-lookup"><span data-stu-id="6ae71-523">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="6ae71-524">Метод `Bind` вызывается в подразделе при прохождении в экземпляр класса `Starship`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-524">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="6ae71-525">После привязки значения экземпляра экземпляру присваивается свойство для преобразования для просмотра.</span><span class="sxs-lookup"><span data-stu-id="6ae71-525">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

<span data-ttu-id="6ae71-526">`GetSection` находится в пакете [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6ae71-526">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="6ae71-527">Привязка к графу объектов</span><span class="sxs-lookup"><span data-stu-id="6ae71-527">Bind to an object graph</span></span>

<span data-ttu-id="6ae71-528"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> способна связывать весь граф объекта POCO.</span><span class="sxs-lookup"><span data-stu-id="6ae71-528"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="6ae71-529">`Bind` находится в пакете [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6ae71-529">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="6ae71-530">Образец содержит модель `TvShow`, в графе объектов которого находятся классы `Metadata` и `Actors` (*Модели/TvShow.cs*).</span><span class="sxs-lookup"><span data-stu-id="6ae71-530">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="6ae71-531">В примере приложения есть файл *tvshow.xml*, содержащий данные конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-531">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="6ae71-532">Конфигурация привязана ко всему ​​графу объектов `TvShow` с помощью метода `Bind`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-532">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="6ae71-533">Привязанный экземпляр присваивается свойству для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="6ae71-533">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="6ae71-534">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) привязывает и возвращает указанный тип.</span><span class="sxs-lookup"><span data-stu-id="6ae71-534">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="6ae71-535">Метод `Get<T>` может быть более удобным, чем использование `Bind`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-535">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="6ae71-536">В следующем коде показано, как использовать `Get<T>` с предыдущим примером, который позволяет привязать связанный экземпляр непосредственно к свойству, используемому для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="6ae71-536">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

<span data-ttu-id="6ae71-537"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> находится в пакете [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6ae71-537"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="6ae71-538">`Get<T>` доступно в ASP.NET Core 1.1 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="6ae71-538">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="6ae71-539">`GetSection` находится в пакете [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6ae71-539">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="6ae71-540">Привязка массива к классу</span><span class="sxs-lookup"><span data-stu-id="6ae71-540">Bind an array to a class</span></span>

<span data-ttu-id="6ae71-541">*Пример приложения демонстрирует концепции, описанные в этом разделе.*</span><span class="sxs-lookup"><span data-stu-id="6ae71-541">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="6ae71-542"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-542">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="6ae71-543">Любой формат массива, который предоставляет сегмент числового ключа (`:0:`, `:1:`, &hellip; `:{n}:`), способен привязать массив к массиву класса POCO.</span><span class="sxs-lookup"><span data-stu-id="6ae71-543">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span> <span data-ttu-id="6ae71-544">"Bind" находится в пакете [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6ae71-544">\`Bind\`\` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

> [!NOTE]
> <span data-ttu-id="6ae71-545">Привязка предоставляется соглашением.</span><span class="sxs-lookup"><span data-stu-id="6ae71-545">Binding is provided by convention.</span></span> <span data-ttu-id="6ae71-546">Пользовательские поставщики конфигурации не обязаны реализовывать привязку массива.</span><span class="sxs-lookup"><span data-stu-id="6ae71-546">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="6ae71-547">**Обработка массива в оперативной памяти**</span><span class="sxs-lookup"><span data-stu-id="6ae71-547">**In-memory array processing**</span></span>

<span data-ttu-id="6ae71-548">Рассмотрите ключи и значения конфигурации, приведенные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="6ae71-548">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="6ae71-549">Ключ</span><span class="sxs-lookup"><span data-stu-id="6ae71-549">Key</span></span>             | <span data-ttu-id="6ae71-550">Значение</span><span class="sxs-lookup"><span data-stu-id="6ae71-550">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="6ae71-551">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="6ae71-551">array:entries:0</span></span> | <span data-ttu-id="6ae71-552">value0</span><span class="sxs-lookup"><span data-stu-id="6ae71-552">value0</span></span> |
| <span data-ttu-id="6ae71-553">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="6ae71-553">array:entries:1</span></span> | <span data-ttu-id="6ae71-554">value1</span><span class="sxs-lookup"><span data-stu-id="6ae71-554">value1</span></span> |
| <span data-ttu-id="6ae71-555">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="6ae71-555">array:entries:2</span></span> | <span data-ttu-id="6ae71-556">value2</span><span class="sxs-lookup"><span data-stu-id="6ae71-556">value2</span></span> |
| <span data-ttu-id="6ae71-557">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="6ae71-557">array:entries:4</span></span> | <span data-ttu-id="6ae71-558">value4</span><span class="sxs-lookup"><span data-stu-id="6ae71-558">value4</span></span> |
| <span data-ttu-id="6ae71-559">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="6ae71-559">array:entries:5</span></span> | <span data-ttu-id="6ae71-560">value5</span><span class="sxs-lookup"><span data-stu-id="6ae71-560">value5</span></span> |

<span data-ttu-id="6ae71-561">Эти ключи и значения загружаются в пример приложения с помощью поставщика конфигурации памяти.</span><span class="sxs-lookup"><span data-stu-id="6ae71-561">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

<span data-ttu-id="6ae71-562">Массив пропускает значения для индекса &num;3.</span><span class="sxs-lookup"><span data-stu-id="6ae71-562">The array skips a value for index &num;3.</span></span> <span data-ttu-id="6ae71-563">Связующее свойство конфигурации не способно связывать нулевые значения или создавать нулевые записи в связанных объектах, что становится ясным в тот момент, когда демонстрируется результат привязки этого массива к объекту.</span><span class="sxs-lookup"><span data-stu-id="6ae71-563">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="6ae71-564">В примере приложения класс POCO доступен для хранения привязанных данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-564">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="6ae71-565">Данные конфигурации, связанные с объектом.</span><span class="sxs-lookup"><span data-stu-id="6ae71-565">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="6ae71-566">`GetSection` находится в пакете [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), который включен в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6ae71-566">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="6ae71-567">Синтаксис [ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) также может использоваться для получения более компактного кода.</span><span class="sxs-lookup"><span data-stu-id="6ae71-567">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="6ae71-568">Связанный объект, экземпляр `ArrayExample`, получает данные массива из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-568">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="6ae71-569">Индекс `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="6ae71-569">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="6ae71-570">Значение `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="6ae71-570">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="6ae71-571">0</span><span class="sxs-lookup"><span data-stu-id="6ae71-571">0</span></span>                            | <span data-ttu-id="6ae71-572">value0</span><span class="sxs-lookup"><span data-stu-id="6ae71-572">value0</span></span>                       |
| <span data-ttu-id="6ae71-573">1</span><span class="sxs-lookup"><span data-stu-id="6ae71-573">1</span></span>                            | <span data-ttu-id="6ae71-574">value1</span><span class="sxs-lookup"><span data-stu-id="6ae71-574">value1</span></span>                       |
| <span data-ttu-id="6ae71-575">2</span><span class="sxs-lookup"><span data-stu-id="6ae71-575">2</span></span>                            | <span data-ttu-id="6ae71-576">value2</span><span class="sxs-lookup"><span data-stu-id="6ae71-576">value2</span></span>                       |
| <span data-ttu-id="6ae71-577">3</span><span class="sxs-lookup"><span data-stu-id="6ae71-577">3</span></span>                            | <span data-ttu-id="6ae71-578">value4</span><span class="sxs-lookup"><span data-stu-id="6ae71-578">value4</span></span>                       |
| <span data-ttu-id="6ae71-579">4</span><span class="sxs-lookup"><span data-stu-id="6ae71-579">4</span></span>                            | <span data-ttu-id="6ae71-580">value5</span><span class="sxs-lookup"><span data-stu-id="6ae71-580">value5</span></span>                       |

<span data-ttu-id="6ae71-581">Индекс &num;3 в связанном объекте содержит данные конфигурации для конфигурационного ключа `array:4` и его значения `value4`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-581">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="6ae71-582">При привязке данных конфигурации, содержащих массив индексов, в ключах конфигурации эти индексы просто используются для выполнения итерации данных конфигурации при создании объекта.</span><span class="sxs-lookup"><span data-stu-id="6ae71-582">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="6ae71-583">Когда массив ключей конфигурации пропускает один или несколько индексов, в данных конфигурации не может быть сохранено нулевое значение и в связанном объекте не создается нулевая запись.</span><span class="sxs-lookup"><span data-stu-id="6ae71-583">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="6ae71-584">Отсутствующий элемент конфигурации для индекса &num;3 может быть предоставлен перед привязкой к экземпляру `ArrayExample` любым поставщиком конфигурации, который создает правильную пару "ключ — значение" в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-584">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="6ae71-585">Если в образец включен дополнительный поставщик конфигурации JSON с отсутствующей парой "ключ — значение", то `ArrayExample.Entries` подойдет полному массиву конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-585">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="6ae71-586">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="6ae71-586">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="6ae71-587">В <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="6ae71-587">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="6ae71-588">Пары "ключ — значение", показанные в таблице, загружаются в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-588">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="6ae71-589">Ключ</span><span class="sxs-lookup"><span data-stu-id="6ae71-589">Key</span></span>             | <span data-ttu-id="6ae71-590">Значение</span><span class="sxs-lookup"><span data-stu-id="6ae71-590">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="6ae71-591">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="6ae71-591">array:entries:3</span></span> | <span data-ttu-id="6ae71-592">value3</span><span class="sxs-lookup"><span data-stu-id="6ae71-592">value3</span></span> |

<span data-ttu-id="6ae71-593">Если экземпляр класса `ArrayExample` связан после того, как поставщик конфигурации JSON включает запись для индекса &num;3, то массив `ArrayExample.Entries` включит значение.</span><span class="sxs-lookup"><span data-stu-id="6ae71-593">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="6ae71-594">Индекс `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="6ae71-594">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="6ae71-595">Значение `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="6ae71-595">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="6ae71-596">0</span><span class="sxs-lookup"><span data-stu-id="6ae71-596">0</span></span>                            | <span data-ttu-id="6ae71-597">value0</span><span class="sxs-lookup"><span data-stu-id="6ae71-597">value0</span></span>                       |
| <span data-ttu-id="6ae71-598">1</span><span class="sxs-lookup"><span data-stu-id="6ae71-598">1</span></span>                            | <span data-ttu-id="6ae71-599">value1</span><span class="sxs-lookup"><span data-stu-id="6ae71-599">value1</span></span>                       |
| <span data-ttu-id="6ae71-600">2</span><span class="sxs-lookup"><span data-stu-id="6ae71-600">2</span></span>                            | <span data-ttu-id="6ae71-601">value2</span><span class="sxs-lookup"><span data-stu-id="6ae71-601">value2</span></span>                       |
| <span data-ttu-id="6ae71-602">3</span><span class="sxs-lookup"><span data-stu-id="6ae71-602">3</span></span>                            | <span data-ttu-id="6ae71-603">value3</span><span class="sxs-lookup"><span data-stu-id="6ae71-603">value3</span></span>                       |
| <span data-ttu-id="6ae71-604">4</span><span class="sxs-lookup"><span data-stu-id="6ae71-604">4</span></span>                            | <span data-ttu-id="6ae71-605">value4</span><span class="sxs-lookup"><span data-stu-id="6ae71-605">value4</span></span>                       |
| <span data-ttu-id="6ae71-606">5</span><span class="sxs-lookup"><span data-stu-id="6ae71-606">5</span></span>                            | <span data-ttu-id="6ae71-607">value5</span><span class="sxs-lookup"><span data-stu-id="6ae71-607">value5</span></span>                       |

<span data-ttu-id="6ae71-608">**Обработка массива JSON**</span><span class="sxs-lookup"><span data-stu-id="6ae71-608">**JSON array processing**</span></span>

<span data-ttu-id="6ae71-609">Если JSON-файл содержит массив, ключи конфигурации будут созданы для элементов массива с индексом раздела, начиная с нуля.</span><span class="sxs-lookup"><span data-stu-id="6ae71-609">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="6ae71-610">В следующем файле конфигурации `subsection` — это массив.</span><span class="sxs-lookup"><span data-stu-id="6ae71-610">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="6ae71-611">Поставщик конфигурации JSON считывает данные конфигурации в следующие пары "ключ — значение".</span><span class="sxs-lookup"><span data-stu-id="6ae71-611">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="6ae71-612">Ключ</span><span class="sxs-lookup"><span data-stu-id="6ae71-612">Key</span></span>                     | <span data-ttu-id="6ae71-613">Значение</span><span class="sxs-lookup"><span data-stu-id="6ae71-613">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="6ae71-614">json_array:key</span><span class="sxs-lookup"><span data-stu-id="6ae71-614">json_array:key</span></span>          | <span data-ttu-id="6ae71-615">valueA</span><span class="sxs-lookup"><span data-stu-id="6ae71-615">valueA</span></span> |
| <span data-ttu-id="6ae71-616">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="6ae71-616">json_array:subsection:0</span></span> | <span data-ttu-id="6ae71-617">valueB</span><span class="sxs-lookup"><span data-stu-id="6ae71-617">valueB</span></span> |
| <span data-ttu-id="6ae71-618">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="6ae71-618">json_array:subsection:1</span></span> | <span data-ttu-id="6ae71-619">valueC</span><span class="sxs-lookup"><span data-stu-id="6ae71-619">valueC</span></span> |
| <span data-ttu-id="6ae71-620">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="6ae71-620">json_array:subsection:2</span></span> | <span data-ttu-id="6ae71-621">valueD</span><span class="sxs-lookup"><span data-stu-id="6ae71-621">valueD</span></span> |

<span data-ttu-id="6ae71-622">В примере приложения для привязки пар "ключ — значение" конфигурации доступен следующий класс POCO.</span><span class="sxs-lookup"><span data-stu-id="6ae71-622">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="6ae71-623">После выполнения привязки `JsonArrayExample.Key` содержит значение `valueA`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-623">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="6ae71-624">Подраздел значения хранится в свойстве массива POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-624">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="6ae71-625">Индекс `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="6ae71-625">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="6ae71-626">Значение `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="6ae71-626">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="6ae71-627">0</span><span class="sxs-lookup"><span data-stu-id="6ae71-627">0</span></span>                                   | <span data-ttu-id="6ae71-628">valueB</span><span class="sxs-lookup"><span data-stu-id="6ae71-628">valueB</span></span>                              |
| <span data-ttu-id="6ae71-629">1</span><span class="sxs-lookup"><span data-stu-id="6ae71-629">1</span></span>                                   | <span data-ttu-id="6ae71-630">valueC</span><span class="sxs-lookup"><span data-stu-id="6ae71-630">valueC</span></span>                              |
| <span data-ttu-id="6ae71-631">2</span><span class="sxs-lookup"><span data-stu-id="6ae71-631">2</span></span>                                   | <span data-ttu-id="6ae71-632">valueD</span><span class="sxs-lookup"><span data-stu-id="6ae71-632">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="6ae71-633">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="6ae71-633">Custom configuration provider</span></span>

<span data-ttu-id="6ae71-634">Пример приложения демонстрирует, как создать базовый поставщик конфигурации, который считывает пары "ключ — значение" конфигурации из базы данных, используя [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="6ae71-634">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="6ae71-635">Поставщик имеет следующие характеристики.</span><span class="sxs-lookup"><span data-stu-id="6ae71-635">The provider has the following characteristics:</span></span>

* <span data-ttu-id="6ae71-636">База данных в памяти EF используется для демонстрационных целей.</span><span class="sxs-lookup"><span data-stu-id="6ae71-636">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="6ae71-637">Чтобы использовать базу данных, для которой требуется строка подключения, выполните вторичный `ConfigurationBuilder`, чтобы предоставить строку подключения от другого поставщика конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6ae71-637">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="6ae71-638">Поставщик считывает таблицу базы данных в конфигурации при запуске.</span><span class="sxs-lookup"><span data-stu-id="6ae71-638">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="6ae71-639">Поставщик не запрашивает базу данных для каждого ключа.</span><span class="sxs-lookup"><span data-stu-id="6ae71-639">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="6ae71-640">Функция перезагрузки на изменение не реализована, поэтому обновление базы данных после запуска приложения не влияет на конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="6ae71-640">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="6ae71-641">Определите сущность `EFConfigurationValue` для хранения значений конфигурации в базе данных.</span><span class="sxs-lookup"><span data-stu-id="6ae71-641">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="6ae71-642">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="6ae71-642">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="6ae71-643">Добавьте `EFConfigurationContext` в хранилище и обратитесь к настроенным значениям.</span><span class="sxs-lookup"><span data-stu-id="6ae71-643">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="6ae71-644">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="6ae71-644">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="6ae71-645">Создайте класс, реализующий <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-645">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="6ae71-646">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="6ae71-646">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="6ae71-647">Создайте пользовательский поставщик конфигурации путем наследования от <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-647">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="6ae71-648">Поставщик конфигурации инициализирует пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="6ae71-648">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="6ae71-649">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="6ae71-649">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="6ae71-650">Метод расширения `AddEFConfiguration` позволяет добавить источник конфигурации к `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-650">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="6ae71-651">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="6ae71-651">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="6ae71-652">В следующем коде показано, как использовать пользовательский `EFConfigurationProvider` в *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="6ae71-652">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="6ae71-653">Доступ к конфигурации во время запуска</span><span class="sxs-lookup"><span data-stu-id="6ae71-653">Access configuration during startup</span></span>

<span data-ttu-id="6ae71-654">Внесите значение `IConfiguration` в конструктор `Startup` для доступа к значениям конфигурации в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-654">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="6ae71-655">Чтобы получить доступ к конфигурации в `Startup.Configure`, либо добавьте значение `IConfiguration` непосредственно в метод, либо используйте экземпляр из конструктора.</span><span class="sxs-lookup"><span data-stu-id="6ae71-655">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="6ae71-656">Дополнительные сведения см. в руководстве по [доступу к конфигурации с использованием удобных методов](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="6ae71-656">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="6ae71-657">Настройте доступ на странице Razor Pages или в представлении MVC</span><span class="sxs-lookup"><span data-stu-id="6ae71-657">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="6ae71-658">Для доступа к параметрам конфигурации на странице Razor Pages или в представлении MVC добавьте [использование директивы](xref:mvc/views/razor#using) ([Справочник по C#: использование директивы](/dotnet/csharp/language-reference/keywords/using-directive)) для [пространства имен Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) и вставьте <xref:Microsoft.Extensions.Configuration.IConfiguration> на страницу или в представление.</span><span class="sxs-lookup"><span data-stu-id="6ae71-658">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="6ae71-659">На странице Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6ae71-659">In a Razor Pages page:</span></span>

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

<span data-ttu-id="6ae71-660">В представлении MVC</span><span class="sxs-lookup"><span data-stu-id="6ae71-660">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="6ae71-661">Добавление конфигурации из внешней сборки</span><span class="sxs-lookup"><span data-stu-id="6ae71-661">Add configuration from an external assembly</span></span>

<span data-ttu-id="6ae71-662">Реализация <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> позволяет при запуске добавлять в приложение улучшения из внешней сборки вне приложения класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="6ae71-662">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="6ae71-663">Для получения дополнительной информации см. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="6ae71-663">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6ae71-664">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6ae71-664">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* <span data-ttu-id="6ae71-665">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/) (Подробные сведения о конфигурации Microsoft)</span><span class="sxs-lookup"><span data-stu-id="6ae71-665">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)</span></span>
