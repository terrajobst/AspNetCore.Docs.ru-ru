---
title: Конфигурация в .NET Core
author: guardrex
description: Узнайте, как использовать API конфигурации для настройки приложения ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/10/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: 3dcabae3f76d81e641057c346dbb9097c2da42c7
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78644104"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="43d09-103">Конфигурация в .NET Core</span><span class="sxs-lookup"><span data-stu-id="43d09-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="43d09-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="43d09-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="43d09-105">Конфигурация приложения в ASP.NET Core основана на парах "ключ — значение", установленных *поставщиками конфигурации*.</span><span class="sxs-lookup"><span data-stu-id="43d09-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="43d09-106">Поставщики конфигурации получают данные конфигурации в парах "ключ — значение" из различных источников:</span><span class="sxs-lookup"><span data-stu-id="43d09-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="43d09-107">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="43d09-107">Azure Key Vault</span></span>
* <span data-ttu-id="43d09-108">конфигурация приложения Azure;</span><span class="sxs-lookup"><span data-stu-id="43d09-108">Azure App Configuration</span></span>
* <span data-ttu-id="43d09-109">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="43d09-109">Command-line arguments</span></span>
* <span data-ttu-id="43d09-110">а также пользовательские поставщики (устанавливаемые или создаваемые).</span><span class="sxs-lookup"><span data-stu-id="43d09-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="43d09-111">справочных файлов;</span><span class="sxs-lookup"><span data-stu-id="43d09-111">Directory files</span></span>
* <span data-ttu-id="43d09-112">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="43d09-112">Environment variables</span></span>
* <span data-ttu-id="43d09-113">объектов .NET в памяти;</span><span class="sxs-lookup"><span data-stu-id="43d09-113">In-memory .NET objects</span></span>
* <span data-ttu-id="43d09-114">файлов параметров;</span><span class="sxs-lookup"><span data-stu-id="43d09-114">Settings files</span></span>

<span data-ttu-id="43d09-115">Пакеты конфигурации для распространенных вариантов провайдеров конфигурации ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) включаются платформой неявным образом.</span><span class="sxs-lookup"><span data-stu-id="43d09-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

