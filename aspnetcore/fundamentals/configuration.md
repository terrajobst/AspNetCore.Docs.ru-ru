---
title: "Конфигурация в .NET Core"
author: rick-anderson
description: "Узнайте, как использовать API конфигурации для настройки приложения ASP.NET Core из нескольких источников."
keywords: "ASP.NET Core, конфигурации JSON, конфигурации"
ms.author: riande
manager: wpickett
ms.date: 6/24/2017
ms.topic: article
ms.assetid: b3a5984d-e172-42eb-8a48-547e4acb6806
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration
ms.openlocfilehash: 379030df4ca91a38fce251aeaab9c5dfaf11e915
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2017
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="bf1dd-104">Конфигурация в .NET Core</span><span class="sxs-lookup"><span data-stu-id="bf1dd-104">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="bf1dd-105">[Рик Андерсон](https://twitter.com/RickAndMSFT), [Марка Михаэлиса](http://intellitect.com/author/mark-michaelis/), [Стив Смит](https://ardalis.com/), [рот Daniel](https://github.com/danroth27), и [Latham Люк](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bf1dd-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="bf1dd-106">API конфигурации предоставляет способ настройки приложения на основе списка пар «имя значение».</span><span class="sxs-lookup"><span data-stu-id="bf1dd-106">The Configuration API provides a way of configuring an app based on a list of name-value pairs.</span></span> <span data-ttu-id="bf1dd-107">Конфигурация для чтения во время выполнения из нескольких источников.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-107">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="bf1dd-108">Пары «имя значение» могут быть сгруппированы в многоуровневой иерархии.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-108">The name-value pairs can be grouped into a multi-level hierarchy.</span></span> <span data-ttu-id="bf1dd-109">Нет поставщиков конфигурации для:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-109">There are configuration providers for:</span></span>

* <span data-ttu-id="bf1dd-110">Форматы файлов (ini-ФАЙЛЕ, JSON и XML)</span><span class="sxs-lookup"><span data-stu-id="bf1dd-110">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="bf1dd-111">Аргументы командной строки</span><span class="sxs-lookup"><span data-stu-id="bf1dd-111">Command-line arguments</span></span>
* <span data-ttu-id="bf1dd-112">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="bf1dd-112">Environment variables</span></span>
* <span data-ttu-id="bf1dd-113">Объекты в памяти .NET</span><span class="sxs-lookup"><span data-stu-id="bf1dd-113">In-memory .NET objects</span></span>
* <span data-ttu-id="bf1dd-114">Зашифрованные пользовательского хранилища</span><span class="sxs-lookup"><span data-stu-id="bf1dd-114">An encrypted user store</span></span>
* [<span data-ttu-id="bf1dd-115">Хранилище ключей Azure</span><span class="sxs-lookup"><span data-stu-id="bf1dd-115">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="bf1dd-116">Пользовательские поставщики, которые можно установить или создать</span><span class="sxs-lookup"><span data-stu-id="bf1dd-116">Custom providers, which you install or create</span></span>

<span data-ttu-id="bf1dd-117">Каждое значение конфигурации сопоставляет строковый ключ.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-117">Each configuration value maps to a string key.</span></span> <span data-ttu-id="bf1dd-118">Поддержка встроенных привязок для десериализации параметров в пользовательский [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) объекта (простой класс .NET со свойствами).</span><span class="sxs-lookup"><span data-stu-id="bf1dd-118">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

[<span data-ttu-id="bf1dd-119">Просмотреть или скачать образец кода</span><span class="sxs-lookup"><span data-stu-id="bf1dd-119">View or download sample code</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)

## <a name="simple-configuration"></a><span data-ttu-id="bf1dd-120">Простая настройка</span><span class="sxs-lookup"><span data-stu-id="bf1dd-120">Simple configuration</span></span>

<span data-ttu-id="bf1dd-121">Следующее приложение консоли использует поставщик конфигурации JSON:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-121">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]

<span data-ttu-id="bf1dd-122">Считывает и отображает следующие параметры конфигурации приложения:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-122">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="bf1dd-123">Конфигурация состоит из иерархический список пар "имя значение", в которых узлы разделяются точкой с запятой.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-123">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="bf1dd-124">Для получения значения, доступ к `Configuration` индексатора с ключом соответствующего элемента:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-124">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="bf1dd-125">Для работы с массивами в источниках конфигурации в формате JSON, используйте индекс массива как часть строки двоеточием.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-125">To work with arrays in JSON-formatted configuration sources, use a array index as part of the colon-separated string.</span></span> <span data-ttu-id="bf1dd-126">В следующем примере возвращается имя первого элемента в предыдущем `wizards` массива:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-126">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="bf1dd-127">Пары имя значение, записанных во встроенную в `Configuration` поставщики **не** сохраняются, однако можно создать пользовательский поставщик, который сохраняет значения.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-127">Name-value pairs written to the built in `Configuration` providers are **not** persisted, however, you can create a custom provider that saves values.</span></span> <span data-ttu-id="bf1dd-128">В разделе [настраиваемый поставщик конфигурации](xref:fundamentals/configuration#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="bf1dd-128">See [custom configuration provider](xref:fundamentals/configuration#custom-config-providers).</span></span>

<span data-ttu-id="bf1dd-129">Предыдущий пример использует конфигурации индексатора для считывания значений.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-129">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="bf1dd-130">Для настройки доступа извне `Startup`, используйте [параметры шаблона](xref:fundamentals/configuration#options-config-objects).</span><span class="sxs-lookup"><span data-stu-id="bf1dd-130">To access configuration outside of `Startup`, use the [options pattern](xref:fundamentals/configuration#options-config-objects).</span></span> <span data-ttu-id="bf1dd-131">*Параметры шаблона* показано далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-131">The *options pattern* is shown later in this article.</span></span>

<span data-ttu-id="bf1dd-132">Это обычно имеют разные параметры конфигурации для разных сред, например, разработки, тестирования и производства.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-132">It's typical to have different configuration settings for different environments, for example, development, test, and production.</span></span> <span data-ttu-id="bf1dd-133">`CreateDefaultBuilder` Метод расширения в приложении ASP.NET Core 2.x (или с помощью `AddJsonFile` и `AddEnvironmentVariables` непосредственно в приложении ASP.NET Core 1.x) добавляет поставщиков конфигурации для чтения файлы JSON и системные настройки источников:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-133">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="bf1dd-134">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="bf1dd-134">*appsettings.json*</span></span>
* <span data-ttu-id="bf1dd-135">*appSettings. \<EnvironmentName > .json*</span><span class="sxs-lookup"><span data-stu-id="bf1dd-135">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="bf1dd-136">переменные среды</span><span class="sxs-lookup"><span data-stu-id="bf1dd-136">environment variables</span></span>

<span data-ttu-id="bf1dd-137">В разделе [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) объяснение параметров.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-137">See [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="bf1dd-138">`reloadOnChange`поддерживается только в ASP.NET Core 1.1 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-138">`reloadOnChange` is only supported in ASP.NET Core 1.1 and higher.</span></span> 

<span data-ttu-id="bf1dd-139">Источники конфигурации считываются в порядке, в котором они указаны.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-139">Configuration sources are read in the order they are specified.</span></span> <span data-ttu-id="bf1dd-140">В приведенном выше коде считываются последнего переменные среды.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-140">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="bf1dd-141">Заменить все значения конфигурации задать с помощью среды были заданы в предыдущих двух поставщиков.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-141">Any configuration values set through the environment would replace those set in the two previous providers.</span></span>

<span data-ttu-id="bf1dd-142">Среды обычно имеет одно из `Development`, `Staging`, или `Production`.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-142">The environment is typically set to one of `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="bf1dd-143">В разделе [работа с несколькими средами](environments.md) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-143">See [Working with Multiple Environments](environments.md) for more information.</span></span>

<span data-ttu-id="bf1dd-144">Рекомендации по настройке:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-144">Configuration considerations:</span></span>

* <span data-ttu-id="bf1dd-145">`IOptionsSnapshot`можно перезагрузить данные конфигурации, при его изменении.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-145">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="bf1dd-146">Используйте `IOptionsSnapshot` Если необходимо перезагрузить данные конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-146">Use `IOptionsSnapshot` if you need to reload configuration data.</span></span>  <span data-ttu-id="bf1dd-147">В разделе [IOptionsSnapshot](#ioptionssnapshot) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-147">See [IOptionsSnapshot](#ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="bf1dd-148">Ключи конфигурации учитывается регистр.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-148">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="bf1dd-149">Рекомендуется указывать последним, переменные среды, чтобы локальной среды можно переопределить действий в файлах конфигурации, развернутые.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-149">A best practice is to specify environment variables last, so that the local environment can override anything set in deployed configuration files.</span></span>
* <span data-ttu-id="bf1dd-150">**Никогда не** хранить пароли и другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации в виде обычного текста.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-150">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="bf1dd-151">Не используйте секреты производства в разработке и тестовую среду.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-151">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="bf1dd-152">Вместо этого укажите секретные данные за пределами дереве проекта, чтобы их может быть сохранена в репозиторий случайно.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-152">Instead, specify secrets outside the project tree, so they cannot be accidentally committed into your repository.</span></span> <span data-ttu-id="bf1dd-153">Дополнительные сведения о [работа с несколькими средами](environments.md) и управление ими [безопасного хранения секрета приложения во время разработки](../security/app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="bf1dd-153">Learn more about [Working with Multiple Environments](environments.md) and managing [safe storage of app secrets during development](../security/app-secrets.md).</span></span>
* <span data-ttu-id="bf1dd-154">Если `:` не может быть использование переменных среды в системе, замените `:` с `__` (двойной символ подчеркивания).</span><span class="sxs-lookup"><span data-stu-id="bf1dd-154">If `:` cannot be used in environment variables in your system,  replace `:`  with `__` (double underscore).</span></span>

<a name=options-config-objects></a>

## <a name="using-options-and-configuration-objects"></a><span data-ttu-id="bf1dd-155">С помощью параметров и объектов конфигурации</span><span class="sxs-lookup"><span data-stu-id="bf1dd-155">Using Options and configuration objects</span></span>

<span data-ttu-id="bf1dd-156">Параметры шаблона используются классы пользовательские параметры для представления группу связанных параметров.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-156">The options pattern uses custom options classes to represent a group of related settings.</span></span> <span data-ttu-id="bf1dd-157">Рекомендуется создать несвязанной классы для каждого компонента, в приложении.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-157">We recommended that you create decoupled classes for each feature within your app.</span></span> <span data-ttu-id="bf1dd-158">Выполните несвязанной классы.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-158">Decoupled classes follow:</span></span>

* <span data-ttu-id="bf1dd-159">[Принцип разделения интерфейс (ISP)](http://deviq.com/interface-segregation-principle/) : классы зависеть только от параметров конфигурации, они используют.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-159">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/) : Classes depend only on the configuration settings they use.</span></span>
* <span data-ttu-id="bf1dd-160">[Разделение областей ответственности](http://deviq.com/separation-of-concerns/) : параметры для разных частей приложения, не зависящие от них или связанные друг с другом.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-160">[Separation of Concerns](http://deviq.com/separation-of-concerns/) : Settings for different parts of your app are not dependent or coupled with one another.</span></span>

<span data-ttu-id="bf1dd-161">Класс параметров должен быть абстрактным открытый конструктор без параметров.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-161">The options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="bf1dd-162">Пример:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-162">For example:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]

<a name=options-example></a>

<span data-ttu-id="bf1dd-163">В следующем коде включен поставщик конфигурации JSON.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-163">In the following code, the JSON configuration provider is enabled.</span></span> <span data-ttu-id="bf1dd-164">`MyOptions` Класса добавляется в контейнер службы и привязан к конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-164">The `MyOptions` class is added to the service container and bound to configuration.</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]

<span data-ttu-id="bf1dd-165">Следующие [контроллера](../mvc/controllers/index.md) использует [конструктор внедрения зависимостей](xref:fundamentals/dependency-injection#what-is-dependency-injection) на [ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) для доступа к параметрам:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-165">The following [controller](../mvc/controllers/index.md)  uses [constructor Dependency Injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) on [`IOptions<TOptions>`](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) to access settings:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="bf1dd-166">Со следующими *appsettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-166">With the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]

<span data-ttu-id="bf1dd-167">`HomeController.Index` Возвращает `option1 = value1_from_json, option2 = 2`.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-167">The `HomeController.Index` method returns `option1 = value1_from_json, option2 = 2`.</span></span>

<span data-ttu-id="bf1dd-168">Типичные приложения не привязки к файлу параметры единого вся конфигурация.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-168">Typical apps won't bind the entire configuration to a single options file.</span></span> <span data-ttu-id="bf1dd-169">Позднее будет показано как использовать `GetSection` для привязки к разделу.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-169">Later on I'll show how to use `GetSection` to bind to a section.</span></span>

<span data-ttu-id="bf1dd-170">В следующем коде секунды `IConfigureOptions<TOptions>` служба добавляется в контейнер службы.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-170">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="bf1dd-171">Она использует делегат для настройки привязки с `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-171">It uses a delegate to configure the binding with `MyOptions`.</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]

<span data-ttu-id="bf1dd-172">Можно добавить несколько поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-172">You can add multiple configuration providers.</span></span> <span data-ttu-id="bf1dd-173">Поставщики конфигурации, доступные в пакетах NuGet.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-173">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="bf1dd-174">Они применяются в порядке, в котором они зарегистрированы.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-174">They are applied in order they are registered.</span></span>

<span data-ttu-id="bf1dd-175">Каждый вызов `Configure<TOptions>` добавляет `IConfigureOptions<TOptions>` службу в контейнер службы.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-175">Each call to `Configure<TOptions>` adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="bf1dd-176">В предыдущем примере значения `Option1` и `Option2` указываются в *appsettings.json* --, но значение `Option1` переопределяется настроенных делегата.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-176">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json* -- but the value of `Option1` is overridden by the configured delegate.</span></span> 

<span data-ttu-id="bf1dd-177">При включении более одной конфигурации службы последнего источник конфигурации указано «побеждает» (задает значение конфигурации).</span><span class="sxs-lookup"><span data-stu-id="bf1dd-177">When more than one configuration service is enabled, the last configuration source specified "wins" (sets the configuration value).</span></span> <span data-ttu-id="bf1dd-178">В приведенном выше коде `HomeController.Index` возвращает `option1 = value1_from_action, option2 = 2`.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-178">In the preceding code, the `HomeController.Index` method returns `option1 = value1_from_action, option2 = 2`.</span></span>

<span data-ttu-id="bf1dd-179">При привязке параметров конфигурации, каждое свойство в типе параметры привязан ключу конфигурации формы `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-179">When you bind options to configuration, each property in your options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="bf1dd-180">Например `MyOptions.Option1` свойства, связанного с ключом `Option1`, который считывается из `option1` свойство в *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-180">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span> <span data-ttu-id="bf1dd-181">Далее в этой статье показан пример вложенного свойства.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-181">A sub-property sample is shown later in this article.</span></span>

<span data-ttu-id="bf1dd-182">В следующем коде третий `IConfigureOptions<TOptions>` служба добавляется в контейнер службы.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-182">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="bf1dd-183">Привязывает `MySubOptions` к разделу `subsection` из *appsettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-183">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]

<span data-ttu-id="bf1dd-184">Примечание: Этот метод расширения требуется `Microsoft.Extensions.Options.ConfigurationExtensions` пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-184">Note: This extension method requires the `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet package.</span></span>

<span data-ttu-id="bf1dd-185">С помощью следующих *appsettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-185">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]

<span data-ttu-id="bf1dd-186">`MySubOptions` Класса:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-186">The `MySubOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="bf1dd-187">Со следующими `Controller`:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-187">With the following `Controller`:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]

<span data-ttu-id="bf1dd-188">`subOption1 = subvalue1_from_json, subOption2 = 200`возвращается.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-188">`subOption1 = subvalue1_from_json, subOption2 = 200` is returned.</span></span>

<span data-ttu-id="bf1dd-189">Можно также предлагаемых в модель представления или вставить `IOptions<TOptions>` непосредственно в представлении:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-189">You can also supply options in a view model or inject `IOptions<TOptions>` directly into a view:</span></span>

[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]

<a name=in-memory-provider></a>

## <a name="ioptionssnapshot"></a><span data-ttu-id="bf1dd-190">IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="bf1dd-190">IOptionsSnapshot</span></span>

<span data-ttu-id="bf1dd-191">*Требуется ASP.NET Core 1.1 или более поздней версии.*</span><span class="sxs-lookup"><span data-stu-id="bf1dd-191">*Requires ASP.NET Core 1.1 or higher.*</span></span>

<span data-ttu-id="bf1dd-192">`IOptionsSnapshot`поддерживает повторной загрузки данных конфигурации при изменении файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-192">`IOptionsSnapshot` supports reloading configuration data when the configuration file has changed.</span></span> <span data-ttu-id="bf1dd-193">Он имеет минимальные издержки.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-193">It has minimal overhead.</span></span> <span data-ttu-id="bf1dd-194">С помощью `IOptionsSnapshot` с `reloadOnChange: true`, параметры привязываются к `IConfiguration` и загружаются в память при изменении.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-194">Using `IOptionsSnapshot` with `reloadOnChange: true`, the options are bound to `IConfiguration` and reloaded when changed.</span></span>

<span data-ttu-id="bf1dd-195">В следующем образце показано, как новый `IOptionsSnapshot` создается после *config.json* изменений.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-195">The following sample demonstrates how a new `IOptionsSnapshot` is created after *config.json* changes.</span></span> <span data-ttu-id="bf1dd-196">Запросы к серверу будут возвращает такой же время, когда *config.json* имеет **не** изменен.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-196">Requests to the server will return the same time when *config.json* has **not** changed.</span></span> <span data-ttu-id="bf1dd-197">После первого запроса *config.json* изменения будут отображаться новое время.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-197">The first request after *config.json* changes will show a new time.</span></span>

[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]

<span data-ttu-id="bf1dd-198">На следующем рисунке показана вывод server:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-198">The following image shows the server output:</span></span>

![Отображение изображений браузера «последнее обновление: 11/22/2016 4:43:00»](configuration/_static/first.png)

<span data-ttu-id="bf1dd-200">Обновить браузер не изменяет значение сообщения или время, отображаемое (когда *config.json* не изменились).</span><span class="sxs-lookup"><span data-stu-id="bf1dd-200">Refreshing the browser doesn't change the message value or time displayed (when *config.json* has not changed).</span></span>

<span data-ttu-id="bf1dd-201">Изменить и сохранить *config.json* , а затем обновить браузер:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-201">Change and save the  *config.json* and then refresh the browser:</span></span>

![Отображение изображений браузера «последнее обновление для e: 11/22/2016 4:53:00»](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="bf1dd-203">Поставщик в памяти и привязки для класса POCO</span><span class="sxs-lookup"><span data-stu-id="bf1dd-203">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="bf1dd-204">Следующий пример демонстрирует использование поставщика в памяти и связать с классом:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-204">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]

<span data-ttu-id="bf1dd-205">Значения конфигурации возвращается в виде строк, но привязка позволяет для создания объектов.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-205">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="bf1dd-206">Привязка позволяет получить POCO объекты или графы даже весь объект.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-206">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span> <span data-ttu-id="bf1dd-207">Следующий пример показано, как выполнить привязку к `MyWindow` и использовать шаблон, параметры с приложением ASP.NET MVC основных компонентов:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-207">The following sample shows how to bind to `MyWindow` and use the options pattern with a ASP.NET Core MVC app:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]

[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]

<span data-ttu-id="bf1dd-208">Привязка пользовательского класса в `ConfigureServices` при создании узла:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-208">Bind the custom class in `ConfigureServices` when building the host:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="bf1dd-209">Параметры отображения из `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-209">Display the settings from the `HomeController`:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]

### <a name="getvalue"></a><span data-ttu-id="bf1dd-210">GetValue</span><span class="sxs-lookup"><span data-stu-id="bf1dd-210">GetValue</span></span>

<span data-ttu-id="bf1dd-211">В следующем образце показано [GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) метода расширения:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-211">The following sample demonstrates the [GetValue<T>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

<span data-ttu-id="bf1dd-212">ConfigurationBinder `GetValue<T>` метод позволяет задать значение по умолчанию (80 в образце).</span><span class="sxs-lookup"><span data-stu-id="bf1dd-212">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="bf1dd-213">`GetValue<T>`для простых сценариях и не привязывается к весь раздел.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-213">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="bf1dd-214">`GetValue<T>`Возвращает скалярные значения из `GetSection(key).Value` преобразован к определенному типу.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-214">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="binding-to-an-object-graph"></a><span data-ttu-id="bf1dd-215">Привязка к графа объекта</span><span class="sxs-lookup"><span data-stu-id="bf1dd-215">Binding to an object graph</span></span>

<span data-ttu-id="bf1dd-216">Можно выполнить привязку рекурсивно для каждого объекта в классе.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-216">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="bf1dd-217">Рассмотрим следующий `AppOptions` класса:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-217">Consider the following `AppOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]

<span data-ttu-id="bf1dd-218">Следующий пример привязывает к `AppOptions` класса:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-218">The following sample binds to the `AppOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="bf1dd-219">**ASP.NET Core 1.1** и более поздней версии можно использовать `Get<T>`, которая работает с весь раздел.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-219">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="bf1dd-220">`Get<T>`может быть более convienent, чем при использовании `Bind`.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-220">`Get<T>` can be more convienent than using `Bind`.</span></span> <span data-ttu-id="bf1dd-221">Следующий код показывает, как использовать `Get<T>` с примером выше:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-221">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

<span data-ttu-id="bf1dd-222">С помощью следующих *appsettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-222">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="bf1dd-223">Программа выводит `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-223">The program displays `Height 11`.</span></span>

<span data-ttu-id="bf1dd-224">Следующий код может использоваться для модульного тестирования конфигурации:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-224">The following code can be used to unit test the configuration:</span></span>

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

    var options = new AppOptions();
    config.GetSection("App").Bind(options);

    Assert.Equal("Rick", options.Profile.Machine);
    Assert.Equal(11, options.Window.Height);
    Assert.Equal(11, options.Window.Width);
    Assert.Equal("connectionstring", options.Connection.Value);
}
```

<a name=custom-config-providers></a>

## <a name="basic-sample-of-entity-framework-custom-provider"></a><span data-ttu-id="bf1dd-225">Базовый образец пользовательского поставщика Entity Framework</span><span class="sxs-lookup"><span data-stu-id="bf1dd-225">Basic sample of Entity Framework custom provider</span></span>

<span data-ttu-id="bf1dd-226">В этом разделе создается основная конфигурация поставщика, который считывает пары "имя значение" из базы данных с помощью EF.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-226">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="bf1dd-227">Определение `ConfigurationValue` сущности для хранения значения конфигурации базы данных:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-227">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="bf1dd-228">Добавить `ConfigurationContext` хранение и доступ к настроенные значения:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-228">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="bf1dd-229">Создайте класс, реализующий [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span><span class="sxs-lookup"><span data-stu-id="bf1dd-229">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="bf1dd-230">Создание настраиваемого поставщика конфигурации путем наследования от [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span><span class="sxs-lookup"><span data-stu-id="bf1dd-230">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="bf1dd-231">Поставщик конфигурации инициализирует базы данных при пустом:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-231">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="bf1dd-232">При запуске образца отображаются выделенные значения из базы данных («value_from_ef_1» и «value_from_ef_2»).</span><span class="sxs-lookup"><span data-stu-id="bf1dd-232">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="bf1dd-233">Можно добавить `EFConfigSource` метод расширения для добавления источник конфигурации:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-233">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="bf1dd-234">Ниже показано, как использовать собственную `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-234">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="bf1dd-235">Обратите внимание, в этом примере добавляется пользовательский `EFConfigProvider` после поставщика JSON, поэтому все параметры из базы данных переопределяют параметры из *appsettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-235">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="bf1dd-236">С помощью следующих *appsettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-236">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="bf1dd-237">Отобразится следующее:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-237">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="bf1dd-238">Поставщик конфигурации Командная строка</span><span class="sxs-lookup"><span data-stu-id="bf1dd-238">CommandLine configuration provider</span></span>

<span data-ttu-id="bf1dd-239">[Командная строка конфигурации поставщика](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) получает пар ключ значение аргумента командной строки для конфигурации во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-239">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="bf1dd-240">Просмотреть или загрузить образец конфигурации Командная строка</span><span class="sxs-lookup"><span data-stu-id="bf1dd-240">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample/CommandLine)

### <a name="setting-up-the-provider"></a><span data-ttu-id="bf1dd-241">Настройка поставщика</span><span class="sxs-lookup"><span data-stu-id="bf1dd-241">Setting up the provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="bf1dd-242">Основная конфигурация</span><span class="sxs-lookup"><span data-stu-id="bf1dd-242">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="bf1dd-243">Чтобы активировать настройки командной строки, вызовите `AddCommandLine` метод расширения в экземпляре [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="bf1dd-243">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="bf1dd-244">Выполнение кода, выводятся следующие данные:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-244">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="bf1dd-245">Передача аргумента пары "ключ значение" в командной строке приводит к изменению значений `Profile:MachineName` и `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-245">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="bf1dd-246">В окне консоли отображается:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-246">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="bf1dd-247">Чтобы переопределить конфигурацию, сформированную другими поставщиками конфигурации с конфигурацией командной строки, вызовите `AddCommandLine` последней в `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-247">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bf1dd-248">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bf1dd-248">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="bf1dd-249">Типичные приложения ASP.NET Core 2.x использовать метод статических удобных `CreateDefaultBuilder` для создания узла:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-249">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](configuration/sample_snapshot/Program.cs?highlight=12)]

<span data-ttu-id="bf1dd-250">`CreateDefaultBuilder`Загружает дополнительной конфигурации из *appsettings.json*, *appsettings. {} Среда} .json*, [секретов пользователя](xref:security/app-secrets) (в `Development` среды), переменные и аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-250">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="bf1dd-251">Поставщик конфигурации CommandLine вызывается.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-251">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="bf1dd-252">Последнего вызова поставщика позволяет ранее вызов аргументы командной строки, которые передаются во время выполнения для переопределения набора конфигурации с других поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-252">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="bf1dd-253">Обратите внимание, что для *appsettings* файлов, в которой `reloadOnChange` включен.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-253">Note that for *appsettings* files that `reloadOnChange` is enabled.</span></span> <span data-ttu-id="bf1dd-254">Аргументы командной строки переопределяются в том случае, если совпадающее значение конфигурации в *appsettings* был изменен после запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-254">Command-line arguments are overridden if a matching configuration value in an *appsettings* file is changed after the app starts.</span></span>

> [!NOTE]
> <span data-ttu-id="bf1dd-255">В качестве альтернативы с помощью `CreateDefaultBuilder` создание узла с помощью метода [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) и вручную создания конфигурации с [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) поддерживается в ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-255">As an alternative to using the `CreateDefaultBuilder` method, creating a host using [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) and manually building configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) is supported in ASP.NET Core 2.x.</span></span> <span data-ttu-id="bf1dd-256">См. на вкладке 1.x ASP.NET Core для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-256">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bf1dd-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bf1dd-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="bf1dd-258">Создание [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) и вызвать `AddCommandLine` метод, чтобы использовать поставщик конфигурации Командная строка.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-258">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="bf1dd-259">Последнего вызова поставщика позволяет ранее вызов аргументы командной строки, которые передаются во время выполнения для переопределения набора конфигурации с других поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-259">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="bf1dd-260">Применить конфигурацию [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) с `UseConfiguration` метод:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-260">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="bf1dd-261">Аргументы</span><span class="sxs-lookup"><span data-stu-id="bf1dd-261">Arguments</span></span>

<span data-ttu-id="bf1dd-262">Аргументы, передаваемые в командной строке должен соответствовать одному из двух форматов, показано в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-262">Arguments passed on the command line must conform to one of two formats shown in the following table.</span></span>

| <span data-ttu-id="bf1dd-263">Аргумент формата</span><span class="sxs-lookup"><span data-stu-id="bf1dd-263">Argument format</span></span>                                                     | <span data-ttu-id="bf1dd-264">Пример</span><span class="sxs-lookup"><span data-stu-id="bf1dd-264">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="bf1dd-265">Один аргумент: пара ключ значение, разделенные знаком равенства (`=`)</span><span class="sxs-lookup"><span data-stu-id="bf1dd-265">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="bf1dd-266">Последовательность из двух аргументов: пара ключ значение, разделенные пробелом</span><span class="sxs-lookup"><span data-stu-id="bf1dd-266">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="bf1dd-267">**Один аргумент**</span><span class="sxs-lookup"><span data-stu-id="bf1dd-267">**Single argument**</span></span>

<span data-ttu-id="bf1dd-268">Значение должно следовать знак равенства (`=`).</span><span class="sxs-lookup"><span data-stu-id="bf1dd-268">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="bf1dd-269">Может иметь значение null (например, `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="bf1dd-269">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="bf1dd-270">Ключ может иметь префикс.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-270">The key may have a prefix.</span></span>

| <span data-ttu-id="bf1dd-271">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="bf1dd-271">Key prefix</span></span>               | <span data-ttu-id="bf1dd-272">Пример</span><span class="sxs-lookup"><span data-stu-id="bf1dd-272">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="bf1dd-273">Без префикса</span><span class="sxs-lookup"><span data-stu-id="bf1dd-273">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="bf1dd-274">Единый тире (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="bf1dd-274">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="bf1dd-275">Двух дефисов (`--`)</span><span class="sxs-lookup"><span data-stu-id="bf1dd-275">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="bf1dd-276">Косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="bf1dd-276">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="bf1dd-277">&#8224; Ключ с префиксом один тире (`-`) должны быть предоставлены в [переключения сопоставления](#switch-mappings), описаны ниже.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-277">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="bf1dd-278">Пример команды:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-278">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="bf1dd-279">Примечание: Если `-key1` не существуют в [переключения сопоставления](#switch-mappings) заданное поставщиком конфигурации `FormatException` возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-279">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="bf1dd-280">**Последовательность из двух аргументов**</span><span class="sxs-lookup"><span data-stu-id="bf1dd-280">**Sequence of two arguments**</span></span>

<span data-ttu-id="bf1dd-281">Значение не может иметь значение null и должны соответствовать ключ, разделенные пробелом.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-281">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="bf1dd-282">Ключ должен иметь префикс.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-282">The key must have a prefix.</span></span>

| <span data-ttu-id="bf1dd-283">Префикс ключа</span><span class="sxs-lookup"><span data-stu-id="bf1dd-283">Key prefix</span></span>               | <span data-ttu-id="bf1dd-284">Пример</span><span class="sxs-lookup"><span data-stu-id="bf1dd-284">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="bf1dd-285">Единый тире (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="bf1dd-285">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="bf1dd-286">Двух дефисов (`--`)</span><span class="sxs-lookup"><span data-stu-id="bf1dd-286">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="bf1dd-287">Косая черта (`/`)</span><span class="sxs-lookup"><span data-stu-id="bf1dd-287">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="bf1dd-288">&#8224; Ключ с префиксом один тире (`-`) должны быть предоставлены в [переключения сопоставления](#switch-mappings), описаны ниже.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-288">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="bf1dd-289">Пример команды:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-289">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="bf1dd-290">Примечание: Если `-key1` не существуют в [переключения сопоставления](#switch-mappings) заданное поставщиком конфигурации `FormatException` возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-290">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="bf1dd-291">Повторяющиеся ключи</span><span class="sxs-lookup"><span data-stu-id="bf1dd-291">Duplicate keys</span></span>

<span data-ttu-id="bf1dd-292">Если указаны повторяющиеся ключи, используется последний пару ключ значение.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-292">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="bf1dd-293">Переключение сопоставления</span><span class="sxs-lookup"><span data-stu-id="bf1dd-293">Switch mappings</span></span>

<span data-ttu-id="bf1dd-294">При построении конфигурации с вручную `ConfigurationBuilder`, при необходимости можно предоставить ключ словаря сопоставления для `AddCommandLine` метод.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-294">When manually building configuration with `ConfigurationBuilder`, you can optionally provide a switch mappings dictionary to the `AddCommandLine` method.</span></span> <span data-ttu-id="bf1dd-295">Коммутатор сопоставления позволяют предоставить логика замены имя ключа.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-295">Switch mappings allow you to provide key name replacement logic.</span></span>

<span data-ttu-id="bf1dd-296">При использовании словарь сопоставлений коммутатора словаре проверяется на ключ, который совпадает с ключом, предоставляемые аргумента командной строки.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-296">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="bf1dd-297">Если ключ командной строки, найден в словаре, значение словаря (замены ключей) передается обратно, чтобы задать конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-297">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="bf1dd-298">Сопоставление коммутатора не требуется для командной строки раздела префиксом с одного дефиса (`-`).</span><span class="sxs-lookup"><span data-stu-id="bf1dd-298">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="bf1dd-299">Переключение ключа правила сопоставления словаря:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-299">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="bf1dd-300">Параметры должны начинаться с дефиса (`-`) или двойной дефис (`--`).</span><span class="sxs-lookup"><span data-stu-id="bf1dd-300">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="bf1dd-301">Словарь сопоставлений коммутатора не должен содержать повторяющиеся ключи.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-301">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="bf1dd-302">В следующем примере `GetSwitchMappings` метод позволяет аргументы командной строки для использования одного тире (`-`) ключа префикс и избежать начальные подраздела префиксы.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-302">In the following example, the `GetSwitchMappings` method allows your command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](configuration/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="bf1dd-303">Без указания аргументов командной строки, передаваемые словаре `AddInMemoryCollection` задает значения конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-303">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="bf1dd-304">Запуск приложения с помощью следующей команды:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-304">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="bf1dd-305">В окне консоли отображается:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-305">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="bf1dd-306">Используйте следующие параметры для передачи в параметрах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-306">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="bf1dd-307">В окне консоли отображается:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-307">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="bf1dd-308">Вновь созданный словаре сопоставлений коммутатора он содержит данные, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-308">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="bf1dd-309">Ключ</span><span class="sxs-lookup"><span data-stu-id="bf1dd-309">Key</span></span>            | <span data-ttu-id="bf1dd-310">Значение</span><span class="sxs-lookup"><span data-stu-id="bf1dd-310">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="bf1dd-311">Чтобы продемонстрировать ключа переключения, используя словарь, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-311">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="bf1dd-312">Ключи командной строки, меняются местами.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-312">The command-line keys are swapped.</span></span> <span data-ttu-id="bf1dd-313">В окне консоли отображаются значения конфигурации для `Profile:MachineName` и `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="bf1dd-313">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="bf1dd-314">В файле web.config</span><span class="sxs-lookup"><span data-stu-id="bf1dd-314">The web.config file</span></span>

<span data-ttu-id="bf1dd-315">Объект *web.config* файл будет необходим для размещения приложения в IIS или IIS Express.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-315">A *web.config* file is required when you host the app in IIS or IIS-Express.</span></span> <span data-ttu-id="bf1dd-316">*Web.config* включает AspNetCoreModule в службах IIS для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-316">*web.config* turns on the AspNetCoreModule in IIS to launch your app.</span></span> <span data-ttu-id="bf1dd-317">Параметры в *web.config* включить AspNetCoreModule в службах IIS для запуска приложения и настройки других параметров IIS и модулей.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-317">Settings in *web.config* enable the AspNetCoreModule in IIS to launch your app and configure other IIS settings and modules.</span></span> <span data-ttu-id="bf1dd-318">Если вы используете Visual Studio и удалить *web.config*, Visual Studio создаст новое.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-318">If you are using Visual Studio and delete *web.config*, Visual Studio will create a new one.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="bf1dd-319">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="bf1dd-319">Additional notes</span></span>

* <span data-ttu-id="bf1dd-320">После внедрения зависимости (DI) не задано до `ConfigureServices` вызывается.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-320">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="bf1dd-321">Система конфигурации не учитывать DI.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-321">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="bf1dd-322">`IConfiguration`имеет две специализации.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-322">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="bf1dd-323">`IConfigurationRoot`Используется для корневого узла.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-323">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="bf1dd-324">Можно инициировать перезагрузку.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-324">Can trigger a reload.</span></span>
  * <span data-ttu-id="bf1dd-325">`IConfigurationSection`Представляет раздел конфигурации значения.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-325">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="bf1dd-326">`GetSection` И `GetChildren` методы возвращают `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="bf1dd-326">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bf1dd-327">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="bf1dd-327">Additional resources</span></span>

* [<span data-ttu-id="bf1dd-328">Работа с несколькими средами</span><span class="sxs-lookup"><span data-stu-id="bf1dd-328">Working with Multiple Environments</span></span>](environments.md)
* [<span data-ttu-id="bf1dd-329">Безопасное хранение секретов приложений во время разработки</span><span class="sxs-lookup"><span data-stu-id="bf1dd-329">Safe storage of app secrets during development</span></span>](../security/app-secrets.md)
* [<span data-ttu-id="bf1dd-330">Размещение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bf1dd-330">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="bf1dd-331">Введение зависимостей</span><span class="sxs-lookup"><span data-stu-id="bf1dd-331">Dependency Injection</span></span>](dependency-injection.md)
* [<span data-ttu-id="bf1dd-332">Поставщик конфигурации Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="bf1dd-332">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
