---
title: "Конфигурация в .NET Core"
author: rick-anderson
description: "Используйте API конфигурации для настройки приложения ASP.NET Core несколькими способами."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/index
ms.openlocfilehash: b662e66ab5b4c46d1a8d10eb7c38bf4064b5b927
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2018
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="a26cc-103">Настройка приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a26cc-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="a26cc-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson), [Марк Михаэлис](http://intellitect.com/author/mark-michaelis/) (Mark Michaelis), [Стив Смит](https://ardalis.com/) (Steve Smith), [Даниэль Рот](https://github.com/danroth27) (Daniel Roth) и [Люк Лэтхэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="a26cc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a26cc-105">API конфигурации позволяет настраивать веб-приложения ASP.NET Core на основе списка пар "имя-значение".</span><span class="sxs-lookup"><span data-stu-id="a26cc-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="a26cc-106">Конфигурация считывается во время выполнения из нескольких источников.</span><span class="sxs-lookup"><span data-stu-id="a26cc-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="a26cc-107">Пары "имя-значение" можно сгруппировать в многоуровневую иерархию.</span><span class="sxs-lookup"><span data-stu-id="a26cc-107">You can group these name-value pairs into a multi-level hierarchy.</span></span> 

<span data-ttu-id="a26cc-108">Существуют следующие поставщики конфигурации:</span><span class="sxs-lookup"><span data-stu-id="a26cc-108">There are configuration providers for:</span></span>

* <span data-ttu-id="a26cc-109">форматов файлов (INI, JSON и XML);</span><span class="sxs-lookup"><span data-stu-id="a26cc-109">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="a26cc-110">аргументов командной строки;</span><span class="sxs-lookup"><span data-stu-id="a26cc-110">Command-line arguments</span></span>
* <span data-ttu-id="a26cc-111">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="a26cc-111">Environment variables</span></span>
* <span data-ttu-id="a26cc-112">объектов .NET в памяти;</span><span class="sxs-lookup"><span data-stu-id="a26cc-112">In-memory .NET objects</span></span>
* <span data-ttu-id="a26cc-113">зашифрованного пользовательского хранилища;</span><span class="sxs-lookup"><span data-stu-id="a26cc-113">An encrypted user store</span></span>
* <span data-ttu-id="a26cc-114">[Azure Key Vault](xref:security/key-vault-configuration);</span><span class="sxs-lookup"><span data-stu-id="a26cc-114">[Azure Key Vault](xref:security/key-vault-configuration)</span></span>
* <span data-ttu-id="a26cc-115">а также пользовательские поставщики (устанавливаемые или создаваемые).</span><span class="sxs-lookup"><span data-stu-id="a26cc-115">Custom providers (installed or created)</span></span>

