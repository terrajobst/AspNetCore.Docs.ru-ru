---
title: Конфигурация в .NET Core
author: rick-anderson
description: Узнайте, как использовать API конфигурации для настройки приложения ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 59ab0cd0f6975d15bd01ce7e4128521938182c24
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228628"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="77fd0-103">Конфигурация в .NET Core</span><span class="sxs-lookup"><span data-stu-id="77fd0-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="77fd0-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson), [Марк Михаэлис](http://intellitect.com/author/mark-michaelis/) (Mark Michaelis), [Стив Смит](https://ardalis.com/) (Steve Smith), [Даниэль Рот](https://github.com/danroth27) (Daniel Roth) и [Люк Лэтхэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="77fd0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="77fd0-105">API конфигурации позволяет настраивать веб-приложения ASP.NET Core на основе списка пар "имя-значение".</span><span class="sxs-lookup"><span data-stu-id="77fd0-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="77fd0-106">Конфигурация считывается во время выполнения из нескольких источников.</span><span class="sxs-lookup"><span data-stu-id="77fd0-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="77fd0-107">Пары "имя-значение" можно сгруппировать в многоуровневую иерархию.</span><span class="sxs-lookup"><span data-stu-id="77fd0-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="77fd0-108">Существуют следующие поставщики конфигурации:</span><span class="sxs-lookup"><span data-stu-id="77fd0-108">There are configuration providers for:</span></span>

