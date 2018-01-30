---
title: "Конфигурация в .NET Core"
author: rick-anderson
description: "Используйте API конфигурации для настройки приложения ASP.NET Core несколькими способами."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/index
ms.openlocfilehash: 20c75d202d67a491890d87cebf549585e0313da0
ms.sourcegitcommit: 18ff1fdaa3e1ae204ed6a2ba9351ce8cf1371c85
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2018
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="e1478-103">Настройка приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e1478-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="e1478-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson), [Марк Михаэлис](http://intellitect.com/author/mark-michaelis/) (Mark Michaelis), [Стив Смит](https://ardalis.com/) (Steve Smith), [Даниэль Рот](https://github.com/danroth27) (Daniel Roth) и [Люк Лэтхэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="e1478-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e1478-105">API конфигурации позволяет настраивать веб-приложения ASP.NET Core на основе списка пар "имя-значение".</span><span class="sxs-lookup"><span data-stu-id="e1478-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="e1478-106">Конфигурация считывается во время выполнения из нескольких источников.</span><span class="sxs-lookup"><span data-stu-id="e1478-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="e1478-107">Пары "имя-значение" можно сгруппировать в многоуровневую иерархию.</span><span class="sxs-lookup"><span data-stu-id="e1478-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="e1478-108">Существуют следующие поставщики конфигурации:</span><span class="sxs-lookup"><span data-stu-id="e1478-108">There are configuration providers for:</span></span>

* <span data-ttu-id="e1478-109">форматов файлов (INI, JSON и XML);</span><span class="sxs-lookup"><span data-stu-id="e1478-109">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="e1478-110">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="e1478-110">Command-line arguments</span></span>
* <span data-ttu-id="e1478-111">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="e1478-111">Environment variables</span></span>
* <span data-ttu-id="e1478-112">объектов .NET в памяти;</span><span class="sxs-lookup"><span data-stu-id="e1478-112">In-memory .NET objects</span></span>
* <span data-ttu-id="e1478-113">зашифрованного пользовательского хранилища;</span><span class="sxs-lookup"><span data-stu-id="e1478-113">An encrypted user store</span></span>
* <span data-ttu-id="e1478-114">[Azure Key Vault](xref:security/key-vault-configuration);</span><span class="sxs-lookup"><span data-stu-id="e1478-114">[Azure Key Vault](xref:security/key-vault-configuration)</span></span>
* <span data-ttu-id="e1478-115">а также пользовательские поставщики (устанавливаемые или создаваемые).</span><span class="sxs-lookup"><span data-stu-id="e1478-115">Custom providers (installed or created)</span></span>