<span data-ttu-id="43d09-116">В приведенных ниже и представленных в образце приложения примерах кода используется пространство имен <xref:Microsoft.Extensions.Configuration>:</span><span class="sxs-lookup"><span data-stu-id="43d09-116">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="43d09-117">*Шаблон параметров* является расширением конфигурации основных понятий, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="43d09-117">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="43d09-118">Параметры используют классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="43d09-118">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="43d09-119">Для получения дополнительной информации см. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="43d09-119">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="43d09-120">[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="43d09-120">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="43d09-121">Конфигурация узла и приложения</span><span class="sxs-lookup"><span data-stu-id="43d09-121">Host versus app configuration</span></span>

<span data-ttu-id="43d09-122">Перед настройкой и запуском приложения настройте и запустите *узел*.</span><span class="sxs-lookup"><span data-stu-id="43d09-122">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="43d09-123">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="43d09-123">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="43d09-124">Как приложение, так и узел настраиваются с использованием поставщиков конфигурации, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="43d09-124">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="43d09-125">Пары "ключ — значение" конфигурации узлов также включаются в конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="43d09-125">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="43d09-126">Дополнительные сведения о том, как используются поставщики конфигурации при создании узла и как влияют источники конфигурации на узел, см. в разделе <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="43d09-126">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="43d09-127">Другая конфигурация</span><span class="sxs-lookup"><span data-stu-id="43d09-127">Other configuration</span></span>

<span data-ttu-id="43d09-128">Этот раздел относится только к *конфигурации приложений*.</span><span class="sxs-lookup"><span data-stu-id="43d09-128">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="43d09-129">Другие аспекты запуска и размещения приложений ASP.NET Core настраиваются с помощью файлов конфигурации, которые не рассматриваются в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="43d09-129">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="43d09-130">*launch.json*/*launchSettings.json* — это файлы конфигурации инструментов для среды разработки, описанные в следующих источниках:</span><span class="sxs-lookup"><span data-stu-id="43d09-130">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="43d09-131">В <xref:fundamentals/environments#development>.</span><span class="sxs-lookup"><span data-stu-id="43d09-131">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="43d09-132">В документации, где показано, как файлы используются для настройки приложений ASP.NET Core для сценариев разработки.</span><span class="sxs-lookup"><span data-stu-id="43d09-132">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="43d09-133">*web.config* — это файл конфигурации сервера, описанный в следующих источниках:</span><span class="sxs-lookup"><span data-stu-id="43d09-133">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="43d09-134">См. сведения о переносе конфигурации приложения из более ранних версий ASP.NET: <xref:migration/proper-to-2x/index#store-configurations>.</span><span class="sxs-lookup"><span data-stu-id="43d09-134">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="43d09-135">Конфигурация по умолчанию</span><span class="sxs-lookup"><span data-stu-id="43d09-135">Default configuration</span></span>

<span data-ttu-id="43d09-136">Веб-приложения на основе шаблонов [dotnet new](/dotnet/core/tools/dotnet-new) ASP.NET Core вызывают <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> при создании узла.</span><span class="sxs-lookup"><span data-stu-id="43d09-136">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="43d09-137">`CreateDefaultBuilder` предоставляет конфигурацию по умолчанию для приложения в следующем порядке:</span><span class="sxs-lookup"><span data-stu-id="43d09-137">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="43d09-138">Приведенные ниже сведения относятся к приложениям, использующим [универсальный узел](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="43d09-138">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="43d09-139">Подробные сведения о конфигурации по умолчанию при использовании [веб-узла](xref:fundamentals/host/web-host) см. в [разделе о версии ASP.NET Core 2.2 в этой статье](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="43d09-139">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="43d09-140">Существуют следующие способы предоставления конфигурации узла.</span><span class="sxs-lookup"><span data-stu-id="43d09-140">Host configuration is provided from:</span></span>
  * <span data-ttu-id="43d09-141">Переменные среды с префиксом `DOTNET_` (например, `DOTNET_ENVIRONMENT`), использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="43d09-141">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="43d09-142">Префикс (`DOTNET_`) исключается при загрузке пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-142">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="43d09-143">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="43d09-143">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="43d09-144">Устанавливается конфигурация веб-узла по умолчанию (`ConfigureWebHostDefaults`):</span><span class="sxs-lookup"><span data-stu-id="43d09-144">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="43d09-145">Kestrel используется в качестве веб-сервера и настраивается с помощью поставщиков конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="43d09-145">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="43d09-146">Добавьте ПО промежуточного слоя фильтрации узлов.</span><span class="sxs-lookup"><span data-stu-id="43d09-146">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="43d09-147">Если переменной среды `ASPNETCORE_FORWARDEDHEADERS_ENABLED` присвоено значение `true`, добавьте ПО промежуточного слоя перенаправления заголовков.</span><span class="sxs-lookup"><span data-stu-id="43d09-147">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="43d09-148">Включите интеграцию служб IIS.</span><span class="sxs-lookup"><span data-stu-id="43d09-148">Enable IIS integration.</span></span>
* <span data-ttu-id="43d09-149">Конфигурация приложения предоставляется из следующих ресурсов:</span><span class="sxs-lookup"><span data-stu-id="43d09-149">App configuration is provided from:</span></span>
  * <span data-ttu-id="43d09-150">Файл *appsettings.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="43d09-150">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="43d09-151">Файл *appsettings.{Environment}.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="43d09-151">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="43d09-152">[Менеджера секретов](xref:security/app-secrets), когда приложение выполняется в среде `Development` с использованием начальных сборок.</span><span class="sxs-lookup"><span data-stu-id="43d09-152">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="43d09-153">Переменные среды, использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="43d09-153">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="43d09-154">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="43d09-154">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="43d09-155">Безопасность</span><span class="sxs-lookup"><span data-stu-id="43d09-155">Security</span></span>

<span data-ttu-id="43d09-156">Применяйте описанные ниже методики для защиты конфиденциальных данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-156">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="43d09-157">Никогда не храните пароли или другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации обычного текста.</span><span class="sxs-lookup"><span data-stu-id="43d09-157">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="43d09-158">Не используйте секреты рабочей среды в средах разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="43d09-158">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="43d09-159">Указывайте секреты вне проекта, чтобы их нельзя было случайно зафиксировать в репозитории с исходным кодом.</span><span class="sxs-lookup"><span data-stu-id="43d09-159">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="43d09-160">Дополнительные сведения см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="43d09-160">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="43d09-161"><xref:security/app-secrets> &ndash; содержит рекомендации по использованию переменных среды для хранения конфиденциальных данных.</span><span class="sxs-lookup"><span data-stu-id="43d09-161"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="43d09-162">Менеджер секретов использует поставщик конфигурации файла для хранения конфиденциальных данных пользователя в файле JSON в локальной системе.</span><span class="sxs-lookup"><span data-stu-id="43d09-162">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="43d09-163">Поставщик конфигурации файлов описан ниже в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="43d09-163">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="43d09-164">В [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) безопасно хранятся секреты приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="43d09-164">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="43d09-165">Для получения дополнительной информации см. <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="43d09-165">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="43d09-166">Иерархическая модель конфигурации</span><span class="sxs-lookup"><span data-stu-id="43d09-166">Hierarchical configuration data</span></span>

<span data-ttu-id="43d09-167">API конфигурации способен поддерживать иерархические данные конфигурации, выполняя преобразование в плоскую структуру иерархических данных с использованием разделителя в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-167">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="43d09-168">В следующем файле JSON существуют четыре ключа в структурированной иерархии двух разделов.</span><span class="sxs-lookup"><span data-stu-id="43d09-168">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="43d09-169">При считывании файла в конфигурацию для сохранения исходной иерархической структуры данных источника конфигурации создаются уникальные ключи.</span><span class="sxs-lookup"><span data-stu-id="43d09-169">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="43d09-170">Разделы и ключи преобразовываются в плоскую структуру с использованием двоеточия (`:`) для сохранения исходной структуры.</span><span class="sxs-lookup"><span data-stu-id="43d09-170">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="43d09-171">section0:key0</span><span class="sxs-lookup"><span data-stu-id="43d09-171">section0:key0</span></span>
* <span data-ttu-id="43d09-172">section0:key1</span><span class="sxs-lookup"><span data-stu-id="43d09-172">section0:key1</span></span>
* <span data-ttu-id="43d09-173">section1:key0</span><span class="sxs-lookup"><span data-stu-id="43d09-173">section1:key0</span></span>
* <span data-ttu-id="43d09-174">section1:key1</span><span class="sxs-lookup"><span data-stu-id="43d09-174">section1:key1</span></span>

<span data-ttu-id="43d09-175">Методы <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> и <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> доступны для изолирования разделов и дочерних элементов раздела в данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-175"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="43d09-176">Эти методы описаны далее в разделе [GetSection, GetChildren и Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="43d09-176">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="43d09-177">Соглашения</span><span class="sxs-lookup"><span data-stu-id="43d09-177">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="43d09-178">Источники и поставщики</span><span class="sxs-lookup"><span data-stu-id="43d09-178">Sources and providers</span></span>

<span data-ttu-id="43d09-179">При запуске приложения источники конфигурации считываются в порядке, в котором были указаны их поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-179">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="43d09-180">Поставщики конфигурации, которые реализуют обнаружение изменений, имеют возможность перезагрузить конфигурацию при изменении базовых параметров.</span><span class="sxs-lookup"><span data-stu-id="43d09-180">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="43d09-181">Так, поставщик файла конфигурации (подробнее о нем далее в этой статье) и [поставщик конфигурации Azure Key Vault](xref:security/key-vault-configuration) реализуют обнаружение изменений.</span><span class="sxs-lookup"><span data-stu-id="43d09-181">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="43d09-182">Объект <xref:Microsoft.Extensions.Configuration.IConfiguration> доступен в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) приложения.</span><span class="sxs-lookup"><span data-stu-id="43d09-182"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="43d09-183"><xref:Microsoft.Extensions.Configuration.IConfiguration> можно внедрить в <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> Razor Pages или <xref:Microsoft.AspNetCore.Mvc.Controller> MVC, чтобы получить конфигурацию для класса.</span><span class="sxs-lookup"><span data-stu-id="43d09-183"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="43d09-184">В следующих примерах поле `_config` используется для доступа к значениям конфигурации:</span><span class="sxs-lookup"><span data-stu-id="43d09-184">In the following examples, the `_config` field is used to access configuration values:</span></span>

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

<span data-ttu-id="43d09-185">Поставщики конфигурации не могут использовать контейнер DI, так как он недоступен при настройке узла.</span><span class="sxs-lookup"><span data-stu-id="43d09-185">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="43d09-186">Клавиши</span><span class="sxs-lookup"><span data-stu-id="43d09-186">Keys</span></span>

<span data-ttu-id="43d09-187">В ключах конфигурации приняты следующие соглашения.</span><span class="sxs-lookup"><span data-stu-id="43d09-187">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="43d09-188">В ключах не учитывается регистр символов.</span><span class="sxs-lookup"><span data-stu-id="43d09-188">Keys are case-insensitive.</span></span> <span data-ttu-id="43d09-189">Например `ConnectionString` и `connectionstring` обрабатываются как эквивалентные ключи.</span><span class="sxs-lookup"><span data-stu-id="43d09-189">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="43d09-190">Если значение для одного и того же ключа установлено одним и тем же или разными поставщиками конфигурации, последним значением, установленным на ключе, является используемое значение.</span><span class="sxs-lookup"><span data-stu-id="43d09-190">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="43d09-191">Иерархические ключи</span><span class="sxs-lookup"><span data-stu-id="43d09-191">Hierarchical keys</span></span>
  * <span data-ttu-id="43d09-192">При взаимодействии с API конфигурации разделитель-двоеточие (`:`) поддерживается на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="43d09-192">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="43d09-193">В переменных среды разделитель-двоеточие может не работать на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="43d09-193">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="43d09-194">Двойной знак подчеркивания (`__`) поддерживается на всех платформах и автоматически преобразовывается в двоеточие.</span><span class="sxs-lookup"><span data-stu-id="43d09-194">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="43d09-195">В хранилище ключей Azure иерархические ключи используют `--` (два дефиса) в качестве разделителя.</span><span class="sxs-lookup"><span data-stu-id="43d09-195">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="43d09-196">Чтобы заменить дефисы двоеточием, при загрузке секретов в конфигурацию приложения напишите код.</span><span class="sxs-lookup"><span data-stu-id="43d09-196">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="43d09-197"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-197">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="43d09-198">Привязка массива описана в разделе [Привязка массива к классу](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="43d09-198">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="43d09-199">Значения</span><span class="sxs-lookup"><span data-stu-id="43d09-199">Values</span></span>

<span data-ttu-id="43d09-200">В значениях конфигурации учитываются следующие соглашения.</span><span class="sxs-lookup"><span data-stu-id="43d09-200">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="43d09-201">Значения являются строками.</span><span class="sxs-lookup"><span data-stu-id="43d09-201">Values are strings.</span></span>
* <span data-ttu-id="43d09-202">Значение NULL не может храниться в конфигурации или быть привязанным к объектам.</span><span class="sxs-lookup"><span data-stu-id="43d09-202">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="43d09-203">Поставщики</span><span class="sxs-lookup"><span data-stu-id="43d09-203">Providers</span></span>

<span data-ttu-id="43d09-204">В следующей таблице показаны поставщики конфигурации, доступные для приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="43d09-204">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="43d09-205">Поставщик</span><span class="sxs-lookup"><span data-stu-id="43d09-205">Provider</span></span> | <span data-ttu-id="43d09-206">Предоставляет конфигурацию из &hellip;</span><span class="sxs-lookup"><span data-stu-id="43d09-206">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="43d09-207">[Поставщик конфигурации хранилища ключей Azure](xref:security/key-vault-configuration) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="43d09-207">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="43d09-208">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="43d09-208">Azure Key Vault</span></span> |
| <span data-ttu-id="43d09-209">[Поставщик конфигурации приложений Azure](/azure/azure-app-configuration/quickstart-aspnet-core-app) (документация Azure)</span><span class="sxs-lookup"><span data-stu-id="43d09-209">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="43d09-210">конфигурация приложения Azure;</span><span class="sxs-lookup"><span data-stu-id="43d09-210">Azure App Configuration</span></span> |
| [<span data-ttu-id="43d09-211">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="43d09-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="43d09-212">Параметры командной строки</span><span class="sxs-lookup"><span data-stu-id="43d09-212">Command-line parameters</span></span> |
| [<span data-ttu-id="43d09-213">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="43d09-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="43d09-214">Источник пользователя</span><span class="sxs-lookup"><span data-stu-id="43d09-214">Custom source</span></span> |
| [<span data-ttu-id="43d09-215">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="43d09-215">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="43d09-216">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="43d09-216">Environment variables</span></span> |
| [<span data-ttu-id="43d09-217">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="43d09-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="43d09-218">Файлы (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="43d09-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="43d09-219">Поставщик конфигурации ключа для каждого файла</span><span class="sxs-lookup"><span data-stu-id="43d09-219">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="43d09-220">Справочные файлы</span><span class="sxs-lookup"><span data-stu-id="43d09-220">Directory files</span></span> |
| [<span data-ttu-id="43d09-221">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="43d09-221">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="43d09-222">Коллекции оперативной памяти</span><span class="sxs-lookup"><span data-stu-id="43d09-222">In-memory collections</span></span> |
| <span data-ttu-id="43d09-223">[Секреты пользователей (Менеджер секретов)](xref:security/app-secrets) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="43d09-223">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="43d09-224">Файл в каталоге профиля пользователя</span><span class="sxs-lookup"><span data-stu-id="43d09-224">File in the user profile directory</span></span> |

<span data-ttu-id="43d09-225">Источники конфигурации считываются в том порядке, в котором при запуске указываются их поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-225">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="43d09-226">Поставщики конфигурации в этом разделе описаны в алфавитном порядке, а не в необходимом вам порядке в коде.</span><span class="sxs-lookup"><span data-stu-id="43d09-226">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="43d09-227">Порядок поставщиков конфигурации в коде соответствует приоритетам ваших основных источников конфигурации, требуемых приложением.</span><span class="sxs-lookup"><span data-stu-id="43d09-227">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="43d09-228">Типичная последовательность поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-228">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="43d09-229">Файлы (*appsettings.json*, *appsettings.{Environment}.json*, где `{Environment}` — это текущая среда размещения приложения)</span><span class="sxs-lookup"><span data-stu-id="43d09-229">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. <span data-ttu-id="43d09-230">[Azure Key Vault](xref:security/key-vault-configuration);</span><span class="sxs-lookup"><span data-stu-id="43d09-230">[Azure Key Vault](xref:security/key-vault-configuration)</span></span>
1. <span data-ttu-id="43d09-231">[Секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки)</span><span class="sxs-lookup"><span data-stu-id="43d09-231">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="43d09-232">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="43d09-232">Environment variables</span></span>
1. <span data-ttu-id="43d09-233">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="43d09-233">Command-line arguments</span></span>

<span data-ttu-id="43d09-234">Общепринятой практикой является размещение поставщика конфигурации командной строки последним в ряду поставщиков, чтобы аргументы командной строки могли переопределять конфигурацию, установленную другими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="43d09-234">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="43d09-235">Предыдущая последовательность поставщиков используется при инициализации нового построителя узла с помощью `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="43d09-235">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="43d09-236">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="43d09-236">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="43d09-237">Настройка построителя узла с помощью ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="43d09-237">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="43d09-238">Чтобы настроить построитель узла, вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> и укажите конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="43d09-238">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="43d09-239">`ConfigureHostConfiguration` используется для инициализации <xref:Microsoft.Extensions.Hosting.IHostEnvironment> для дальнейшего использования в процессе сборки.</span><span class="sxs-lookup"><span data-stu-id="43d09-239">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="43d09-240">Метод `ConfigureHostConfiguration` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="43d09-240">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

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

## <a name="configureappconfiguration"></a><span data-ttu-id="43d09-241">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="43d09-241">ConfigureAppConfiguration</span></span>

<span data-ttu-id="43d09-242">Вызовите `ConfigureAppConfiguration` при сборке узла, чтобы указать поставщики конфигурации приложения в дополнение к тем, которые автоматически добавлены `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="43d09-242">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="43d09-243">Переопределение предыдущей конфигурации с помощью аргументов командной строки</span><span class="sxs-lookup"><span data-stu-id="43d09-243">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="43d09-244">Чтобы предоставить конфигурацию приложения, которую можно переопределить с помощью аргументов командной строки, вызовите метод `AddCommandLine` последним:</span><span class="sxs-lookup"><span data-stu-id="43d09-244">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="43d09-245">Удаление поставщиков, добавленных CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="43d09-245">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="43d09-246">Чтобы удалить поставщиков, добавленных `CreateDefaultBuilder`, сначала вызовите [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) в [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources):</span><span class="sxs-lookup"><span data-stu-id="43d09-246">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="43d09-247">Использование конфигурации во время запуска приложения</span><span class="sxs-lookup"><span data-stu-id="43d09-247">Consume configuration during app startup</span></span>

<span data-ttu-id="43d09-248">Конфигурация, предоставленная приложению в `ConfigureAppConfiguration`, доступна во время запуска приложения, включая `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="43d09-248">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="43d09-249">Дополнительные сведения см. в разделе [Доступ к конфигурации во время запуска](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="43d09-249">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="43d09-250">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="43d09-250">Command-line Configuration Provider</span></span>

<span data-ttu-id="43d09-251"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> загружает конфигурацию из пары "ключ — значение" аргумента командной строки в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="43d09-251">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="43d09-252">Чтобы активировать конфигурацию командной строки, вызывается метод расширения <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> для экземпляра <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="43d09-252">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="43d09-253">`AddCommandLine` вызывается автоматически при вызове `CreateDefaultBuilder(string [])`.</span><span class="sxs-lookup"><span data-stu-id="43d09-253">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="43d09-254">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="43d09-254">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="43d09-255">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="43d09-255">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="43d09-256">дополнительную конфигурацию из файлов *appsettings.json* и *appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="43d09-256">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="43d09-257">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="43d09-257">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="43d09-258">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="43d09-258">Environment variables.</span></span>

<span data-ttu-id="43d09-259">`CreateDefaultBuilder` добавляет последним поставщика конфигурации командной строки.</span><span class="sxs-lookup"><span data-stu-id="43d09-259">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="43d09-260">Аргументы командной строки передаются в набор конфигурации переопределения среды выполнения, установленной другими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="43d09-260">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="43d09-261">`CreateDefaultBuilder` действует, когда создается узел.</span><span class="sxs-lookup"><span data-stu-id="43d09-261">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="43d09-262">Поэтому конфигурация командной строки, активированная с помощью `CreateDefaultBuilder`, может повлиять на настройку узла.</span><span class="sxs-lookup"><span data-stu-id="43d09-262">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="43d09-263">Для приложений на основе шаблонов ASP.NET Core метод `AddCommandLine` уже был вызван методом `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="43d09-263">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="43d09-264">Чтобы добавить дополнительные поставщики конфигурации и обеспечить возможность переопределения конфигурации от этих поставщиков с помощью аргументов командной строки, вызовите дополнительные поставщики приложения в `ConfigureAppConfiguration` и вызовите `AddCommandLine` в последнюю очередь.</span><span class="sxs-lookup"><span data-stu-id="43d09-264">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="43d09-265">**Пример**</span><span class="sxs-lookup"><span data-stu-id="43d09-265">**Example**</span></span>

<span data-ttu-id="43d09-266">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="43d09-266">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="43d09-267">Откройте командную строку в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="43d09-267">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="43d09-268">Поставьте аргумент командной строки в команду `dotnet run`: `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="43d09-268">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="43d09-269">После запуска приложения откройте браузер в приложении по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="43d09-269">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="43d09-270">Обратите внимание, что вывод содержит пару "ключ — значение" для аргумента командной строки конфигурации, предоставленного для `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="43d09-270">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="43d09-271">Аргументы</span><span class="sxs-lookup"><span data-stu-id="43d09-271">Arguments</span></span>

<span data-ttu-id="43d09-272">Значение должно соответствовать знаку равенства (`=`), или ключ должен иметь префикс (`--` или `/`), когда значение следует за пробелом.</span><span class="sxs-lookup"><span data-stu-id="43d09-272">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="43d09-273">Значение не требуется, если используется знак равенства (например, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="43d09-273">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="43d09-274">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="43d09-274">Key prefix</span></span>               | <span data-ttu-id="43d09-275">Пример</span><span class="sxs-lookup"><span data-stu-id="43d09-275">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="43d09-276">Без префикса</span><span class="sxs-lookup"><span data-stu-id="43d09-276">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="43d09-277">Два дефиса (`--`)</span><span class="sxs-lookup"><span data-stu-id="43d09-277">Two dashes (`--`)</span></span>        | <span data-ttu-id="43d09-278">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="43d09-278">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="43d09-279">Прямая косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="43d09-279">Forward slash (`/`)</span></span>      | <span data-ttu-id="43d09-280">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="43d09-280">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="43d09-281">В рамках одной и той же команды не смешивайте пары "ключ — значение" аргумента командной строки, которые используют знак равенства, с парами "ключ — значение", которые используют пробел.</span><span class="sxs-lookup"><span data-stu-id="43d09-281">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="43d09-282">Примеры команд.</span><span class="sxs-lookup"><span data-stu-id="43d09-282">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="43d09-283">Сопоставления переключений</span><span class="sxs-lookup"><span data-stu-id="43d09-283">Switch mappings</span></span>

<span data-ttu-id="43d09-284">Сопоставление параметров позволяет указать логику замены имен ключей.</span><span class="sxs-lookup"><span data-stu-id="43d09-284">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="43d09-285">Когда вручную создается конфигурация с помощью <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, методу <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> можно предоставить словарь сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="43d09-285">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="43d09-286">В словаре сопоставлений переключений выполняется поиск ключа, который совпадает с ключом, предоставляемым аргументом командной строки.</span><span class="sxs-lookup"><span data-stu-id="43d09-286">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="43d09-287">Если ключ в командной строке находится в словаре, значение словаря (замена ключа) передается обратно, чтобы установить пару "ключ — значение" в конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="43d09-287">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="43d09-288">Сопоставление переключений необходимо для любого ключа командной строки с префиксом из одного дефиса (`-`).</span><span class="sxs-lookup"><span data-stu-id="43d09-288">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="43d09-289">Правила ключей из словаря сопоставления переключений:</span><span class="sxs-lookup"><span data-stu-id="43d09-289">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="43d09-290">Переключения должны начинаться с дефиса (`-`) или двойного дефиса (`--`).</span><span class="sxs-lookup"><span data-stu-id="43d09-290">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="43d09-291">Словарь сопоставлений переключений не должен содержать повторяющиеся ключи.</span><span class="sxs-lookup"><span data-stu-id="43d09-291">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="43d09-292">Создайте словарь сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="43d09-292">Create a switch mappings dictionary.</span></span> <span data-ttu-id="43d09-293">В следующем примере создаются два сопоставления переключений:</span><span class="sxs-lookup"><span data-stu-id="43d09-293">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="43d09-294">При сборке узла вызовите `AddCommandLine` со словарем сопоставлений переключений:</span><span class="sxs-lookup"><span data-stu-id="43d09-294">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="43d09-295">Для приложений, использующих сопоставления переключений, в вызове `CreateDefaultBuilder` аргументы передаваться не должны.</span><span class="sxs-lookup"><span data-stu-id="43d09-295">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="43d09-296">Вызов команды `AddCommandLine` метода `CreateDefaultBuilder` не включает сопоставленные переключения, и нет возможности передать словарь сопоставления переключений в `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="43d09-296">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="43d09-297">Чтобы решить эту проблему, нужно не передавать аргументы команде `CreateDefaultBuilder`, а позволить методу `AddCommandLine` метода `ConfigurationBuilder` обрабатывать как аргументы, так и словарь сопоставления параметров.</span><span class="sxs-lookup"><span data-stu-id="43d09-297">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="43d09-298">Созданный словарь сопоставлений переключений содержит данные, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="43d09-298">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="43d09-299">Ключ</span><span class="sxs-lookup"><span data-stu-id="43d09-299">Key</span></span>       | <span data-ttu-id="43d09-300">Значение</span><span class="sxs-lookup"><span data-stu-id="43d09-300">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="43d09-301">Если ключи сопоставления переключений используются при запуске приложения, конфигурация принимает значение конфигурации в ключе, предоставленном в словаре.</span><span class="sxs-lookup"><span data-stu-id="43d09-301">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="43d09-302">После выполнения предыдущей команды конфигурация содержит значения, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="43d09-302">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="43d09-303">Ключ</span><span class="sxs-lookup"><span data-stu-id="43d09-303">Key</span></span>               | <span data-ttu-id="43d09-304">Значение</span><span class="sxs-lookup"><span data-stu-id="43d09-304">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="43d09-305">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="43d09-305">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="43d09-306"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> загружает конфигурацию из пары "ключ — значение" переменной среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="43d09-306">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="43d09-307">Чтобы активировать конфигурацию переменных среды, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="43d09-307">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="43d09-308">[Служба приложений Azure](https://azure.microsoft.com/services/app-service/) позволяет задать переменные среды на портале Azure, который может переопределить конфигурацию приложения, используя поставщик конфигурации переменных среды.</span><span class="sxs-lookup"><span data-stu-id="43d09-308">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="43d09-309">Дополнительные сведения см. в руководстве по [переопределению конфигурации приложения Azure с помощью портала Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="43d09-309">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="43d09-310">`AddEnvironmentVariables` используется для загрузки переменных среды, имеющих префикс `DOTNET_`, для [конфигурации узла](#host-versus-app-configuration) при инициализации нового построителя узла с [универсальным узлом](xref:fundamentals/host/generic-host) и вызове `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="43d09-310">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="43d09-311">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="43d09-311">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="43d09-312">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="43d09-312">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="43d09-313">конфигурация приложения на основе переменных среды без префикса путем вызова `AddEnvironmentVariables` без префикса;</span><span class="sxs-lookup"><span data-stu-id="43d09-313">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="43d09-314">дополнительную конфигурацию из файлов *appsettings.json* и *appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="43d09-314">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="43d09-315">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="43d09-315">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="43d09-316">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="43d09-316">Command-line arguments.</span></span>

<span data-ttu-id="43d09-317">Поставщик конфигурации переменных среды вызывается после выполнения настройки с помощью секретов пользователя и файлов *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="43d09-317">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="43d09-318">Вызов поставщика в этой позиции разрешает чтение переменных среды выполнения, чтобы переопределить конфигурацию, заданную секретом пользователя и файлом *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="43d09-318">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="43d09-319">Чтобы добавить конфигурацию приложения из дополнительных переменных среды, вызовите дополнительные поставщики приложения в `ConfigureAppConfiguration`, а затем вызовите `AddEnvironmentVariables` с префиксом:</span><span class="sxs-lookup"><span data-stu-id="43d09-319">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="43d09-320">Вызовите `AddEnvironmentVariables` последним, чтобы разрешить переменным среды с заданным префиксом переопределять значения от других поставщиков.</span><span class="sxs-lookup"><span data-stu-id="43d09-320">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="43d09-321">**Пример**</span><span class="sxs-lookup"><span data-stu-id="43d09-321">**Example**</span></span>

<span data-ttu-id="43d09-322">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="43d09-322">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="43d09-323">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="43d09-323">Run the sample app.</span></span> <span data-ttu-id="43d09-324">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="43d09-324">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="43d09-325">Обратите внимание, что выходные данные содержат пару "ключ — значение" для переменной среды `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="43d09-325">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="43d09-326">Значение отражает среду, в которой выполняется приложение, обычно при локальном запуске это `Development`.</span><span class="sxs-lookup"><span data-stu-id="43d09-326">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="43d09-327">Чтобы список переменных среды, отображаемый приложением, был коротким, приложение фильтрует переменные среды.</span><span class="sxs-lookup"><span data-stu-id="43d09-327">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="43d09-328">См. пример файла *Pages/Index.cshtml.cs* приложения.</span><span class="sxs-lookup"><span data-stu-id="43d09-328">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="43d09-329">Чтобы просмотреть все переменные среды, доступные для приложения, измените значение `FilteredConfiguration` в *Pages/Index.cshtml.cs* на следующее:</span><span class="sxs-lookup"><span data-stu-id="43d09-329">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="43d09-330">Префиксы</span><span class="sxs-lookup"><span data-stu-id="43d09-330">Prefixes</span></span>

<span data-ttu-id="43d09-331">Переменные среды, которые загружаются в конфигурацию приложения, фильтруются при добавлении префикса к методу `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="43d09-331">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="43d09-332">Например, чтобы отфильтровать переменные среды по префиксу `CUSTOM_`, введите префикс поставщику конфигурации:</span><span class="sxs-lookup"><span data-stu-id="43d09-332">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="43d09-333">Префикс отделяется при создании пары конфигурации "ключ — значение".</span><span class="sxs-lookup"><span data-stu-id="43d09-333">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="43d09-334">При создании построителя узла конфигурация узла предоставляется переменными среды.</span><span class="sxs-lookup"><span data-stu-id="43d09-334">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="43d09-335">Дополнительные сведения о префиксе, используемом для этих переменных среды, см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="43d09-335">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="43d09-336">**Префиксы строк подключения**</span><span class="sxs-lookup"><span data-stu-id="43d09-336">**Connection string prefixes**</span></span>

<span data-ttu-id="43d09-337">API конфигурации имеет специальные правила обработки для четырех строк подключения переменных среды, связанных с настройкой строк подключения Azure для среды приложения.</span><span class="sxs-lookup"><span data-stu-id="43d09-337">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="43d09-338">Если префикс не указан в `AddEnvironmentVariables`, переменные среды с префиксами, указанными в таблице, загружаются в приложение.</span><span class="sxs-lookup"><span data-stu-id="43d09-338">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="43d09-339">Префикс строки подключения</span><span class="sxs-lookup"><span data-stu-id="43d09-339">Connection string prefix</span></span> | <span data-ttu-id="43d09-340">Поставщик</span><span class="sxs-lookup"><span data-stu-id="43d09-340">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="43d09-341">Поставщик пользователя</span><span class="sxs-lookup"><span data-stu-id="43d09-341">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="43d09-342">MySQL</span><span class="sxs-lookup"><span data-stu-id="43d09-342">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="43d09-343">База данных SQL Azure</span><span class="sxs-lookup"><span data-stu-id="43d09-343">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="43d09-344">SQL Server</span><span class="sxs-lookup"><span data-stu-id="43d09-344">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="43d09-345">Когда переменная среды обнаруживается и загружается в конфигурацию с одним из четырех префиксов, приведенных в таблице, происходит следующее.</span><span class="sxs-lookup"><span data-stu-id="43d09-345">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="43d09-346">Ключ конфигурации создается путем удаления префикса переменных среды и добавления ключа раздела конфигурации (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="43d09-346">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="43d09-347">Создается новая пара "ключ — значение" конфигурации, которая представляет поставщика подключения базы данных (за исключением `CUSTOMCONNSTR_`, который не имеет указанного поставщика).</span><span class="sxs-lookup"><span data-stu-id="43d09-347">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="43d09-348">Ключ переменной среды</span><span class="sxs-lookup"><span data-stu-id="43d09-348">Environment variable key</span></span> | <span data-ttu-id="43d09-349">Преобразованный ключ конфигурации</span><span class="sxs-lookup"><span data-stu-id="43d09-349">Converted configuration key</span></span> | <span data-ttu-id="43d09-350">Запись конфигурации поставщика</span><span class="sxs-lookup"><span data-stu-id="43d09-350">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="43d09-351">Запись конфигурации не создана.</span><span class="sxs-lookup"><span data-stu-id="43d09-351">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="43d09-352">Ключ: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="43d09-352">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="43d09-353">Значение: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="43d09-353">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="43d09-354">Ключ: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="43d09-354">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="43d09-355">Значение: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="43d09-355">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="43d09-356">Ключ: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="43d09-356">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="43d09-357">Значение: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="43d09-357">Value: `System.Data.SqlClient`</span></span>  |

<span data-ttu-id="43d09-358">**Пример**</span><span class="sxs-lookup"><span data-stu-id="43d09-358">**Example**</span></span>

<span data-ttu-id="43d09-359">На сервере создается пользовательская переменная среды строки подключения:</span><span class="sxs-lookup"><span data-stu-id="43d09-359">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="43d09-360">Имя &ndash; `CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="43d09-360">Name &ndash; `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="43d09-361">Значение &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span><span class="sxs-lookup"><span data-stu-id="43d09-361">Value &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="43d09-362">Если `IConfiguration` вставляется и назначается полю с именем `_config`, считайте значение:</span><span class="sxs-lookup"><span data-stu-id="43d09-362">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="43d09-363">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="43d09-363">File Configuration Provider</span></span>

<span data-ttu-id="43d09-364"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> является базовым классом для загрузки конфигурации из файловой системы.</span><span class="sxs-lookup"><span data-stu-id="43d09-364"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="43d09-365">Следующие поставщики конфигурации предназначены для определенных типов файлов:</span><span class="sxs-lookup"><span data-stu-id="43d09-365">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="43d09-366">Поставщик конфигурации INI</span><span class="sxs-lookup"><span data-stu-id="43d09-366">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="43d09-367">Поставщик конфигурации JSON</span><span class="sxs-lookup"><span data-stu-id="43d09-367">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="43d09-368">Поставщик конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="43d09-368">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="43d09-369">Поставщик конфигурации INI</span><span class="sxs-lookup"><span data-stu-id="43d09-369">INI Configuration Provider</span></span>

<span data-ttu-id="43d09-370"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> загружает конфигурацию из пары "ключ — значение" INI-файла во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="43d09-370">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="43d09-371">Чтобы активировать конфигурацию INI-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="43d09-371">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="43d09-372">Двоеточие можно использовать как разделитель раздела в конфигурации файла INI.</span><span class="sxs-lookup"><span data-stu-id="43d09-372">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="43d09-373">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="43d09-373">Overloads permit specifying:</span></span>

* <span data-ttu-id="43d09-374">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="43d09-374">Whether the file is optional.</span></span>
* <span data-ttu-id="43d09-375">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="43d09-375">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="43d09-376"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="43d09-376">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="43d09-377">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="43d09-377">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="43d09-378">Общий пример конфигурации INI-файла.</span><span class="sxs-lookup"><span data-stu-id="43d09-378">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="43d09-379">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="43d09-379">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="43d09-380">section0:key0</span><span class="sxs-lookup"><span data-stu-id="43d09-380">section0:key0</span></span>
* <span data-ttu-id="43d09-381">section0:key1</span><span class="sxs-lookup"><span data-stu-id="43d09-381">section0:key1</span></span>
* <span data-ttu-id="43d09-382">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="43d09-382">section1:subsection:key</span></span>
* <span data-ttu-id="43d09-383">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="43d09-383">section2:subsection0:key</span></span>
* <span data-ttu-id="43d09-384">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="43d09-384">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="43d09-385">Поставщик конфигурации JSON</span><span class="sxs-lookup"><span data-stu-id="43d09-385">JSON Configuration Provider</span></span>

<span data-ttu-id="43d09-386"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> загружает конфигурацию из пары "ключ — значение" JSON-файла в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="43d09-386">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="43d09-387">Чтобы активировать конфигурацию JSON-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="43d09-387">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="43d09-388">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="43d09-388">Overloads permit specifying:</span></span>

* <span data-ttu-id="43d09-389">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="43d09-389">Whether the file is optional.</span></span>
* <span data-ttu-id="43d09-390">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="43d09-390">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="43d09-391"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="43d09-391">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="43d09-392">`AddJsonFile` автоматически вызывается дважды при инициализации нового построителя узла с `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="43d09-392">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="43d09-393">Метод вызывается для загрузки конфигурации из:</span><span class="sxs-lookup"><span data-stu-id="43d09-393">The method is called to load configuration from:</span></span>

* <span data-ttu-id="43d09-394">*appsettings.json* &ndash; первым читается этот файл.</span><span class="sxs-lookup"><span data-stu-id="43d09-394">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="43d09-395">Версия файла среды может переопределить значения, предоставленные *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="43d09-395">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="43d09-396">*appsettings.{Environment}.json* &ndash; версия среды файла загружается на основе [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="43d09-396">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="43d09-397">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="43d09-397">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="43d09-398">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="43d09-398">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="43d09-399">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="43d09-399">Environment variables.</span></span>
* <span data-ttu-id="43d09-400">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="43d09-400">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="43d09-401">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="43d09-401">Command-line arguments.</span></span>

<span data-ttu-id="43d09-402">Сначала устанавливается поставщик конфигурации JSON-файлов.</span><span class="sxs-lookup"><span data-stu-id="43d09-402">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="43d09-403">Таким образом, секреты пользователя, переменные среды и аргументы командной строки переопределят конфигурацию, заданную файлами *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="43d09-403">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="43d09-404">Вызовите `ConfigureAppConfiguration` при сборке узла, чтобы указать конфигурацию приложения для файлов, отличных от *appsettings.json* и *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="43d09-404">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="43d09-405">**Пример**</span><span class="sxs-lookup"><span data-stu-id="43d09-405">**Example**</span></span>

<span data-ttu-id="43d09-406">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает два вызова `AddJsonFile`:</span><span class="sxs-lookup"><span data-stu-id="43d09-406">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="43d09-407">Первый вызов `AddJsonFile` загружает конфигурацию из *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="43d09-407">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="43d09-408">Второй вызов `AddJsonFile` загружает конфигурацию из *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="43d09-408">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="43d09-409">Для *appsettings.Development.json* в примере приложения загружается следующий файл:</span><span class="sxs-lookup"><span data-stu-id="43d09-409">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="43d09-410">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="43d09-410">Run the sample app.</span></span> <span data-ttu-id="43d09-411">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="43d09-411">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="43d09-412">Выходные данные содержат пары "ключ-значение" для конфигурации в зависимости от среды приложения.</span><span class="sxs-lookup"><span data-stu-id="43d09-412">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="43d09-413">Уровень ведения журнала для ключа `Logging:LogLevel:Default` имеет значение `Debug` при запуске приложения в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="43d09-413">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="43d09-414">Запустите пример приложения еще раз в рабочей среде:</span><span class="sxs-lookup"><span data-stu-id="43d09-414">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="43d09-415">Откройте файл *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="43d09-415">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="43d09-416">В профиле `ConfigurationSample` измените значение переменной среды `ASPNETCORE_ENVIRONMENT` на `Production`.</span><span class="sxs-lookup"><span data-stu-id="43d09-416">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="43d09-417">Сохраните файл и запустите приложение с помощью `dotnet run` в командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="43d09-417">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="43d09-418">Параметры в *appsettings.Development.json* больше не переопределяют параметры в *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="43d09-418">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="43d09-419">Уровень ведения журнала для ключа `Logging:LogLevel:Default` имеет значение `Information`.</span><span class="sxs-lookup"><span data-stu-id="43d09-419">The log level for the key `Logging:LogLevel:Default` is `Information`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="43d09-420">Поставщик конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="43d09-420">XML Configuration Provider</span></span>

<span data-ttu-id="43d09-421"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> загружает конфигурацию из пары "ключ — значение" XML-файла в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="43d09-421">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="43d09-422">Чтобы активировать конфигурацию XML-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="43d09-422">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="43d09-423">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="43d09-423">Overloads permit specifying:</span></span>

* <span data-ttu-id="43d09-424">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="43d09-424">Whether the file is optional.</span></span>
* <span data-ttu-id="43d09-425">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="43d09-425">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="43d09-426"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="43d09-426">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="43d09-427">Корневой узел файла конфигурации не учитывается при создании пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-427">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="43d09-428">Не указывайте в файле Document Type Definition (определение типа документа, DTD) или пространство имен.</span><span class="sxs-lookup"><span data-stu-id="43d09-428">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="43d09-429">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="43d09-429">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="43d09-430">XML-файлы конфигурации могут использовать имена отдельных элементов для повторяющихся разделов.</span><span class="sxs-lookup"><span data-stu-id="43d09-430">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="43d09-431">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="43d09-431">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="43d09-432">section0:key0</span><span class="sxs-lookup"><span data-stu-id="43d09-432">section0:key0</span></span>
* <span data-ttu-id="43d09-433">section0:key1</span><span class="sxs-lookup"><span data-stu-id="43d09-433">section0:key1</span></span>
* <span data-ttu-id="43d09-434">section1:key0</span><span class="sxs-lookup"><span data-stu-id="43d09-434">section1:key0</span></span>
* <span data-ttu-id="43d09-435">section1:key1</span><span class="sxs-lookup"><span data-stu-id="43d09-435">section1:key1</span></span>

<span data-ttu-id="43d09-436">Повторяющиеся элементы, использующие то же имя элемента, работают, если атрибут `name` используется для различения элементов.</span><span class="sxs-lookup"><span data-stu-id="43d09-436">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="43d09-437">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="43d09-437">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="43d09-438">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="43d09-438">section:section0:key:key0</span></span>
* <span data-ttu-id="43d09-439">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="43d09-439">section:section0:key:key1</span></span>
* <span data-ttu-id="43d09-440">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="43d09-440">section:section1:key:key0</span></span>
* <span data-ttu-id="43d09-441">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="43d09-441">section:section1:key:key1</span></span>

<span data-ttu-id="43d09-442">Атрибуты можно использовать для предоставления значений.</span><span class="sxs-lookup"><span data-stu-id="43d09-442">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="43d09-443">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="43d09-443">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="43d09-444">key:attribute</span><span class="sxs-lookup"><span data-stu-id="43d09-444">key:attribute</span></span>
* <span data-ttu-id="43d09-445">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="43d09-445">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="43d09-446">Поставщик конфигурации ключа для каждого файла</span><span class="sxs-lookup"><span data-stu-id="43d09-446">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="43d09-447"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> использует файлы каталога как пары "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-447">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="43d09-448">Ключ является именем файла.</span><span class="sxs-lookup"><span data-stu-id="43d09-448">The key is the file name.</span></span> <span data-ttu-id="43d09-449">Значение содержит содержимое файла.</span><span class="sxs-lookup"><span data-stu-id="43d09-449">The value contains the file's contents.</span></span> <span data-ttu-id="43d09-450">Поставщик конфигурации ключа для каждого файла используется в сценариях размещения Docker.</span><span class="sxs-lookup"><span data-stu-id="43d09-450">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="43d09-451">Чтобы активировать конфигурацию ключа для каждого файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="43d09-451">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="43d09-452">Значение параметра `directoryPath` должно быть абсолютным путем к файлам.</span><span class="sxs-lookup"><span data-stu-id="43d09-452">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="43d09-453">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="43d09-453">Overloads permit specifying:</span></span>

* <span data-ttu-id="43d09-454">`Action<KeyPerFileConfigurationSource>` — делегат, который настраивает источник.</span><span class="sxs-lookup"><span data-stu-id="43d09-454">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="43d09-455">Обязательно ли указывать каталог и путь к каталогу.</span><span class="sxs-lookup"><span data-stu-id="43d09-455">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="43d09-456">Двойное подчеркивание (`__`) используется в качестве разделителя ключа конфигурации в именах файлов.</span><span class="sxs-lookup"><span data-stu-id="43d09-456">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="43d09-457">Например, в имени файла `Logging__LogLevel__System` создается ключ конфигурации `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="43d09-457">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="43d09-458">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="43d09-458">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="43d09-459">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="43d09-459">Memory Configuration Provider</span></span>

<span data-ttu-id="43d09-460"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> использует коллекцию памяти в качестве пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-460">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="43d09-461">Чтобы активировать конфигурацию коллекции в памяти, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="43d09-461">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="43d09-462">Поставщик конфигурации может инициализироваться значением `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="43d09-462">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="43d09-463">Чтобы указать конфигурацию приложения, при сборке узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="43d09-463">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="43d09-464">В следующем примере создается словарь конфигурации:</span><span class="sxs-lookup"><span data-stu-id="43d09-464">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="43d09-465">Словарь используется с вызовом `AddInMemoryCollection` для предоставления конфигурации:</span><span class="sxs-lookup"><span data-stu-id="43d09-465">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="43d09-466">GetValue</span><span class="sxs-lookup"><span data-stu-id="43d09-466">GetValue</span></span>

<span data-ttu-id="43d09-467">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) извлекает одно значение из конфигурации с указанным ключом и преобразует его в указанный тип, не являющийся коллекцией.</span><span class="sxs-lookup"><span data-stu-id="43d09-467">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="43d09-468">Перегрузка принимает значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="43d09-468">An overload accepts a default value.</span></span>

<span data-ttu-id="43d09-469">В следующем примере происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="43d09-469">The following example:</span></span>

* <span data-ttu-id="43d09-470">Из конфигурации извлекается строковое значение с ключом `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="43d09-470">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="43d09-471">Если `NumberKey` отсутствует в ключах конфигурации, используется значение по умолчанию `99`.</span><span class="sxs-lookup"><span data-stu-id="43d09-471">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="43d09-472">Значение получает тип `int`.</span><span class="sxs-lookup"><span data-stu-id="43d09-472">Types the value as an `int`.</span></span>
* <span data-ttu-id="43d09-473">Значение сохраняется в свойстве `NumberConfig` для использования на странице.</span><span class="sxs-lookup"><span data-stu-id="43d09-473">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="43d09-474">GetSection, GetChildren и Exists</span><span class="sxs-lookup"><span data-stu-id="43d09-474">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="43d09-475">В следующих примерах рассмотрим файл JSON.</span><span class="sxs-lookup"><span data-stu-id="43d09-475">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="43d09-476">Четыре ключа находятся в двух разделах, один из которых содержит пару из подразделов.</span><span class="sxs-lookup"><span data-stu-id="43d09-476">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="43d09-477">Когда файл считывается в конфигурацию, для сохранения значений конфигурации создаются следующие уникальные иерархические ключи.</span><span class="sxs-lookup"><span data-stu-id="43d09-477">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="43d09-478">section0:key0</span><span class="sxs-lookup"><span data-stu-id="43d09-478">section0:key0</span></span>
* <span data-ttu-id="43d09-479">section0:key1</span><span class="sxs-lookup"><span data-stu-id="43d09-479">section0:key1</span></span>
* <span data-ttu-id="43d09-480">section1:key0</span><span class="sxs-lookup"><span data-stu-id="43d09-480">section1:key0</span></span>
* <span data-ttu-id="43d09-481">section1:key1</span><span class="sxs-lookup"><span data-stu-id="43d09-481">section1:key1</span></span>
* <span data-ttu-id="43d09-482">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="43d09-482">section2:subsection0:key0</span></span>
* <span data-ttu-id="43d09-483">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="43d09-483">section2:subsection0:key1</span></span>
* <span data-ttu-id="43d09-484">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="43d09-484">section2:subsection1:key0</span></span>
* <span data-ttu-id="43d09-485">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="43d09-485">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="43d09-486">GetSection</span><span class="sxs-lookup"><span data-stu-id="43d09-486">GetSection</span></span>

<span data-ttu-id="43d09-487">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) извлекает подраздел конфигурации с указанным ключом подраздела.</span><span class="sxs-lookup"><span data-stu-id="43d09-487">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="43d09-488">Чтобы вернуть <xref:Microsoft.Extensions.Configuration.IConfigurationSection>, содержащий только пары "ключ — значение" в `section1`, вызовите `GetSection` и укажите имя раздела.</span><span class="sxs-lookup"><span data-stu-id="43d09-488">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="43d09-489">`configSection` не содержит значение, только ключ и путь.</span><span class="sxs-lookup"><span data-stu-id="43d09-489">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="43d09-490">Аналогично, чтобы получить значения для ключей в `section2:subsection0`, вызовите `GetSection` и укажите путь к разделу.</span><span class="sxs-lookup"><span data-stu-id="43d09-490">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="43d09-491">Значение `GetSection` никогда не возвращает значение `null`.</span><span class="sxs-lookup"><span data-stu-id="43d09-491">`GetSection` never returns `null`.</span></span> <span data-ttu-id="43d09-492">Если соответствующий раздел не найден, возвращается пустой параметр `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="43d09-492">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="43d09-493">Когда `GetSection` возвращает соответствующий раздел, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> не заполняется.</span><span class="sxs-lookup"><span data-stu-id="43d09-493">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="43d09-494"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> и <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> возвращаются, если раздел существует.</span><span class="sxs-lookup"><span data-stu-id="43d09-494">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="43d09-495">GetChildren</span><span class="sxs-lookup"><span data-stu-id="43d09-495">GetChildren</span></span>

<span data-ttu-id="43d09-496">Вызов [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) в `section2` получает значение `IEnumerable<IConfigurationSection>`, которое включает:</span><span class="sxs-lookup"><span data-stu-id="43d09-496">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="43d09-497">Exists</span><span class="sxs-lookup"><span data-stu-id="43d09-497">Exists</span></span>

<span data-ttu-id="43d09-498">Используйте [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*), чтобы определить, существует ли раздел конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-498">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="43d09-499">Учитывая данные примера, `sectionExists` является `false`, потому что в данных конфигурации нет `section2:subsection2`.</span><span class="sxs-lookup"><span data-stu-id="43d09-499">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="43d09-500">Привязка к классу</span><span class="sxs-lookup"><span data-stu-id="43d09-500">Bind to a class</span></span>

<span data-ttu-id="43d09-501">Конфигурация может быть привязана к классам, которые представляют группы связанных параметров, используя шаблон *вариантов*.</span><span class="sxs-lookup"><span data-stu-id="43d09-501">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="43d09-502">Для получения дополнительной информации см. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="43d09-502">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="43d09-503">Значения конфигурации возвращаются как строки, но вызов <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> позволяет построить объекты [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="43d09-503">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="43d09-504">Модуль привязки привязывает значения ко всем открытым свойствам чтения и записи предоставленного типа.</span><span class="sxs-lookup"><span data-stu-id="43d09-504">The binder binds values to all of the public read/write properties of the type provided.</span></span> <span data-ttu-id="43d09-505">Поля **не** привязаны.</span><span class="sxs-lookup"><span data-stu-id="43d09-505">Fields are **not** bound.</span></span>

<span data-ttu-id="43d09-506">Пример приложения содержит модель `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="43d09-506">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="43d09-507">Раздел `starship` файла *starship.json* создает конфигурацию, когда образец приложения использует поставщик конфигурации JSON для загрузки конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-507">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

<span data-ttu-id="43d09-508">Создаются следующие пары "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-508">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="43d09-509">Ключ</span><span class="sxs-lookup"><span data-stu-id="43d09-509">Key</span></span>                   | <span data-ttu-id="43d09-510">Значение</span><span class="sxs-lookup"><span data-stu-id="43d09-510">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="43d09-511">starship:name</span><span class="sxs-lookup"><span data-stu-id="43d09-511">starship:name</span></span>         | <span data-ttu-id="43d09-512">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="43d09-512">USS Enterprise</span></span>                                    |
| <span data-ttu-id="43d09-513">starship:registry</span><span class="sxs-lookup"><span data-stu-id="43d09-513">starship:registry</span></span>     | <span data-ttu-id="43d09-514">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="43d09-514">NCC-1701</span></span>                                          |
| <span data-ttu-id="43d09-515">starship:class</span><span class="sxs-lookup"><span data-stu-id="43d09-515">starship:class</span></span>        | <span data-ttu-id="43d09-516">Constitution</span><span class="sxs-lookup"><span data-stu-id="43d09-516">Constitution</span></span>                                      |
| <span data-ttu-id="43d09-517">starship:length</span><span class="sxs-lookup"><span data-stu-id="43d09-517">starship:length</span></span>       | <span data-ttu-id="43d09-518">304.8</span><span class="sxs-lookup"><span data-stu-id="43d09-518">304.8</span></span>                                             |
| <span data-ttu-id="43d09-519">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="43d09-519">starship:commissioned</span></span> | <span data-ttu-id="43d09-520">False</span><span class="sxs-lookup"><span data-stu-id="43d09-520">False</span></span>                                             |
| <span data-ttu-id="43d09-521">trademark</span><span class="sxs-lookup"><span data-stu-id="43d09-521">trademark</span></span>             | <span data-ttu-id="43d09-522">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="43d09-522">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="43d09-523">Пример приложения вызывает `GetSection` с помощью ключа `starship`.</span><span class="sxs-lookup"><span data-stu-id="43d09-523">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="43d09-524">Пары "ключ — значение" `starship` изолированы.</span><span class="sxs-lookup"><span data-stu-id="43d09-524">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="43d09-525">Метод `Bind` вызывается в подразделе при прохождении в экземпляр класса `Starship`.</span><span class="sxs-lookup"><span data-stu-id="43d09-525">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="43d09-526">После привязки значения экземпляра экземпляру присваивается свойство для преобразования для просмотра.</span><span class="sxs-lookup"><span data-stu-id="43d09-526">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="43d09-527">Привязка к графу объектов</span><span class="sxs-lookup"><span data-stu-id="43d09-527">Bind to an object graph</span></span>

<span data-ttu-id="43d09-528"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> способна связывать весь граф объекта POCO.</span><span class="sxs-lookup"><span data-stu-id="43d09-528"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="43d09-529">Как и при привязке простого объекта, привязываются только открытые свойства чтения и записи.</span><span class="sxs-lookup"><span data-stu-id="43d09-529">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="43d09-530">Образец содержит модель `TvShow`, в графе объектов которого находятся классы `Metadata` и `Actors` (*Модели/TvShow.cs*).</span><span class="sxs-lookup"><span data-stu-id="43d09-530">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="43d09-531">В примере приложения есть файл *tvshow.xml*, содержащий данные конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-531">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="43d09-532">Конфигурация привязана ко всему ​​графу объектов `TvShow` с помощью метода `Bind`.</span><span class="sxs-lookup"><span data-stu-id="43d09-532">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="43d09-533">Привязанный экземпляр присваивается свойству для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="43d09-533">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="43d09-534">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) привязывает и возвращает указанный тип.</span><span class="sxs-lookup"><span data-stu-id="43d09-534">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="43d09-535">Метод `Get<T>` может быть более удобным, чем использование `Bind`.</span><span class="sxs-lookup"><span data-stu-id="43d09-535">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="43d09-536">В следующем коде показано, как использовать `Get<T>` с предыдущим примером, который позволяет привязать связанный экземпляр непосредственно к свойству, используемому для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="43d09-536">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="43d09-537">Привязка массива к классу</span><span class="sxs-lookup"><span data-stu-id="43d09-537">Bind an array to a class</span></span>

<span data-ttu-id="43d09-538">*Пример приложения демонстрирует концепции, описанные в этом разделе.*</span><span class="sxs-lookup"><span data-stu-id="43d09-538">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="43d09-539"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-539">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="43d09-540">Любой формат массива, который предоставляет сегмент числового ключа (`:0:`, `:1:`, &hellip; `:{n}:`), способен привязать массив к массиву класса POCO.</span><span class="sxs-lookup"><span data-stu-id="43d09-540">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="43d09-541">Привязка предоставляется соглашением.</span><span class="sxs-lookup"><span data-stu-id="43d09-541">Binding is provided by convention.</span></span> <span data-ttu-id="43d09-542">Пользовательские поставщики конфигурации не обязаны реализовывать привязку массива.</span><span class="sxs-lookup"><span data-stu-id="43d09-542">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="43d09-543">**Обработка массива в оперативной памяти**</span><span class="sxs-lookup"><span data-stu-id="43d09-543">**In-memory array processing**</span></span>

<span data-ttu-id="43d09-544">Рассмотрите ключи и значения конфигурации, приведенные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="43d09-544">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="43d09-545">Ключ</span><span class="sxs-lookup"><span data-stu-id="43d09-545">Key</span></span>             | <span data-ttu-id="43d09-546">Значение</span><span class="sxs-lookup"><span data-stu-id="43d09-546">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="43d09-547">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="43d09-547">array:entries:0</span></span> | <span data-ttu-id="43d09-548">value0</span><span class="sxs-lookup"><span data-stu-id="43d09-548">value0</span></span> |
| <span data-ttu-id="43d09-549">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="43d09-549">array:entries:1</span></span> | <span data-ttu-id="43d09-550">value1</span><span class="sxs-lookup"><span data-stu-id="43d09-550">value1</span></span> |
| <span data-ttu-id="43d09-551">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="43d09-551">array:entries:2</span></span> | <span data-ttu-id="43d09-552">value2</span><span class="sxs-lookup"><span data-stu-id="43d09-552">value2</span></span> |
| <span data-ttu-id="43d09-553">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="43d09-553">array:entries:4</span></span> | <span data-ttu-id="43d09-554">value4</span><span class="sxs-lookup"><span data-stu-id="43d09-554">value4</span></span> |
| <span data-ttu-id="43d09-555">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="43d09-555">array:entries:5</span></span> | <span data-ttu-id="43d09-556">value5</span><span class="sxs-lookup"><span data-stu-id="43d09-556">value5</span></span> |

<span data-ttu-id="43d09-557">Эти ключи и значения загружаются в пример приложения с помощью поставщика конфигурации памяти.</span><span class="sxs-lookup"><span data-stu-id="43d09-557">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="43d09-558">Массив пропускает значения для индекса &num;3.</span><span class="sxs-lookup"><span data-stu-id="43d09-558">The array skips a value for index &num;3.</span></span> <span data-ttu-id="43d09-559">Связующее свойство конфигурации не способно связывать нулевые значения или создавать нулевые записи в связанных объектах, что становится ясным в тот момент, когда демонстрируется результат привязки этого массива к объекту.</span><span class="sxs-lookup"><span data-stu-id="43d09-559">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="43d09-560">В примере приложения класс POCO доступен для хранения привязанных данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-560">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="43d09-561">Данные конфигурации, связанные с объектом.</span><span class="sxs-lookup"><span data-stu-id="43d09-561">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="43d09-562">Синтаксис [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) также может использоваться для получения более компактного кода.</span><span class="sxs-lookup"><span data-stu-id="43d09-562">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="43d09-563">Связанный объект, экземпляр `ArrayExample`, получает данные массива из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-563">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="43d09-564">Индекс `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="43d09-564">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="43d09-565">Значение `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="43d09-565">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="43d09-566">0</span><span class="sxs-lookup"><span data-stu-id="43d09-566">0</span></span>                            | <span data-ttu-id="43d09-567">value0</span><span class="sxs-lookup"><span data-stu-id="43d09-567">value0</span></span>                       |
| <span data-ttu-id="43d09-568">1</span><span class="sxs-lookup"><span data-stu-id="43d09-568">1</span></span>                            | <span data-ttu-id="43d09-569">value1</span><span class="sxs-lookup"><span data-stu-id="43d09-569">value1</span></span>                       |
| <span data-ttu-id="43d09-570">2</span><span class="sxs-lookup"><span data-stu-id="43d09-570">2</span></span>                            | <span data-ttu-id="43d09-571">value2</span><span class="sxs-lookup"><span data-stu-id="43d09-571">value2</span></span>                       |
| <span data-ttu-id="43d09-572">3</span><span class="sxs-lookup"><span data-stu-id="43d09-572">3</span></span>                            | <span data-ttu-id="43d09-573">value4</span><span class="sxs-lookup"><span data-stu-id="43d09-573">value4</span></span>                       |
| <span data-ttu-id="43d09-574">4</span><span class="sxs-lookup"><span data-stu-id="43d09-574">4</span></span>                            | <span data-ttu-id="43d09-575">value5</span><span class="sxs-lookup"><span data-stu-id="43d09-575">value5</span></span>                       |

<span data-ttu-id="43d09-576">Индекс &num;3 в связанном объекте содержит данные конфигурации для конфигурационного ключа `array:4` и его значения `value4`.</span><span class="sxs-lookup"><span data-stu-id="43d09-576">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="43d09-577">При привязке данных конфигурации, содержащих массив индексов, в ключах конфигурации эти индексы просто используются для выполнения итерации данных конфигурации при создании объекта.</span><span class="sxs-lookup"><span data-stu-id="43d09-577">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="43d09-578">Когда массив ключей конфигурации пропускает один или несколько индексов, в данных конфигурации не может быть сохранено нулевое значение и в связанном объекте не создается нулевая запись.</span><span class="sxs-lookup"><span data-stu-id="43d09-578">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="43d09-579">Отсутствующий элемент конфигурации для индекса &num;3 может быть предоставлен перед привязкой к экземпляру `ArrayExample` любым поставщиком конфигурации, который создает правильную пару "ключ — значение" в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-579">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="43d09-580">Если в образец включен дополнительный поставщик конфигурации JSON с отсутствующей парой "ключ — значение", то `ArrayExample.Entries` подойдет полному массиву конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-580">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="43d09-581">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="43d09-581">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="43d09-582">В `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="43d09-582">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="43d09-583">Пары "ключ — значение", показанные в таблице, загружаются в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-583">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="43d09-584">Ключ</span><span class="sxs-lookup"><span data-stu-id="43d09-584">Key</span></span>             | <span data-ttu-id="43d09-585">Значение</span><span class="sxs-lookup"><span data-stu-id="43d09-585">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="43d09-586">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="43d09-586">array:entries:3</span></span> | <span data-ttu-id="43d09-587">value3</span><span class="sxs-lookup"><span data-stu-id="43d09-587">value3</span></span> |

<span data-ttu-id="43d09-588">Если экземпляр класса `ArrayExample` связан после того, как поставщик конфигурации JSON включает запись для индекса &num;3, то массив `ArrayExample.Entries` включит значение.</span><span class="sxs-lookup"><span data-stu-id="43d09-588">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="43d09-589">Индекс `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="43d09-589">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="43d09-590">Значение `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="43d09-590">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="43d09-591">0</span><span class="sxs-lookup"><span data-stu-id="43d09-591">0</span></span>                            | <span data-ttu-id="43d09-592">value0</span><span class="sxs-lookup"><span data-stu-id="43d09-592">value0</span></span>                       |
| <span data-ttu-id="43d09-593">1</span><span class="sxs-lookup"><span data-stu-id="43d09-593">1</span></span>                            | <span data-ttu-id="43d09-594">value1</span><span class="sxs-lookup"><span data-stu-id="43d09-594">value1</span></span>                       |
| <span data-ttu-id="43d09-595">2</span><span class="sxs-lookup"><span data-stu-id="43d09-595">2</span></span>                            | <span data-ttu-id="43d09-596">value2</span><span class="sxs-lookup"><span data-stu-id="43d09-596">value2</span></span>                       |
| <span data-ttu-id="43d09-597">3</span><span class="sxs-lookup"><span data-stu-id="43d09-597">3</span></span>                            | <span data-ttu-id="43d09-598">value3</span><span class="sxs-lookup"><span data-stu-id="43d09-598">value3</span></span>                       |
| <span data-ttu-id="43d09-599">4</span><span class="sxs-lookup"><span data-stu-id="43d09-599">4</span></span>                            | <span data-ttu-id="43d09-600">value4</span><span class="sxs-lookup"><span data-stu-id="43d09-600">value4</span></span>                       |
| <span data-ttu-id="43d09-601">5</span><span class="sxs-lookup"><span data-stu-id="43d09-601">5</span></span>                            | <span data-ttu-id="43d09-602">value5</span><span class="sxs-lookup"><span data-stu-id="43d09-602">value5</span></span>                       |

<span data-ttu-id="43d09-603">**Обработка массива JSON**</span><span class="sxs-lookup"><span data-stu-id="43d09-603">**JSON array processing**</span></span>

<span data-ttu-id="43d09-604">Если JSON-файл содержит массив, ключи конфигурации будут созданы для элементов массива с индексом раздела, начиная с нуля.</span><span class="sxs-lookup"><span data-stu-id="43d09-604">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="43d09-605">В следующем файле конфигурации `subsection` — это массив.</span><span class="sxs-lookup"><span data-stu-id="43d09-605">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="43d09-606">Поставщик конфигурации JSON считывает данные конфигурации в следующие пары "ключ — значение".</span><span class="sxs-lookup"><span data-stu-id="43d09-606">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="43d09-607">Ключ</span><span class="sxs-lookup"><span data-stu-id="43d09-607">Key</span></span>                     | <span data-ttu-id="43d09-608">Значение</span><span class="sxs-lookup"><span data-stu-id="43d09-608">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="43d09-609">json_array:key</span><span class="sxs-lookup"><span data-stu-id="43d09-609">json_array:key</span></span>          | <span data-ttu-id="43d09-610">valueA</span><span class="sxs-lookup"><span data-stu-id="43d09-610">valueA</span></span> |
| <span data-ttu-id="43d09-611">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="43d09-611">json_array:subsection:0</span></span> | <span data-ttu-id="43d09-612">valueB</span><span class="sxs-lookup"><span data-stu-id="43d09-612">valueB</span></span> |
| <span data-ttu-id="43d09-613">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="43d09-613">json_array:subsection:1</span></span> | <span data-ttu-id="43d09-614">valueC</span><span class="sxs-lookup"><span data-stu-id="43d09-614">valueC</span></span> |
| <span data-ttu-id="43d09-615">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="43d09-615">json_array:subsection:2</span></span> | <span data-ttu-id="43d09-616">valueD</span><span class="sxs-lookup"><span data-stu-id="43d09-616">valueD</span></span> |

<span data-ttu-id="43d09-617">В примере приложения для привязки пар "ключ — значение" конфигурации доступен следующий класс POCO.</span><span class="sxs-lookup"><span data-stu-id="43d09-617">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="43d09-618">После выполнения привязки `JsonArrayExample.Key` содержит значение `valueA`.</span><span class="sxs-lookup"><span data-stu-id="43d09-618">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="43d09-619">Подраздел значения хранится в свойстве массива POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="43d09-619">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="43d09-620">Индекс `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="43d09-620">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="43d09-621">Значение `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="43d09-621">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="43d09-622">0</span><span class="sxs-lookup"><span data-stu-id="43d09-622">0</span></span>                                   | <span data-ttu-id="43d09-623">valueB</span><span class="sxs-lookup"><span data-stu-id="43d09-623">valueB</span></span>                              |
| <span data-ttu-id="43d09-624">1</span><span class="sxs-lookup"><span data-stu-id="43d09-624">1</span></span>                                   | <span data-ttu-id="43d09-625">valueC</span><span class="sxs-lookup"><span data-stu-id="43d09-625">valueC</span></span>                              |
| <span data-ttu-id="43d09-626">2</span><span class="sxs-lookup"><span data-stu-id="43d09-626">2</span></span>                                   | <span data-ttu-id="43d09-627">valueD</span><span class="sxs-lookup"><span data-stu-id="43d09-627">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="43d09-628">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="43d09-628">Custom configuration provider</span></span>

<span data-ttu-id="43d09-629">Пример приложения демонстрирует, как создать базовый поставщик конфигурации, который считывает пары "ключ — значение" конфигурации из базы данных, используя [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="43d09-629">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="43d09-630">Поставщик имеет следующие характеристики.</span><span class="sxs-lookup"><span data-stu-id="43d09-630">The provider has the following characteristics:</span></span>

* <span data-ttu-id="43d09-631">База данных в памяти EF используется для демонстрационных целей.</span><span class="sxs-lookup"><span data-stu-id="43d09-631">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="43d09-632">Чтобы использовать базу данных, для которой требуется строка подключения, выполните вторичный `ConfigurationBuilder`, чтобы предоставить строку подключения от другого поставщика конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-632">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="43d09-633">Поставщик считывает таблицу базы данных в конфигурации при запуске.</span><span class="sxs-lookup"><span data-stu-id="43d09-633">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="43d09-634">Поставщик не запрашивает базу данных для каждого ключа.</span><span class="sxs-lookup"><span data-stu-id="43d09-634">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="43d09-635">Функция перезагрузки на изменение не реализована, поэтому обновление базы данных после запуска приложения не влияет на конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="43d09-635">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="43d09-636">Определите сущность `EFConfigurationValue` для хранения значений конфигурации в базе данных.</span><span class="sxs-lookup"><span data-stu-id="43d09-636">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="43d09-637">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="43d09-637">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="43d09-638">Добавьте `EFConfigurationContext` в хранилище и обратитесь к настроенным значениям.</span><span class="sxs-lookup"><span data-stu-id="43d09-638">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="43d09-639">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="43d09-639">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="43d09-640">Создайте класс, реализующий <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="43d09-640">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="43d09-641">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="43d09-641">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="43d09-642">Создайте пользовательский поставщик конфигурации путем наследования от <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="43d09-642">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="43d09-643">Поставщик конфигурации инициализирует пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="43d09-643">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="43d09-644">Поскольку [конфигурационные ключи не учитывают регистр](#keys), словарь, используемый для инициализации базы данных, создается с помощью функции сравнения без учета регистра ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span><span class="sxs-lookup"><span data-stu-id="43d09-644">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="43d09-645">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="43d09-645">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="43d09-646">Метод расширения `AddEFConfiguration` позволяет добавить источник конфигурации к `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="43d09-646">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="43d09-647">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="43d09-647">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="43d09-648">В следующем коде показано, как использовать пользовательский `EFConfigurationProvider` в *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="43d09-648">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="43d09-649">Доступ к конфигурации во время запуска</span><span class="sxs-lookup"><span data-stu-id="43d09-649">Access configuration during startup</span></span>

<span data-ttu-id="43d09-650">Внесите значение `IConfiguration` в конструктор `Startup` для доступа к значениям конфигурации в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="43d09-650">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="43d09-651">Чтобы получить доступ к конфигурации в `Startup.Configure`, либо добавьте значение `IConfiguration` непосредственно в метод, либо используйте экземпляр из конструктора.</span><span class="sxs-lookup"><span data-stu-id="43d09-651">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="43d09-652">Дополнительные сведения см. в руководстве по [доступу к конфигурации с использованием удобных методов](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="43d09-652">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="43d09-653">Настройте доступ на странице Razor Pages или в представлении MVC</span><span class="sxs-lookup"><span data-stu-id="43d09-653">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="43d09-654">Для доступа к параметрам конфигурации на странице Razor Pages или в представлении MVC добавьте [использование директивы](xref:mvc/views/razor#using) ([Справочник по C#: использование директивы](/dotnet/csharp/language-reference/keywords/using-directive)) для [пространства имен Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) и вставьте <xref:Microsoft.Extensions.Configuration.IConfiguration> на страницу или в представление.</span><span class="sxs-lookup"><span data-stu-id="43d09-654">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="43d09-655">На странице Razor Pages</span><span class="sxs-lookup"><span data-stu-id="43d09-655">In a Razor Pages page:</span></span>

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

<span data-ttu-id="43d09-656">В представлении MVC</span><span class="sxs-lookup"><span data-stu-id="43d09-656">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="43d09-657">Добавление конфигурации из внешней сборки</span><span class="sxs-lookup"><span data-stu-id="43d09-657">Add configuration from an external assembly</span></span>

<span data-ttu-id="43d09-658">Реализация <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> позволяет при запуске добавлять в приложение улучшения из внешней сборки вне приложения класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="43d09-658">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="43d09-659">Для получения дополнительной информации см. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="43d09-659">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="43d09-660">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="43d09-660">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="43d09-661">Конфигурация приложения в ASP.NET Core основана на парах "ключ — значение", установленных *поставщиками конфигурации*.</span><span class="sxs-lookup"><span data-stu-id="43d09-661">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="43d09-662">Поставщики конфигурации получают данные конфигурации в парах "ключ — значение" из различных источников:</span><span class="sxs-lookup"><span data-stu-id="43d09-662">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="43d09-663">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="43d09-663">Azure Key Vault</span></span>
* <span data-ttu-id="43d09-664">конфигурация приложения Azure;</span><span class="sxs-lookup"><span data-stu-id="43d09-664">Azure App Configuration</span></span>
* <span data-ttu-id="43d09-665">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="43d09-665">Command-line arguments</span></span>
* <span data-ttu-id="43d09-666">а также пользовательские поставщики (устанавливаемые или создаваемые).</span><span class="sxs-lookup"><span data-stu-id="43d09-666">Custom providers (installed or created)</span></span>
* <span data-ttu-id="43d09-667">справочных файлов;</span><span class="sxs-lookup"><span data-stu-id="43d09-667">Directory files</span></span>
* <span data-ttu-id="43d09-668">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="43d09-668">Environment variables</span></span>
* <span data-ttu-id="43d09-669">объектов .NET в памяти;</span><span class="sxs-lookup"><span data-stu-id="43d09-669">In-memory .NET objects</span></span>
* <span data-ttu-id="43d09-670">файлов параметров;</span><span class="sxs-lookup"><span data-stu-id="43d09-670">Settings files</span></span>

<span data-ttu-id="43d09-671">Пакеты конфигурации для распространенных вариантов провайдеров конфигурации ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) включаются в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="43d09-671">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="43d09-672">В приведенных ниже и представленных в образце приложения примерах кода используется пространство имен <xref:Microsoft.Extensions.Configuration>:</span><span class="sxs-lookup"><span data-stu-id="43d09-672">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="43d09-673">*Шаблон параметров* является расширением конфигурации основных понятий, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="43d09-673">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="43d09-674">Параметры используют классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="43d09-674">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="43d09-675">Для получения дополнительной информации см. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="43d09-675">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="43d09-676">[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="43d09-676">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="43d09-677">Конфигурация узла и приложения</span><span class="sxs-lookup"><span data-stu-id="43d09-677">Host versus app configuration</span></span>

<span data-ttu-id="43d09-678">Перед настройкой и запуском приложения настройте и запустите *узел*.</span><span class="sxs-lookup"><span data-stu-id="43d09-678">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="43d09-679">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="43d09-679">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="43d09-680">Как приложение, так и узел настраиваются с использованием поставщиков конфигурации, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="43d09-680">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="43d09-681">Пары "ключ — значение" конфигурации узлов также включаются в конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="43d09-681">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="43d09-682">Дополнительные сведения о том, как используются поставщики конфигурации при создании узла и как влияют источники конфигурации на узел, см. в разделе <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="43d09-682">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="43d09-683">Другая конфигурация</span><span class="sxs-lookup"><span data-stu-id="43d09-683">Other configuration</span></span>

<span data-ttu-id="43d09-684">Этот раздел относится только к *конфигурации приложений*.</span><span class="sxs-lookup"><span data-stu-id="43d09-684">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="43d09-685">Другие аспекты запуска и размещения приложений ASP.NET Core настраиваются с помощью файлов конфигурации, которые не рассматриваются в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="43d09-685">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="43d09-686">*launch.json*/*launchSettings.json* — это файлы конфигурации инструментов для среды разработки, описанные в следующих источниках:</span><span class="sxs-lookup"><span data-stu-id="43d09-686">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="43d09-687">В <xref:fundamentals/environments#development>.</span><span class="sxs-lookup"><span data-stu-id="43d09-687">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="43d09-688">В документации, где показано, как файлы используются для настройки приложений ASP.NET Core для сценариев разработки.</span><span class="sxs-lookup"><span data-stu-id="43d09-688">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="43d09-689">*web.config* — это файл конфигурации сервера, описанный в следующих источниках:</span><span class="sxs-lookup"><span data-stu-id="43d09-689">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="43d09-690">См. сведения о переносе конфигурации приложения из более ранних версий ASP.NET: <xref:migration/proper-to-2x/index#store-configurations>.</span><span class="sxs-lookup"><span data-stu-id="43d09-690">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="43d09-691">Конфигурация по умолчанию</span><span class="sxs-lookup"><span data-stu-id="43d09-691">Default configuration</span></span>

<span data-ttu-id="43d09-692">Веб-приложения на основе шаблонов [dotnet new](/dotnet/core/tools/dotnet-new) ASP.NET Core вызывают <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> при создании узла.</span><span class="sxs-lookup"><span data-stu-id="43d09-692">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="43d09-693">`CreateDefaultBuilder` предоставляет конфигурацию по умолчанию для приложения в следующем порядке:</span><span class="sxs-lookup"><span data-stu-id="43d09-693">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="43d09-694">Приведенные ниже сведения относятся к приложениям, использующим [веб-узел](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="43d09-694">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="43d09-695">Подробные сведения о конфигурации по умолчанию при использовании [универсального узла](xref:fundamentals/host/generic-host) см. в [последней версии этой статьи](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="43d09-695">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="43d09-696">Существуют следующие способы предоставления конфигурации узла.</span><span class="sxs-lookup"><span data-stu-id="43d09-696">Host configuration is provided from:</span></span>
  * <span data-ttu-id="43d09-697">Переменные среды с префиксом `ASPNETCORE_` (например, `ASPNETCORE_ENVIRONMENT`), использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="43d09-697">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="43d09-698">Префикс (`ASPNETCORE_`) исключается при загрузке пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-698">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="43d09-699">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="43d09-699">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="43d09-700">Конфигурация приложения предоставляется из следующих ресурсов:</span><span class="sxs-lookup"><span data-stu-id="43d09-700">App configuration is provided from:</span></span>
  * <span data-ttu-id="43d09-701">Файл *appsettings.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="43d09-701">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="43d09-702">Файл *appsettings.{Environment}.json*, использующий [поставщик конфигурации файлов](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="43d09-702">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="43d09-703">[Менеджера секретов](xref:security/app-secrets), когда приложение выполняется в среде `Development` с использованием начальных сборок.</span><span class="sxs-lookup"><span data-stu-id="43d09-703">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="43d09-704">Переменные среды, использующие [поставщик конфигурации переменных среды](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="43d09-704">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="43d09-705">Аргументы командной строки, использующие [поставщик конфигурации командной строки](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="43d09-705">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="43d09-706">Безопасность</span><span class="sxs-lookup"><span data-stu-id="43d09-706">Security</span></span>

<span data-ttu-id="43d09-707">Применяйте описанные ниже методики для защиты конфиденциальных данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-707">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="43d09-708">Никогда не храните пароли или другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации обычного текста.</span><span class="sxs-lookup"><span data-stu-id="43d09-708">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="43d09-709">Не используйте секреты рабочей среды в средах разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="43d09-709">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="43d09-710">Указывайте секреты вне проекта, чтобы их нельзя было случайно зафиксировать в репозитории с исходным кодом.</span><span class="sxs-lookup"><span data-stu-id="43d09-710">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="43d09-711">Дополнительные сведения см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="43d09-711">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="43d09-712"><xref:security/app-secrets> &ndash; содержит рекомендации по использованию переменных среды для хранения конфиденциальных данных.</span><span class="sxs-lookup"><span data-stu-id="43d09-712"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="43d09-713">Менеджер секретов использует поставщик конфигурации файла для хранения конфиденциальных данных пользователя в файле JSON в локальной системе.</span><span class="sxs-lookup"><span data-stu-id="43d09-713">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="43d09-714">Поставщик конфигурации файлов описан ниже в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="43d09-714">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="43d09-715">В [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) безопасно хранятся секреты приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="43d09-715">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="43d09-716">Для получения дополнительной информации см. <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="43d09-716">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="43d09-717">Иерархическая модель конфигурации</span><span class="sxs-lookup"><span data-stu-id="43d09-717">Hierarchical configuration data</span></span>

<span data-ttu-id="43d09-718">API конфигурации способен поддерживать иерархические данные конфигурации, выполняя преобразование в плоскую структуру иерархических данных с использованием разделителя в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-718">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="43d09-719">В следующем файле JSON существуют четыре ключа в структурированной иерархии двух разделов.</span><span class="sxs-lookup"><span data-stu-id="43d09-719">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="43d09-720">При считывании файла в конфигурацию для сохранения исходной иерархической структуры данных источника конфигурации создаются уникальные ключи.</span><span class="sxs-lookup"><span data-stu-id="43d09-720">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="43d09-721">Разделы и ключи преобразовываются в плоскую структуру с использованием двоеточия (`:`) для сохранения исходной структуры.</span><span class="sxs-lookup"><span data-stu-id="43d09-721">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="43d09-722">section0:key0</span><span class="sxs-lookup"><span data-stu-id="43d09-722">section0:key0</span></span>
* <span data-ttu-id="43d09-723">section0:key1</span><span class="sxs-lookup"><span data-stu-id="43d09-723">section0:key1</span></span>
* <span data-ttu-id="43d09-724">section1:key0</span><span class="sxs-lookup"><span data-stu-id="43d09-724">section1:key0</span></span>
* <span data-ttu-id="43d09-725">section1:key1</span><span class="sxs-lookup"><span data-stu-id="43d09-725">section1:key1</span></span>

<span data-ttu-id="43d09-726">Методы <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> и <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> доступны для изолирования разделов и дочерних элементов раздела в данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-726"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="43d09-727">Эти методы описаны далее в разделе [GetSection, GetChildren и Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="43d09-727">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="43d09-728">Соглашения</span><span class="sxs-lookup"><span data-stu-id="43d09-728">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="43d09-729">Источники и поставщики</span><span class="sxs-lookup"><span data-stu-id="43d09-729">Sources and providers</span></span>

<span data-ttu-id="43d09-730">При запуске приложения источники конфигурации считываются в порядке, в котором были указаны их поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-730">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="43d09-731">Поставщики конфигурации, которые реализуют обнаружение изменений, имеют возможность перезагрузить конфигурацию при изменении базовых параметров.</span><span class="sxs-lookup"><span data-stu-id="43d09-731">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="43d09-732">Так, поставщик файла конфигурации (подробнее о нем далее в этой статье) и [поставщик конфигурации Azure Key Vault](xref:security/key-vault-configuration) реализуют обнаружение изменений.</span><span class="sxs-lookup"><span data-stu-id="43d09-732">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="43d09-733">Объект <xref:Microsoft.Extensions.Configuration.IConfiguration> доступен в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) приложения.</span><span class="sxs-lookup"><span data-stu-id="43d09-733"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="43d09-734"><xref:Microsoft.Extensions.Configuration.IConfiguration> можно внедрить в <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> Razor Pages или <xref:Microsoft.AspNetCore.Mvc.Controller> MVC, чтобы получить конфигурацию для класса.</span><span class="sxs-lookup"><span data-stu-id="43d09-734"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="43d09-735">В следующих примерах поле `_config` используется для доступа к значениям конфигурации:</span><span class="sxs-lookup"><span data-stu-id="43d09-735">In the following examples, the `_config` field is used to access configuration values:</span></span>

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

<span data-ttu-id="43d09-736">Поставщики конфигурации не могут использовать контейнер DI, так как он недоступен при настройке узла.</span><span class="sxs-lookup"><span data-stu-id="43d09-736">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="43d09-737">Клавиши</span><span class="sxs-lookup"><span data-stu-id="43d09-737">Keys</span></span>

<span data-ttu-id="43d09-738">В ключах конфигурации приняты следующие соглашения.</span><span class="sxs-lookup"><span data-stu-id="43d09-738">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="43d09-739">В ключах не учитывается регистр символов.</span><span class="sxs-lookup"><span data-stu-id="43d09-739">Keys are case-insensitive.</span></span> <span data-ttu-id="43d09-740">Например `ConnectionString` и `connectionstring` обрабатываются как эквивалентные ключи.</span><span class="sxs-lookup"><span data-stu-id="43d09-740">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="43d09-741">Если значение для одного и того же ключа установлено одним и тем же или разными поставщиками конфигурации, последним значением, установленным на ключе, является используемое значение.</span><span class="sxs-lookup"><span data-stu-id="43d09-741">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="43d09-742">Иерархические ключи</span><span class="sxs-lookup"><span data-stu-id="43d09-742">Hierarchical keys</span></span>
  * <span data-ttu-id="43d09-743">При взаимодействии с API конфигурации разделитель-двоеточие (`:`) поддерживается на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="43d09-743">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="43d09-744">В переменных среды разделитель-двоеточие может не работать на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="43d09-744">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="43d09-745">Двойной знак подчеркивания (`__`) поддерживается на всех платформах и автоматически преобразовывается в двоеточие.</span><span class="sxs-lookup"><span data-stu-id="43d09-745">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="43d09-746">В хранилище ключей Azure иерархические ключи используют `--` (два дефиса) в качестве разделителя.</span><span class="sxs-lookup"><span data-stu-id="43d09-746">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="43d09-747">Чтобы заменить дефисы двоеточием, при загрузке секретов в конфигурацию приложения напишите код.</span><span class="sxs-lookup"><span data-stu-id="43d09-747">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="43d09-748"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-748">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="43d09-749">Привязка массива описана в разделе [Привязка массива к классу](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="43d09-749">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="43d09-750">Значения</span><span class="sxs-lookup"><span data-stu-id="43d09-750">Values</span></span>

<span data-ttu-id="43d09-751">В значениях конфигурации учитываются следующие соглашения.</span><span class="sxs-lookup"><span data-stu-id="43d09-751">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="43d09-752">Значения являются строками.</span><span class="sxs-lookup"><span data-stu-id="43d09-752">Values are strings.</span></span>
* <span data-ttu-id="43d09-753">Значение NULL не может храниться в конфигурации или быть привязанным к объектам.</span><span class="sxs-lookup"><span data-stu-id="43d09-753">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="43d09-754">Поставщики</span><span class="sxs-lookup"><span data-stu-id="43d09-754">Providers</span></span>

<span data-ttu-id="43d09-755">В следующей таблице показаны поставщики конфигурации, доступные для приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="43d09-755">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="43d09-756">Поставщик</span><span class="sxs-lookup"><span data-stu-id="43d09-756">Provider</span></span> | <span data-ttu-id="43d09-757">Предоставляет конфигурацию из &hellip;</span><span class="sxs-lookup"><span data-stu-id="43d09-757">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="43d09-758">[Поставщик конфигурации хранилища ключей Azure](xref:security/key-vault-configuration) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="43d09-758">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="43d09-759">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="43d09-759">Azure Key Vault</span></span> |
| <span data-ttu-id="43d09-760">[Поставщик конфигурации приложений Azure](/azure/azure-app-configuration/quickstart-aspnet-core-app) (документация Azure)</span><span class="sxs-lookup"><span data-stu-id="43d09-760">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="43d09-761">конфигурация приложения Azure;</span><span class="sxs-lookup"><span data-stu-id="43d09-761">Azure App Configuration</span></span> |
| [<span data-ttu-id="43d09-762">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="43d09-762">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="43d09-763">Параметры командной строки</span><span class="sxs-lookup"><span data-stu-id="43d09-763">Command-line parameters</span></span> |
| [<span data-ttu-id="43d09-764">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="43d09-764">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="43d09-765">Источник пользователя</span><span class="sxs-lookup"><span data-stu-id="43d09-765">Custom source</span></span> |
| [<span data-ttu-id="43d09-766">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="43d09-766">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="43d09-767">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="43d09-767">Environment variables</span></span> |
| [<span data-ttu-id="43d09-768">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="43d09-768">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="43d09-769">Файлы (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="43d09-769">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="43d09-770">Поставщик конфигурации ключа для каждого файла</span><span class="sxs-lookup"><span data-stu-id="43d09-770">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="43d09-771">Справочные файлы</span><span class="sxs-lookup"><span data-stu-id="43d09-771">Directory files</span></span> |
| [<span data-ttu-id="43d09-772">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="43d09-772">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="43d09-773">Коллекции оперативной памяти</span><span class="sxs-lookup"><span data-stu-id="43d09-773">In-memory collections</span></span> |
| <span data-ttu-id="43d09-774">[Секреты пользователей (Менеджер секретов)](xref:security/app-secrets) (раздел *Безопасность*)</span><span class="sxs-lookup"><span data-stu-id="43d09-774">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="43d09-775">Файл в каталоге профиля пользователя</span><span class="sxs-lookup"><span data-stu-id="43d09-775">File in the user profile directory</span></span> |

<span data-ttu-id="43d09-776">Источники конфигурации считываются в том порядке, в котором при запуске указываются их поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-776">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="43d09-777">Поставщики конфигурации в этом разделе описаны в алфавитном порядке, а не в необходимом вам порядке в коде.</span><span class="sxs-lookup"><span data-stu-id="43d09-777">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="43d09-778">Порядок поставщиков конфигурации в коде соответствует приоритетам ваших основных источников конфигурации, требуемых приложением.</span><span class="sxs-lookup"><span data-stu-id="43d09-778">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="43d09-779">Типичная последовательность поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-779">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="43d09-780">Файлы (*appsettings.json*, *appsettings.{Environment}.json*, где `{Environment}` — это текущая среда размещения приложения)</span><span class="sxs-lookup"><span data-stu-id="43d09-780">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. <span data-ttu-id="43d09-781">[Azure Key Vault](xref:security/key-vault-configuration);</span><span class="sxs-lookup"><span data-stu-id="43d09-781">[Azure Key Vault](xref:security/key-vault-configuration)</span></span>
1. <span data-ttu-id="43d09-782">[Секреты пользователя (Менеджер секретов)](xref:security/app-secrets) (только в среде разработки)</span><span class="sxs-lookup"><span data-stu-id="43d09-782">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="43d09-783">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="43d09-783">Environment variables</span></span>
1. <span data-ttu-id="43d09-784">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="43d09-784">Command-line arguments</span></span>

<span data-ttu-id="43d09-785">Общепринятой практикой является размещение поставщика конфигурации командной строки последним в ряду поставщиков, чтобы аргументы командной строки могли переопределять конфигурацию, установленную другими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="43d09-785">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="43d09-786">Предыдущая последовательность поставщиков используется при инициализации нового построителя узла с помощью `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="43d09-786">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="43d09-787">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="43d09-787">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="43d09-788">Настройка построителя узла с помощью UseConfiguration</span><span class="sxs-lookup"><span data-stu-id="43d09-788">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="43d09-789">Чтобы настроить построитель узла, вызовите <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> в построителе узла с конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="43d09-789">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

## <a name="configureappconfiguration"></a><span data-ttu-id="43d09-790">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="43d09-790">ConfigureAppConfiguration</span></span>

<span data-ttu-id="43d09-791">Вызовите `ConfigureAppConfiguration` при сборке узла, чтобы указать поставщики конфигурации приложения в дополнение к тем, которые автоматически добавлены `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="43d09-791">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="43d09-792">Переопределение предыдущей конфигурации с помощью аргументов командной строки</span><span class="sxs-lookup"><span data-stu-id="43d09-792">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="43d09-793">Чтобы предоставить конфигурацию приложения, которую можно переопределить с помощью аргументов командной строки, вызовите метод `AddCommandLine` последним:</span><span class="sxs-lookup"><span data-stu-id="43d09-793">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="43d09-794">Удаление поставщиков, добавленных CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="43d09-794">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="43d09-795">Чтобы удалить поставщиков, добавленных `CreateDefaultBuilder`, сначала вызовите [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) в [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources):</span><span class="sxs-lookup"><span data-stu-id="43d09-795">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="43d09-796">Использование конфигурации во время запуска приложения</span><span class="sxs-lookup"><span data-stu-id="43d09-796">Consume configuration during app startup</span></span>

<span data-ttu-id="43d09-797">Конфигурация, предоставленная приложению в `ConfigureAppConfiguration`, доступна во время запуска приложения, включая `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="43d09-797">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="43d09-798">Дополнительные сведения см. в разделе [Доступ к конфигурации во время запуска](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="43d09-798">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="43d09-799">Поставщик конфигурации командной строки</span><span class="sxs-lookup"><span data-stu-id="43d09-799">Command-line Configuration Provider</span></span>

<span data-ttu-id="43d09-800"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> загружает конфигурацию из пары "ключ — значение" аргумента командной строки в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="43d09-800">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="43d09-801">Чтобы активировать конфигурацию командной строки, вызывается метод расширения <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> для экземпляра <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="43d09-801">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="43d09-802">`AddCommandLine` вызывается автоматически при вызове `CreateDefaultBuilder(string [])`.</span><span class="sxs-lookup"><span data-stu-id="43d09-802">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="43d09-803">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="43d09-803">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="43d09-804">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="43d09-804">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="43d09-805">дополнительную конфигурацию из файлов *appsettings.json* и *appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="43d09-805">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="43d09-806">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="43d09-806">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="43d09-807">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="43d09-807">Environment variables.</span></span>

<span data-ttu-id="43d09-808">`CreateDefaultBuilder` добавляет последним поставщика конфигурации командной строки.</span><span class="sxs-lookup"><span data-stu-id="43d09-808">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="43d09-809">Аргументы командной строки передаются в набор конфигурации переопределения среды выполнения, установленной другими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="43d09-809">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="43d09-810">`CreateDefaultBuilder` действует, когда создается узел.</span><span class="sxs-lookup"><span data-stu-id="43d09-810">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="43d09-811">Поэтому конфигурация командной строки, активированная с помощью `CreateDefaultBuilder`, может повлиять на настройку узла.</span><span class="sxs-lookup"><span data-stu-id="43d09-811">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="43d09-812">Для приложений на основе шаблонов ASP.NET Core метод `AddCommandLine` уже был вызван методом `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="43d09-812">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="43d09-813">Чтобы добавить дополнительные поставщики конфигурации и обеспечить возможность переопределения конфигурации от этих поставщиков с помощью аргументов командной строки, вызовите дополнительные поставщики приложения в `ConfigureAppConfiguration` и вызовите `AddCommandLine` в последнюю очередь.</span><span class="sxs-lookup"><span data-stu-id="43d09-813">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="43d09-814">**Пример**</span><span class="sxs-lookup"><span data-stu-id="43d09-814">**Example**</span></span>

<span data-ttu-id="43d09-815">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="43d09-815">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="43d09-816">Откройте командную строку в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="43d09-816">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="43d09-817">Поставьте аргумент командной строки в команду `dotnet run`: `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="43d09-817">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="43d09-818">После запуска приложения откройте браузер в приложении по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="43d09-818">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="43d09-819">Обратите внимание, что вывод содержит пару "ключ — значение" для аргумента командной строки конфигурации, предоставленного для `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="43d09-819">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="43d09-820">Аргументы</span><span class="sxs-lookup"><span data-stu-id="43d09-820">Arguments</span></span>

<span data-ttu-id="43d09-821">Значение должно соответствовать знаку равенства (`=`), или ключ должен иметь префикс (`--` или `/`), когда значение следует за пробелом.</span><span class="sxs-lookup"><span data-stu-id="43d09-821">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="43d09-822">Значение не требуется, если используется знак равенства (например, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="43d09-822">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="43d09-823">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="43d09-823">Key prefix</span></span>               | <span data-ttu-id="43d09-824">Пример</span><span class="sxs-lookup"><span data-stu-id="43d09-824">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="43d09-825">Без префикса</span><span class="sxs-lookup"><span data-stu-id="43d09-825">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="43d09-826">Два дефиса (`--`)</span><span class="sxs-lookup"><span data-stu-id="43d09-826">Two dashes (`--`)</span></span>        | <span data-ttu-id="43d09-827">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="43d09-827">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="43d09-828">Прямая косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="43d09-828">Forward slash (`/`)</span></span>      | <span data-ttu-id="43d09-829">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="43d09-829">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="43d09-830">В рамках одной и той же команды не смешивайте пары "ключ — значение" аргумента командной строки, которые используют знак равенства, с парами "ключ — значение", которые используют пробел.</span><span class="sxs-lookup"><span data-stu-id="43d09-830">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="43d09-831">Примеры команд.</span><span class="sxs-lookup"><span data-stu-id="43d09-831">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="43d09-832">Сопоставления переключений</span><span class="sxs-lookup"><span data-stu-id="43d09-832">Switch mappings</span></span>

<span data-ttu-id="43d09-833">Сопоставление параметров позволяет указать логику замены имен ключей.</span><span class="sxs-lookup"><span data-stu-id="43d09-833">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="43d09-834">Когда вручную создается конфигурация с помощью <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, методу <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> можно предоставить словарь сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="43d09-834">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="43d09-835">В словаре сопоставлений переключений выполняется поиск ключа, который совпадает с ключом, предоставляемым аргументом командной строки.</span><span class="sxs-lookup"><span data-stu-id="43d09-835">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="43d09-836">Если ключ в командной строке находится в словаре, значение словаря (замена ключа) передается обратно, чтобы установить пару "ключ — значение" в конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="43d09-836">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="43d09-837">Сопоставление переключений необходимо для любого ключа командной строки с префиксом из одного дефиса (`-`).</span><span class="sxs-lookup"><span data-stu-id="43d09-837">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="43d09-838">Правила ключей из словаря сопоставления переключений:</span><span class="sxs-lookup"><span data-stu-id="43d09-838">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="43d09-839">Переключения должны начинаться с дефиса (`-`) или двойного дефиса (`--`).</span><span class="sxs-lookup"><span data-stu-id="43d09-839">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="43d09-840">Словарь сопоставлений переключений не должен содержать повторяющиеся ключи.</span><span class="sxs-lookup"><span data-stu-id="43d09-840">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="43d09-841">Создайте словарь сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="43d09-841">Create a switch mappings dictionary.</span></span> <span data-ttu-id="43d09-842">В следующем примере создаются два сопоставления переключений:</span><span class="sxs-lookup"><span data-stu-id="43d09-842">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="43d09-843">При сборке узла вызовите `AddCommandLine` со словарем сопоставлений переключений:</span><span class="sxs-lookup"><span data-stu-id="43d09-843">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="43d09-844">Для приложений, использующих сопоставления переключений, в вызове `CreateDefaultBuilder` аргументы передаваться не должны.</span><span class="sxs-lookup"><span data-stu-id="43d09-844">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="43d09-845">Вызов команды `AddCommandLine` метода `CreateDefaultBuilder` не включает сопоставленные переключения, и нет возможности передать словарь сопоставления переключений в `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="43d09-845">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="43d09-846">Чтобы решить эту проблему, нужно не передавать аргументы команде `CreateDefaultBuilder`, а позволить методу `AddCommandLine` метода `ConfigurationBuilder` обрабатывать как аргументы, так и словарь сопоставления параметров.</span><span class="sxs-lookup"><span data-stu-id="43d09-846">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="43d09-847">Созданный словарь сопоставлений переключений содержит данные, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="43d09-847">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="43d09-848">Ключ</span><span class="sxs-lookup"><span data-stu-id="43d09-848">Key</span></span>       | <span data-ttu-id="43d09-849">Значение</span><span class="sxs-lookup"><span data-stu-id="43d09-849">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="43d09-850">Если ключи сопоставления переключений используются при запуске приложения, конфигурация принимает значение конфигурации в ключе, предоставленном в словаре.</span><span class="sxs-lookup"><span data-stu-id="43d09-850">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="43d09-851">После выполнения предыдущей команды конфигурация содержит значения, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="43d09-851">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="43d09-852">Ключ</span><span class="sxs-lookup"><span data-stu-id="43d09-852">Key</span></span>               | <span data-ttu-id="43d09-853">Значение</span><span class="sxs-lookup"><span data-stu-id="43d09-853">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="43d09-854">Поставщик конфигурации переменных среды</span><span class="sxs-lookup"><span data-stu-id="43d09-854">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="43d09-855"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> загружает конфигурацию из пары "ключ — значение" переменной среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="43d09-855">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="43d09-856">Чтобы активировать конфигурацию переменных среды, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="43d09-856">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="43d09-857">[Служба приложений Azure](https://azure.microsoft.com/services/app-service/) позволяет задать переменные среды на портале Azure, который может переопределить конфигурацию приложения, используя поставщик конфигурации переменных среды.</span><span class="sxs-lookup"><span data-stu-id="43d09-857">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="43d09-858">Дополнительные сведения см. в руководстве по [переопределению конфигурации приложения Azure с помощью портала Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="43d09-858">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="43d09-859">`AddEnvironmentVariables` используется для загрузки переменных среды, имеющих префикс `ASPNETCORE_`, для [конфигурации узла](#host-versus-app-configuration) при инициализации нового построителя узла с [веб-узлом](xref:fundamentals/host/web-host) и вызове `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="43d09-859">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="43d09-860">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="43d09-860">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="43d09-861">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="43d09-861">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="43d09-862">конфигурация приложения на основе переменных среды без префикса путем вызова `AddEnvironmentVariables` без префикса;</span><span class="sxs-lookup"><span data-stu-id="43d09-862">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="43d09-863">дополнительную конфигурацию из файлов *appsettings.json* и *appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="43d09-863">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="43d09-864">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="43d09-864">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="43d09-865">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="43d09-865">Command-line arguments.</span></span>

<span data-ttu-id="43d09-866">Поставщик конфигурации переменных среды вызывается после выполнения настройки с помощью секретов пользователя и файлов *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="43d09-866">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="43d09-867">Вызов поставщика в этой позиции разрешает чтение переменных среды выполнения, чтобы переопределить конфигурацию, заданную секретом пользователя и файлом *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="43d09-867">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="43d09-868">Чтобы добавить конфигурацию приложения из дополнительных переменных среды, вызовите дополнительные поставщики приложения в `ConfigureAppConfiguration`, а затем вызовите `AddEnvironmentVariables` с префиксом:</span><span class="sxs-lookup"><span data-stu-id="43d09-868">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="43d09-869">Вызовите `AddEnvironmentVariables` последним, чтобы разрешить переменным среды с заданным префиксом переопределять значения от других поставщиков.</span><span class="sxs-lookup"><span data-stu-id="43d09-869">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="43d09-870">**Пример**</span><span class="sxs-lookup"><span data-stu-id="43d09-870">**Example**</span></span>

<span data-ttu-id="43d09-871">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает вызов `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="43d09-871">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="43d09-872">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="43d09-872">Run the sample app.</span></span> <span data-ttu-id="43d09-873">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="43d09-873">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="43d09-874">Обратите внимание, что выходные данные содержат пару "ключ — значение" для переменной среды `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="43d09-874">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="43d09-875">Значение отражает среду, в которой выполняется приложение, обычно при локальном запуске это `Development`.</span><span class="sxs-lookup"><span data-stu-id="43d09-875">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="43d09-876">Чтобы список переменных среды, отображаемый приложением, был коротким, приложение фильтрует переменные среды.</span><span class="sxs-lookup"><span data-stu-id="43d09-876">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="43d09-877">См. пример файла *Pages/Index.cshtml.cs* приложения.</span><span class="sxs-lookup"><span data-stu-id="43d09-877">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="43d09-878">Чтобы просмотреть все переменные среды, доступные для приложения, измените значение `FilteredConfiguration` в *Pages/Index.cshtml.cs* на следующее:</span><span class="sxs-lookup"><span data-stu-id="43d09-878">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="43d09-879">Префиксы</span><span class="sxs-lookup"><span data-stu-id="43d09-879">Prefixes</span></span>

<span data-ttu-id="43d09-880">Переменные среды, которые загружаются в конфигурацию приложения, фильтруются при добавлении префикса к методу `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="43d09-880">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="43d09-881">Например, чтобы отфильтровать переменные среды по префиксу `CUSTOM_`, введите префикс поставщику конфигурации:</span><span class="sxs-lookup"><span data-stu-id="43d09-881">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="43d09-882">Префикс отделяется при создании пары конфигурации "ключ — значение".</span><span class="sxs-lookup"><span data-stu-id="43d09-882">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="43d09-883">При создании построителя узла конфигурация узла предоставляется переменными среды.</span><span class="sxs-lookup"><span data-stu-id="43d09-883">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="43d09-884">Дополнительные сведения о префиксе, используемом для этих переменных среды, см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="43d09-884">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="43d09-885">**Префиксы строк подключения**</span><span class="sxs-lookup"><span data-stu-id="43d09-885">**Connection string prefixes**</span></span>

<span data-ttu-id="43d09-886">API конфигурации имеет специальные правила обработки для четырех строк подключения переменных среды, связанных с настройкой строк подключения Azure для среды приложения.</span><span class="sxs-lookup"><span data-stu-id="43d09-886">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="43d09-887">Если префикс не указан в `AddEnvironmentVariables`, переменные среды с префиксами, указанными в таблице, загружаются в приложение.</span><span class="sxs-lookup"><span data-stu-id="43d09-887">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="43d09-888">Префикс строки подключения</span><span class="sxs-lookup"><span data-stu-id="43d09-888">Connection string prefix</span></span> | <span data-ttu-id="43d09-889">Поставщик</span><span class="sxs-lookup"><span data-stu-id="43d09-889">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="43d09-890">Поставщик пользователя</span><span class="sxs-lookup"><span data-stu-id="43d09-890">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="43d09-891">MySQL</span><span class="sxs-lookup"><span data-stu-id="43d09-891">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="43d09-892">База данных SQL Azure</span><span class="sxs-lookup"><span data-stu-id="43d09-892">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="43d09-893">SQL Server</span><span class="sxs-lookup"><span data-stu-id="43d09-893">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="43d09-894">Когда переменная среды обнаруживается и загружается в конфигурацию с одним из четырех префиксов, приведенных в таблице, происходит следующее.</span><span class="sxs-lookup"><span data-stu-id="43d09-894">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="43d09-895">Ключ конфигурации создается путем удаления префикса переменных среды и добавления ключа раздела конфигурации (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="43d09-895">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="43d09-896">Создается новая пара "ключ — значение" конфигурации, которая представляет поставщика подключения базы данных (за исключением `CUSTOMCONNSTR_`, который не имеет указанного поставщика).</span><span class="sxs-lookup"><span data-stu-id="43d09-896">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="43d09-897">Ключ переменной среды</span><span class="sxs-lookup"><span data-stu-id="43d09-897">Environment variable key</span></span> | <span data-ttu-id="43d09-898">Преобразованный ключ конфигурации</span><span class="sxs-lookup"><span data-stu-id="43d09-898">Converted configuration key</span></span> | <span data-ttu-id="43d09-899">Запись конфигурации поставщика</span><span class="sxs-lookup"><span data-stu-id="43d09-899">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="43d09-900">Запись конфигурации не создана.</span><span class="sxs-lookup"><span data-stu-id="43d09-900">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="43d09-901">Ключ: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="43d09-901">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="43d09-902">Значение: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="43d09-902">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="43d09-903">Ключ: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="43d09-903">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="43d09-904">Значение: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="43d09-904">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="43d09-905">Ключ: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="43d09-905">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="43d09-906">Значение: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="43d09-906">Value: `System.Data.SqlClient`</span></span>  |

<span data-ttu-id="43d09-907">**Пример**</span><span class="sxs-lookup"><span data-stu-id="43d09-907">**Example**</span></span>

<span data-ttu-id="43d09-908">На сервере создается пользовательская переменная среды строки подключения:</span><span class="sxs-lookup"><span data-stu-id="43d09-908">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="43d09-909">Имя &ndash; `CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="43d09-909">Name &ndash; `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="43d09-910">Значение &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span><span class="sxs-lookup"><span data-stu-id="43d09-910">Value &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="43d09-911">Если `IConfiguration` вставляется и назначается полю с именем `_config`, считайте значение:</span><span class="sxs-lookup"><span data-stu-id="43d09-911">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="43d09-912">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="43d09-912">File Configuration Provider</span></span>

<span data-ttu-id="43d09-913"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> является базовым классом для загрузки конфигурации из файловой системы.</span><span class="sxs-lookup"><span data-stu-id="43d09-913"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="43d09-914">Следующие поставщики конфигурации предназначены для определенных типов файлов:</span><span class="sxs-lookup"><span data-stu-id="43d09-914">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="43d09-915">Поставщик конфигурации INI</span><span class="sxs-lookup"><span data-stu-id="43d09-915">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="43d09-916">Поставщик конфигурации JSON</span><span class="sxs-lookup"><span data-stu-id="43d09-916">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="43d09-917">Поставщик конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="43d09-917">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="43d09-918">Поставщик конфигурации INI</span><span class="sxs-lookup"><span data-stu-id="43d09-918">INI Configuration Provider</span></span>

<span data-ttu-id="43d09-919"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> загружает конфигурацию из пары "ключ — значение" INI-файла во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="43d09-919">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="43d09-920">Чтобы активировать конфигурацию INI-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="43d09-920">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="43d09-921">Двоеточие можно использовать как разделитель раздела в конфигурации файла INI.</span><span class="sxs-lookup"><span data-stu-id="43d09-921">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="43d09-922">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="43d09-922">Overloads permit specifying:</span></span>

* <span data-ttu-id="43d09-923">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="43d09-923">Whether the file is optional.</span></span>
* <span data-ttu-id="43d09-924">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="43d09-924">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="43d09-925"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="43d09-925">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="43d09-926">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="43d09-926">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="43d09-927">Общий пример конфигурации INI-файла.</span><span class="sxs-lookup"><span data-stu-id="43d09-927">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="43d09-928">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="43d09-928">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="43d09-929">section0:key0</span><span class="sxs-lookup"><span data-stu-id="43d09-929">section0:key0</span></span>
* <span data-ttu-id="43d09-930">section0:key1</span><span class="sxs-lookup"><span data-stu-id="43d09-930">section0:key1</span></span>
* <span data-ttu-id="43d09-931">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="43d09-931">section1:subsection:key</span></span>
* <span data-ttu-id="43d09-932">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="43d09-932">section2:subsection0:key</span></span>
* <span data-ttu-id="43d09-933">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="43d09-933">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="43d09-934">Поставщик конфигурации JSON</span><span class="sxs-lookup"><span data-stu-id="43d09-934">JSON Configuration Provider</span></span>

<span data-ttu-id="43d09-935"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> загружает конфигурацию из пары "ключ — значение" JSON-файла в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="43d09-935">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="43d09-936">Чтобы активировать конфигурацию JSON-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="43d09-936">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="43d09-937">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="43d09-937">Overloads permit specifying:</span></span>

* <span data-ttu-id="43d09-938">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="43d09-938">Whether the file is optional.</span></span>
* <span data-ttu-id="43d09-939">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="43d09-939">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="43d09-940"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="43d09-940">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="43d09-941">`AddJsonFile` автоматически вызывается дважды при инициализации нового построителя узла с `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="43d09-941">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="43d09-942">Метод вызывается для загрузки конфигурации из:</span><span class="sxs-lookup"><span data-stu-id="43d09-942">The method is called to load configuration from:</span></span>

* <span data-ttu-id="43d09-943">*appsettings.json* &ndash; первым читается этот файл.</span><span class="sxs-lookup"><span data-stu-id="43d09-943">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="43d09-944">Версия файла среды может переопределить значения, предоставленные *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="43d09-944">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="43d09-945">*appsettings.{Environment}.json* &ndash; версия среды файла загружается на основе [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="43d09-945">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="43d09-946">Дополнительные сведения см. в разделе [Конфигурация по умолчанию](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="43d09-946">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="43d09-947">`CreateDefaultBuilder` также загружает следующее:</span><span class="sxs-lookup"><span data-stu-id="43d09-947">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="43d09-948">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="43d09-948">Environment variables.</span></span>
* <span data-ttu-id="43d09-949">[секреты пользователя (Менеджер секретов)](xref:security/app-secrets) в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="43d09-949">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="43d09-950">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="43d09-950">Command-line arguments.</span></span>

<span data-ttu-id="43d09-951">Сначала устанавливается поставщик конфигурации JSON-файлов.</span><span class="sxs-lookup"><span data-stu-id="43d09-951">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="43d09-952">Таким образом, секреты пользователя, переменные среды и аргументы командной строки переопределят конфигурацию, заданную файлами *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="43d09-952">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="43d09-953">Вызовите `ConfigureAppConfiguration` при сборке узла, чтобы указать конфигурацию приложения для файлов, отличных от *appsettings.json* и *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="43d09-953">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="43d09-954">**Пример**</span><span class="sxs-lookup"><span data-stu-id="43d09-954">**Example**</span></span>

<span data-ttu-id="43d09-955">Пример приложения использует преимущества статически удобного метода `CreateDefaultBuilder` для создания узла, который включает два вызова `AddJsonFile`:</span><span class="sxs-lookup"><span data-stu-id="43d09-955">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="43d09-956">Первый вызов `AddJsonFile` загружает конфигурацию из *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="43d09-956">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="43d09-957">Второй вызов `AddJsonFile` загружает конфигурацию из *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="43d09-957">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="43d09-958">Для *appsettings.Development.json* в примере приложения загружается следующий файл:</span><span class="sxs-lookup"><span data-stu-id="43d09-958">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="43d09-959">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="43d09-959">Run the sample app.</span></span> <span data-ttu-id="43d09-960">Откройте в приложении браузер с адресом `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="43d09-960">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="43d09-961">Выходные данные содержат пары "ключ-значение" для конфигурации в зависимости от среды приложения.</span><span class="sxs-lookup"><span data-stu-id="43d09-961">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="43d09-962">Уровень ведения журнала для ключа `Logging:LogLevel:Default` имеет значение `Debug` при запуске приложения в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="43d09-962">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="43d09-963">Запустите пример приложения еще раз в рабочей среде:</span><span class="sxs-lookup"><span data-stu-id="43d09-963">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="43d09-964">Откройте файл *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="43d09-964">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="43d09-965">В профиле `ConfigurationSample` измените значение переменной среды `ASPNETCORE_ENVIRONMENT` на `Production`.</span><span class="sxs-lookup"><span data-stu-id="43d09-965">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="43d09-966">Сохраните файл и запустите приложение с помощью `dotnet run` в командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="43d09-966">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="43d09-967">Параметры в *appsettings.Development.json* больше не переопределяют параметры в *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="43d09-967">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="43d09-968">Уровень ведения журнала для ключа `Logging:LogLevel:Default` имеет значение `Warning`.</span><span class="sxs-lookup"><span data-stu-id="43d09-968">The log level for the key `Logging:LogLevel:Default` is `Warning`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="43d09-969">Поставщик конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="43d09-969">XML Configuration Provider</span></span>

<span data-ttu-id="43d09-970"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> загружает конфигурацию из пары "ключ — значение" XML-файла в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="43d09-970">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="43d09-971">Чтобы активировать конфигурацию XML-файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="43d09-971">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="43d09-972">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="43d09-972">Overloads permit specifying:</span></span>

* <span data-ttu-id="43d09-973">Файл является обязательным или нет.</span><span class="sxs-lookup"><span data-stu-id="43d09-973">Whether the file is optional.</span></span>
* <span data-ttu-id="43d09-974">Будет ли перезагружена конфигурация, если файл изменится.</span><span class="sxs-lookup"><span data-stu-id="43d09-974">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="43d09-975"><xref:Microsoft.Extensions.FileProviders.IFileProvider> используется для доступа к файлу.</span><span class="sxs-lookup"><span data-stu-id="43d09-975">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="43d09-976">Корневой узел файла конфигурации не учитывается при создании пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-976">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="43d09-977">Не указывайте в файле Document Type Definition (определение типа документа, DTD) или пространство имен.</span><span class="sxs-lookup"><span data-stu-id="43d09-977">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="43d09-978">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="43d09-978">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="43d09-979">XML-файлы конфигурации могут использовать имена отдельных элементов для повторяющихся разделов.</span><span class="sxs-lookup"><span data-stu-id="43d09-979">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="43d09-980">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="43d09-980">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="43d09-981">section0:key0</span><span class="sxs-lookup"><span data-stu-id="43d09-981">section0:key0</span></span>
* <span data-ttu-id="43d09-982">section0:key1</span><span class="sxs-lookup"><span data-stu-id="43d09-982">section0:key1</span></span>
* <span data-ttu-id="43d09-983">section1:key0</span><span class="sxs-lookup"><span data-stu-id="43d09-983">section1:key0</span></span>
* <span data-ttu-id="43d09-984">section1:key1</span><span class="sxs-lookup"><span data-stu-id="43d09-984">section1:key1</span></span>

<span data-ttu-id="43d09-985">Повторяющиеся элементы, использующие то же имя элемента, работают, если атрибут `name` используется для различения элементов.</span><span class="sxs-lookup"><span data-stu-id="43d09-985">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="43d09-986">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="43d09-986">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="43d09-987">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="43d09-987">section:section0:key:key0</span></span>
* <span data-ttu-id="43d09-988">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="43d09-988">section:section0:key:key1</span></span>
* <span data-ttu-id="43d09-989">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="43d09-989">section:section1:key:key0</span></span>
* <span data-ttu-id="43d09-990">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="43d09-990">section:section1:key:key1</span></span>

<span data-ttu-id="43d09-991">Атрибуты можно использовать для предоставления значений.</span><span class="sxs-lookup"><span data-stu-id="43d09-991">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="43d09-992">Предыдущий файл конфигурации загружает следующие ключи с помощью `value`.</span><span class="sxs-lookup"><span data-stu-id="43d09-992">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="43d09-993">key:attribute</span><span class="sxs-lookup"><span data-stu-id="43d09-993">key:attribute</span></span>
* <span data-ttu-id="43d09-994">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="43d09-994">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="43d09-995">Поставщик конфигурации ключа для каждого файла</span><span class="sxs-lookup"><span data-stu-id="43d09-995">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="43d09-996"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> использует файлы каталога как пары "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-996">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="43d09-997">Ключ является именем файла.</span><span class="sxs-lookup"><span data-stu-id="43d09-997">The key is the file name.</span></span> <span data-ttu-id="43d09-998">Значение содержит содержимое файла.</span><span class="sxs-lookup"><span data-stu-id="43d09-998">The value contains the file's contents.</span></span> <span data-ttu-id="43d09-999">Поставщик конфигурации ключа для каждого файла используется в сценариях размещения Docker.</span><span class="sxs-lookup"><span data-stu-id="43d09-999">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="43d09-1000">Чтобы активировать конфигурацию ключа для каждого файла, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="43d09-1000">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="43d09-1001">Значение параметра `directoryPath` должно быть абсолютным путем к файлам.</span><span class="sxs-lookup"><span data-stu-id="43d09-1001">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="43d09-1002">Перегрузки позволяют указать следующее.</span><span class="sxs-lookup"><span data-stu-id="43d09-1002">Overloads permit specifying:</span></span>

* <span data-ttu-id="43d09-1003">`Action<KeyPerFileConfigurationSource>` — делегат, который настраивает источник.</span><span class="sxs-lookup"><span data-stu-id="43d09-1003">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="43d09-1004">Обязательно ли указывать каталог и путь к каталогу.</span><span class="sxs-lookup"><span data-stu-id="43d09-1004">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="43d09-1005">Двойное подчеркивание (`__`) используется в качестве разделителя ключа конфигурации в именах файлов.</span><span class="sxs-lookup"><span data-stu-id="43d09-1005">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="43d09-1006">Например, в имени файла `Logging__LogLevel__System` создается ключ конфигурации `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="43d09-1006">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="43d09-1007">Чтобы указать конфигурацию приложения, при сборке веб-узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="43d09-1007">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="43d09-1008">Поставщик конфигурации памяти</span><span class="sxs-lookup"><span data-stu-id="43d09-1008">Memory Configuration Provider</span></span>

<span data-ttu-id="43d09-1009"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> использует коллекцию памяти в качестве пар "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-1009">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="43d09-1010">Чтобы активировать конфигурацию коллекции в памяти, вызовите метод расширения <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> в экземпляре <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="43d09-1010">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="43d09-1011">Поставщик конфигурации может инициализироваться значением `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="43d09-1011">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="43d09-1012">Чтобы указать конфигурацию приложения, при сборке узла вызовите `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="43d09-1012">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="43d09-1013">В следующем примере создается словарь конфигурации:</span><span class="sxs-lookup"><span data-stu-id="43d09-1013">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="43d09-1014">Словарь используется с вызовом `AddInMemoryCollection` для предоставления конфигурации:</span><span class="sxs-lookup"><span data-stu-id="43d09-1014">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="43d09-1015">GetValue</span><span class="sxs-lookup"><span data-stu-id="43d09-1015">GetValue</span></span>

<span data-ttu-id="43d09-1016">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) извлекает одно значение из конфигурации с указанным ключом и преобразует его в указанный тип, не являющийся коллекцией.</span><span class="sxs-lookup"><span data-stu-id="43d09-1016">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="43d09-1017">Перегрузка принимает значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="43d09-1017">An overload accepts a default value.</span></span>

<span data-ttu-id="43d09-1018">В следующем примере происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="43d09-1018">The following example:</span></span>

* <span data-ttu-id="43d09-1019">Из конфигурации извлекается строковое значение с ключом `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="43d09-1019">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="43d09-1020">Если `NumberKey` отсутствует в ключах конфигурации, используется значение по умолчанию `99`.</span><span class="sxs-lookup"><span data-stu-id="43d09-1020">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="43d09-1021">Значение получает тип `int`.</span><span class="sxs-lookup"><span data-stu-id="43d09-1021">Types the value as an `int`.</span></span>
* <span data-ttu-id="43d09-1022">Значение сохраняется в свойстве `NumberConfig` для использования на странице.</span><span class="sxs-lookup"><span data-stu-id="43d09-1022">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="43d09-1023">GetSection, GetChildren и Exists</span><span class="sxs-lookup"><span data-stu-id="43d09-1023">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="43d09-1024">В следующих примерах рассмотрим файл JSON.</span><span class="sxs-lookup"><span data-stu-id="43d09-1024">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="43d09-1025">Четыре ключа находятся в двух разделах, один из которых содержит пару из подразделов.</span><span class="sxs-lookup"><span data-stu-id="43d09-1025">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="43d09-1026">Когда файл считывается в конфигурацию, для сохранения значений конфигурации создаются следующие уникальные иерархические ключи.</span><span class="sxs-lookup"><span data-stu-id="43d09-1026">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="43d09-1027">section0:key0</span><span class="sxs-lookup"><span data-stu-id="43d09-1027">section0:key0</span></span>
* <span data-ttu-id="43d09-1028">section0:key1</span><span class="sxs-lookup"><span data-stu-id="43d09-1028">section0:key1</span></span>
* <span data-ttu-id="43d09-1029">section1:key0</span><span class="sxs-lookup"><span data-stu-id="43d09-1029">section1:key0</span></span>
* <span data-ttu-id="43d09-1030">section1:key1</span><span class="sxs-lookup"><span data-stu-id="43d09-1030">section1:key1</span></span>
* <span data-ttu-id="43d09-1031">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="43d09-1031">section2:subsection0:key0</span></span>
* <span data-ttu-id="43d09-1032">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="43d09-1032">section2:subsection0:key1</span></span>
* <span data-ttu-id="43d09-1033">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="43d09-1033">section2:subsection1:key0</span></span>
* <span data-ttu-id="43d09-1034">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="43d09-1034">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="43d09-1035">GetSection</span><span class="sxs-lookup"><span data-stu-id="43d09-1035">GetSection</span></span>

<span data-ttu-id="43d09-1036">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) извлекает подраздел конфигурации с указанным ключом подраздела.</span><span class="sxs-lookup"><span data-stu-id="43d09-1036">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="43d09-1037">Чтобы вернуть <xref:Microsoft.Extensions.Configuration.IConfigurationSection>, содержащий только пары "ключ — значение" в `section1`, вызовите `GetSection` и укажите имя раздела.</span><span class="sxs-lookup"><span data-stu-id="43d09-1037">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="43d09-1038">`configSection` не содержит значение, только ключ и путь.</span><span class="sxs-lookup"><span data-stu-id="43d09-1038">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="43d09-1039">Аналогично, чтобы получить значения для ключей в `section2:subsection0`, вызовите `GetSection` и укажите путь к разделу.</span><span class="sxs-lookup"><span data-stu-id="43d09-1039">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="43d09-1040">Значение `GetSection` никогда не возвращает значение `null`.</span><span class="sxs-lookup"><span data-stu-id="43d09-1040">`GetSection` never returns `null`.</span></span> <span data-ttu-id="43d09-1041">Если соответствующий раздел не найден, возвращается пустой параметр `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="43d09-1041">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="43d09-1042">Когда `GetSection` возвращает соответствующий раздел, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> не заполняется.</span><span class="sxs-lookup"><span data-stu-id="43d09-1042">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="43d09-1043"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> и <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> возвращаются, если раздел существует.</span><span class="sxs-lookup"><span data-stu-id="43d09-1043">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="43d09-1044">GetChildren</span><span class="sxs-lookup"><span data-stu-id="43d09-1044">GetChildren</span></span>

<span data-ttu-id="43d09-1045">Вызов [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) в `section2` получает значение `IEnumerable<IConfigurationSection>`, которое включает:</span><span class="sxs-lookup"><span data-stu-id="43d09-1045">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="43d09-1046">Exists</span><span class="sxs-lookup"><span data-stu-id="43d09-1046">Exists</span></span>

<span data-ttu-id="43d09-1047">Используйте [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*), чтобы определить, существует ли раздел конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-1047">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="43d09-1048">Учитывая данные примера, `sectionExists` является `false`, потому что в данных конфигурации нет `section2:subsection2`.</span><span class="sxs-lookup"><span data-stu-id="43d09-1048">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="43d09-1049">Привязка к классу</span><span class="sxs-lookup"><span data-stu-id="43d09-1049">Bind to a class</span></span>

<span data-ttu-id="43d09-1050">Конфигурация может быть привязана к классам, которые представляют группы связанных параметров, используя шаблон *вариантов*.</span><span class="sxs-lookup"><span data-stu-id="43d09-1050">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="43d09-1051">Для получения дополнительной информации см. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="43d09-1051">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="43d09-1052">Значения конфигурации возвращаются как строки, но вызов <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> позволяет построить объекты [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="43d09-1052">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="43d09-1053">Модуль привязки привязывает значения ко всем открытым свойствам чтения и записи предоставленного типа.</span><span class="sxs-lookup"><span data-stu-id="43d09-1053">The binder binds values to all of the public read/write properties of the type provided.</span></span> <span data-ttu-id="43d09-1054">Поля **не** привязаны.</span><span class="sxs-lookup"><span data-stu-id="43d09-1054">Fields are **not** bound.</span></span>

<span data-ttu-id="43d09-1055">Пример приложения содержит модель `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="43d09-1055">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="43d09-1056">Раздел `starship` файла *starship.json* создает конфигурацию, когда образец приложения использует поставщик конфигурации JSON для загрузки конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-1056">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

<span data-ttu-id="43d09-1057">Создаются следующие пары "ключ — значение" конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-1057">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="43d09-1058">Ключ</span><span class="sxs-lookup"><span data-stu-id="43d09-1058">Key</span></span>                   | <span data-ttu-id="43d09-1059">Значение</span><span class="sxs-lookup"><span data-stu-id="43d09-1059">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="43d09-1060">starship:name</span><span class="sxs-lookup"><span data-stu-id="43d09-1060">starship:name</span></span>         | <span data-ttu-id="43d09-1061">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="43d09-1061">USS Enterprise</span></span>                                    |
| <span data-ttu-id="43d09-1062">starship:registry</span><span class="sxs-lookup"><span data-stu-id="43d09-1062">starship:registry</span></span>     | <span data-ttu-id="43d09-1063">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="43d09-1063">NCC-1701</span></span>                                          |
| <span data-ttu-id="43d09-1064">starship:class</span><span class="sxs-lookup"><span data-stu-id="43d09-1064">starship:class</span></span>        | <span data-ttu-id="43d09-1065">Constitution</span><span class="sxs-lookup"><span data-stu-id="43d09-1065">Constitution</span></span>                                      |
| <span data-ttu-id="43d09-1066">starship:length</span><span class="sxs-lookup"><span data-stu-id="43d09-1066">starship:length</span></span>       | <span data-ttu-id="43d09-1067">304.8</span><span class="sxs-lookup"><span data-stu-id="43d09-1067">304.8</span></span>                                             |
| <span data-ttu-id="43d09-1068">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="43d09-1068">starship:commissioned</span></span> | <span data-ttu-id="43d09-1069">False</span><span class="sxs-lookup"><span data-stu-id="43d09-1069">False</span></span>                                             |
| <span data-ttu-id="43d09-1070">trademark</span><span class="sxs-lookup"><span data-stu-id="43d09-1070">trademark</span></span>             | <span data-ttu-id="43d09-1071">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="43d09-1071">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="43d09-1072">Пример приложения вызывает `GetSection` с помощью ключа `starship`.</span><span class="sxs-lookup"><span data-stu-id="43d09-1072">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="43d09-1073">Пары "ключ — значение" `starship` изолированы.</span><span class="sxs-lookup"><span data-stu-id="43d09-1073">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="43d09-1074">Метод `Bind` вызывается в подразделе при прохождении в экземпляр класса `Starship`.</span><span class="sxs-lookup"><span data-stu-id="43d09-1074">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="43d09-1075">После привязки значения экземпляра экземпляру присваивается свойство для преобразования для просмотра.</span><span class="sxs-lookup"><span data-stu-id="43d09-1075">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="43d09-1076">Привязка к графу объектов</span><span class="sxs-lookup"><span data-stu-id="43d09-1076">Bind to an object graph</span></span>

<span data-ttu-id="43d09-1077"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> способна связывать весь граф объекта POCO.</span><span class="sxs-lookup"><span data-stu-id="43d09-1077"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="43d09-1078">Как и при привязке простого объекта, привязываются только открытые свойства чтения и записи.</span><span class="sxs-lookup"><span data-stu-id="43d09-1078">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="43d09-1079">Образец содержит модель `TvShow`, в графе объектов которого находятся классы `Metadata` и `Actors` (*Модели/TvShow.cs*).</span><span class="sxs-lookup"><span data-stu-id="43d09-1079">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="43d09-1080">В примере приложения есть файл *tvshow.xml*, содержащий данные конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-1080">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="43d09-1081">Конфигурация привязана ко всему ​​графу объектов `TvShow` с помощью метода `Bind`.</span><span class="sxs-lookup"><span data-stu-id="43d09-1081">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="43d09-1082">Привязанный экземпляр присваивается свойству для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="43d09-1082">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="43d09-1083">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) привязывает и возвращает указанный тип.</span><span class="sxs-lookup"><span data-stu-id="43d09-1083">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="43d09-1084">Метод `Get<T>` может быть более удобным, чем использование `Bind`.</span><span class="sxs-lookup"><span data-stu-id="43d09-1084">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="43d09-1085">В следующем коде показано, как использовать `Get<T>` с предыдущим примером, который позволяет привязать связанный экземпляр непосредственно к свойству, используемому для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="43d09-1085">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="43d09-1086">Привязка массива к классу</span><span class="sxs-lookup"><span data-stu-id="43d09-1086">Bind an array to a class</span></span>

<span data-ttu-id="43d09-1087">*Пример приложения демонстрирует концепции, описанные в этом разделе.*</span><span class="sxs-lookup"><span data-stu-id="43d09-1087">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="43d09-1088"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> поддерживает массивы привязки к объектам с помощью массива индексов в ключах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-1088">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="43d09-1089">Любой формат массива, который предоставляет сегмент числового ключа (`:0:`, `:1:`, &hellip; `:{n}:`), способен привязать массив к массиву класса POCO.</span><span class="sxs-lookup"><span data-stu-id="43d09-1089">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="43d09-1090">Привязка предоставляется соглашением.</span><span class="sxs-lookup"><span data-stu-id="43d09-1090">Binding is provided by convention.</span></span> <span data-ttu-id="43d09-1091">Пользовательские поставщики конфигурации не обязаны реализовывать привязку массива.</span><span class="sxs-lookup"><span data-stu-id="43d09-1091">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="43d09-1092">**Обработка массива в оперативной памяти**</span><span class="sxs-lookup"><span data-stu-id="43d09-1092">**In-memory array processing**</span></span>

<span data-ttu-id="43d09-1093">Рассмотрите ключи и значения конфигурации, приведенные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="43d09-1093">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="43d09-1094">Ключ</span><span class="sxs-lookup"><span data-stu-id="43d09-1094">Key</span></span>             | <span data-ttu-id="43d09-1095">Значение</span><span class="sxs-lookup"><span data-stu-id="43d09-1095">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="43d09-1096">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="43d09-1096">array:entries:0</span></span> | <span data-ttu-id="43d09-1097">value0</span><span class="sxs-lookup"><span data-stu-id="43d09-1097">value0</span></span> |
| <span data-ttu-id="43d09-1098">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="43d09-1098">array:entries:1</span></span> | <span data-ttu-id="43d09-1099">value1</span><span class="sxs-lookup"><span data-stu-id="43d09-1099">value1</span></span> |
| <span data-ttu-id="43d09-1100">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="43d09-1100">array:entries:2</span></span> | <span data-ttu-id="43d09-1101">value2</span><span class="sxs-lookup"><span data-stu-id="43d09-1101">value2</span></span> |
| <span data-ttu-id="43d09-1102">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="43d09-1102">array:entries:4</span></span> | <span data-ttu-id="43d09-1103">value4</span><span class="sxs-lookup"><span data-stu-id="43d09-1103">value4</span></span> |
| <span data-ttu-id="43d09-1104">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="43d09-1104">array:entries:5</span></span> | <span data-ttu-id="43d09-1105">value5</span><span class="sxs-lookup"><span data-stu-id="43d09-1105">value5</span></span> |

<span data-ttu-id="43d09-1106">Эти ключи и значения загружаются в пример приложения с помощью поставщика конфигурации памяти.</span><span class="sxs-lookup"><span data-stu-id="43d09-1106">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="43d09-1107">Массив пропускает значения для индекса &num;3.</span><span class="sxs-lookup"><span data-stu-id="43d09-1107">The array skips a value for index &num;3.</span></span> <span data-ttu-id="43d09-1108">Связующее свойство конфигурации не способно связывать нулевые значения или создавать нулевые записи в связанных объектах, что становится ясным в тот момент, когда демонстрируется результат привязки этого массива к объекту.</span><span class="sxs-lookup"><span data-stu-id="43d09-1108">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="43d09-1109">В примере приложения класс POCO доступен для хранения привязанных данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-1109">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="43d09-1110">Данные конфигурации, связанные с объектом.</span><span class="sxs-lookup"><span data-stu-id="43d09-1110">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="43d09-1111">Синтаксис [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) также может использоваться для получения более компактного кода.</span><span class="sxs-lookup"><span data-stu-id="43d09-1111">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="43d09-1112">Связанный объект, экземпляр `ArrayExample`, получает данные массива из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-1112">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="43d09-1113">Индекс `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="43d09-1113">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="43d09-1114">Значение `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="43d09-1114">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="43d09-1115">0</span><span class="sxs-lookup"><span data-stu-id="43d09-1115">0</span></span>                            | <span data-ttu-id="43d09-1116">value0</span><span class="sxs-lookup"><span data-stu-id="43d09-1116">value0</span></span>                       |
| <span data-ttu-id="43d09-1117">1</span><span class="sxs-lookup"><span data-stu-id="43d09-1117">1</span></span>                            | <span data-ttu-id="43d09-1118">value1</span><span class="sxs-lookup"><span data-stu-id="43d09-1118">value1</span></span>                       |
| <span data-ttu-id="43d09-1119">2</span><span class="sxs-lookup"><span data-stu-id="43d09-1119">2</span></span>                            | <span data-ttu-id="43d09-1120">value2</span><span class="sxs-lookup"><span data-stu-id="43d09-1120">value2</span></span>                       |
| <span data-ttu-id="43d09-1121">3</span><span class="sxs-lookup"><span data-stu-id="43d09-1121">3</span></span>                            | <span data-ttu-id="43d09-1122">value4</span><span class="sxs-lookup"><span data-stu-id="43d09-1122">value4</span></span>                       |
| <span data-ttu-id="43d09-1123">4</span><span class="sxs-lookup"><span data-stu-id="43d09-1123">4</span></span>                            | <span data-ttu-id="43d09-1124">value5</span><span class="sxs-lookup"><span data-stu-id="43d09-1124">value5</span></span>                       |

<span data-ttu-id="43d09-1125">Индекс &num;3 в связанном объекте содержит данные конфигурации для конфигурационного ключа `array:4` и его значения `value4`.</span><span class="sxs-lookup"><span data-stu-id="43d09-1125">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="43d09-1126">При привязке данных конфигурации, содержащих массив индексов, в ключах конфигурации эти индексы просто используются для выполнения итерации данных конфигурации при создании объекта.</span><span class="sxs-lookup"><span data-stu-id="43d09-1126">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="43d09-1127">Когда массив ключей конфигурации пропускает один или несколько индексов, в данных конфигурации не может быть сохранено нулевое значение и в связанном объекте не создается нулевая запись.</span><span class="sxs-lookup"><span data-stu-id="43d09-1127">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="43d09-1128">Отсутствующий элемент конфигурации для индекса &num;3 может быть предоставлен перед привязкой к экземпляру `ArrayExample` любым поставщиком конфигурации, который создает правильную пару "ключ — значение" в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-1128">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="43d09-1129">Если в образец включен дополнительный поставщик конфигурации JSON с отсутствующей парой "ключ — значение", то `ArrayExample.Entries` подойдет полному массиву конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-1129">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="43d09-1130">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="43d09-1130">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="43d09-1131">В `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="43d09-1131">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="43d09-1132">Пары "ключ — значение", показанные в таблице, загружаются в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-1132">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="43d09-1133">Ключ</span><span class="sxs-lookup"><span data-stu-id="43d09-1133">Key</span></span>             | <span data-ttu-id="43d09-1134">Значение</span><span class="sxs-lookup"><span data-stu-id="43d09-1134">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="43d09-1135">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="43d09-1135">array:entries:3</span></span> | <span data-ttu-id="43d09-1136">value3</span><span class="sxs-lookup"><span data-stu-id="43d09-1136">value3</span></span> |

<span data-ttu-id="43d09-1137">Если экземпляр класса `ArrayExample` связан после того, как поставщик конфигурации JSON включает запись для индекса &num;3, то массив `ArrayExample.Entries` включит значение.</span><span class="sxs-lookup"><span data-stu-id="43d09-1137">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="43d09-1138">Индекс `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="43d09-1138">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="43d09-1139">Значение `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="43d09-1139">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="43d09-1140">0</span><span class="sxs-lookup"><span data-stu-id="43d09-1140">0</span></span>                            | <span data-ttu-id="43d09-1141">value0</span><span class="sxs-lookup"><span data-stu-id="43d09-1141">value0</span></span>                       |
| <span data-ttu-id="43d09-1142">1</span><span class="sxs-lookup"><span data-stu-id="43d09-1142">1</span></span>                            | <span data-ttu-id="43d09-1143">value1</span><span class="sxs-lookup"><span data-stu-id="43d09-1143">value1</span></span>                       |
| <span data-ttu-id="43d09-1144">2</span><span class="sxs-lookup"><span data-stu-id="43d09-1144">2</span></span>                            | <span data-ttu-id="43d09-1145">value2</span><span class="sxs-lookup"><span data-stu-id="43d09-1145">value2</span></span>                       |
| <span data-ttu-id="43d09-1146">3</span><span class="sxs-lookup"><span data-stu-id="43d09-1146">3</span></span>                            | <span data-ttu-id="43d09-1147">value3</span><span class="sxs-lookup"><span data-stu-id="43d09-1147">value3</span></span>                       |
| <span data-ttu-id="43d09-1148">4</span><span class="sxs-lookup"><span data-stu-id="43d09-1148">4</span></span>                            | <span data-ttu-id="43d09-1149">value4</span><span class="sxs-lookup"><span data-stu-id="43d09-1149">value4</span></span>                       |
| <span data-ttu-id="43d09-1150">5</span><span class="sxs-lookup"><span data-stu-id="43d09-1150">5</span></span>                            | <span data-ttu-id="43d09-1151">value5</span><span class="sxs-lookup"><span data-stu-id="43d09-1151">value5</span></span>                       |

<span data-ttu-id="43d09-1152">**Обработка массива JSON**</span><span class="sxs-lookup"><span data-stu-id="43d09-1152">**JSON array processing**</span></span>

<span data-ttu-id="43d09-1153">Если JSON-файл содержит массив, ключи конфигурации будут созданы для элементов массива с индексом раздела, начиная с нуля.</span><span class="sxs-lookup"><span data-stu-id="43d09-1153">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="43d09-1154">В следующем файле конфигурации `subsection` — это массив.</span><span class="sxs-lookup"><span data-stu-id="43d09-1154">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="43d09-1155">Поставщик конфигурации JSON считывает данные конфигурации в следующие пары "ключ — значение".</span><span class="sxs-lookup"><span data-stu-id="43d09-1155">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="43d09-1156">Ключ</span><span class="sxs-lookup"><span data-stu-id="43d09-1156">Key</span></span>                     | <span data-ttu-id="43d09-1157">Значение</span><span class="sxs-lookup"><span data-stu-id="43d09-1157">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="43d09-1158">json_array:key</span><span class="sxs-lookup"><span data-stu-id="43d09-1158">json_array:key</span></span>          | <span data-ttu-id="43d09-1159">valueA</span><span class="sxs-lookup"><span data-stu-id="43d09-1159">valueA</span></span> |
| <span data-ttu-id="43d09-1160">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="43d09-1160">json_array:subsection:0</span></span> | <span data-ttu-id="43d09-1161">valueB</span><span class="sxs-lookup"><span data-stu-id="43d09-1161">valueB</span></span> |
| <span data-ttu-id="43d09-1162">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="43d09-1162">json_array:subsection:1</span></span> | <span data-ttu-id="43d09-1163">valueC</span><span class="sxs-lookup"><span data-stu-id="43d09-1163">valueC</span></span> |
| <span data-ttu-id="43d09-1164">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="43d09-1164">json_array:subsection:2</span></span> | <span data-ttu-id="43d09-1165">valueD</span><span class="sxs-lookup"><span data-stu-id="43d09-1165">valueD</span></span> |

<span data-ttu-id="43d09-1166">В примере приложения для привязки пар "ключ — значение" конфигурации доступен следующий класс POCO.</span><span class="sxs-lookup"><span data-stu-id="43d09-1166">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="43d09-1167">После выполнения привязки `JsonArrayExample.Key` содержит значение `valueA`.</span><span class="sxs-lookup"><span data-stu-id="43d09-1167">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="43d09-1168">Подраздел значения хранится в свойстве массива POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="43d09-1168">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="43d09-1169">Индекс `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="43d09-1169">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="43d09-1170">Значение `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="43d09-1170">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="43d09-1171">0</span><span class="sxs-lookup"><span data-stu-id="43d09-1171">0</span></span>                                   | <span data-ttu-id="43d09-1172">valueB</span><span class="sxs-lookup"><span data-stu-id="43d09-1172">valueB</span></span>                              |
| <span data-ttu-id="43d09-1173">1</span><span class="sxs-lookup"><span data-stu-id="43d09-1173">1</span></span>                                   | <span data-ttu-id="43d09-1174">valueC</span><span class="sxs-lookup"><span data-stu-id="43d09-1174">valueC</span></span>                              |
| <span data-ttu-id="43d09-1175">2</span><span class="sxs-lookup"><span data-stu-id="43d09-1175">2</span></span>                                   | <span data-ttu-id="43d09-1176">valueD</span><span class="sxs-lookup"><span data-stu-id="43d09-1176">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="43d09-1177">Поставщик пользовательской конфигурации</span><span class="sxs-lookup"><span data-stu-id="43d09-1177">Custom configuration provider</span></span>

<span data-ttu-id="43d09-1178">Пример приложения демонстрирует, как создать базовый поставщик конфигурации, который считывает пары "ключ — значение" конфигурации из базы данных, используя [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="43d09-1178">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="43d09-1179">Поставщик имеет следующие характеристики.</span><span class="sxs-lookup"><span data-stu-id="43d09-1179">The provider has the following characteristics:</span></span>

* <span data-ttu-id="43d09-1180">База данных в памяти EF используется для демонстрационных целей.</span><span class="sxs-lookup"><span data-stu-id="43d09-1180">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="43d09-1181">Чтобы использовать базу данных, для которой требуется строка подключения, выполните вторичный `ConfigurationBuilder`, чтобы предоставить строку подключения от другого поставщика конфигурации.</span><span class="sxs-lookup"><span data-stu-id="43d09-1181">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="43d09-1182">Поставщик считывает таблицу базы данных в конфигурации при запуске.</span><span class="sxs-lookup"><span data-stu-id="43d09-1182">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="43d09-1183">Поставщик не запрашивает базу данных для каждого ключа.</span><span class="sxs-lookup"><span data-stu-id="43d09-1183">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="43d09-1184">Функция перезагрузки на изменение не реализована, поэтому обновление базы данных после запуска приложения не влияет на конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="43d09-1184">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="43d09-1185">Определите сущность `EFConfigurationValue` для хранения значений конфигурации в базе данных.</span><span class="sxs-lookup"><span data-stu-id="43d09-1185">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="43d09-1186">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="43d09-1186">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="43d09-1187">Добавьте `EFConfigurationContext` в хранилище и обратитесь к настроенным значениям.</span><span class="sxs-lookup"><span data-stu-id="43d09-1187">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="43d09-1188">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="43d09-1188">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="43d09-1189">Создайте класс, реализующий <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="43d09-1189">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="43d09-1190">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="43d09-1190">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="43d09-1191">Создайте пользовательский поставщик конфигурации путем наследования от <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="43d09-1191">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="43d09-1192">Поставщик конфигурации инициализирует пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="43d09-1192">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="43d09-1193">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="43d09-1193">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="43d09-1194">Метод расширения `AddEFConfiguration` позволяет добавить источник конфигурации к `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="43d09-1194">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="43d09-1195">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="43d09-1195">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="43d09-1196">В следующем коде показано, как использовать пользовательский `EFConfigurationProvider` в *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="43d09-1196">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="43d09-1197">Доступ к конфигурации во время запуска</span><span class="sxs-lookup"><span data-stu-id="43d09-1197">Access configuration during startup</span></span>

<span data-ttu-id="43d09-1198">Внесите значение `IConfiguration` в конструктор `Startup` для доступа к значениям конфигурации в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="43d09-1198">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="43d09-1199">Чтобы получить доступ к конфигурации в `Startup.Configure`, либо добавьте значение `IConfiguration` непосредственно в метод, либо используйте экземпляр из конструктора.</span><span class="sxs-lookup"><span data-stu-id="43d09-1199">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="43d09-1200">Дополнительные сведения см. в руководстве по [доступу к конфигурации с использованием удобных методов](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="43d09-1200">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="43d09-1201">Настройте доступ на странице Razor Pages или в представлении MVC</span><span class="sxs-lookup"><span data-stu-id="43d09-1201">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="43d09-1202">Для доступа к параметрам конфигурации на странице Razor Pages или в представлении MVC добавьте [использование директивы](xref:mvc/views/razor#using) ([Справочник по C#: использование директивы](/dotnet/csharp/language-reference/keywords/using-directive)) для [пространства имен Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) и вставьте <xref:Microsoft.Extensions.Configuration.IConfiguration> на страницу или в представление.</span><span class="sxs-lookup"><span data-stu-id="43d09-1202">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="43d09-1203">На странице Razor Pages</span><span class="sxs-lookup"><span data-stu-id="43d09-1203">In a Razor Pages page:</span></span>

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

<span data-ttu-id="43d09-1204">В представлении MVC</span><span class="sxs-lookup"><span data-stu-id="43d09-1204">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="43d09-1205">Добавление конфигурации из внешней сборки</span><span class="sxs-lookup"><span data-stu-id="43d09-1205">Add configuration from an external assembly</span></span>

<span data-ttu-id="43d09-1206">Реализация <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> позволяет при запуске добавлять в приложение улучшения из внешней сборки вне приложения класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="43d09-1206">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="43d09-1207">Для получения дополнительной информации см. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="43d09-1207">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="43d09-1208">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="43d09-1208">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end
