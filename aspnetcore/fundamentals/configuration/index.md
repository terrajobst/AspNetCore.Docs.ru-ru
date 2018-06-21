---
title: Конфигурация в .NET Core
author: rick-anderson
description: Узнайте, как использовать API конфигурации для настройки приложения ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 70e9e73eeb5d08baf9ef190ebfbda998ace60d77
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278329"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="a33cb-103">Конфигурация в .NET Core</span><span class="sxs-lookup"><span data-stu-id="a33cb-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="a33cb-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson), [Марк Михаэлис](http://intellitect.com/author/mark-michaelis/) (Mark Michaelis), [Стив Смит](https://ardalis.com/) (Steve Smith), [Даниэль Рот](https://github.com/danroth27) (Daniel Roth) и [Люк Лэтхэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="a33cb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a33cb-105">API конфигурации позволяет настраивать веб-приложения ASP.NET Core на основе списка пар "имя-значение".</span><span class="sxs-lookup"><span data-stu-id="a33cb-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="a33cb-106">Конфигурация считывается во время выполнения из нескольких источников.</span><span class="sxs-lookup"><span data-stu-id="a33cb-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="a33cb-107">Пары "имя-значение" можно сгруппировать в многоуровневую иерархию.</span><span class="sxs-lookup"><span data-stu-id="a33cb-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="a33cb-108">Существуют следующие поставщики конфигурации:</span><span class="sxs-lookup"><span data-stu-id="a33cb-108">There are configuration providers for:</span></span>