<span data-ttu-id="e1478-116">Каждое значение конфигурации сопоставляется строковому ключу.</span><span class="sxs-lookup"><span data-stu-id="e1478-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="e1478-117">Имеется встроенная поддержка привязки для десериализации параметров в пользовательский объект [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) (простой класс .NET со свойствами).</span><span class="sxs-lookup"><span data-stu-id="e1478-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="e1478-118">Шаблон параметров использует классы параметров для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="e1478-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="e1478-119">Дополнительные сведения об использовании шаблона параметров см. в разделе [Параметры](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="e1478-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="e1478-120">[Просмотреть или скачать образец кода](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e1478-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="e1478-121">Конфигурация JSON</span><span class="sxs-lookup"><span data-stu-id="e1478-121">JSON configuration</span></span>

<span data-ttu-id="e1478-122">В следующем консольном приложении используется поставщик конфигурации JSON:</span><span class="sxs-lookup"><span data-stu-id="e1478-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="e1478-123">Приложение считывает и отображает следующие параметры конфигурации приложения:</span><span class="sxs-lookup"><span data-stu-id="e1478-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="e1478-124">Конфигурация — это иерархический список пар "имя-значение", в котором узлы разделяются двоеточием.</span><span class="sxs-lookup"><span data-stu-id="e1478-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="e1478-125">Чтобы получить значение, обратитесь к индексатору `Configuration` с ключом соответствующего элемента:</span><span class="sxs-lookup"><span data-stu-id="e1478-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="e1478-126">Для работы с массивами в источниках конфигурации в формате JSON используйте индекс массива в составе строки с разделителем-двоеточием.</span><span class="sxs-lookup"><span data-stu-id="e1478-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="e1478-127">В следующем примере возвращается имя первого элемента в предыдущем массиве `wizards`:</span><span class="sxs-lookup"><span data-stu-id="e1478-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="e1478-128">Пары "имя-значение", записываемые во встроенные поставщики [конфигурации](/dotnet/api/microsoft.extensions.configuration), **не сохраняются**.</span><span class="sxs-lookup"><span data-stu-id="e1478-128">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="e1478-129">Но вы можете создать настраиваемый поставщик, который сохраняет значения.</span><span class="sxs-lookup"><span data-stu-id="e1478-129">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="e1478-130">См. раздел [Пользовательский поставщик конфигурации](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="e1478-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="e1478-131">В предыдущем примере для считывания значений используется индексатор конфигурации.</span><span class="sxs-lookup"><span data-stu-id="e1478-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="e1478-132">Для доступа к конфигурации вне `Startup` воспользуйтесь *шаблоном параметров*.</span><span class="sxs-lookup"><span data-stu-id="e1478-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="e1478-133">Дополнительные сведения см. в разделе [Параметры](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="e1478-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>


## <a name="configuration-by-environment"></a><span data-ttu-id="e1478-134">Конфигурация для разных сред</span><span class="sxs-lookup"><span data-stu-id="e1478-134">Configuration by environment</span></span>

<span data-ttu-id="e1478-135">Как правило, для разных сред (например, разработки, тестирования и производства) существуют разные параметры конфигурации.</span><span class="sxs-lookup"><span data-stu-id="e1478-135">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="e1478-136">Метод расширения `CreateDefaultBuilder` в приложении ASP.NET Core 2.x (или при использовании `AddJsonFile` и `AddEnvironmentVariables` непосредственно в приложении ASP.NET Core 1.x) добавляет поставщики конфигурации для чтения JSON-файлов и источников системной конфигурации:</span><span class="sxs-lookup"><span data-stu-id="e1478-136">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="e1478-137">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="e1478-137">*appsettings.json*</span></span>
* <span data-ttu-id="e1478-138">*appsettings.\<имя_среды>.json*</span><span class="sxs-lookup"><span data-stu-id="e1478-138">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="e1478-139">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="e1478-139">Environment variables</span></span>

<span data-ttu-id="e1478-140">Приложения ASP.NET Core 1.x должны вызывать `AddJsonFile` и [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span><span class="sxs-lookup"><span data-stu-id="e1478-140">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="e1478-141">Объяснение параметров см. в разделе [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions).</span><span class="sxs-lookup"><span data-stu-id="e1478-141">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="e1478-142">`reloadOnChange` поддерживается только в ASP.NET Core 1.1 и более поздних версиях.</span><span class="sxs-lookup"><span data-stu-id="e1478-142">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="e1478-143">Источники конфигурации считываются в порядке их указания.</span><span class="sxs-lookup"><span data-stu-id="e1478-143">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="e1478-144">В приведенном выше коде переменные сред считываются последними.</span><span class="sxs-lookup"><span data-stu-id="e1478-144">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="e1478-145">Все значения конфигурации, заданные с помощью среды, заменяют значения, заданные в предыдущих двух поставщиках.</span><span class="sxs-lookup"><span data-stu-id="e1478-145">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="e1478-146">Рассмотрим следующий файл *appsettings.Staging.JSON*.</span><span class="sxs-lookup"><span data-stu-id="e1478-146">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[Main](index/sample/appsettings.Staging.json)]

<span data-ttu-id="e1478-147">Когда для среды задано значение `Staging`, следующий метод `Configure` считывает значение `MyConfig`.</span><span class="sxs-lookup"><span data-stu-id="e1478-147">When the environment is set to `Staging`, the following `Configure` method reads the value of `MyConfig`:</span></span>

[!code-csharp[Main](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]


<span data-ttu-id="e1478-148">Для среды обычно задаются значения `Development`, `Staging` или `Production`.</span><span class="sxs-lookup"><span data-stu-id="e1478-148">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="e1478-149">Дополнительные сведения см. в разделе [Работа с несколькими средами](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="e1478-149">For more information, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="e1478-150">Рекомендации по конфигурации:</span><span class="sxs-lookup"><span data-stu-id="e1478-150">Configuration considerations:</span></span>

* <span data-ttu-id="e1478-151">`IOptionsSnapshot` может перезагрузить данные конфигурации после их изменения.</span><span class="sxs-lookup"><span data-stu-id="e1478-151">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="e1478-152">Дополнительные сведения: [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="e1478-152">For more information, see [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).,</span></span>
* <span data-ttu-id="e1478-153">Ключи конфигурации **не учитывают** регистр.</span><span class="sxs-lookup"><span data-stu-id="e1478-153">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="e1478-154">**Никогда** не храните пароли и другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации в виде обычного текста.</span><span class="sxs-lookup"><span data-stu-id="e1478-154">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="e1478-155">Не используйте секреты рабочей среды в средах разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="e1478-155">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="e1478-156">Указывайте секреты вне проекта, чтобы их нельзя было случайно зафиксировать в репозитории с исходным кодом.</span><span class="sxs-lookup"><span data-stu-id="e1478-156">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="e1478-157">Узнайте больше о [работе с несколькими средами](xref:fundamentals/environments) и управлении [безопасным хранением секретов приложения во время разработки](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="e1478-157">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="e1478-158">Если система не позволяет использовать двоеточие (`:`) в переменных среды, замените двоеточие (`:`) двойным подчеркиванием (`__`).</span><span class="sxs-lookup"><span data-stu-id="e1478-158">If a colon (`:`) can't be used in environment variables on a system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="e1478-159">Поставщик в памяти и привязка к классу POCO</span><span class="sxs-lookup"><span data-stu-id="e1478-159">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="e1478-160">В следующем примере демонстрируется использование поставщика в памяти и привязка к классу:</span><span class="sxs-lookup"><span data-stu-id="e1478-160">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

<span data-ttu-id="e1478-161">Значения конфигурации возвращаются в виде строк, но с помощью привязки можно создавать объекты.</span><span class="sxs-lookup"><span data-stu-id="e1478-161">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="e1478-162">Привязка позволяет извлекать объекты POCO и даже целые графы объектов.</span><span class="sxs-lookup"><span data-stu-id="e1478-162">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="e1478-163">GetValue</span><span class="sxs-lookup"><span data-stu-id="e1478-163">GetValue</span></span>

<span data-ttu-id="e1478-164">В следующем примере показан метод расширения [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_):</span><span class="sxs-lookup"><span data-stu-id="e1478-164">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="e1478-165">Метод `GetValue<T>` из ConfigurationBinder позволяет задавать значение по умолчанию (в примере — 80).</span><span class="sxs-lookup"><span data-stu-id="e1478-165">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="e1478-166">`GetValue<T>` используется для простых сценариев и не выполняет привязку ко всем разделам.</span><span class="sxs-lookup"><span data-stu-id="e1478-166">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="e1478-167">Метод `GetValue<T>` получает из `GetSection(key).Value` скалярные значения, преобразованные в определенный тип.</span><span class="sxs-lookup"><span data-stu-id="e1478-167">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="e1478-168">Привязка к графу объектов</span><span class="sxs-lookup"><span data-stu-id="e1478-168">Bind to an object graph</span></span>

<span data-ttu-id="e1478-169">Для каждого объекта в классе возможна рекурсивная привязка.</span><span class="sxs-lookup"><span data-stu-id="e1478-169">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="e1478-170">Рассмотрим следующий класс `AppSettings`:</span><span class="sxs-lookup"><span data-stu-id="e1478-170">Consider the following `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="e1478-171">В следующем примере выполняется привязка к классу `AppSettings`:</span><span class="sxs-lookup"><span data-stu-id="e1478-171">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="e1478-172">**ASP.NET Core 1.1** и более поздние версии могут использовать метод `Get<T>`, который работает с целыми разделами.</span><span class="sxs-lookup"><span data-stu-id="e1478-172">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="e1478-173">Метод `Get<T>` может быть более удобным, чем `Bind`.</span><span class="sxs-lookup"><span data-stu-id="e1478-173">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="e1478-174">Следующий код показывает, как использовать `Get<T>` с предыдущим примером.</span><span class="sxs-lookup"><span data-stu-id="e1478-174">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="e1478-175">Используется следующий файл *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="e1478-175">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="e1478-176">Программа выводит `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="e1478-176">The program displays `Height 11`.</span></span>

<span data-ttu-id="e1478-177">Для модульного тестирования конфигурации можно выполнить следующий код:</span><span class="sxs-lookup"><span data-stu-id="e1478-177">The following code can be used to unit test the configuration:</span></span>

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

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="e1478-178">Создание пользовательского поставщика Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e1478-178">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="e1478-179">В этом разделе создается поставщик базовой конфигурации, который считывает пары "имя-значение" из базы данных с помощью EF.</span><span class="sxs-lookup"><span data-stu-id="e1478-179">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="e1478-180">Определите сущность `ConfigurationValue` для хранения значений конфигурации в базе данных:</span><span class="sxs-lookup"><span data-stu-id="e1478-180">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="e1478-181">Добавьте `ConfigurationContext` в хранилище и обратитесь к настроенным значениям:</span><span class="sxs-lookup"><span data-stu-id="e1478-181">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="e1478-182">Создайте класс, реализующий [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource).</span><span class="sxs-lookup"><span data-stu-id="e1478-182">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="e1478-183">Создайте пользовательский поставщик конфигурации путем наследования от [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span><span class="sxs-lookup"><span data-stu-id="e1478-183">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="e1478-184">Поставщик конфигурации инициализирует пустую базу данных:</span><span class="sxs-lookup"><span data-stu-id="e1478-184">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="e1478-185">При выполнении примера отображаются выделенные значения из базы данных ("value_from_ef_1" и "value_from_ef_2").</span><span class="sxs-lookup"><span data-stu-id="e1478-185">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="e1478-186">Вы можете использовать метод расширения `EFConfigSource` для добавления источника конфигурации.</span><span class="sxs-lookup"><span data-stu-id="e1478-186">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="e1478-187">В следующем примере кода демонстрируется использование настраиваемого `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="e1478-187">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="e1478-188">Обратите внимание, что в этом примере пользовательский `EFConfigProvider` добавляется после поставщика JSON, поэтому все параметры из базы данных переопределяют параметры из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e1478-188">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="e1478-189">Используется следующий файл *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="e1478-189">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="e1478-190">Выводится следующий результат.</span><span class="sxs-lookup"><span data-stu-id="e1478-190">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="e1478-191">Поставщик конфигурации CommandLine</span><span class="sxs-lookup"><span data-stu-id="e1478-191">CommandLine configuration provider</span></span>

<span data-ttu-id="e1478-192">[Поставщик конфигурации CommandLine](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) получает пары "ключ-значение" аргумента командной строки для конфигурации во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="e1478-192">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

<span data-ttu-id="e1478-193">[Просмотрите или скачайте пример конфигурации CommandLine](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="e1478-193">[View or download the CommandLine configuration sample](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)</span></span>

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="e1478-194">Настройка и использование поставщика конфигурации CommandLine</span><span class="sxs-lookup"><span data-stu-id="e1478-194">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="e1478-195">Базовая конфигурация</span><span class="sxs-lookup"><span data-stu-id="e1478-195">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="e1478-196">Чтобы активировать конфигурацию командной строки, вызовите метод расширения `AddCommandLine` в экземпляре [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="e1478-196">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="e1478-197">После выполнения кода отобразятся следующие выходные данные:</span><span class="sxs-lookup"><span data-stu-id="e1478-197">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="e1478-198">Передача пар "ключ-значение" аргументов в командной строке приведет к изменению значений `Profile:MachineName` и `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="e1478-198">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="e1478-199">В окне консоли отображается следующее:</span><span class="sxs-lookup"><span data-stu-id="e1478-199">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="e1478-200">Чтобы переопределить конфигурацию, предоставленную другими поставщиками конфигурации, конфигурацией командной строки, вызовите `AddCommandLine` последним в `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="e1478-200">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e1478-201">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e1478-201">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e1478-202">Для создания узла стандартные приложения ASP.NET Core 2.x используют статический удобный метод `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="e1478-202">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="e1478-203">`CreateDefaultBuilder` загружает дополнительную конфигурацию из *appsettings.json*, *appsettings.{среда}.json*, [секреты пользователя](xref:security/app-secrets) (в среде `Development`), переменные среды и аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="e1478-203">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="e1478-204">Поставщик конфигурации CommandLine вызывается последним.</span><span class="sxs-lookup"><span data-stu-id="e1478-204">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="e1478-205">В этом случае аргументы командной строки, передаваемые во время выполнения, переопределяют конфигурацию, заданную другими вызванными ранее поставщиками конфигурации.</span><span class="sxs-lookup"><span data-stu-id="e1478-205">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="e1478-206">Предположим, что в файлах *appsettings*</span><span class="sxs-lookup"><span data-stu-id="e1478-206">For *appsettings* files where:</span></span>

* <span data-ttu-id="e1478-207">Включено `reloadOnChange`.</span><span class="sxs-lookup"><span data-stu-id="e1478-207">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="e1478-208">Аргументы командной строки и файл *appsettings* содержат одинаковые параметры.</span><span class="sxs-lookup"><span data-stu-id="e1478-208">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="e1478-209">Файл *appsettings*, содержащий соответствующий аргумент командной строки, изменяется после запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="e1478-209">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="e1478-210">Если все вышеперечисленное верно, аргументы командной строки переопределяются.</span><span class="sxs-lookup"><span data-stu-id="e1478-210">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="e1478-211">Приложение ASP.NET Core 2.x может использовать [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) вместо `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e1478-211">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="e1478-212">При использовании `WebHostBuilder` задайте конфигурацию вручную с помощью [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span><span class="sxs-lookup"><span data-stu-id="e1478-212">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="e1478-213">Дополнительные сведения см. в разделе о ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="e1478-213">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e1478-214">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e1478-214">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e1478-215">Чтобы использовать поставщик конфигурации CommandLine, создайте [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) и вызовите метод `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="e1478-215">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="e1478-216">В этом случае аргументы командной строки, передаваемые во время выполнения, переопределяют конфигурацию, заданную другими вызванными ранее поставщиками конфигурации.</span><span class="sxs-lookup"><span data-stu-id="e1478-216">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="e1478-217">Примените конфигурацию к [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) с помощью метода `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="e1478-217">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="e1478-218">Аргументы</span><span class="sxs-lookup"><span data-stu-id="e1478-218">Arguments</span></span>

<span data-ttu-id="e1478-219">Аргументы, передаваемые в командной строке, должны соответствовать одному из двух форматов, приведенных в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="e1478-219">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="e1478-220">Формат аргумента</span><span class="sxs-lookup"><span data-stu-id="e1478-220">Argument format</span></span>                                                     | <span data-ttu-id="e1478-221">Пример</span><span class="sxs-lookup"><span data-stu-id="e1478-221">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="e1478-222">Один аргумент: пара "ключ-значение", разделенная знаком равенства (`=`)</span><span class="sxs-lookup"><span data-stu-id="e1478-222">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="e1478-223">Последовательность из двух аргументов: пара "ключ-значение", разделенная пробелом</span><span class="sxs-lookup"><span data-stu-id="e1478-223">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="e1478-224">**Один аргумент**</span><span class="sxs-lookup"><span data-stu-id="e1478-224">**Single argument**</span></span>

<span data-ttu-id="e1478-225">Значение должно стоять после знака равенства (`=`).</span><span class="sxs-lookup"><span data-stu-id="e1478-225">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="e1478-226">Значение может быть равно NULL (например, `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="e1478-226">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="e1478-227">Ключ может иметь префикс.</span><span class="sxs-lookup"><span data-stu-id="e1478-227">The key may have a prefix.</span></span>

| <span data-ttu-id="e1478-228">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="e1478-228">Key prefix</span></span>               | <span data-ttu-id="e1478-229">Пример</span><span class="sxs-lookup"><span data-stu-id="e1478-229">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="e1478-230">Без префикса</span><span class="sxs-lookup"><span data-stu-id="e1478-230">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="e1478-231">Один дефис (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="e1478-231">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="e1478-232">Два дефиса (`--`)</span><span class="sxs-lookup"><span data-stu-id="e1478-232">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="e1478-233">Прямая косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="e1478-233">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="e1478-234">&#8224;Ключ с префиксом из одного дефиса (`-`) должен быть указан в [сопоставлениях переключений](#switch-mappings), описанных ниже.</span><span class="sxs-lookup"><span data-stu-id="e1478-234">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="e1478-235">Пример команды:</span><span class="sxs-lookup"><span data-stu-id="e1478-235">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="e1478-236">Примечание. Если `-key1` отсутствует в [сопоставлениях переключений](#switch-mappings), переданных поставщику конфигурации, возникает исключение `FormatException`.</span><span class="sxs-lookup"><span data-stu-id="e1478-236">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="e1478-237">**Последовательность из двух аргументов**</span><span class="sxs-lookup"><span data-stu-id="e1478-237">**Sequence of two arguments**</span></span>

<span data-ttu-id="e1478-238">Значение не может быть равно NULL и должно следовать за ключом, разделенным пробелом.</span><span class="sxs-lookup"><span data-stu-id="e1478-238">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="e1478-239">Ключ должен иметь префикс.</span><span class="sxs-lookup"><span data-stu-id="e1478-239">The key must have a prefix.</span></span>

| <span data-ttu-id="e1478-240">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="e1478-240">Key prefix</span></span>               | <span data-ttu-id="e1478-241">Пример</span><span class="sxs-lookup"><span data-stu-id="e1478-241">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="e1478-242">Один дефис (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="e1478-242">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="e1478-243">Два дефиса (`--`)</span><span class="sxs-lookup"><span data-stu-id="e1478-243">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="e1478-244">Прямая косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="e1478-244">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="e1478-245">&#8224;Ключ с префиксом из одного дефиса (`-`) должен быть указан в [сопоставлениях переключений](#switch-mappings), описанных ниже.</span><span class="sxs-lookup"><span data-stu-id="e1478-245">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="e1478-246">Пример команды:</span><span class="sxs-lookup"><span data-stu-id="e1478-246">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="e1478-247">Примечание. Если `-key1` отсутствует в [сопоставлениях переключений](#switch-mappings), переданных поставщику конфигурации, возникает исключение `FormatException`.</span><span class="sxs-lookup"><span data-stu-id="e1478-247">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="e1478-248">Повторяющиеся ключи</span><span class="sxs-lookup"><span data-stu-id="e1478-248">Duplicate keys</span></span>

<span data-ttu-id="e1478-249">Если указаны повторяющиеся ключи, используется последняя пара "ключ-значение".</span><span class="sxs-lookup"><span data-stu-id="e1478-249">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="e1478-250">Сопоставления переключений</span><span class="sxs-lookup"><span data-stu-id="e1478-250">Switch mappings</span></span>

<span data-ttu-id="e1478-251">При создании конфигурации вручную с помощью `ConfigurationBuilder` вы можете добавить в метод `AddCommandLine` словарь сопоставления параметров.</span><span class="sxs-lookup"><span data-stu-id="e1478-251">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="e1478-252">Сопоставление параметров позволяет указать логику замены имен ключей.</span><span class="sxs-lookup"><span data-stu-id="e1478-252">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="e1478-253">В словаре сопоставлений переключений выполняется поиск ключа, который совпадает с ключом, предоставляемым аргументом командной строки.</span><span class="sxs-lookup"><span data-stu-id="e1478-253">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="e1478-254">Если ключ командной строки найден, значение словаря (новый ключ) передается обратно, чтобы задать конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="e1478-254">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="e1478-255">Сопоставление переключений необходимо для любого ключа командной строки с префиксом из одного дефиса (`-`).</span><span class="sxs-lookup"><span data-stu-id="e1478-255">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="e1478-256">Правила ключей из словаря сопоставления переключений:</span><span class="sxs-lookup"><span data-stu-id="e1478-256">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="e1478-257">Переключения должны начинаться с дефиса (`-`) или двойного дефиса (`--`).</span><span class="sxs-lookup"><span data-stu-id="e1478-257">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="e1478-258">Словарь сопоставлений переключений не должен содержать повторяющиеся ключи.</span><span class="sxs-lookup"><span data-stu-id="e1478-258">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="e1478-259">В следующем примере метод `GetSwitchMappings` позволяет аргументам командной строки использовать префикс ключа с одним дефисом (`-`) вместо начальных префиксов подключа.</span><span class="sxs-lookup"><span data-stu-id="e1478-259">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="e1478-260">Если аргументы командной строки не указаны, словарь для `AddInMemoryCollection` задает значения конфигурации.</span><span class="sxs-lookup"><span data-stu-id="e1478-260">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="e1478-261">Запустите приложение с помощью следующей команды:</span><span class="sxs-lookup"><span data-stu-id="e1478-261">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="e1478-262">В окне консоли отображается следующее:</span><span class="sxs-lookup"><span data-stu-id="e1478-262">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="e1478-263">Используйте следующее для передачи параметров конфигурации:</span><span class="sxs-lookup"><span data-stu-id="e1478-263">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="e1478-264">В окне консоли отображается следующее:</span><span class="sxs-lookup"><span data-stu-id="e1478-264">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="e1478-265">Созданный словарь сопоставления параметров содержит данные, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="e1478-265">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="e1478-266">Ключ</span><span class="sxs-lookup"><span data-stu-id="e1478-266">Key</span></span>            | <span data-ttu-id="e1478-267">Значение</span><span class="sxs-lookup"><span data-stu-id="e1478-267">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="e1478-268">Чтобы продемонстрировать переключение ключей с помощью словаря, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="e1478-268">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="e1478-269">Ключи командной строки меняются.</span><span class="sxs-lookup"><span data-stu-id="e1478-269">The command-line keys are swapped.</span></span> <span data-ttu-id="e1478-270">В окне консоли отображаются значения конфигурации для `Profile:MachineName` и `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="e1478-270">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="e1478-271">Файл web.config</span><span class="sxs-lookup"><span data-stu-id="e1478-271">The web.config file</span></span>

<span data-ttu-id="e1478-272">Файл *web.config* необходим для размещения приложения в службах IIS или IIS Express.</span><span class="sxs-lookup"><span data-stu-id="e1478-272">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="e1478-273">Параметры в файле *web.config* включают [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) для запуска приложения и настройки других параметров и модулей IIS.</span><span class="sxs-lookup"><span data-stu-id="e1478-273">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="e1478-274">Если файл *web.config* отсутствует и файл проекта содержит `<Project Sdk="Microsoft.NET.Sdk.Web">`, при публикации проекта файл *web.config* создается в опубликованных выходных данных (папка *publish*).</span><span class="sxs-lookup"><span data-stu-id="e1478-274">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="e1478-275">Дополнительные сведения см. в разделе [Размещение ASP.NET Core в Windows со службами IIS](xref:host-and-deploy/iis/index#webconfig).</span><span class="sxs-lookup"><span data-stu-id="e1478-275">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig).</span></span>

## <a name="accessing-configuration-during-startup"></a><span data-ttu-id="e1478-276">Доступ к конфигурации во время запуска</span><span class="sxs-lookup"><span data-stu-id="e1478-276">Accessing configuration during startup</span></span>

<span data-ttu-id="e1478-277">Чтобы получить доступ к конфигурации в `ConfigureServices` или `Configure` во время запуска, см. примеры в разделе [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="e1478-277">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="e1478-278">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="e1478-278">Additional notes</span></span>

* <span data-ttu-id="e1478-279">Внедрение зависимостей (DI) настраивается после вызова `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e1478-279">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="e1478-280">Система конфигурации не поддерживает внедрение зависимостей.</span><span class="sxs-lookup"><span data-stu-id="e1478-280">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="e1478-281">`IConfiguration` имеет две специализации:</span><span class="sxs-lookup"><span data-stu-id="e1478-281">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="e1478-282">`IConfigurationRoot` используется для корневого узла.</span><span class="sxs-lookup"><span data-stu-id="e1478-282">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="e1478-283">Может инициировать перезагрузку.</span><span class="sxs-lookup"><span data-stu-id="e1478-283">Can trigger a reload.</span></span>
  * <span data-ttu-id="e1478-284">`IConfigurationSection` представляет раздел значений конфигурации.</span><span class="sxs-lookup"><span data-stu-id="e1478-284">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="e1478-285">Методы `GetSection` и `GetChildren` возвращают `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="e1478-285">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="e1478-286">Используйте [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) при перезагрузке конфигурации или для доступа к каждому поставщику.</span><span class="sxs-lookup"><span data-stu-id="e1478-286">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="e1478-287">Оба этих случая нетипичные.</span><span class="sxs-lookup"><span data-stu-id="e1478-287">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e1478-288">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e1478-288">Additional resources</span></span>

* [<span data-ttu-id="e1478-289">Параметры</span><span class="sxs-lookup"><span data-stu-id="e1478-289">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="e1478-290">Работа с несколькими средами</span><span class="sxs-lookup"><span data-stu-id="e1478-290">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="e1478-291">Безопасное хранение секретов приложений во время разработки</span><span class="sxs-lookup"><span data-stu-id="e1478-291">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="e1478-292">Размещение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e1478-292">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="e1478-293">Введение зависимостей</span><span class="sxs-lookup"><span data-stu-id="e1478-293">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="e1478-294">Поставщик конфигурации Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e1478-294">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