* <span data-ttu-id="77fd0-109">Форматы файлов (INI, JSON и XML).</span><span class="sxs-lookup"><span data-stu-id="77fd0-109">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="77fd0-110">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="77fd0-110">Command-line arguments.</span></span>
* <span data-ttu-id="77fd0-111">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="77fd0-111">Environment variables.</span></span>
* <span data-ttu-id="77fd0-112">Объекты .NET в памяти.</span><span class="sxs-lookup"><span data-stu-id="77fd0-112">In-memory .NET objects.</span></span>
* <span data-ttu-id="77fd0-113">Незашифрованное хранилище [Secret Manager](xref:security/app-secrets) (Диспетчер секретов).</span><span class="sxs-lookup"><span data-stu-id="77fd0-113">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="77fd0-114">Зашифрованное пользовательское хранилище, например [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="77fd0-114">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="77fd0-115">Пользовательские поставщики (установленные или созданные).</span><span class="sxs-lookup"><span data-stu-id="77fd0-115">Custom providers (installed or created).</span></span>

<span data-ttu-id="77fd0-116">Каждое значение конфигурации сопоставляется строковому ключу.</span><span class="sxs-lookup"><span data-stu-id="77fd0-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="77fd0-117">Имеется встроенная поддержка привязки для десериализации параметров в пользовательский объект [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) (простой класс .NET со свойствами).</span><span class="sxs-lookup"><span data-stu-id="77fd0-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="77fd0-118">Шаблон параметров использует классы параметров для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="77fd0-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="77fd0-119">Дополнительные сведения об использовании шаблона параметров см. в разделе [Параметры](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="77fd0-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="77fd0-120">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="77fd0-120">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="77fd0-121">Примеры, приведенные в этом разделе, основаны на следующем.</span><span class="sxs-lookup"><span data-stu-id="77fd0-121">Examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="77fd0-122">Указание базового пути приложения с помощью [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span><span class="sxs-lookup"><span data-stu-id="77fd0-122">Setting the base path of the app with [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span></span> <span data-ttu-id="77fd0-123">`SetBasePath` предоставляется приложению с помощью ссылки на пакет [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/).</span><span class="sxs-lookup"><span data-stu-id="77fd0-123">`SetBasePath` is made available to the app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="77fd0-124">Разрешение разделов файлов конфигурации с помощью [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span><span class="sxs-lookup"><span data-stu-id="77fd0-124">Resolving sections of configuration files with [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span></span> <span data-ttu-id="77fd0-125">`GetSection` предоставляется приложению с помощью ссылки на пакет [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/).</span><span class="sxs-lookup"><span data-stu-id="77fd0-125">`GetSection` is made available to the app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="77fd0-126">Настройка привязки с помощью [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span><span class="sxs-lookup"><span data-stu-id="77fd0-126">Binding configuration with [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span></span> <span data-ttu-id="77fd0-127">`Bind` предоставляется приложению с помощью ссылки на пакет [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/).</span><span class="sxs-lookup"><span data-stu-id="77fd0-127">`Bind` is made available to the app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span>

<span data-ttu-id="77fd0-128">Пакеты включены в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="77fd0-128">These packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="77fd0-129">Примеры, приведенные в этом разделе, основаны на следующем.</span><span class="sxs-lookup"><span data-stu-id="77fd0-129">Examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="77fd0-130">Указание базового пути приложения с помощью [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span><span class="sxs-lookup"><span data-stu-id="77fd0-130">Setting the base path of the app with [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span></span> <span data-ttu-id="77fd0-131">`SetBasePath` предоставляется приложению с помощью ссылки на пакет [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/).</span><span class="sxs-lookup"><span data-stu-id="77fd0-131">`SetBasePath` is made available to the app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="77fd0-132">Разрешение разделов файлов конфигурации с помощью [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span><span class="sxs-lookup"><span data-stu-id="77fd0-132">Resolving sections of configuration files with [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span></span> <span data-ttu-id="77fd0-133">`GetSection` предоставляется приложению с помощью ссылки на пакет [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/).</span><span class="sxs-lookup"><span data-stu-id="77fd0-133">`GetSection` is made available to the app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="77fd0-134">Настройка привязки с помощью [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span><span class="sxs-lookup"><span data-stu-id="77fd0-134">Binding configuration with [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span></span> <span data-ttu-id="77fd0-135">`Bind` предоставляется приложению с помощью ссылки на пакет [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/).</span><span class="sxs-lookup"><span data-stu-id="77fd0-135">`Bind` is made available to the app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span>

<span data-ttu-id="77fd0-136">Эти пакеты включены в [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="77fd0-136">These packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="77fd0-137">Примеры, приведенные в этом разделе, основаны на следующем.</span><span class="sxs-lookup"><span data-stu-id="77fd0-137">Examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="77fd0-138">Указание базового пути приложения с помощью [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span><span class="sxs-lookup"><span data-stu-id="77fd0-138">Setting the base path of the app with [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span></span> <span data-ttu-id="77fd0-139">`SetBasePath` предоставляется приложению с помощью ссылки на пакет [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/).</span><span class="sxs-lookup"><span data-stu-id="77fd0-139">`SetBasePath` is made available to the app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="77fd0-140">Разрешение разделов файлов конфигурации с помощью [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span><span class="sxs-lookup"><span data-stu-id="77fd0-140">Resolving sections of configuration files with [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span></span> <span data-ttu-id="77fd0-141">`GetSection` предоставляется приложению с помощью ссылки на пакет [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/).</span><span class="sxs-lookup"><span data-stu-id="77fd0-141">`GetSection` is made available to the app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="77fd0-142">Настройка привязки с помощью [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span><span class="sxs-lookup"><span data-stu-id="77fd0-142">Binding configuration with [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span></span> <span data-ttu-id="77fd0-143">`Bind` предоставляется приложению с помощью ссылки на пакет [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/).</span><span class="sxs-lookup"><span data-stu-id="77fd0-143">`Bind` is made available to the app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span>

::: moniker-end

## <a name="json-configuration"></a><span data-ttu-id="77fd0-144">Конфигурация JSON</span><span class="sxs-lookup"><span data-stu-id="77fd0-144">JSON configuration</span></span>

<span data-ttu-id="77fd0-145">В следующем консольном приложении используется поставщик конфигурации JSON:</span><span class="sxs-lookup"><span data-stu-id="77fd0-145">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="77fd0-146">Приложение считывает и отображает следующие параметры конфигурации приложения:</span><span class="sxs-lookup"><span data-stu-id="77fd0-146">The app reads and displays the following configuration settings:</span></span>

[!code-json[](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="77fd0-147">Конфигурация — это иерархический список пар "имя-значение", в котором узлы разделяются двоеточием (`:`).</span><span class="sxs-lookup"><span data-stu-id="77fd0-147">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon (`:`).</span></span> <span data-ttu-id="77fd0-148">Чтобы получить значение, обратитесь к индексатору `Configuration` с ключом соответствующего элемента:</span><span class="sxs-lookup"><span data-stu-id="77fd0-148">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="77fd0-149">Для работы с массивами в источниках конфигурации в формате JSON используйте индекс массива в составе строки с разделителем-двоеточием.</span><span class="sxs-lookup"><span data-stu-id="77fd0-149">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="77fd0-150">В следующем примере возвращается имя первого элемента в предыдущем массиве `wizards`:</span><span class="sxs-lookup"><span data-stu-id="77fd0-150">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="77fd0-151">Пары "имя-значение", записываемые во встроенные поставщики [конфигурации](/dotnet/api/microsoft.extensions.configuration), **не сохраняются**.</span><span class="sxs-lookup"><span data-stu-id="77fd0-151">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="77fd0-152">Но вы можете создать настраиваемый поставщик, который сохраняет значения.</span><span class="sxs-lookup"><span data-stu-id="77fd0-152">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="77fd0-153">См. раздел [Пользовательский поставщик конфигурации](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="77fd0-153">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="77fd0-154">В предыдущем примере для считывания значений используется индексатор конфигурации.</span><span class="sxs-lookup"><span data-stu-id="77fd0-154">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="77fd0-155">Для доступа к конфигурации вне `Startup` воспользуйтесь *шаблоном параметров*.</span><span class="sxs-lookup"><span data-stu-id="77fd0-155">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="77fd0-156">Дополнительные сведения см. в разделе [Параметры](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="77fd0-156">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

## <a name="xml-configuration"></a><span data-ttu-id="77fd0-157">Конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="77fd0-157">XML configuration</span></span>

<span data-ttu-id="77fd0-158">Для работы с массивами в источниках конфигураций с XML-форматированием укажите индекс `name` для каждого элемента.</span><span class="sxs-lookup"><span data-stu-id="77fd0-158">To work with arrays in XML-formatted configuration sources, provide a `name` index to each element.</span></span> <span data-ttu-id="77fd0-159">Используйте этот индекс для доступа к значениям.</span><span class="sxs-lookup"><span data-stu-id="77fd0-159">Use the index to access the values:</span></span>

```xml
<wizards>
  <wizard name="Gandalf">
    <age>1000</age>
  </wizard>
  <wizard name="Harry">
    <age>17</age>
  </wizard>
</wizards>
```

```csharp
Console.Write($"{Configuration["wizard:Harry:age"]}");
// Output: 17
```

## <a name="configuration-by-environment"></a><span data-ttu-id="77fd0-160">Конфигурация для разных сред</span><span class="sxs-lookup"><span data-stu-id="77fd0-160">Configuration by environment</span></span>

<span data-ttu-id="77fd0-161">Как правило, для разных сред (например, разработки, тестирования и производства) существуют разные параметры конфигурации.</span><span class="sxs-lookup"><span data-stu-id="77fd0-161">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="77fd0-162">Метод расширения `CreateDefaultBuilder` в приложении ASP.NET Core 2.x (или при использовании `AddJsonFile` и `AddEnvironmentVariables` непосредственно в приложении ASP.NET Core 1.x) добавляет поставщики конфигурации для чтения JSON-файлов и источников системной конфигурации:</span><span class="sxs-lookup"><span data-stu-id="77fd0-162">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="77fd0-163">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="77fd0-163">*appsettings.json*</span></span>
* <span data-ttu-id="77fd0-164">*appsettings.\<имя_среды>.json*</span><span class="sxs-lookup"><span data-stu-id="77fd0-164">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="77fd0-165">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="77fd0-165">Environment variables</span></span>

<span data-ttu-id="77fd0-166">Приложения ASP.NET Core 1.x должны вызывать `AddJsonFile` и [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span><span class="sxs-lookup"><span data-stu-id="77fd0-166">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="77fd0-167">Объяснение параметров см. в разделе [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions).</span><span class="sxs-lookup"><span data-stu-id="77fd0-167">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="77fd0-168">`reloadOnChange` поддерживается только в ASP.NET Core 1.1 и более поздних версиях.</span><span class="sxs-lookup"><span data-stu-id="77fd0-168">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="77fd0-169">Источники конфигурации считываются в порядке их указания.</span><span class="sxs-lookup"><span data-stu-id="77fd0-169">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="77fd0-170">В приведенном выше коде переменные сред считываются последними.</span><span class="sxs-lookup"><span data-stu-id="77fd0-170">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="77fd0-171">Все значения конфигурации, заданные с помощью среды, заменяют значения, заданные в предыдущих двух поставщиках.</span><span class="sxs-lookup"><span data-stu-id="77fd0-171">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="77fd0-172">Рассмотрим следующий файл *appsettings.Staging.JSON*.</span><span class="sxs-lookup"><span data-stu-id="77fd0-172">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[](index/sample/appsettings.Staging.json)]

<span data-ttu-id="77fd0-173">В следующем коде `Configure` считывает значение `MyConfig`.</span><span class="sxs-lookup"><span data-stu-id="77fd0-173">In the following code, `Configure` reads the value of `MyConfig`:</span></span>

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]

<span data-ttu-id="77fd0-174">Для среды обычно задаются значения `Development`, `Staging` или `Production`.</span><span class="sxs-lookup"><span data-stu-id="77fd0-174">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="77fd0-175">Дополнительные сведения см. в статье [Использование нескольких сред](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="77fd0-175">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="77fd0-176">Рекомендации по конфигурации:</span><span class="sxs-lookup"><span data-stu-id="77fd0-176">Configuration considerations:</span></span>

* <span data-ttu-id="77fd0-177">[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) может перезагрузить данные конфигурации после их изменения.</span><span class="sxs-lookup"><span data-stu-id="77fd0-177">[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) can reload configuration data when it changes.</span></span>
* <span data-ttu-id="77fd0-178">Ключи конфигурации **не учитывают** регистр.</span><span class="sxs-lookup"><span data-stu-id="77fd0-178">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="77fd0-179">**Никогда** не храните пароли и другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации в виде обычного текста.</span><span class="sxs-lookup"><span data-stu-id="77fd0-179">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="77fd0-180">Не используйте секреты рабочей среды в средах разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="77fd0-180">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="77fd0-181">Указывайте секреты вне проекта, чтобы их нельзя было случайно зафиксировать в репозитории с исходным кодом.</span><span class="sxs-lookup"><span data-stu-id="77fd0-181">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="77fd0-182">Узнайте больше об [использовании нескольких сред](xref:fundamentals/environments) и управлении [безопасным хранением секретов приложения во время разработки](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="77fd0-182">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets in development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="77fd0-183">Для значений иерархической конфигурации, указанных в переменных среды, двоеточие (`:`) поддерживается не на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="77fd0-183">For hierarchical config values specified in environment variables, a colon (`:`) may not work on all platforms.</span></span> <span data-ttu-id="77fd0-184">Двойной знак подчеркивания (`__`) поддерживается на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="77fd0-184">Double underscore (`__`) is supported by all platforms.</span></span>
* <span data-ttu-id="77fd0-185">При взаимодействии с API конфигурации двоеточие (`:`) поддерживается на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="77fd0-185">When interacting with the configuration API, a colon (`:`) works on all platforms.</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="77fd0-186">Поставщик в памяти и привязка к классу POCO</span><span class="sxs-lookup"><span data-stu-id="77fd0-186">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="77fd0-187">В следующем примере демонстрируется использование поставщика в памяти и привязка к классу:</span><span class="sxs-lookup"><span data-stu-id="77fd0-187">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[](index/sample/InMemory/Program.cs)]

<span data-ttu-id="77fd0-188">Значения конфигурации возвращаются в виде строк, но с помощью привязки можно создавать объекты.</span><span class="sxs-lookup"><span data-stu-id="77fd0-188">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="77fd0-189">Привязка позволяет извлекать объекты POCO и даже целые графы объектов.</span><span class="sxs-lookup"><span data-stu-id="77fd0-189">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="77fd0-190">GetValue</span><span class="sxs-lookup"><span data-stu-id="77fd0-190">GetValue</span></span>

<span data-ttu-id="77fd0-191">В следующем примере показан метод расширения [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_):</span><span class="sxs-lookup"><span data-stu-id="77fd0-191">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="77fd0-192">Метод `GetValue<T>` из ConfigurationBinder позволяет задавать значение по умолчанию (в примере — 80).</span><span class="sxs-lookup"><span data-stu-id="77fd0-192">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="77fd0-193">`GetValue<T>` используется для простых сценариев и не выполняет привязку ко всем разделам.</span><span class="sxs-lookup"><span data-stu-id="77fd0-193">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="77fd0-194">Метод `GetValue<T>` получает из `GetSection(key).Value` скалярные значения, преобразованные в определенный тип.</span><span class="sxs-lookup"><span data-stu-id="77fd0-194">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="77fd0-195">Привязка к графу объектов</span><span class="sxs-lookup"><span data-stu-id="77fd0-195">Bind to an object graph</span></span>

<span data-ttu-id="77fd0-196">Для каждого объекта в классе возможна рекурсивная привязка.</span><span class="sxs-lookup"><span data-stu-id="77fd0-196">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="77fd0-197">Рассмотрим следующий класс `AppSettings`:</span><span class="sxs-lookup"><span data-stu-id="77fd0-197">Consider the following `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="77fd0-198">В следующем примере выполняется привязка к классу `AppSettings`:</span><span class="sxs-lookup"><span data-stu-id="77fd0-198">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="77fd0-199">**ASP.NET Core 1.1** и более поздние версии могут использовать метод `Get<T>`, который работает с целыми разделами.</span><span class="sxs-lookup"><span data-stu-id="77fd0-199">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="77fd0-200">Метод `Get<T>` может быть более удобным, чем `Bind`.</span><span class="sxs-lookup"><span data-stu-id="77fd0-200">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="77fd0-201">Следующий код показывает, как использовать `Get<T>` с предыдущим примером.</span><span class="sxs-lookup"><span data-stu-id="77fd0-201">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="77fd0-202">Используется следующий файл *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="77fd0-202">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="77fd0-203">Программа выводит `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="77fd0-203">The program displays `Height 11`.</span></span>

<span data-ttu-id="77fd0-204">Для модульного тестирования конфигурации можно выполнить следующий код:</span><span class="sxs-lookup"><span data-stu-id="77fd0-204">The following code can be used to unit test the configuration:</span></span>

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="77fd0-205">Создание пользовательского поставщика Entity Framework</span><span class="sxs-lookup"><span data-stu-id="77fd0-205">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="77fd0-206">В этом разделе создается поставщик базовой конфигурации, который считывает пары "имя-значение" из базы данных с помощью EF.</span><span class="sxs-lookup"><span data-stu-id="77fd0-206">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="77fd0-207">Определите сущность `ConfigurationValue` для хранения значений конфигурации в базе данных:</span><span class="sxs-lookup"><span data-stu-id="77fd0-207">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="77fd0-208">Добавьте `ConfigurationContext` в хранилище и обратитесь к настроенным значениям:</span><span class="sxs-lookup"><span data-stu-id="77fd0-208">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="77fd0-209">Создайте класс, реализующий [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource).</span><span class="sxs-lookup"><span data-stu-id="77fd0-209">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="77fd0-210">Создайте пользовательский поставщик конфигурации путем наследования от [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span><span class="sxs-lookup"><span data-stu-id="77fd0-210">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="77fd0-211">Поставщик конфигурации инициализирует пустую базу данных:</span><span class="sxs-lookup"><span data-stu-id="77fd0-211">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="77fd0-212">При выполнении примера отображаются выделенные значения из базы данных ("value_from_ef_1" и "value_from_ef_2").</span><span class="sxs-lookup"><span data-stu-id="77fd0-212">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="77fd0-213">Вы можете использовать метод расширения `EFConfigSource` для добавления источника конфигурации.</span><span class="sxs-lookup"><span data-stu-id="77fd0-213">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="77fd0-214">В следующем примере кода демонстрируется использование настраиваемого `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="77fd0-214">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="77fd0-215">Обратите внимание, что в этом примере пользовательский `EFConfigProvider` добавляется после поставщика JSON, поэтому все параметры из базы данных переопределяют параметры из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="77fd0-215">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="77fd0-216">Используется следующий файл *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="77fd0-216">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="77fd0-217">Выводится следующий результат.</span><span class="sxs-lookup"><span data-stu-id="77fd0-217">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="77fd0-218">Поставщик конфигурации CommandLine</span><span class="sxs-lookup"><span data-stu-id="77fd0-218">CommandLine configuration provider</span></span>

<span data-ttu-id="77fd0-219">[Поставщик конфигурации CommandLine](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) получает пары "ключ-значение" аргумента командной строки для конфигурации во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="77fd0-219">The [CommandLine configuration provider](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

<span data-ttu-id="77fd0-220">[Просмотрите или скачайте пример конфигурации CommandLine](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="77fd0-220">[View or download the CommandLine configuration sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample/CommandLine)</span></span>

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="77fd0-221">Настройка и использование поставщика конфигурации CommandLine</span><span class="sxs-lookup"><span data-stu-id="77fd0-221">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="77fd0-222">Базовая конфигурация</span><span class="sxs-lookup"><span data-stu-id="77fd0-222">Basic Configuration</span></span>](#tab/basicconfiguration/)

<span data-ttu-id="77fd0-223">Чтобы активировать конфигурацию командной строки, вызовите метод расширения `AddCommandLine` в экземпляре [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="77fd0-223">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="77fd0-224">После выполнения кода отобразятся следующие выходные данные:</span><span class="sxs-lookup"><span data-stu-id="77fd0-224">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="77fd0-225">Передача пар "ключ-значение" аргументов в командной строке приведет к изменению значений `Profile:MachineName` и `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="77fd0-225">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="77fd0-226">В окне консоли отображается следующее:</span><span class="sxs-lookup"><span data-stu-id="77fd0-226">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="77fd0-227">Чтобы переопределить конфигурацию, предоставленную другими поставщиками конфигурации, конфигурацией командной строки, вызовите `AddCommandLine` последним в `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="77fd0-227">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="77fd0-228">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="77fd0-228">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="77fd0-229">Для создания узла стандартные приложения ASP.NET Core 2.x используют статический удобный метод `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="77fd0-229">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="77fd0-230">`CreateDefaultBuilder` загружает дополнительную конфигурацию из *appsettings.json*, *appsettings.{среда}.json*, [секреты пользователя](xref:security/app-secrets) (в среде `Development`), переменные среды и аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="77fd0-230">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="77fd0-231">Поставщик конфигурации CommandLine вызывается последним.</span><span class="sxs-lookup"><span data-stu-id="77fd0-231">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="77fd0-232">В этом случае аргументы командной строки, передаваемые во время выполнения, переопределяют конфигурацию, заданную другими вызванными ранее поставщиками конфигурации.</span><span class="sxs-lookup"><span data-stu-id="77fd0-232">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="77fd0-233">Предположим, что в файлах *appsettings*</span><span class="sxs-lookup"><span data-stu-id="77fd0-233">For *appsettings* files where:</span></span>

* <span data-ttu-id="77fd0-234">Включено `reloadOnChange`.</span><span class="sxs-lookup"><span data-stu-id="77fd0-234">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="77fd0-235">Аргументы командной строки и файл *appsettings* содержат одинаковые параметры.</span><span class="sxs-lookup"><span data-stu-id="77fd0-235">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="77fd0-236">Файл *appsettings*, содержащий соответствующий аргумент командной строки, изменяется после запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="77fd0-236">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="77fd0-237">Если все вышеперечисленное верно, аргументы командной строки переопределяются.</span><span class="sxs-lookup"><span data-stu-id="77fd0-237">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="77fd0-238">Приложение ASP.NET Core 2.x может использовать [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) вместо `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="77fd0-238">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="77fd0-239">При использовании `WebHostBuilder` задайте конфигурацию вручную с помощью [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span><span class="sxs-lookup"><span data-stu-id="77fd0-239">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="77fd0-240">Дополнительные сведения см. в разделе о ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="77fd0-240">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="77fd0-241">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="77fd0-241">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="77fd0-242">Чтобы использовать поставщик конфигурации CommandLine, создайте [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) и вызовите метод `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="77fd0-242">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="77fd0-243">В этом случае аргументы командной строки, передаваемые во время выполнения, переопределяют конфигурацию, заданную другими вызванными ранее поставщиками конфигурации.</span><span class="sxs-lookup"><span data-stu-id="77fd0-243">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="77fd0-244">Примените конфигурацию к [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) с помощью метода `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="77fd0-244">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="77fd0-245">Аргументы</span><span class="sxs-lookup"><span data-stu-id="77fd0-245">Arguments</span></span>

<span data-ttu-id="77fd0-246">Аргументы, передаваемые в командной строке, должны соответствовать одному из двух форматов, приведенных в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="77fd0-246">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="77fd0-247">Формат аргумента</span><span class="sxs-lookup"><span data-stu-id="77fd0-247">Argument format</span></span>                                                     | <span data-ttu-id="77fd0-248">Пример</span><span class="sxs-lookup"><span data-stu-id="77fd0-248">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="77fd0-249">Один аргумент: пара "ключ-значение", разделенная знаком равенства (`=`)</span><span class="sxs-lookup"><span data-stu-id="77fd0-249">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="77fd0-250">Последовательность из двух аргументов: пара "ключ-значение", разделенная пробелом</span><span class="sxs-lookup"><span data-stu-id="77fd0-250">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="77fd0-251">**Один аргумент**</span><span class="sxs-lookup"><span data-stu-id="77fd0-251">**Single argument**</span></span>

<span data-ttu-id="77fd0-252">Значение должно стоять после знака равенства (`=`).</span><span class="sxs-lookup"><span data-stu-id="77fd0-252">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="77fd0-253">Значение может быть равно NULL (например, `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="77fd0-253">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="77fd0-254">Ключ может иметь префикс.</span><span class="sxs-lookup"><span data-stu-id="77fd0-254">The key may have a prefix.</span></span>

| <span data-ttu-id="77fd0-255">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="77fd0-255">Key prefix</span></span>               | <span data-ttu-id="77fd0-256">Пример</span><span class="sxs-lookup"><span data-stu-id="77fd0-256">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="77fd0-257">Без префикса</span><span class="sxs-lookup"><span data-stu-id="77fd0-257">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="77fd0-258">Один дефис (`-`) & #8224;</span><span class="sxs-lookup"><span data-stu-id="77fd0-258">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="77fd0-259">Два дефиса (`--`)</span><span class="sxs-lookup"><span data-stu-id="77fd0-259">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="77fd0-260">Прямая косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="77fd0-260">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="77fd0-261">& #8224;Ключ с префиксом из одного дефиса (`-`) должен быть указан в [сопоставлениях переключений](#switch-mappings), описанных ниже.</span><span class="sxs-lookup"><span data-stu-id="77fd0-261">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="77fd0-262">Пример команды:</span><span class="sxs-lookup"><span data-stu-id="77fd0-262">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="77fd0-263">Примечание. Если `-key2` отсутствует в [сопоставлениях переключений](#switch-mappings), переданных поставщику конфигурации, возникает исключение `FormatException`.</span><span class="sxs-lookup"><span data-stu-id="77fd0-263">Note: If `-key2` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="77fd0-264">**Последовательность из двух аргументов**</span><span class="sxs-lookup"><span data-stu-id="77fd0-264">**Sequence of two arguments**</span></span>

<span data-ttu-id="77fd0-265">Значение не может быть равно NULL и должно следовать за ключом, разделенным пробелом.</span><span class="sxs-lookup"><span data-stu-id="77fd0-265">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="77fd0-266">Ключ должен иметь префикс.</span><span class="sxs-lookup"><span data-stu-id="77fd0-266">The key must have a prefix.</span></span>

| <span data-ttu-id="77fd0-267">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="77fd0-267">Key prefix</span></span>               | <span data-ttu-id="77fd0-268">Пример</span><span class="sxs-lookup"><span data-stu-id="77fd0-268">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="77fd0-269">Один дефис (`-`) & #8224;</span><span class="sxs-lookup"><span data-stu-id="77fd0-269">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="77fd0-270">Два дефиса (`--`)</span><span class="sxs-lookup"><span data-stu-id="77fd0-270">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="77fd0-271">Прямая косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="77fd0-271">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="77fd0-272">& #8224;Ключ с префиксом из одного дефиса (`-`) должен быть указан в [сопоставлениях переключений](#switch-mappings), описанных ниже.</span><span class="sxs-lookup"><span data-stu-id="77fd0-272">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="77fd0-273">Пример команды:</span><span class="sxs-lookup"><span data-stu-id="77fd0-273">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="77fd0-274">Примечание. Если `-key1` отсутствует в [сопоставлениях переключений](#switch-mappings), переданных поставщику конфигурации, возникает исключение `FormatException`.</span><span class="sxs-lookup"><span data-stu-id="77fd0-274">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="77fd0-275">Повторяющиеся ключи</span><span class="sxs-lookup"><span data-stu-id="77fd0-275">Duplicate keys</span></span>

<span data-ttu-id="77fd0-276">Если указаны повторяющиеся ключи, используется последняя пара "ключ-значение".</span><span class="sxs-lookup"><span data-stu-id="77fd0-276">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="77fd0-277">Сопоставления переключений</span><span class="sxs-lookup"><span data-stu-id="77fd0-277">Switch mappings</span></span>

<span data-ttu-id="77fd0-278">При создании конфигурации вручную с помощью `ConfigurationBuilder` вы можете добавить в метод `AddCommandLine` словарь сопоставления параметров.</span><span class="sxs-lookup"><span data-stu-id="77fd0-278">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="77fd0-279">Сопоставление параметров позволяет указать логику замены имен ключей.</span><span class="sxs-lookup"><span data-stu-id="77fd0-279">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="77fd0-280">В словаре сопоставлений переключений выполняется поиск ключа, который совпадает с ключом, предоставляемым аргументом командной строки.</span><span class="sxs-lookup"><span data-stu-id="77fd0-280">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="77fd0-281">Если ключ командной строки найден, значение словаря (новый ключ) передается обратно, чтобы задать конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="77fd0-281">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="77fd0-282">Сопоставление переключений необходимо для любого ключа командной строки с префиксом из одного дефиса (`-`).</span><span class="sxs-lookup"><span data-stu-id="77fd0-282">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="77fd0-283">Правила ключей из словаря сопоставления переключений:</span><span class="sxs-lookup"><span data-stu-id="77fd0-283">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="77fd0-284">Переключения должны начинаться с дефиса (`-`) или двойного дефиса (`--`).</span><span class="sxs-lookup"><span data-stu-id="77fd0-284">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="77fd0-285">Словарь сопоставлений переключений не должен содержать повторяющиеся ключи.</span><span class="sxs-lookup"><span data-stu-id="77fd0-285">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="77fd0-286">В следующем примере метод `GetSwitchMappings` позволяет аргументам командной строки использовать префикс ключа с одним дефисом (`-`) вместо начальных префиксов подключа.</span><span class="sxs-lookup"><span data-stu-id="77fd0-286">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="77fd0-287">Если аргументы командной строки не указаны, словарь для `AddInMemoryCollection` задает значения конфигурации.</span><span class="sxs-lookup"><span data-stu-id="77fd0-287">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="77fd0-288">Запустите приложение с помощью следующей команды:</span><span class="sxs-lookup"><span data-stu-id="77fd0-288">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="77fd0-289">В окне консоли отображается следующее:</span><span class="sxs-lookup"><span data-stu-id="77fd0-289">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="77fd0-290">Используйте следующее для передачи параметров конфигурации:</span><span class="sxs-lookup"><span data-stu-id="77fd0-290">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="77fd0-291">В окне консоли отображается следующее:</span><span class="sxs-lookup"><span data-stu-id="77fd0-291">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="77fd0-292">Созданный словарь сопоставления параметров содержит данные, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="77fd0-292">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="77fd0-293">Ключ</span><span class="sxs-lookup"><span data-stu-id="77fd0-293">Key</span></span>            | <span data-ttu-id="77fd0-294">Значение</span><span class="sxs-lookup"><span data-stu-id="77fd0-294">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="77fd0-295">Чтобы продемонстрировать переключение ключей с помощью словаря, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="77fd0-295">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="77fd0-296">Ключи командной строки меняются.</span><span class="sxs-lookup"><span data-stu-id="77fd0-296">The command-line keys are swapped.</span></span> <span data-ttu-id="77fd0-297">В окне консоли отображаются значения конфигурации для `Profile:MachineName` и `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="77fd0-297">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a><span data-ttu-id="77fd0-298">Файл web.config</span><span class="sxs-lookup"><span data-stu-id="77fd0-298">web.config file</span></span>

<span data-ttu-id="77fd0-299">Файл *web.config* необходим для размещения приложения в службах IIS или IIS Express.</span><span class="sxs-lookup"><span data-stu-id="77fd0-299">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="77fd0-300">Параметры в файле *web.config* включают [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) для запуска приложения и настройки других параметров и модулей IIS.</span><span class="sxs-lookup"><span data-stu-id="77fd0-300">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="77fd0-301">Если файл *web.config* отсутствует и файл проекта содержит `<Project Sdk="Microsoft.NET.Sdk.Web">`, при публикации проекта файл *web.config* создается в опубликованных выходных данных (папка *publish*).</span><span class="sxs-lookup"><span data-stu-id="77fd0-301">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="77fd0-302">Дополнительные сведения см. в разделе [Размещение ASP.NET Core в Windows со службами IIS](xref:host-and-deploy/iis/index#webconfig-file).</span><span class="sxs-lookup"><span data-stu-id="77fd0-302">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig-file).</span></span>

## <a name="access-configuration-during-startup"></a><span data-ttu-id="77fd0-303">Доступ к конфигурации во время запуска</span><span class="sxs-lookup"><span data-stu-id="77fd0-303">Access configuration during startup</span></span>

<span data-ttu-id="77fd0-304">Чтобы получить доступ к конфигурации в `ConfigureServices` или `Configure` во время запуска, см. примеры в разделе [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="77fd0-304">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="adding-configuration-from-an-external-assembly"></a><span data-ttu-id="77fd0-305">Добавление конфигурации из внешней сборки</span><span class="sxs-lookup"><span data-stu-id="77fd0-305">Adding configuration from an external assembly</span></span>

<span data-ttu-id="77fd0-306">Реализация [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) позволяет при запуске добавлять в приложение улучшения из внешней сборки вне класса `Startup` приложения.</span><span class="sxs-lookup"><span data-stu-id="77fd0-306">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="77fd0-307">Дополнительные сведения см. в разделе [Усовершенствование приложения из внешней сборки](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="77fd0-307">For more information, see [Enhance an app from an external assembly](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="access-configuration-in-a-razor-page-or-mvc-view"></a><span data-ttu-id="77fd0-308">Настройка доступа на странице Razor или в представлении MVC</span><span class="sxs-lookup"><span data-stu-id="77fd0-308">Access configuration in a Razor Page or MVC view</span></span>

<span data-ttu-id="77fd0-309">Для доступа к параметрам конфигурации на странице Razor Pages или в представлении MVC добавьте [использование директивы](xref:mvc/views/razor#using) ([Справочник по C#: использование директивы](/dotnet/csharp/language-reference/keywords/using-directive)) для [пространства имен Microsoft.Extensions.Configuration](/dotnet/api/microsoft.extensions.configuration) и вставьте [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) на страницу или в представление.</span><span class="sxs-lookup"><span data-stu-id="77fd0-309">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](/dotnet/api/microsoft.extensions.configuration) and inject [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) into the page or view.</span></span>

<span data-ttu-id="77fd0-310">На странице Razor Pages</span><span class="sxs-lookup"><span data-stu-id="77fd0-310">In a Razor Pages page:</span></span>

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
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="77fd0-311">В представлении MVC</span><span class="sxs-lookup"><span data-stu-id="77fd0-311">In an MVC view:</span></span>

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
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

## <a name="additional-notes"></a><span data-ttu-id="77fd0-312">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="77fd0-312">Additional notes</span></span>

* <span data-ttu-id="77fd0-313">Внедрение зависимостей (DI) настраивается после вызова `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="77fd0-313">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="77fd0-314">Система конфигурации не поддерживает внедрение зависимостей.</span><span class="sxs-lookup"><span data-stu-id="77fd0-314">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="77fd0-315">`IConfiguration` имеет две специализации:</span><span class="sxs-lookup"><span data-stu-id="77fd0-315">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="77fd0-316">`IConfigurationRoot` используется для корневого узла.</span><span class="sxs-lookup"><span data-stu-id="77fd0-316">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="77fd0-317">Может инициировать перезагрузку.</span><span class="sxs-lookup"><span data-stu-id="77fd0-317">Can trigger a reload.</span></span>
  * <span data-ttu-id="77fd0-318">`IConfigurationSection` представляет раздел значений конфигурации.</span><span class="sxs-lookup"><span data-stu-id="77fd0-318">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="77fd0-319">Методы `GetSection` и `GetChildren` возвращают `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="77fd0-319">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="77fd0-320">Используйте [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) при перезагрузке конфигурации или для доступа к каждому поставщику.</span><span class="sxs-lookup"><span data-stu-id="77fd0-320">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="77fd0-321">Оба этих случая нетипичные.</span><span class="sxs-lookup"><span data-stu-id="77fd0-321">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="77fd0-322">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="77fd0-322">Additional resources</span></span>

* [<span data-ttu-id="77fd0-323">Параметры</span><span class="sxs-lookup"><span data-stu-id="77fd0-323">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="77fd0-324">Использование нескольких сред</span><span class="sxs-lookup"><span data-stu-id="77fd0-324">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="77fd0-325">Безопасное хранение секретов приложений во время разработки</span><span class="sxs-lookup"><span data-stu-id="77fd0-325">Safe storage of app secrets in development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="77fd0-326">Размещение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="77fd0-326">Host in ASP.NET Core</span></span>](xref:fundamentals/host/index)
* [<span data-ttu-id="77fd0-327">Введение зависимостей</span><span class="sxs-lookup"><span data-stu-id="77fd0-327">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="77fd0-328">Поставщик конфигурации Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="77fd0-328">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
