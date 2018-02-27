---
title: "Конфигурация в .NET Core"
author: rick-anderson
description: "Используйте API конфигурации для настройки приложения ASP.NET Core несколькими способами."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/index
ms.openlocfilehash: 12635c66bacdeed7360a9d6c689212bba81439e3
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/23/2018
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="327ef-103">Настройка приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="327ef-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="327ef-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson), [Марк Михаэлис](http://intellitect.com/author/mark-michaelis/) (Mark Michaelis), [Стив Смит](https://ardalis.com/) (Steve Smith), [Даниэль Рот](https://github.com/danroth27) (Daniel Roth) и [Люк Лэтхэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="327ef-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="327ef-105">API конфигурации позволяет настраивать веб-приложения ASP.NET Core на основе списка пар "имя-значение".</span><span class="sxs-lookup"><span data-stu-id="327ef-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="327ef-106">Конфигурация считывается во время выполнения из нескольких источников.</span><span class="sxs-lookup"><span data-stu-id="327ef-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="327ef-107">Пары "имя-значение" можно сгруппировать в многоуровневую иерархию.</span><span class="sxs-lookup"><span data-stu-id="327ef-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="327ef-108">Существуют следующие поставщики конфигурации:</span><span class="sxs-lookup"><span data-stu-id="327ef-108">There are configuration providers for:</span></span>

* <span data-ttu-id="327ef-109">форматов файлов (INI, JSON и XML);</span><span class="sxs-lookup"><span data-stu-id="327ef-109">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="327ef-110">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="327ef-110">Command-line arguments</span></span>
* <span data-ttu-id="327ef-111">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="327ef-111">Environment variables</span></span>
* <span data-ttu-id="327ef-112">объектов .NET в памяти;</span><span class="sxs-lookup"><span data-stu-id="327ef-112">In-memory .NET objects</span></span>
* <span data-ttu-id="327ef-113">зашифрованного пользовательского хранилища;</span><span class="sxs-lookup"><span data-stu-id="327ef-113">An encrypted user store</span></span>
* <span data-ttu-id="327ef-114">[Azure Key Vault](xref:security/key-vault-configuration);</span><span class="sxs-lookup"><span data-stu-id="327ef-114">[Azure Key Vault](xref:security/key-vault-configuration)</span></span>
* <span data-ttu-id="327ef-115">а также пользовательские поставщики (устанавливаемые или создаваемые).</span><span class="sxs-lookup"><span data-stu-id="327ef-115">Custom providers (installed or created)</span></span>

<span data-ttu-id="327ef-116">Каждое значение конфигурации сопоставляется строковому ключу.</span><span class="sxs-lookup"><span data-stu-id="327ef-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="327ef-117">Имеется встроенная поддержка привязки для десериализации параметров в пользовательский объект [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) (простой класс .NET со свойствами).</span><span class="sxs-lookup"><span data-stu-id="327ef-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="327ef-118">Шаблон параметров использует классы параметров для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="327ef-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="327ef-119">Дополнительные сведения об использовании шаблона параметров см. в разделе [Параметры](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="327ef-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="327ef-120">[Просмотреть или скачать образец кода](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="327ef-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="327ef-121">Конфигурация JSON</span><span class="sxs-lookup"><span data-stu-id="327ef-121">JSON configuration</span></span>

<span data-ttu-id="327ef-122">В следующем консольном приложении используется поставщик конфигурации JSON:</span><span class="sxs-lookup"><span data-stu-id="327ef-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="327ef-123">Приложение считывает и отображает следующие параметры конфигурации приложения:</span><span class="sxs-lookup"><span data-stu-id="327ef-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="327ef-124">Конфигурация — это иерархический список пар "имя-значение", в котором узлы разделяются двоеточием.</span><span class="sxs-lookup"><span data-stu-id="327ef-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="327ef-125">Чтобы получить значение, обратитесь к индексатору `Configuration` с ключом соответствующего элемента:</span><span class="sxs-lookup"><span data-stu-id="327ef-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="327ef-126">Для работы с массивами в источниках конфигурации в формате JSON используйте индекс массива в составе строки с разделителем-двоеточием.</span><span class="sxs-lookup"><span data-stu-id="327ef-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="327ef-127">В следующем примере возвращается имя первого элемента в предыдущем массиве `wizards`:</span><span class="sxs-lookup"><span data-stu-id="327ef-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="327ef-128">Пары "имя-значение", записываемые во встроенные поставщики [конфигурации](/dotnet/api/microsoft.extensions.configuration), **не сохраняются**.</span><span class="sxs-lookup"><span data-stu-id="327ef-128">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="327ef-129">Но вы можете создать настраиваемый поставщик, который сохраняет значения.</span><span class="sxs-lookup"><span data-stu-id="327ef-129">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="327ef-130">См. раздел [Пользовательский поставщик конфигурации](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="327ef-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="327ef-131">В предыдущем примере для считывания значений используется индексатор конфигурации.</span><span class="sxs-lookup"><span data-stu-id="327ef-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="327ef-132">Для доступа к конфигурации вне `Startup` воспользуйтесь *шаблоном параметров*.</span><span class="sxs-lookup"><span data-stu-id="327ef-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="327ef-133">Дополнительные сведения см. в разделе [Параметры](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="327ef-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

## <a name="xml-configuration"></a><span data-ttu-id="327ef-134">Конфигурации XML</span><span class="sxs-lookup"><span data-stu-id="327ef-134">XML configuration</span></span>

<span data-ttu-id="327ef-135">Для работы с массивами в источниках конфигураций с XML-форматированием укажите индекс `name` для каждого элемента.</span><span class="sxs-lookup"><span data-stu-id="327ef-135">To work with arrays in XML-formatted configuration sources, provide a `name` index to each element.</span></span> <span data-ttu-id="327ef-136">Используйте этот индекс для доступа к значениям.</span><span class="sxs-lookup"><span data-stu-id="327ef-136">Use the index to access the values:</span></span>

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

## <a name="configuration-by-environment"></a><span data-ttu-id="327ef-137">Конфигурация для разных сред</span><span class="sxs-lookup"><span data-stu-id="327ef-137">Configuration by environment</span></span>

<span data-ttu-id="327ef-138">Как правило, для разных сред (например, разработки, тестирования и производства) существуют разные параметры конфигурации.</span><span class="sxs-lookup"><span data-stu-id="327ef-138">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="327ef-139">Метод расширения `CreateDefaultBuilder` в приложении ASP.NET Core 2.x (или при использовании `AddJsonFile` и `AddEnvironmentVariables` непосредственно в приложении ASP.NET Core 1.x) добавляет поставщики конфигурации для чтения JSON-файлов и источников системной конфигурации:</span><span class="sxs-lookup"><span data-stu-id="327ef-139">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="327ef-140">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="327ef-140">*appsettings.json*</span></span>
* <span data-ttu-id="327ef-141">*appsettings.\<имя_среды>.json*</span><span class="sxs-lookup"><span data-stu-id="327ef-141">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="327ef-142">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="327ef-142">Environment variables</span></span>

<span data-ttu-id="327ef-143">Приложения ASP.NET Core 1.x должны вызывать `AddJsonFile` и [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span><span class="sxs-lookup"><span data-stu-id="327ef-143">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="327ef-144">Объяснение параметров см. в разделе [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions).</span><span class="sxs-lookup"><span data-stu-id="327ef-144">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="327ef-145">`reloadOnChange` поддерживается только в ASP.NET Core 1.1 и более поздних версиях.</span><span class="sxs-lookup"><span data-stu-id="327ef-145">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="327ef-146">Источники конфигурации считываются в порядке их указания.</span><span class="sxs-lookup"><span data-stu-id="327ef-146">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="327ef-147">В приведенном выше коде переменные сред считываются последними.</span><span class="sxs-lookup"><span data-stu-id="327ef-147">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="327ef-148">Все значения конфигурации, заданные с помощью среды, заменяют значения, заданные в предыдущих двух поставщиках.</span><span class="sxs-lookup"><span data-stu-id="327ef-148">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="327ef-149">Рассмотрим следующий файл *appsettings.Staging.JSON*.</span><span class="sxs-lookup"><span data-stu-id="327ef-149">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[Main](index/sample/appsettings.Staging.json)]

<span data-ttu-id="327ef-150">Когда для среды задано значение `Staging`, следующий метод `Configure` считывает значение `MyConfig`.</span><span class="sxs-lookup"><span data-stu-id="327ef-150">When the environment is set to `Staging`, the following `Configure` method reads the value of `MyConfig`:</span></span>

[!code-csharp[Main](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]


<span data-ttu-id="327ef-151">Для среды обычно задаются значения `Development`, `Staging` или `Production`.</span><span class="sxs-lookup"><span data-stu-id="327ef-151">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="327ef-152">Дополнительные сведения см. в разделе [Работа с несколькими средами](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="327ef-152">For more information, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="327ef-153">Рекомендации по конфигурации:</span><span class="sxs-lookup"><span data-stu-id="327ef-153">Configuration considerations:</span></span>

* <span data-ttu-id="327ef-154">`IOptionsSnapshot` может перезагрузить данные конфигурации после их изменения.</span><span class="sxs-lookup"><span data-stu-id="327ef-154">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="327ef-155">Дополнительные сведения: [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="327ef-155">For more information, see [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).,</span></span>
* <span data-ttu-id="327ef-156">Ключи конфигурации **не учитывают** регистр.</span><span class="sxs-lookup"><span data-stu-id="327ef-156">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="327ef-157">**Никогда** не храните пароли и другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации в виде обычного текста.</span><span class="sxs-lookup"><span data-stu-id="327ef-157">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="327ef-158">Не используйте секреты рабочей среды в средах разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="327ef-158">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="327ef-159">Указывайте секреты вне проекта, чтобы их нельзя было случайно зафиксировать в репозитории с исходным кодом.</span><span class="sxs-lookup"><span data-stu-id="327ef-159">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="327ef-160">Узнайте больше о [работе с несколькими средами](xref:fundamentals/environments) и управлении [безопасным хранением секретов приложения во время разработки](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="327ef-160">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="327ef-161">Если система не позволяет использовать двоеточие (`:`) в переменных среды, замените двоеточие (`:`) двойным подчеркиванием (`__`).</span><span class="sxs-lookup"><span data-stu-id="327ef-161">If a colon (`:`) can't be used in environment variables on a system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="327ef-162">Поставщик в памяти и привязка к классу POCO</span><span class="sxs-lookup"><span data-stu-id="327ef-162">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="327ef-163">В следующем примере демонстрируется использование поставщика в памяти и привязка к классу:</span><span class="sxs-lookup"><span data-stu-id="327ef-163">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

<span data-ttu-id="327ef-164">Значения конфигурации возвращаются в виде строк, но с помощью привязки можно создавать объекты.</span><span class="sxs-lookup"><span data-stu-id="327ef-164">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="327ef-165">Привязка позволяет извлекать объекты POCO и даже целые графы объектов.</span><span class="sxs-lookup"><span data-stu-id="327ef-165">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="327ef-166">GetValue</span><span class="sxs-lookup"><span data-stu-id="327ef-166">GetValue</span></span>

<span data-ttu-id="327ef-167">В следующем примере показан метод расширения [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_):</span><span class="sxs-lookup"><span data-stu-id="327ef-167">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="327ef-168">Метод `GetValue<T>` из ConfigurationBinder позволяет задавать значение по умолчанию (в примере — 80).</span><span class="sxs-lookup"><span data-stu-id="327ef-168">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="327ef-169">`GetValue<T>` используется для простых сценариев и не выполняет привязку ко всем разделам.</span><span class="sxs-lookup"><span data-stu-id="327ef-169">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="327ef-170">Метод `GetValue<T>` получает из `GetSection(key).Value` скалярные значения, преобразованные в определенный тип.</span><span class="sxs-lookup"><span data-stu-id="327ef-170">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="327ef-171">Привязка к графу объектов</span><span class="sxs-lookup"><span data-stu-id="327ef-171">Bind to an object graph</span></span>

<span data-ttu-id="327ef-172">Для каждого объекта в классе возможна рекурсивная привязка.</span><span class="sxs-lookup"><span data-stu-id="327ef-172">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="327ef-173">Рассмотрим следующий класс `AppSettings`:</span><span class="sxs-lookup"><span data-stu-id="327ef-173">Consider the following `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="327ef-174">В следующем примере выполняется привязка к классу `AppSettings`:</span><span class="sxs-lookup"><span data-stu-id="327ef-174">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="327ef-175">**ASP.NET Core 1.1** и более поздние версии могут использовать метод `Get<T>`, который работает с целыми разделами.</span><span class="sxs-lookup"><span data-stu-id="327ef-175">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="327ef-176">Метод `Get<T>` может быть более удобным, чем `Bind`.</span><span class="sxs-lookup"><span data-stu-id="327ef-176">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="327ef-177">Следующий код показывает, как использовать `Get<T>` с предыдущим примером.</span><span class="sxs-lookup"><span data-stu-id="327ef-177">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="327ef-178">Используется следующий файл *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="327ef-178">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="327ef-179">Программа выводит `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="327ef-179">The program displays `Height 11`.</span></span>

<span data-ttu-id="327ef-180">Для модульного тестирования конфигурации можно выполнить следующий код:</span><span class="sxs-lookup"><span data-stu-id="327ef-180">The following code can be used to unit test the configuration:</span></span>

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

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="327ef-181">Создание пользовательского поставщика Entity Framework</span><span class="sxs-lookup"><span data-stu-id="327ef-181">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="327ef-182">В этом разделе создается поставщик базовой конфигурации, который считывает пары "имя-значение" из базы данных с помощью EF.</span><span class="sxs-lookup"><span data-stu-id="327ef-182">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="327ef-183">Определите сущность `ConfigurationValue` для хранения значений конфигурации в базе данных:</span><span class="sxs-lookup"><span data-stu-id="327ef-183">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="327ef-184">Добавьте `ConfigurationContext` в хранилище и обратитесь к настроенным значениям:</span><span class="sxs-lookup"><span data-stu-id="327ef-184">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="327ef-185">Создайте класс, реализующий [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource).</span><span class="sxs-lookup"><span data-stu-id="327ef-185">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="327ef-186">Создайте пользовательский поставщик конфигурации путем наследования от [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span><span class="sxs-lookup"><span data-stu-id="327ef-186">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="327ef-187">Поставщик конфигурации инициализирует пустую базу данных:</span><span class="sxs-lookup"><span data-stu-id="327ef-187">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="327ef-188">При выполнении примера отображаются выделенные значения из базы данных ("value_from_ef_1" и "value_from_ef_2").</span><span class="sxs-lookup"><span data-stu-id="327ef-188">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="327ef-189">Вы можете использовать метод расширения `EFConfigSource` для добавления источника конфигурации.</span><span class="sxs-lookup"><span data-stu-id="327ef-189">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="327ef-190">В следующем примере кода демонстрируется использование настраиваемого `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="327ef-190">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="327ef-191">Обратите внимание, что в этом примере пользовательский `EFConfigProvider` добавляется после поставщика JSON, поэтому все параметры из базы данных переопределяют параметры из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="327ef-191">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="327ef-192">Используется следующий файл *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="327ef-192">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="327ef-193">Выводится следующий результат.</span><span class="sxs-lookup"><span data-stu-id="327ef-193">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="327ef-194">Поставщик конфигурации CommandLine</span><span class="sxs-lookup"><span data-stu-id="327ef-194">CommandLine configuration provider</span></span>

<span data-ttu-id="327ef-195">[Поставщик конфигурации CommandLine](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) получает пары "ключ-значение" аргумента командной строки для конфигурации во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="327ef-195">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

<span data-ttu-id="327ef-196">[Просмотрите или скачайте пример конфигурации CommandLine](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="327ef-196">[View or download the CommandLine configuration sample](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)</span></span>

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="327ef-197">Настройка и использование поставщика конфигурации CommandLine</span><span class="sxs-lookup"><span data-stu-id="327ef-197">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="327ef-198">Базовая конфигурация</span><span class="sxs-lookup"><span data-stu-id="327ef-198">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="327ef-199">Чтобы активировать конфигурацию командной строки, вызовите метод расширения `AddCommandLine` в экземпляре [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="327ef-199">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="327ef-200">После выполнения кода отобразятся следующие выходные данные:</span><span class="sxs-lookup"><span data-stu-id="327ef-200">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="327ef-201">Передача пар "ключ-значение" аргументов в командной строке приведет к изменению значений `Profile:MachineName` и `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="327ef-201">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="327ef-202">В окне консоли отображается следующее:</span><span class="sxs-lookup"><span data-stu-id="327ef-202">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="327ef-203">Чтобы переопределить конфигурацию, предоставленную другими поставщиками конфигурации, конфигурацией командной строки, вызовите `AddCommandLine` последним в `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="327ef-203">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="327ef-204">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="327ef-204">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="327ef-205">Для создания узла стандартные приложения ASP.NET Core 2.x используют статический удобный метод `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="327ef-205">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="327ef-206">`CreateDefaultBuilder` загружает дополнительную конфигурацию из *appsettings.json*, *appsettings.{среда}.json*, [секреты пользователя](xref:security/app-secrets) (в среде `Development`), переменные среды и аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="327ef-206">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="327ef-207">Поставщик конфигурации CommandLine вызывается последним.</span><span class="sxs-lookup"><span data-stu-id="327ef-207">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="327ef-208">В этом случае аргументы командной строки, передаваемые во время выполнения, переопределяют конфигурацию, заданную другими вызванными ранее поставщиками конфигурации.</span><span class="sxs-lookup"><span data-stu-id="327ef-208">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="327ef-209">Предположим, что в файлах *appsettings*</span><span class="sxs-lookup"><span data-stu-id="327ef-209">For *appsettings* files where:</span></span>

* <span data-ttu-id="327ef-210">Включено `reloadOnChange`.</span><span class="sxs-lookup"><span data-stu-id="327ef-210">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="327ef-211">Аргументы командной строки и файл *appsettings* содержат одинаковые параметры.</span><span class="sxs-lookup"><span data-stu-id="327ef-211">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="327ef-212">Файл *appsettings*, содержащий соответствующий аргумент командной строки, изменяется после запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="327ef-212">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="327ef-213">Если все вышеперечисленное верно, аргументы командной строки переопределяются.</span><span class="sxs-lookup"><span data-stu-id="327ef-213">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="327ef-214">Приложение ASP.NET Core 2.x может использовать [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) вместо `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="327ef-214">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="327ef-215">При использовании `WebHostBuilder` задайте конфигурацию вручную с помощью [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span><span class="sxs-lookup"><span data-stu-id="327ef-215">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="327ef-216">Дополнительные сведения см. в разделе о ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="327ef-216">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="327ef-217">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="327ef-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="327ef-218">Чтобы использовать поставщик конфигурации CommandLine, создайте [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) и вызовите метод `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="327ef-218">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="327ef-219">В этом случае аргументы командной строки, передаваемые во время выполнения, переопределяют конфигурацию, заданную другими вызванными ранее поставщиками конфигурации.</span><span class="sxs-lookup"><span data-stu-id="327ef-219">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="327ef-220">Примените конфигурацию к [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) с помощью метода `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="327ef-220">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="327ef-221">Аргументы</span><span class="sxs-lookup"><span data-stu-id="327ef-221">Arguments</span></span>

<span data-ttu-id="327ef-222">Аргументы, передаваемые в командной строке, должны соответствовать одному из двух форматов, приведенных в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="327ef-222">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="327ef-223">Формат аргумента</span><span class="sxs-lookup"><span data-stu-id="327ef-223">Argument format</span></span>                                                     | <span data-ttu-id="327ef-224">Пример</span><span class="sxs-lookup"><span data-stu-id="327ef-224">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="327ef-225">Один аргумент: пара "ключ-значение", разделенная знаком равенства (`=`)</span><span class="sxs-lookup"><span data-stu-id="327ef-225">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="327ef-226">Последовательность из двух аргументов: пара "ключ-значение", разделенная пробелом</span><span class="sxs-lookup"><span data-stu-id="327ef-226">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="327ef-227">**Один аргумент**</span><span class="sxs-lookup"><span data-stu-id="327ef-227">**Single argument**</span></span>

<span data-ttu-id="327ef-228">Значение должно стоять после знака равенства (`=`).</span><span class="sxs-lookup"><span data-stu-id="327ef-228">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="327ef-229">Значение может быть равно NULL (например, `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="327ef-229">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="327ef-230">Ключ может иметь префикс.</span><span class="sxs-lookup"><span data-stu-id="327ef-230">The key may have a prefix.</span></span>

| <span data-ttu-id="327ef-231">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="327ef-231">Key prefix</span></span>               | <span data-ttu-id="327ef-232">Пример</span><span class="sxs-lookup"><span data-stu-id="327ef-232">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="327ef-233">Без префикса</span><span class="sxs-lookup"><span data-stu-id="327ef-233">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="327ef-234">Один дефис (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="327ef-234">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="327ef-235">Два дефиса (`--`)</span><span class="sxs-lookup"><span data-stu-id="327ef-235">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="327ef-236">Прямая косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="327ef-236">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="327ef-237">&#8224;Ключ с префиксом из одного дефиса (`-`) должен быть указан в [сопоставлениях переключений](#switch-mappings), описанных ниже.</span><span class="sxs-lookup"><span data-stu-id="327ef-237">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="327ef-238">Пример команды:</span><span class="sxs-lookup"><span data-stu-id="327ef-238">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="327ef-239">Примечание. Если `-key1` отсутствует в [сопоставлениях переключений](#switch-mappings), переданных поставщику конфигурации, возникает исключение `FormatException`.</span><span class="sxs-lookup"><span data-stu-id="327ef-239">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="327ef-240">**Последовательность из двух аргументов**</span><span class="sxs-lookup"><span data-stu-id="327ef-240">**Sequence of two arguments**</span></span>

<span data-ttu-id="327ef-241">Значение не может быть равно NULL и должно следовать за ключом, разделенным пробелом.</span><span class="sxs-lookup"><span data-stu-id="327ef-241">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="327ef-242">Ключ должен иметь префикс.</span><span class="sxs-lookup"><span data-stu-id="327ef-242">The key must have a prefix.</span></span>

| <span data-ttu-id="327ef-243">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="327ef-243">Key prefix</span></span>               | <span data-ttu-id="327ef-244">Пример</span><span class="sxs-lookup"><span data-stu-id="327ef-244">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="327ef-245">Один дефис (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="327ef-245">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="327ef-246">Два дефиса (`--`)</span><span class="sxs-lookup"><span data-stu-id="327ef-246">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="327ef-247">Прямая косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="327ef-247">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="327ef-248">&#8224;Ключ с префиксом из одного дефиса (`-`) должен быть указан в [сопоставлениях переключений](#switch-mappings), описанных ниже.</span><span class="sxs-lookup"><span data-stu-id="327ef-248">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="327ef-249">Пример команды:</span><span class="sxs-lookup"><span data-stu-id="327ef-249">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="327ef-250">Примечание. Если `-key1` отсутствует в [сопоставлениях переключений](#switch-mappings), переданных поставщику конфигурации, возникает исключение `FormatException`.</span><span class="sxs-lookup"><span data-stu-id="327ef-250">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="327ef-251">Повторяющиеся ключи</span><span class="sxs-lookup"><span data-stu-id="327ef-251">Duplicate keys</span></span>

<span data-ttu-id="327ef-252">Если указаны повторяющиеся ключи, используется последняя пара "ключ-значение".</span><span class="sxs-lookup"><span data-stu-id="327ef-252">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="327ef-253">Сопоставления переключений</span><span class="sxs-lookup"><span data-stu-id="327ef-253">Switch mappings</span></span>

<span data-ttu-id="327ef-254">При создании конфигурации вручную с помощью `ConfigurationBuilder` вы можете добавить в метод `AddCommandLine` словарь сопоставления параметров.</span><span class="sxs-lookup"><span data-stu-id="327ef-254">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="327ef-255">Сопоставление параметров позволяет указать логику замены имен ключей.</span><span class="sxs-lookup"><span data-stu-id="327ef-255">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="327ef-256">В словаре сопоставлений переключений выполняется поиск ключа, который совпадает с ключом, предоставляемым аргументом командной строки.</span><span class="sxs-lookup"><span data-stu-id="327ef-256">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="327ef-257">Если ключ командной строки найден, значение словаря (новый ключ) передается обратно, чтобы задать конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="327ef-257">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="327ef-258">Сопоставление переключений необходимо для любого ключа командной строки с префиксом из одного дефиса (`-`).</span><span class="sxs-lookup"><span data-stu-id="327ef-258">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="327ef-259">Правила ключей из словаря сопоставления переключений:</span><span class="sxs-lookup"><span data-stu-id="327ef-259">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="327ef-260">Переключения должны начинаться с дефиса (`-`) или двойного дефиса (`--`).</span><span class="sxs-lookup"><span data-stu-id="327ef-260">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="327ef-261">Словарь сопоставлений переключений не должен содержать повторяющиеся ключи.</span><span class="sxs-lookup"><span data-stu-id="327ef-261">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="327ef-262">В следующем примере метод `GetSwitchMappings` позволяет аргументам командной строки использовать префикс ключа с одним дефисом (`-`) вместо начальных префиксов подключа.</span><span class="sxs-lookup"><span data-stu-id="327ef-262">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="327ef-263">Если аргументы командной строки не указаны, словарь для `AddInMemoryCollection` задает значения конфигурации.</span><span class="sxs-lookup"><span data-stu-id="327ef-263">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="327ef-264">Запустите приложение с помощью следующей команды:</span><span class="sxs-lookup"><span data-stu-id="327ef-264">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="327ef-265">В окне консоли отображается следующее:</span><span class="sxs-lookup"><span data-stu-id="327ef-265">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="327ef-266">Используйте следующее для передачи параметров конфигурации:</span><span class="sxs-lookup"><span data-stu-id="327ef-266">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="327ef-267">В окне консоли отображается следующее:</span><span class="sxs-lookup"><span data-stu-id="327ef-267">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="327ef-268">Созданный словарь сопоставления параметров содержит данные, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="327ef-268">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="327ef-269">Ключ</span><span class="sxs-lookup"><span data-stu-id="327ef-269">Key</span></span>            | <span data-ttu-id="327ef-270">Значение</span><span class="sxs-lookup"><span data-stu-id="327ef-270">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="327ef-271">Чтобы продемонстрировать переключение ключей с помощью словаря, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="327ef-271">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="327ef-272">Ключи командной строки меняются.</span><span class="sxs-lookup"><span data-stu-id="327ef-272">The command-line keys are swapped.</span></span> <span data-ttu-id="327ef-273">В окне консоли отображаются значения конфигурации для `Profile:MachineName` и `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="327ef-273">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a><span data-ttu-id="327ef-274">Файл web.config</span><span class="sxs-lookup"><span data-stu-id="327ef-274">web.config file</span></span>

<span data-ttu-id="327ef-275">Файл *web.config* необходим для размещения приложения в службах IIS или IIS Express.</span><span class="sxs-lookup"><span data-stu-id="327ef-275">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="327ef-276">Параметры в файле *web.config* включают [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) для запуска приложения и настройки других параметров и модулей IIS.</span><span class="sxs-lookup"><span data-stu-id="327ef-276">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="327ef-277">Если файл *web.config* отсутствует и файл проекта содержит `<Project Sdk="Microsoft.NET.Sdk.Web">`, при публикации проекта файл *web.config* создается в опубликованных выходных данных (папка *publish*).</span><span class="sxs-lookup"><span data-stu-id="327ef-277">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="327ef-278">Дополнительные сведения см. в разделе [Размещение ASP.NET Core в Windows со службами IIS](xref:host-and-deploy/iis/index#webconfig-file).</span><span class="sxs-lookup"><span data-stu-id="327ef-278">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig-file).</span></span>

## <a name="accessing-configuration-during-startup"></a><span data-ttu-id="327ef-279">Доступ к конфигурации во время запуска</span><span class="sxs-lookup"><span data-stu-id="327ef-279">Accessing configuration during startup</span></span>

<span data-ttu-id="327ef-280">Чтобы получить доступ к конфигурации в `ConfigureServices` или `Configure` во время запуска, см. примеры в разделе [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="327ef-280">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="327ef-281">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="327ef-281">Additional notes</span></span>

* <span data-ttu-id="327ef-282">Внедрение зависимостей (DI) настраивается после вызова `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="327ef-282">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="327ef-283">Система конфигурации не поддерживает внедрение зависимостей.</span><span class="sxs-lookup"><span data-stu-id="327ef-283">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="327ef-284">`IConfiguration` имеет две специализации:</span><span class="sxs-lookup"><span data-stu-id="327ef-284">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="327ef-285">`IConfigurationRoot` используется для корневого узла.</span><span class="sxs-lookup"><span data-stu-id="327ef-285">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="327ef-286">Может инициировать перезагрузку.</span><span class="sxs-lookup"><span data-stu-id="327ef-286">Can trigger a reload.</span></span>
  * <span data-ttu-id="327ef-287">`IConfigurationSection` представляет раздел значений конфигурации.</span><span class="sxs-lookup"><span data-stu-id="327ef-287">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="327ef-288">Методы `GetSection` и `GetChildren` возвращают `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="327ef-288">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="327ef-289">Используйте [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) при перезагрузке конфигурации или для доступа к каждому поставщику.</span><span class="sxs-lookup"><span data-stu-id="327ef-289">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="327ef-290">Оба этих случая нетипичные.</span><span class="sxs-lookup"><span data-stu-id="327ef-290">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="327ef-291">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="327ef-291">Additional resources</span></span>

* [<span data-ttu-id="327ef-292">Параметры</span><span class="sxs-lookup"><span data-stu-id="327ef-292">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="327ef-293">Работа с несколькими средами</span><span class="sxs-lookup"><span data-stu-id="327ef-293">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="327ef-294">Безопасное хранение секретов приложений во время разработки</span><span class="sxs-lookup"><span data-stu-id="327ef-294">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="327ef-295">Размещение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="327ef-295">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="327ef-296">Введение зависимостей</span><span class="sxs-lookup"><span data-stu-id="327ef-296">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="327ef-297">Поставщик конфигурации Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="327ef-297">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