* <span data-ttu-id="a33cb-109">Форматы файлов (INI, JSON и XML).</span><span class="sxs-lookup"><span data-stu-id="a33cb-109">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="a33cb-110">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="a33cb-110">Command-line arguments.</span></span>
* <span data-ttu-id="a33cb-111">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="a33cb-111">Environment variables.</span></span>
* <span data-ttu-id="a33cb-112">Объекты .NET в памяти.</span><span class="sxs-lookup"><span data-stu-id="a33cb-112">In-memory .NET objects.</span></span>
* <span data-ttu-id="a33cb-113">Незашифрованное хранилище [Secret Manager](xref:security/app-secrets) (Диспетчер секретов).</span><span class="sxs-lookup"><span data-stu-id="a33cb-113">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="a33cb-114">Зашифрованное пользовательское хранилище, например [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="a33cb-114">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="a33cb-115">Пользовательские поставщики (установленные или созданные).</span><span class="sxs-lookup"><span data-stu-id="a33cb-115">Custom providers (installed or created).</span></span>

<span data-ttu-id="a33cb-116">Каждое значение конфигурации сопоставляется строковому ключу.</span><span class="sxs-lookup"><span data-stu-id="a33cb-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="a33cb-117">Имеется встроенная поддержка привязки для десериализации параметров в пользовательский объект [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) (простой класс .NET со свойствами).</span><span class="sxs-lookup"><span data-stu-id="a33cb-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="a33cb-118">Шаблон параметров использует классы параметров для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="a33cb-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="a33cb-119">Дополнительные сведения об использовании шаблона параметров см. в разделе [Параметры](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="a33cb-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="a33cb-120">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a33cb-120">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="a33cb-121">Конфигурация JSON</span><span class="sxs-lookup"><span data-stu-id="a33cb-121">JSON configuration</span></span>

<span data-ttu-id="a33cb-122">В следующем консольном приложении используется поставщик конфигурации JSON:</span><span class="sxs-lookup"><span data-stu-id="a33cb-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="a33cb-123">Приложение считывает и отображает следующие параметры конфигурации приложения:</span><span class="sxs-lookup"><span data-stu-id="a33cb-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="a33cb-124">Конфигурация — это иерархический список пар "имя-значение", в котором узлы разделяются двоеточием (`:`).</span><span class="sxs-lookup"><span data-stu-id="a33cb-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon (`:`).</span></span> <span data-ttu-id="a33cb-125">Чтобы получить значение, обратитесь к индексатору `Configuration` с ключом соответствующего элемента:</span><span class="sxs-lookup"><span data-stu-id="a33cb-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="a33cb-126">Для работы с массивами в источниках конфигурации в формате JSON используйте индекс массива в составе строки с разделителем-двоеточием.</span><span class="sxs-lookup"><span data-stu-id="a33cb-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="a33cb-127">В следующем примере возвращается имя первого элемента в предыдущем массиве `wizards`:</span><span class="sxs-lookup"><span data-stu-id="a33cb-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="a33cb-128">Пары "имя-значение", записываемые во встроенные поставщики [конфигурации](/dotnet/api/microsoft.extensions.configuration), **не сохраняются**.</span><span class="sxs-lookup"><span data-stu-id="a33cb-128">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="a33cb-129">Но вы можете создать настраиваемый поставщик, который сохраняет значения.</span><span class="sxs-lookup"><span data-stu-id="a33cb-129">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="a33cb-130">См. раздел [Пользовательский поставщик конфигурации](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="a33cb-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="a33cb-131">В предыдущем примере для считывания значений используется индексатор конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a33cb-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="a33cb-132">Для доступа к конфигурации вне `Startup` воспользуйтесь *шаблоном параметров*.</span><span class="sxs-lookup"><span data-stu-id="a33cb-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="a33cb-133">Дополнительные сведения см. в разделе [Параметры](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="a33cb-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

## <a name="xml-configuration"></a><span data-ttu-id="a33cb-134">Конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="a33cb-134">XML configuration</span></span>

<span data-ttu-id="a33cb-135">Для работы с массивами в источниках конфигураций с XML-форматированием укажите индекс `name` для каждого элемента.</span><span class="sxs-lookup"><span data-stu-id="a33cb-135">To work with arrays in XML-formatted configuration sources, provide a `name` index to each element.</span></span> <span data-ttu-id="a33cb-136">Используйте этот индекс для доступа к значениям.</span><span class="sxs-lookup"><span data-stu-id="a33cb-136">Use the index to access the values:</span></span>

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

## <a name="configuration-by-environment"></a><span data-ttu-id="a33cb-137">Конфигурация для разных сред</span><span class="sxs-lookup"><span data-stu-id="a33cb-137">Configuration by environment</span></span>

<span data-ttu-id="a33cb-138">Как правило, для разных сред (например, разработки, тестирования и производства) существуют разные параметры конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a33cb-138">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="a33cb-139">Метод расширения `CreateDefaultBuilder` в приложении ASP.NET Core 2.x (или при использовании `AddJsonFile` и `AddEnvironmentVariables` непосредственно в приложении ASP.NET Core 1.x) добавляет поставщики конфигурации для чтения JSON-файлов и источников системной конфигурации:</span><span class="sxs-lookup"><span data-stu-id="a33cb-139">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="a33cb-140">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="a33cb-140">*appsettings.json*</span></span>
* <span data-ttu-id="a33cb-141">*appsettings.\<имя_среды>.json*</span><span class="sxs-lookup"><span data-stu-id="a33cb-141">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="a33cb-142">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="a33cb-142">Environment variables</span></span>

<span data-ttu-id="a33cb-143">Приложения ASP.NET Core 1.x должны вызывать `AddJsonFile` и [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span><span class="sxs-lookup"><span data-stu-id="a33cb-143">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="a33cb-144">Объяснение параметров см. в разделе [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions).</span><span class="sxs-lookup"><span data-stu-id="a33cb-144">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="a33cb-145">`reloadOnChange` поддерживается только в ASP.NET Core 1.1 и более поздних версиях.</span><span class="sxs-lookup"><span data-stu-id="a33cb-145">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="a33cb-146">Источники конфигурации считываются в порядке их указания.</span><span class="sxs-lookup"><span data-stu-id="a33cb-146">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="a33cb-147">В приведенном выше коде переменные сред считываются последними.</span><span class="sxs-lookup"><span data-stu-id="a33cb-147">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="a33cb-148">Все значения конфигурации, заданные с помощью среды, заменяют значения, заданные в предыдущих двух поставщиках.</span><span class="sxs-lookup"><span data-stu-id="a33cb-148">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="a33cb-149">Рассмотрим следующий файл *appsettings.Staging.JSON*.</span><span class="sxs-lookup"><span data-stu-id="a33cb-149">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[](index/sample/appsettings.Staging.json)]

<span data-ttu-id="a33cb-150">В следующем коде `Configure` считывает значение `MyConfig`.</span><span class="sxs-lookup"><span data-stu-id="a33cb-150">In the following code, `Configure` reads the value of `MyConfig`:</span></span>

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]

<span data-ttu-id="a33cb-151">Для среды обычно задаются значения `Development`, `Staging` или `Production`.</span><span class="sxs-lookup"><span data-stu-id="a33cb-151">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="a33cb-152">Дополнительные сведения см. в статье [Использование нескольких сред](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="a33cb-152">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="a33cb-153">Рекомендации по конфигурации:</span><span class="sxs-lookup"><span data-stu-id="a33cb-153">Configuration considerations:</span></span>

* <span data-ttu-id="a33cb-154">[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) может перезагрузить данные конфигурации после их изменения.</span><span class="sxs-lookup"><span data-stu-id="a33cb-154">[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) can reload configuration data when it changes.</span></span>
* <span data-ttu-id="a33cb-155">Ключи конфигурации **не учитывают** регистр.</span><span class="sxs-lookup"><span data-stu-id="a33cb-155">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="a33cb-156">**Никогда** не храните пароли и другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации в виде обычного текста.</span><span class="sxs-lookup"><span data-stu-id="a33cb-156">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="a33cb-157">Не используйте секреты рабочей среды в средах разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="a33cb-157">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="a33cb-158">Указывайте секреты вне проекта, чтобы их нельзя было случайно зафиксировать в репозитории с исходным кодом.</span><span class="sxs-lookup"><span data-stu-id="a33cb-158">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="a33cb-159">Узнайте больше об [использовании нескольких сред](xref:fundamentals/environments) и управлении [безопасным хранением секретов приложения во время разработки](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="a33cb-159">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets in development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="a33cb-160">Для значений иерархической конфигурации, указанных в переменных среды, двоеточие (`:`) поддерживается не на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="a33cb-160">For hierarchical config values specified in environment variables, a colon (`:`) may not work on all platforms.</span></span> <span data-ttu-id="a33cb-161">Двойной знак подчеркивания (`__`) поддерживается на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="a33cb-161">Double underscore (`__`) is supported by all platforms.</span></span>
* <span data-ttu-id="a33cb-162">При взаимодействии с API конфигурации двоеточие (`:`) поддерживается на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="a33cb-162">When interacting with the configuration API, a colon (`:`) works on all platforms.</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="a33cb-163">Поставщик в памяти и привязка к классу POCO</span><span class="sxs-lookup"><span data-stu-id="a33cb-163">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="a33cb-164">В следующем примере демонстрируется использование поставщика в памяти и привязка к классу:</span><span class="sxs-lookup"><span data-stu-id="a33cb-164">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[](index/sample/InMemory/Program.cs)]

<span data-ttu-id="a33cb-165">Значения конфигурации возвращаются в виде строк, но с помощью привязки можно создавать объекты.</span><span class="sxs-lookup"><span data-stu-id="a33cb-165">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="a33cb-166">Привязка позволяет извлекать объекты POCO и даже целые графы объектов.</span><span class="sxs-lookup"><span data-stu-id="a33cb-166">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="a33cb-167">GetValue</span><span class="sxs-lookup"><span data-stu-id="a33cb-167">GetValue</span></span>

<span data-ttu-id="a33cb-168">В следующем примере показан метод расширения [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_):</span><span class="sxs-lookup"><span data-stu-id="a33cb-168">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="a33cb-169">Метод `GetValue<T>` из ConfigurationBinder позволяет задавать значение по умолчанию (в примере — 80).</span><span class="sxs-lookup"><span data-stu-id="a33cb-169">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="a33cb-170">`GetValue<T>` используется для простых сценариев и не выполняет привязку ко всем разделам.</span><span class="sxs-lookup"><span data-stu-id="a33cb-170">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="a33cb-171">Метод `GetValue<T>` получает из `GetSection(key).Value` скалярные значения, преобразованные в определенный тип.</span><span class="sxs-lookup"><span data-stu-id="a33cb-171">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="a33cb-172">Привязка к графу объектов</span><span class="sxs-lookup"><span data-stu-id="a33cb-172">Bind to an object graph</span></span>

<span data-ttu-id="a33cb-173">Для каждого объекта в классе возможна рекурсивная привязка.</span><span class="sxs-lookup"><span data-stu-id="a33cb-173">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="a33cb-174">Рассмотрим следующий класс `AppSettings`:</span><span class="sxs-lookup"><span data-stu-id="a33cb-174">Consider the following `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="a33cb-175">В следующем примере выполняется привязка к классу `AppSettings`:</span><span class="sxs-lookup"><span data-stu-id="a33cb-175">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="a33cb-176">**ASP.NET Core 1.1** и более поздние версии могут использовать метод `Get<T>`, который работает с целыми разделами.</span><span class="sxs-lookup"><span data-stu-id="a33cb-176">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="a33cb-177">Метод `Get<T>` может быть более удобным, чем `Bind`.</span><span class="sxs-lookup"><span data-stu-id="a33cb-177">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="a33cb-178">Следующий код показывает, как использовать `Get<T>` с предыдущим примером.</span><span class="sxs-lookup"><span data-stu-id="a33cb-178">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="a33cb-179">Используется следующий файл *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a33cb-179">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="a33cb-180">Программа выводит `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="a33cb-180">The program displays `Height 11`.</span></span>

<span data-ttu-id="a33cb-181">Для модульного тестирования конфигурации можно выполнить следующий код:</span><span class="sxs-lookup"><span data-stu-id="a33cb-181">The following code can be used to unit test the configuration:</span></span>

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

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="a33cb-182">Создание пользовательского поставщика Entity Framework</span><span class="sxs-lookup"><span data-stu-id="a33cb-182">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="a33cb-183">В этом разделе создается поставщик базовой конфигурации, который считывает пары "имя-значение" из базы данных с помощью EF.</span><span class="sxs-lookup"><span data-stu-id="a33cb-183">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="a33cb-184">Определите сущность `ConfigurationValue` для хранения значений конфигурации в базе данных:</span><span class="sxs-lookup"><span data-stu-id="a33cb-184">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="a33cb-185">Добавьте `ConfigurationContext` в хранилище и обратитесь к настроенным значениям:</span><span class="sxs-lookup"><span data-stu-id="a33cb-185">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="a33cb-186">Создайте класс, реализующий [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource).</span><span class="sxs-lookup"><span data-stu-id="a33cb-186">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="a33cb-187">Создайте пользовательский поставщик конфигурации путем наследования от [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span><span class="sxs-lookup"><span data-stu-id="a33cb-187">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="a33cb-188">Поставщик конфигурации инициализирует пустую базу данных:</span><span class="sxs-lookup"><span data-stu-id="a33cb-188">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="a33cb-189">При выполнении примера отображаются выделенные значения из базы данных ("value_from_ef_1" и "value_from_ef_2").</span><span class="sxs-lookup"><span data-stu-id="a33cb-189">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="a33cb-190">Вы можете использовать метод расширения `EFConfigSource` для добавления источника конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a33cb-190">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="a33cb-191">В следующем примере кода демонстрируется использование настраиваемого `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="a33cb-191">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="a33cb-192">Обратите внимание, что в этом примере пользовательский `EFConfigProvider` добавляется после поставщика JSON, поэтому все параметры из базы данных переопределяют параметры из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a33cb-192">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="a33cb-193">Используется следующий файл *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a33cb-193">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="a33cb-194">Выводится следующий результат.</span><span class="sxs-lookup"><span data-stu-id="a33cb-194">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="a33cb-195">Поставщик конфигурации CommandLine</span><span class="sxs-lookup"><span data-stu-id="a33cb-195">CommandLine configuration provider</span></span>

<span data-ttu-id="a33cb-196">[Поставщик конфигурации CommandLine](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) получает пары "ключ-значение" аргумента командной строки для конфигурации во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="a33cb-196">The [CommandLine configuration provider](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

<span data-ttu-id="a33cb-197">[Просмотрите или скачайте пример конфигурации CommandLine](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="a33cb-197">[View or download the CommandLine configuration sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample/CommandLine)</span></span>

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="a33cb-198">Настройка и использование поставщика конфигурации CommandLine</span><span class="sxs-lookup"><span data-stu-id="a33cb-198">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="a33cb-199">Базовая конфигурация</span><span class="sxs-lookup"><span data-stu-id="a33cb-199">Basic Configuration</span></span>](#tab/basicconfiguration/)

<span data-ttu-id="a33cb-200">Чтобы активировать конфигурацию командной строки, вызовите метод расширения `AddCommandLine` в экземпляре [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="a33cb-200">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="a33cb-201">После выполнения кода отобразятся следующие выходные данные:</span><span class="sxs-lookup"><span data-stu-id="a33cb-201">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="a33cb-202">Передача пар "ключ-значение" аргументов в командной строке приведет к изменению значений `Profile:MachineName` и `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="a33cb-202">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="a33cb-203">В окне консоли отображается следующее:</span><span class="sxs-lookup"><span data-stu-id="a33cb-203">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="a33cb-204">Чтобы переопределить конфигурацию, предоставленную другими поставщиками конфигурации, конфигурацией командной строки, вызовите `AddCommandLine` последним в `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="a33cb-204">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a33cb-205">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a33cb-205">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="a33cb-206">Для создания узла стандартные приложения ASP.NET Core 2.x используют статический удобный метод `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="a33cb-206">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="a33cb-207">`CreateDefaultBuilder` загружает дополнительную конфигурацию из *appsettings.json*, *appsettings.{среда}.json*, [секреты пользователя](xref:security/app-secrets) (в среде `Development`), переменные среды и аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="a33cb-207">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="a33cb-208">Поставщик конфигурации CommandLine вызывается последним.</span><span class="sxs-lookup"><span data-stu-id="a33cb-208">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="a33cb-209">В этом случае аргументы командной строки, передаваемые во время выполнения, переопределяют конфигурацию, заданную другими вызванными ранее поставщиками конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a33cb-209">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="a33cb-210">Предположим, что в файлах *appsettings*</span><span class="sxs-lookup"><span data-stu-id="a33cb-210">For *appsettings* files where:</span></span>

* <span data-ttu-id="a33cb-211">Включено `reloadOnChange`.</span><span class="sxs-lookup"><span data-stu-id="a33cb-211">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="a33cb-212">Аргументы командной строки и файл *appsettings* содержат одинаковые параметры.</span><span class="sxs-lookup"><span data-stu-id="a33cb-212">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="a33cb-213">Файл *appsettings*, содержащий соответствующий аргумент командной строки, изменяется после запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="a33cb-213">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="a33cb-214">Если все вышеперечисленное верно, аргументы командной строки переопределяются.</span><span class="sxs-lookup"><span data-stu-id="a33cb-214">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="a33cb-215">Приложение ASP.NET Core 2.x может использовать [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) вместо `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a33cb-215">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="a33cb-216">При использовании `WebHostBuilder` задайте конфигурацию вручную с помощью [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span><span class="sxs-lookup"><span data-stu-id="a33cb-216">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="a33cb-217">Дополнительные сведения см. в разделе о ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="a33cb-217">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a33cb-218">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a33cb-218">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="a33cb-219">Чтобы использовать поставщик конфигурации CommandLine, создайте [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) и вызовите метод `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="a33cb-219">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="a33cb-220">В этом случае аргументы командной строки, передаваемые во время выполнения, переопределяют конфигурацию, заданную другими вызванными ранее поставщиками конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a33cb-220">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="a33cb-221">Примените конфигурацию к [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) с помощью метода `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="a33cb-221">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="a33cb-222">Аргументы</span><span class="sxs-lookup"><span data-stu-id="a33cb-222">Arguments</span></span>

<span data-ttu-id="a33cb-223">Аргументы, передаваемые в командной строке, должны соответствовать одному из двух форматов, приведенных в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="a33cb-223">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="a33cb-224">Формат аргумента</span><span class="sxs-lookup"><span data-stu-id="a33cb-224">Argument format</span></span>                                                     | <span data-ttu-id="a33cb-225">Пример</span><span class="sxs-lookup"><span data-stu-id="a33cb-225">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="a33cb-226">Один аргумент: пара "ключ-значение", разделенная знаком равенства (`=`)</span><span class="sxs-lookup"><span data-stu-id="a33cb-226">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="a33cb-227">Последовательность из двух аргументов: пара "ключ-значение", разделенная пробелом</span><span class="sxs-lookup"><span data-stu-id="a33cb-227">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="a33cb-228">**Один аргумент**</span><span class="sxs-lookup"><span data-stu-id="a33cb-228">**Single argument**</span></span>

<span data-ttu-id="a33cb-229">Значение должно стоять после знака равенства (`=`).</span><span class="sxs-lookup"><span data-stu-id="a33cb-229">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="a33cb-230">Значение может быть равно NULL (например, `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="a33cb-230">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="a33cb-231">Ключ может иметь префикс.</span><span class="sxs-lookup"><span data-stu-id="a33cb-231">The key may have a prefix.</span></span>

| <span data-ttu-id="a33cb-232">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="a33cb-232">Key prefix</span></span>               | <span data-ttu-id="a33cb-233">Пример</span><span class="sxs-lookup"><span data-stu-id="a33cb-233">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="a33cb-234">Без префикса</span><span class="sxs-lookup"><span data-stu-id="a33cb-234">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="a33cb-235">Один дефис (`-`) & #8224;</span><span class="sxs-lookup"><span data-stu-id="a33cb-235">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="a33cb-236">Два дефиса (`--`)</span><span class="sxs-lookup"><span data-stu-id="a33cb-236">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="a33cb-237">Прямая косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="a33cb-237">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="a33cb-238">& #8224;Ключ с префиксом из одного дефиса (`-`) должен быть указан в [сопоставлениях переключений](#switch-mappings), описанных ниже.</span><span class="sxs-lookup"><span data-stu-id="a33cb-238">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="a33cb-239">Пример команды:</span><span class="sxs-lookup"><span data-stu-id="a33cb-239">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="a33cb-240">Примечание. Если `-key2` отсутствует в [сопоставлениях переключений](#switch-mappings), переданных поставщику конфигурации, возникает исключение `FormatException`.</span><span class="sxs-lookup"><span data-stu-id="a33cb-240">Note: If `-key2` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="a33cb-241">**Последовательность из двух аргументов**</span><span class="sxs-lookup"><span data-stu-id="a33cb-241">**Sequence of two arguments**</span></span>

<span data-ttu-id="a33cb-242">Значение не может быть равно NULL и должно следовать за ключом, разделенным пробелом.</span><span class="sxs-lookup"><span data-stu-id="a33cb-242">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="a33cb-243">Ключ должен иметь префикс.</span><span class="sxs-lookup"><span data-stu-id="a33cb-243">The key must have a prefix.</span></span>

| <span data-ttu-id="a33cb-244">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="a33cb-244">Key prefix</span></span>               | <span data-ttu-id="a33cb-245">Пример</span><span class="sxs-lookup"><span data-stu-id="a33cb-245">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="a33cb-246">Один дефис (`-`) & #8224;</span><span class="sxs-lookup"><span data-stu-id="a33cb-246">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="a33cb-247">Два дефиса (`--`)</span><span class="sxs-lookup"><span data-stu-id="a33cb-247">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="a33cb-248">Прямая косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="a33cb-248">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="a33cb-249">& #8224;Ключ с префиксом из одного дефиса (`-`) должен быть указан в [сопоставлениях переключений](#switch-mappings), описанных ниже.</span><span class="sxs-lookup"><span data-stu-id="a33cb-249">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="a33cb-250">Пример команды:</span><span class="sxs-lookup"><span data-stu-id="a33cb-250">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="a33cb-251">Примечание. Если `-key1` отсутствует в [сопоставлениях переключений](#switch-mappings), переданных поставщику конфигурации, возникает исключение `FormatException`.</span><span class="sxs-lookup"><span data-stu-id="a33cb-251">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="a33cb-252">Повторяющиеся ключи</span><span class="sxs-lookup"><span data-stu-id="a33cb-252">Duplicate keys</span></span>

<span data-ttu-id="a33cb-253">Если указаны повторяющиеся ключи, используется последняя пара "ключ-значение".</span><span class="sxs-lookup"><span data-stu-id="a33cb-253">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="a33cb-254">Сопоставления переключений</span><span class="sxs-lookup"><span data-stu-id="a33cb-254">Switch mappings</span></span>

<span data-ttu-id="a33cb-255">При создании конфигурации вручную с помощью `ConfigurationBuilder` вы можете добавить в метод `AddCommandLine` словарь сопоставления параметров.</span><span class="sxs-lookup"><span data-stu-id="a33cb-255">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="a33cb-256">Сопоставление параметров позволяет указать логику замены имен ключей.</span><span class="sxs-lookup"><span data-stu-id="a33cb-256">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="a33cb-257">В словаре сопоставлений переключений выполняется поиск ключа, который совпадает с ключом, предоставляемым аргументом командной строки.</span><span class="sxs-lookup"><span data-stu-id="a33cb-257">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="a33cb-258">Если ключ командной строки найден, значение словаря (новый ключ) передается обратно, чтобы задать конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="a33cb-258">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="a33cb-259">Сопоставление переключений необходимо для любого ключа командной строки с префиксом из одного дефиса (`-`).</span><span class="sxs-lookup"><span data-stu-id="a33cb-259">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="a33cb-260">Правила ключей из словаря сопоставления переключений:</span><span class="sxs-lookup"><span data-stu-id="a33cb-260">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="a33cb-261">Переключения должны начинаться с дефиса (`-`) или двойного дефиса (`--`).</span><span class="sxs-lookup"><span data-stu-id="a33cb-261">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="a33cb-262">Словарь сопоставлений переключений не должен содержать повторяющиеся ключи.</span><span class="sxs-lookup"><span data-stu-id="a33cb-262">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="a33cb-263">В следующем примере метод `GetSwitchMappings` позволяет аргументам командной строки использовать префикс ключа с одним дефисом (`-`) вместо начальных префиксов подключа.</span><span class="sxs-lookup"><span data-stu-id="a33cb-263">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="a33cb-264">Если аргументы командной строки не указаны, словарь для `AddInMemoryCollection` задает значения конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a33cb-264">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="a33cb-265">Запустите приложение с помощью следующей команды:</span><span class="sxs-lookup"><span data-stu-id="a33cb-265">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="a33cb-266">В окне консоли отображается следующее:</span><span class="sxs-lookup"><span data-stu-id="a33cb-266">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="a33cb-267">Используйте следующее для передачи параметров конфигурации:</span><span class="sxs-lookup"><span data-stu-id="a33cb-267">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="a33cb-268">В окне консоли отображается следующее:</span><span class="sxs-lookup"><span data-stu-id="a33cb-268">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="a33cb-269">Созданный словарь сопоставления параметров содержит данные, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="a33cb-269">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="a33cb-270">Ключ</span><span class="sxs-lookup"><span data-stu-id="a33cb-270">Key</span></span>            | <span data-ttu-id="a33cb-271">Значение</span><span class="sxs-lookup"><span data-stu-id="a33cb-271">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="a33cb-272">Чтобы продемонстрировать переключение ключей с помощью словаря, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="a33cb-272">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="a33cb-273">Ключи командной строки меняются.</span><span class="sxs-lookup"><span data-stu-id="a33cb-273">The command-line keys are swapped.</span></span> <span data-ttu-id="a33cb-274">В окне консоли отображаются значения конфигурации для `Profile:MachineName` и `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="a33cb-274">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a><span data-ttu-id="a33cb-275">Файл web.config</span><span class="sxs-lookup"><span data-stu-id="a33cb-275">web.config file</span></span>

<span data-ttu-id="a33cb-276">Файл *web.config* необходим для размещения приложения в службах IIS или IIS Express.</span><span class="sxs-lookup"><span data-stu-id="a33cb-276">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="a33cb-277">Параметры в файле *web.config* включают [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) для запуска приложения и настройки других параметров и модулей IIS.</span><span class="sxs-lookup"><span data-stu-id="a33cb-277">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="a33cb-278">Если файл *web.config* отсутствует и файл проекта содержит `<Project Sdk="Microsoft.NET.Sdk.Web">`, при публикации проекта файл *web.config* создается в опубликованных выходных данных (папка *publish*).</span><span class="sxs-lookup"><span data-stu-id="a33cb-278">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="a33cb-279">Дополнительные сведения см. в разделе [Размещение ASP.NET Core в Windows со службами IIS](xref:host-and-deploy/iis/index#webconfig-file).</span><span class="sxs-lookup"><span data-stu-id="a33cb-279">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig-file).</span></span>

## <a name="access-configuration-during-startup"></a><span data-ttu-id="a33cb-280">Доступ к конфигурации во время запуска</span><span class="sxs-lookup"><span data-stu-id="a33cb-280">Access configuration during startup</span></span>

<span data-ttu-id="a33cb-281">Чтобы получить доступ к конфигурации в `ConfigureServices` или `Configure` во время запуска, см. примеры в разделе [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="a33cb-281">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="adding-configuration-from-an-external-assembly"></a><span data-ttu-id="a33cb-282">Добавление конфигурации из внешней сборки</span><span class="sxs-lookup"><span data-stu-id="a33cb-282">Adding configuration from an external assembly</span></span>

<span data-ttu-id="a33cb-283">Реализация [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) позволяет при запуске добавлять в приложение улучшения из внешней сборки вне класса `Startup` приложения.</span><span class="sxs-lookup"><span data-stu-id="a33cb-283">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="a33cb-284">Дополнительные сведения см. в разделе [Усовершенствование приложения из внешней сборки](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="a33cb-284">For more information, see [Enhance an app from an external assembly](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="access-configuration-in-a-razor-page-or-mvc-view"></a><span data-ttu-id="a33cb-285">Настройка доступа на странице Razor или в представлении MVC</span><span class="sxs-lookup"><span data-stu-id="a33cb-285">Access configuration in a Razor Page or MVC view</span></span>

<span data-ttu-id="a33cb-286">Для доступа к параметрам конфигурации на странице Razor Pages или в представлении MVC добавьте [использование директивы](xref:mvc/views/razor#using) ([Справочник по C#: использование директивы](/dotnet/csharp/language-reference/keywords/using-directive)) для [пространства имен Microsoft.Extensions.Configuration](/dotnet/api/microsoft.extensions.configuration) и вставьте [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) на страницу или в представление.</span><span class="sxs-lookup"><span data-stu-id="a33cb-286">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](/dotnet/api/microsoft.extensions.configuration) and inject [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) into the page or view.</span></span>

<span data-ttu-id="a33cb-287">На странице Razor Pages</span><span class="sxs-lookup"><span data-stu-id="a33cb-287">In a Razor Pages page:</span></span>

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

<span data-ttu-id="a33cb-288">В представлении MVC</span><span class="sxs-lookup"><span data-stu-id="a33cb-288">In an MVC view:</span></span>

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

## <a name="additional-notes"></a><span data-ttu-id="a33cb-289">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="a33cb-289">Additional notes</span></span>

* <span data-ttu-id="a33cb-290">Внедрение зависимостей (DI) настраивается после вызова `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a33cb-290">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="a33cb-291">Система конфигурации не поддерживает внедрение зависимостей.</span><span class="sxs-lookup"><span data-stu-id="a33cb-291">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="a33cb-292">`IConfiguration` имеет две специализации:</span><span class="sxs-lookup"><span data-stu-id="a33cb-292">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="a33cb-293">`IConfigurationRoot` используется для корневого узла.</span><span class="sxs-lookup"><span data-stu-id="a33cb-293">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="a33cb-294">Может инициировать перезагрузку.</span><span class="sxs-lookup"><span data-stu-id="a33cb-294">Can trigger a reload.</span></span>
  * <span data-ttu-id="a33cb-295">`IConfigurationSection` представляет раздел значений конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a33cb-295">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="a33cb-296">Методы `GetSection` и `GetChildren` возвращают `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="a33cb-296">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="a33cb-297">Используйте [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) при перезагрузке конфигурации или для доступа к каждому поставщику.</span><span class="sxs-lookup"><span data-stu-id="a33cb-297">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="a33cb-298">Оба этих случая нетипичные.</span><span class="sxs-lookup"><span data-stu-id="a33cb-298">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a33cb-299">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a33cb-299">Additional resources</span></span>

* [<span data-ttu-id="a33cb-300">Параметры</span><span class="sxs-lookup"><span data-stu-id="a33cb-300">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="a33cb-301">Использование нескольких сред</span><span class="sxs-lookup"><span data-stu-id="a33cb-301">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="a33cb-302">Безопасное хранение секретов приложений во время разработки</span><span class="sxs-lookup"><span data-stu-id="a33cb-302">Safe storage of app secrets in development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="a33cb-303">Размещение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a33cb-303">Host in ASP.NET Core</span></span>](xref:fundamentals/host/index)
* [<span data-ttu-id="a33cb-304">Введение зависимостей</span><span class="sxs-lookup"><span data-stu-id="a33cb-304">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="a33cb-305">Поставщик конфигурации Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="a33cb-305">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