<span data-ttu-id="a26cc-116">Каждое значение конфигурации сопоставляется строковому ключу.</span><span class="sxs-lookup"><span data-stu-id="a26cc-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="a26cc-117">Имеется встроенная поддержка привязки для десериализации параметров в пользовательский объект [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) (простой класс .NET со свойствами).</span><span class="sxs-lookup"><span data-stu-id="a26cc-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="a26cc-118">Шаблон параметров использует классы параметров для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="a26cc-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="a26cc-119">Дополнительные сведения об использовании шаблона параметров см. в разделе [Параметры](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="a26cc-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="a26cc-120">[Просмотреть или скачать образец кода](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a26cc-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="a26cc-121">Конфигурация JSON</span><span class="sxs-lookup"><span data-stu-id="a26cc-121">JSON configuration</span></span>

<span data-ttu-id="a26cc-122">В следующем консольном приложении используется поставщик конфигурации JSON:</span><span class="sxs-lookup"><span data-stu-id="a26cc-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="a26cc-123">Приложение считывает и отображает следующие параметры конфигурации приложения:</span><span class="sxs-lookup"><span data-stu-id="a26cc-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="a26cc-124">Конфигурация — это иерархический список пар "имя-значение", в котором узлы разделяются двоеточием.</span><span class="sxs-lookup"><span data-stu-id="a26cc-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="a26cc-125">Чтобы получить значение, обратитесь к индексатору `Configuration` с ключом соответствующего элемента:</span><span class="sxs-lookup"><span data-stu-id="a26cc-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="a26cc-126">Для работы с массивами в источниках конфигурации в формате JSON используйте индекс массива в составе строки с разделителем-двоеточием.</span><span class="sxs-lookup"><span data-stu-id="a26cc-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="a26cc-127">В следующем примере возвращается имя первого элемента в предыдущем массиве `wizards`:</span><span class="sxs-lookup"><span data-stu-id="a26cc-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="a26cc-128">Пары "имя-значение", записанные во встроенные поставщики `Configuration`, **не** сохраняются.</span><span class="sxs-lookup"><span data-stu-id="a26cc-128">Name-value pairs written to the built-in `Configuration` providers are **not** persisted.</span></span> <span data-ttu-id="a26cc-129">Однако можно создать пользовательский поставщик, который сохраняет значения.</span><span class="sxs-lookup"><span data-stu-id="a26cc-129">However, you can create a custom provider that saves values.</span></span> <span data-ttu-id="a26cc-130">См. раздел [Пользовательский поставщик конфигурации](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="a26cc-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="a26cc-131">В предыдущем примере для считывания значений используется индексатор конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a26cc-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="a26cc-132">Для доступа к конфигурации вне `Startup` воспользуйтесь *шаблоном параметров*.</span><span class="sxs-lookup"><span data-stu-id="a26cc-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="a26cc-133">Дополнительные сведения см. в разделе [Параметры](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="a26cc-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="a26cc-134">Как правило, для разных сред (например, разработки, тестирования и производства) существуют разные параметры конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a26cc-134">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="a26cc-135">Метод расширения `CreateDefaultBuilder` в приложении ASP.NET Core 2.x (или при использовании `AddJsonFile` и `AddEnvironmentVariables` непосредственно в приложении ASP.NET Core 1.x) добавляет поставщики конфигурации для чтения JSON-файлов и источников системной конфигурации:</span><span class="sxs-lookup"><span data-stu-id="a26cc-135">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="a26cc-136">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="a26cc-136">*appsettings.json*</span></span>
* <span data-ttu-id="a26cc-137">*appsettings.\<имя_среды>.json*</span><span class="sxs-lookup"><span data-stu-id="a26cc-137">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="a26cc-138">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="a26cc-138">Environment variables</span></span>

<span data-ttu-id="a26cc-139">Объяснение параметров см. в разделе [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions).</span><span class="sxs-lookup"><span data-stu-id="a26cc-139">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="a26cc-140">`reloadOnChange` поддерживается только в ASP.NET Core 1.1 и более поздних версиях.</span><span class="sxs-lookup"><span data-stu-id="a26cc-140">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span> 

<span data-ttu-id="a26cc-141">Источники конфигурации считываются в порядке их указания.</span><span class="sxs-lookup"><span data-stu-id="a26cc-141">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="a26cc-142">В приведенном выше коде переменные среды считываются последними.</span><span class="sxs-lookup"><span data-stu-id="a26cc-142">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="a26cc-143">Все значения конфигурации, заданные с помощью среды, заменяют значения, заданные в предыдущих двух поставщиках.</span><span class="sxs-lookup"><span data-stu-id="a26cc-143">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="a26cc-144">Для среды обычно задаются значения `Development`, `Staging` или `Production`.</span><span class="sxs-lookup"><span data-stu-id="a26cc-144">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="a26cc-145">Дополнительные сведения см. в статье [Работа с несколькими средами](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="a26cc-145">See [Working with multiple environments](xref:fundamentals/environments) for more information.</span></span>

<span data-ttu-id="a26cc-146">Рекомендации по конфигурации:</span><span class="sxs-lookup"><span data-stu-id="a26cc-146">Configuration considerations:</span></span>

* <span data-ttu-id="a26cc-147">`IOptionsSnapshot` может перезагрузить данные конфигурации после их изменения.</span><span class="sxs-lookup"><span data-stu-id="a26cc-147">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="a26cc-148">Дополнительные сведения см. в разделе [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="a26cc-148">See [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="a26cc-149">Ключи конфигурации зависят от регистра.</span><span class="sxs-lookup"><span data-stu-id="a26cc-149">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="a26cc-150">Указывайте переменные среды последними, чтобы локальная среда могла переопределять параметры в развернутых файлах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a26cc-150">Specify environment variables last so that the local environment can override settings in deployed configuration files.</span></span>
* <span data-ttu-id="a26cc-151">**Никогда** не храните пароли и другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации в виде обычного текста.</span><span class="sxs-lookup"><span data-stu-id="a26cc-151">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="a26cc-152">Не используйте секреты производства в средах разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="a26cc-152">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="a26cc-153">Указывайте секретные данные вне проекта, чтобы их нельзя было случайно зафиксировать в репозитории.</span><span class="sxs-lookup"><span data-stu-id="a26cc-153">Instead, specify secrets outside of the project so that they can't be accidentally committed to your repository.</span></span> <span data-ttu-id="a26cc-154">Узнайте больше о [работе с несколькими средами](xref:fundamentals/environments) и управлении [безопасным хранением секретов приложения во время разработки](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="a26cc-154">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="a26cc-155">Если двоеточие (`:`) не может быть используется в переменных среды в вашей системе, замените двоеточием (`:`) с двойным подчеркиванием (`__`).</span><span class="sxs-lookup"><span data-stu-id="a26cc-155">If a colon (`:`) can't be used in environment variables on your system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="a26cc-156">Поставщик в памяти и привязка к классу POCO</span><span class="sxs-lookup"><span data-stu-id="a26cc-156">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="a26cc-157">В следующем примере демонстрируется использование поставщика в памяти и привязка к классу:</span><span class="sxs-lookup"><span data-stu-id="a26cc-157">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

<span data-ttu-id="a26cc-158">Значения конфигурации возвращаются в виде строк, но с помощью привязки можно создавать объекты.</span><span class="sxs-lookup"><span data-stu-id="a26cc-158">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="a26cc-159">Привязка позволяет получать объекты POCO или даже графы объектов.</span><span class="sxs-lookup"><span data-stu-id="a26cc-159">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="a26cc-160">GetValue</span><span class="sxs-lookup"><span data-stu-id="a26cc-160">GetValue</span></span>

<span data-ttu-id="a26cc-161">В следующем примере показан метод расширения [GetValue&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_):</span><span class="sxs-lookup"><span data-stu-id="a26cc-161">The following sample demonstrates the [GetValue&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

<span data-ttu-id="a26cc-162">Метод `GetValue<T>` ConfigurationBinder позволяет задавать значение по умолчанию (в примере — 80).</span><span class="sxs-lookup"><span data-stu-id="a26cc-162">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="a26cc-163">Метод `GetValue<T>` предназначен для простых сценариях и не выполняет привязку ко всем разделам.</span><span class="sxs-lookup"><span data-stu-id="a26cc-163">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="a26cc-164">Метод `GetValue<T>` возвращает скалярные значения из `GetSection(key).Value`, преобразованного в конкретный тип.</span><span class="sxs-lookup"><span data-stu-id="a26cc-164">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="a26cc-165">Привязка к графу объектов</span><span class="sxs-lookup"><span data-stu-id="a26cc-165">Bind to an object graph</span></span>

<span data-ttu-id="a26cc-166">Можно реализовать рекурсивную привязку к каждому объекту в классе.</span><span class="sxs-lookup"><span data-stu-id="a26cc-166">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="a26cc-167">Рассмотрим следующий класс `AppSettings`:</span><span class="sxs-lookup"><span data-stu-id="a26cc-167">Consider the following `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="a26cc-168">В следующем примере выполняется привязка к классу `AppSettings`:</span><span class="sxs-lookup"><span data-stu-id="a26cc-168">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="a26cc-169">В **ASP.NET Core 1.1** и более поздних версиях можно использовать метод `Get<T>`, который работает со всеми разделами.</span><span class="sxs-lookup"><span data-stu-id="a26cc-169">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="a26cc-170">Метод `Get<T>` может быть более удобным, чем `Bind`.</span><span class="sxs-lookup"><span data-stu-id="a26cc-170">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="a26cc-171">В следующем коде демонстрируется применение метода `Get<T>` с примером выше.</span><span class="sxs-lookup"><span data-stu-id="a26cc-171">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="a26cc-172">Используется следующий файл *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a26cc-172">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="a26cc-173">Программа выводит `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="a26cc-173">The program displays `Height 11`.</span></span>

<span data-ttu-id="a26cc-174">Для модульного тестирования конфигурации можно выполнить следующий код:</span><span class="sxs-lookup"><span data-stu-id="a26cc-174">The following code can be used to unit test the configuration:</span></span>

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

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="a26cc-175">Создание пользовательского поставщика Entity Framework</span><span class="sxs-lookup"><span data-stu-id="a26cc-175">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="a26cc-176">В этом разделе создается поставщик базовой конфигурации, который считывает пары "имя-значение" из базы данных с помощью EF.</span><span class="sxs-lookup"><span data-stu-id="a26cc-176">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="a26cc-177">Определите сущность `ConfigurationValue` для хранения значений конфигурации в базе данных:</span><span class="sxs-lookup"><span data-stu-id="a26cc-177">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="a26cc-178">Добавьте `ConfigurationContext` в хранилище и обратитесь к настроенным значениям:</span><span class="sxs-lookup"><span data-stu-id="a26cc-178">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="a26cc-179">Создайте класс, реализующий [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span><span class="sxs-lookup"><span data-stu-id="a26cc-179">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="a26cc-180">Создайте пользовательский поставщик конфигурации путем наследования от [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span><span class="sxs-lookup"><span data-stu-id="a26cc-180">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="a26cc-181">Поставщик конфигурации инициализирует пустую базу данных:</span><span class="sxs-lookup"><span data-stu-id="a26cc-181">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="a26cc-182">При выполнении примера отображаются выделенные значения из базы данных ("value_from_ef_1" и "value_from_ef_2").</span><span class="sxs-lookup"><span data-stu-id="a26cc-182">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="a26cc-183">Можно добавить метод расширения `EFConfigSource` для добавления источник конфигурации:</span><span class="sxs-lookup"><span data-stu-id="a26cc-183">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="a26cc-184">В следующем примере кода демонстрируется использование настраиваемого `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="a26cc-184">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="a26cc-185">Обратите внимание, что в этом примере пользовательский `EFConfigProvider` добавляется после поставщика JSON, поэтому все параметры из базы данных переопределяют параметры из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a26cc-185">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="a26cc-186">Используется следующий файл *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a26cc-186">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="a26cc-187">Отобразится следующее:</span><span class="sxs-lookup"><span data-stu-id="a26cc-187">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="a26cc-188">Поставщик конфигурации CommandLine</span><span class="sxs-lookup"><span data-stu-id="a26cc-188">CommandLine configuration provider</span></span>

<span data-ttu-id="a26cc-189">[Поставщик конфигурации CommandLine](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) получает пары "ключ-значение" аргумента командной строки для конфигурации во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="a26cc-189">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

<span data-ttu-id="a26cc-190">[Просмотрите или скачайте пример конфигурации CommandLine](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="a26cc-190">[View or download the CommandLine configuration sample](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)</span></span>

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="a26cc-191">Настройка и использование поставщика конфигурации CommandLine</span><span class="sxs-lookup"><span data-stu-id="a26cc-191">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="a26cc-192">Базовая конфигурация</span><span class="sxs-lookup"><span data-stu-id="a26cc-192">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="a26cc-193">Чтобы активировать конфигурацию командной строки, вызовите метод расширения `AddCommandLine` в экземпляре [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="a26cc-193">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="a26cc-194">После выполнения кода отобразятся следующие выходные данные:</span><span class="sxs-lookup"><span data-stu-id="a26cc-194">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="a26cc-195">Передача пар "ключ-значение" аргументов в командной строке приведет к изменению значений `Profile:MachineName` и `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="a26cc-195">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="a26cc-196">В окне консоли отображается следующее:</span><span class="sxs-lookup"><span data-stu-id="a26cc-196">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="a26cc-197">Чтобы переопределить конфигурацию, предоставленную другими поставщиками конфигурации, конфигурацией командной строки, вызовите `AddCommandLine` последним в `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="a26cc-197">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a26cc-198">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a26cc-198">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a26cc-199">Для создания узла стандартные приложения ASP.NET Core 2.x используют статический удобный метод `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="a26cc-199">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="a26cc-200">`CreateDefaultBuilder` загружает дополнительную конфигурацию из *appsettings.json*, *appsettings.{среда}.json*, [секреты пользователя](xref:security/app-secrets) (в среде `Development`), переменные среды и аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="a26cc-200">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="a26cc-201">Поставщик конфигурации CommandLine вызывается последним.</span><span class="sxs-lookup"><span data-stu-id="a26cc-201">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="a26cc-202">В этом случае аргументы командной строки, передаваемые во время выполнения, переопределяют конфигурацию, заданную другими вызванными ранее поставщиками конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a26cc-202">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="a26cc-203">Обратите внимание, что `reloadOnChange` включен для файлов *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="a26cc-203">Note that for *appsettings* files that `reloadOnChange` is enabled.</span></span> <span data-ttu-id="a26cc-204">Аргументы командной строки переопределяются в том случае, если после запуска приложения изменяется совпадающее значение конфигурации в файле *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="a26cc-204">Command-line arguments are overridden if a matching configuration value in an *appsettings* file is changed after the app starts.</span></span>

> [!NOTE]
> <span data-ttu-id="a26cc-205">В качестве альтернативы использования метода `CreateDefaultBuilder` в ASP.NET Core 2.x поддерживается создание узла с помощью [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) и формирование конфигурации вручную с помощью [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span><span class="sxs-lookup"><span data-stu-id="a26cc-205">As an alternative to using the `CreateDefaultBuilder` method, creating a host using [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) and manually building configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) is supported in ASP.NET Core 2.x.</span></span> <span data-ttu-id="a26cc-206">Дополнительные сведения см. в разделе о ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="a26cc-206">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a26cc-207">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a26cc-207">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a26cc-208">Чтобы использовать поставщик конфигурации CommandLine, создайте [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) и вызовите метод `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="a26cc-208">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="a26cc-209">В этом случае аргументы командной строки, передаваемые во время выполнения, переопределяют конфигурацию, заданную другими вызванными ранее поставщиками конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a26cc-209">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="a26cc-210">Примените конфигурацию к [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) с помощью метода `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="a26cc-210">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="a26cc-211">Аргументы</span><span class="sxs-lookup"><span data-stu-id="a26cc-211">Arguments</span></span>

<span data-ttu-id="a26cc-212">Аргументы, передаваемые в командной строке, должны соответствовать одному из двух форматов, приведенных в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="a26cc-212">Arguments passed on the command line must conform to one of two formats shown in the following table.</span></span>

| <span data-ttu-id="a26cc-213">Формат аргумента</span><span class="sxs-lookup"><span data-stu-id="a26cc-213">Argument format</span></span>                                                     | <span data-ttu-id="a26cc-214">Пример</span><span class="sxs-lookup"><span data-stu-id="a26cc-214">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="a26cc-215">Один аргумент: пара "ключ-значение", разделенная знаком равенства (`=`)</span><span class="sxs-lookup"><span data-stu-id="a26cc-215">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="a26cc-216">Последовательность из двух аргументов: пара "ключ-значение", разделенная пробелом</span><span class="sxs-lookup"><span data-stu-id="a26cc-216">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="a26cc-217">**Один аргумент**</span><span class="sxs-lookup"><span data-stu-id="a26cc-217">**Single argument**</span></span>

<span data-ttu-id="a26cc-218">Значение должно стоять после знака равенства (`=`).</span><span class="sxs-lookup"><span data-stu-id="a26cc-218">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="a26cc-219">Значение может быть равно NULL (например, `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="a26cc-219">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="a26cc-220">Ключ может иметь префикс.</span><span class="sxs-lookup"><span data-stu-id="a26cc-220">The key may have a prefix.</span></span>

| <span data-ttu-id="a26cc-221">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="a26cc-221">Key prefix</span></span>               | <span data-ttu-id="a26cc-222">Пример</span><span class="sxs-lookup"><span data-stu-id="a26cc-222">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="a26cc-223">Без префикса</span><span class="sxs-lookup"><span data-stu-id="a26cc-223">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="a26cc-224">Один дефис (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="a26cc-224">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="a26cc-225">Два дефиса (`--`)</span><span class="sxs-lookup"><span data-stu-id="a26cc-225">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="a26cc-226">Прямая косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="a26cc-226">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="a26cc-227">&#8224;Ключ с префиксом из одного дефиса (`-`) должен быть указан в [сопоставлениях переключений](#switch-mappings), описанных ниже.</span><span class="sxs-lookup"><span data-stu-id="a26cc-227">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="a26cc-228">Пример команды:</span><span class="sxs-lookup"><span data-stu-id="a26cc-228">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="a26cc-229">Примечание. Если `-key1` отсутствует в [сопоставлениях переключений](#switch-mappings), переданных поставщику конфигурации, возникает исключение `FormatException`.</span><span class="sxs-lookup"><span data-stu-id="a26cc-229">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="a26cc-230">**Последовательность из двух аргументов**</span><span class="sxs-lookup"><span data-stu-id="a26cc-230">**Sequence of two arguments**</span></span>

<span data-ttu-id="a26cc-231">Значение не может быть равно NULL и должно следовать за ключом, разделенным пробелом.</span><span class="sxs-lookup"><span data-stu-id="a26cc-231">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="a26cc-232">Ключ должен иметь префикс.</span><span class="sxs-lookup"><span data-stu-id="a26cc-232">The key must have a prefix.</span></span>

| <span data-ttu-id="a26cc-233">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="a26cc-233">Key prefix</span></span>               | <span data-ttu-id="a26cc-234">Пример</span><span class="sxs-lookup"><span data-stu-id="a26cc-234">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="a26cc-235">Один дефис (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="a26cc-235">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="a26cc-236">Два дефиса (`--`)</span><span class="sxs-lookup"><span data-stu-id="a26cc-236">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="a26cc-237">Прямая косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="a26cc-237">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="a26cc-238">&#8224;Ключ с префиксом из одного дефиса (`-`) должен быть указан в [сопоставлениях переключений](#switch-mappings), описанных ниже.</span><span class="sxs-lookup"><span data-stu-id="a26cc-238">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="a26cc-239">Пример команды:</span><span class="sxs-lookup"><span data-stu-id="a26cc-239">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="a26cc-240">Примечание. Если `-key1` отсутствует в [сопоставлениях переключений](#switch-mappings), переданных поставщику конфигурации, возникает исключение `FormatException`.</span><span class="sxs-lookup"><span data-stu-id="a26cc-240">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="a26cc-241">Повторяющиеся ключи</span><span class="sxs-lookup"><span data-stu-id="a26cc-241">Duplicate keys</span></span>

<span data-ttu-id="a26cc-242">Если указаны повторяющиеся ключи, используется последняя пара "ключ-значение".</span><span class="sxs-lookup"><span data-stu-id="a26cc-242">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="a26cc-243">Сопоставления переключений</span><span class="sxs-lookup"><span data-stu-id="a26cc-243">Switch mappings</span></span>

<span data-ttu-id="a26cc-244">При построении конфигурации вручную с помощью `ConfigurationBuilder` при необходимости можно предоставить методу `AddCommandLine` словарь сопоставления переключений.</span><span class="sxs-lookup"><span data-stu-id="a26cc-244">When manually building configuration with `ConfigurationBuilder`, you can optionally provide a switch mappings dictionary to the `AddCommandLine` method.</span></span> <span data-ttu-id="a26cc-245">Сопоставления переключений позволяют указать логику замены имени ключа.</span><span class="sxs-lookup"><span data-stu-id="a26cc-245">Switch mappings allow you to provide key name replacement logic.</span></span>

<span data-ttu-id="a26cc-246">В словаре сопоставлений переключений выполняется поиск ключа, который совпадает с ключом, предоставляемым аргументом командной строки.</span><span class="sxs-lookup"><span data-stu-id="a26cc-246">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="a26cc-247">Если ключ командной строки найден, значение словаря (новый ключ) передается обратно, чтобы задать конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="a26cc-247">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="a26cc-248">Сопоставление переключений необходимо для любого ключа командной строки с префиксом из одного дефиса (`-`).</span><span class="sxs-lookup"><span data-stu-id="a26cc-248">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="a26cc-249">Правила ключей из словаря сопоставления переключений:</span><span class="sxs-lookup"><span data-stu-id="a26cc-249">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="a26cc-250">Переключения должны начинаться с дефиса (`-`) или двойного дефиса (`--`).</span><span class="sxs-lookup"><span data-stu-id="a26cc-250">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="a26cc-251">Словарь сопоставлений переключений не должен содержать повторяющиеся ключи.</span><span class="sxs-lookup"><span data-stu-id="a26cc-251">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="a26cc-252">В следующем примере метод `GetSwitchMappings` позволяет аргументам командной строки использовать префикс ключа с одним дефисом (`-`) и исключает начальные префиксы подключа.</span><span class="sxs-lookup"><span data-stu-id="a26cc-252">In the following example, the `GetSwitchMappings` method allows your command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="a26cc-253">Если аргументы командной строки не указаны, словарь для `AddInMemoryCollection` задает значения конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a26cc-253">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="a26cc-254">Запустите приложение с помощью следующей команды:</span><span class="sxs-lookup"><span data-stu-id="a26cc-254">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="a26cc-255">В окне консоли отображается следующее:</span><span class="sxs-lookup"><span data-stu-id="a26cc-255">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="a26cc-256">Используйте следующее для передачи параметров конфигурации:</span><span class="sxs-lookup"><span data-stu-id="a26cc-256">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="a26cc-257">В окне консоли отображается следующее:</span><span class="sxs-lookup"><span data-stu-id="a26cc-257">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="a26cc-258">Созданный словарь сопоставлений переключений содержит данные, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="a26cc-258">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="a26cc-259">Ключ</span><span class="sxs-lookup"><span data-stu-id="a26cc-259">Key</span></span>            | <span data-ttu-id="a26cc-260">Значение</span><span class="sxs-lookup"><span data-stu-id="a26cc-260">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="a26cc-261">Чтобы продемонстрировать переключение ключей с помощью словаря, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="a26cc-261">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="a26cc-262">Ключи командной строки меняются.</span><span class="sxs-lookup"><span data-stu-id="a26cc-262">The command-line keys are swapped.</span></span> <span data-ttu-id="a26cc-263">В окне консоли отображаются значения конфигурации для `Profile:MachineName` и `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="a26cc-263">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="a26cc-264">Файл web.config</span><span class="sxs-lookup"><span data-stu-id="a26cc-264">The web.config file</span></span>

<span data-ttu-id="a26cc-265">Файл *web.config* необходим для размещения приложения в службах IIS или IIS Express.</span><span class="sxs-lookup"><span data-stu-id="a26cc-265">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="a26cc-266">Параметры в файле *web.config* включают [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) для запуска приложения и настройки других параметров и модулей IIS.</span><span class="sxs-lookup"><span data-stu-id="a26cc-266">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="a26cc-267">Если файл *web.config* отсутствует и файл проекта содержит `<Project Sdk="Microsoft.NET.Sdk.Web">`, при публикации проекта файл *web.config* создается в опубликованных выходных данных (папка *publish*).</span><span class="sxs-lookup"><span data-stu-id="a26cc-267">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="a26cc-268">Дополнительные сведения см. в разделе [Размещение ASP.NET Core в Windows со службами IIS](xref:host-and-deploy/iis/index#webconfig).</span><span class="sxs-lookup"><span data-stu-id="a26cc-268">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig).</span></span>

## <a name="additional-notes"></a><span data-ttu-id="a26cc-269">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="a26cc-269">Additional notes</span></span>

* <span data-ttu-id="a26cc-270">Внедрение зависимостей (DI) настраивается после вызова `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a26cc-270">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="a26cc-271">Система конфигурации не поддерживает внедрение зависимостей.</span><span class="sxs-lookup"><span data-stu-id="a26cc-271">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="a26cc-272">`IConfiguration` имеет две специализации:</span><span class="sxs-lookup"><span data-stu-id="a26cc-272">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="a26cc-273">`IConfigurationRoot` используется для корневого узла.</span><span class="sxs-lookup"><span data-stu-id="a26cc-273">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="a26cc-274">Может инициировать перезагрузку.</span><span class="sxs-lookup"><span data-stu-id="a26cc-274">Can trigger a reload.</span></span>
  * <span data-ttu-id="a26cc-275">`IConfigurationSection` представляет раздел значений конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a26cc-275">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="a26cc-276">Методы `GetSection` и `GetChildren` возвращают `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="a26cc-276">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a26cc-277">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a26cc-277">Additional resources</span></span>

* [<span data-ttu-id="a26cc-278">Параметры</span><span class="sxs-lookup"><span data-stu-id="a26cc-278">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="a26cc-279">Работа с несколькими средами</span><span class="sxs-lookup"><span data-stu-id="a26cc-279">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="a26cc-280">Безопасное хранение секретов приложений во время разработки</span><span class="sxs-lookup"><span data-stu-id="a26cc-280">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="a26cc-281">Размещение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a26cc-281">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="a26cc-282">Введение зависимостей</span><span class="sxs-lookup"><span data-stu-id="a26cc-282">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="a26cc-283">Поставщик конфигурации Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="a26cc-283">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
