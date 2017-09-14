---
title: "Конфигурации в ASP.NET Core"
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
ms.openlocfilehash: 041bb04a3a3699a166a03338865da154403d8c07
ms.sourcegitcommit: f535ce61c6a5e615bc6399b5d763c734396231f4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2017
---
<a name=fundamentals-configuration></a>

# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="78f80-104">Конфигурации в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="78f80-104">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="78f80-105">[Рик Андерсон](https://twitter.com/RickAndMSFT), [Марка Михаэлиса](http://intellitect.com/author/mark-michaelis/), [Стив Смит](https://ardalis.com/), и [Дэниэла рот](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="78f80-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="78f80-106">API конфигурации предоставляет способ настройки приложения на основе списка пар «имя значение».</span><span class="sxs-lookup"><span data-stu-id="78f80-106">The Configuration API provides a way of configuring an app based on a list of name-value pairs.</span></span> <span data-ttu-id="78f80-107">Конфигурация для чтения во время выполнения из нескольких источников.</span><span class="sxs-lookup"><span data-stu-id="78f80-107">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="78f80-108">Пары «имя значение» могут быть сгруппированы в многоуровневой иерархии.</span><span class="sxs-lookup"><span data-stu-id="78f80-108">The name-value pairs can be grouped into a multi-level hierarchy.</span></span> <span data-ttu-id="78f80-109">Нет поставщиков конфигурации для:</span><span class="sxs-lookup"><span data-stu-id="78f80-109">There are configuration providers for:</span></span>

* <span data-ttu-id="78f80-110">Форматы файлов (ini-ФАЙЛЕ, JSON и XML)</span><span class="sxs-lookup"><span data-stu-id="78f80-110">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="78f80-111">Аргументы командной строки</span><span class="sxs-lookup"><span data-stu-id="78f80-111">Command-line arguments</span></span>
* <span data-ttu-id="78f80-112">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="78f80-112">Environment variables</span></span>
* <span data-ttu-id="78f80-113">Объекты в памяти .NET</span><span class="sxs-lookup"><span data-stu-id="78f80-113">In-memory .NET objects</span></span>
* <span data-ttu-id="78f80-114">Зашифрованные пользовательского хранилища</span><span class="sxs-lookup"><span data-stu-id="78f80-114">An encrypted user store</span></span>
* [<span data-ttu-id="78f80-115">Хранилище ключей Azure</span><span class="sxs-lookup"><span data-stu-id="78f80-115">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="78f80-116">Пользовательские поставщики, которые можно установить или создать</span><span class="sxs-lookup"><span data-stu-id="78f80-116">Custom providers, which you install or create</span></span>

<span data-ttu-id="78f80-117">Каждое значение конфигурации сопоставляет строковый ключ.</span><span class="sxs-lookup"><span data-stu-id="78f80-117">Each configuration value maps to a string key.</span></span> <span data-ttu-id="78f80-118">Поддержка встроенных привязок для десериализации параметров в пользовательский [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) объекта (простой класс .NET со свойствами).</span><span class="sxs-lookup"><span data-stu-id="78f80-118">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

[<span data-ttu-id="78f80-119">Просмотреть или скачать образец кода</span><span class="sxs-lookup"><span data-stu-id="78f80-119">View or download sample code</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)

## <a name="simple-configuration"></a><span data-ttu-id="78f80-120">Простая настройка</span><span class="sxs-lookup"><span data-stu-id="78f80-120">Simple configuration</span></span>

<span data-ttu-id="78f80-121">Следующее приложение консоли использует поставщик конфигурации JSON:</span><span class="sxs-lookup"><span data-stu-id="78f80-121">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]

<span data-ttu-id="78f80-122">Считывает и отображает следующие параметры конфигурации приложения:</span><span class="sxs-lookup"><span data-stu-id="78f80-122">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="78f80-123">Конфигурация состоит из иерархический список пар "имя значение", в которых узлы разделяются точкой с запятой.</span><span class="sxs-lookup"><span data-stu-id="78f80-123">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="78f80-124">Для получения значения, доступ к `Configuration` индексатора с ключом соответствующего элемента:</span><span class="sxs-lookup"><span data-stu-id="78f80-124">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="78f80-125">Для работы с массивами в источниках конфигурации в формате JSON, используйте индекс массива как часть строки двоеточием.</span><span class="sxs-lookup"><span data-stu-id="78f80-125">To work with arrays in JSON-formatted configuration sources, use a array index as part of the colon-separated string.</span></span> <span data-ttu-id="78f80-126">В следующем примере возвращается имя первого элемента в предыдущем `wizards` массива:</span><span class="sxs-lookup"><span data-stu-id="78f80-126">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="78f80-127">Пары имя значение, записанных во встроенную в `Configuration` поставщики **не** сохраняются, однако можно создать пользовательский поставщик, который сохраняет значения.</span><span class="sxs-lookup"><span data-stu-id="78f80-127">Name-value pairs written to the built in `Configuration` providers are **not** persisted, however, you can create a custom provider that saves values.</span></span> <span data-ttu-id="78f80-128">В разделе [настраиваемый поставщик конфигурации](xref:fundamentals/configuration#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="78f80-128">See [custom configuration provider](xref:fundamentals/configuration#custom-config-providers).</span></span>

<span data-ttu-id="78f80-129">Предыдущий пример использует конфигурации индексатора для считывания значений.</span><span class="sxs-lookup"><span data-stu-id="78f80-129">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="78f80-130">Для настройки доступа извне `Startup`, используйте [параметры шаблона](xref:fundamentals/configuration#options-config-objects).</span><span class="sxs-lookup"><span data-stu-id="78f80-130">To access configuration outside of `Startup`, use the [options pattern](xref:fundamentals/configuration#options-config-objects).</span></span> <span data-ttu-id="78f80-131">*Параметры шаблона* показано далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="78f80-131">The *options pattern* is shown later in this article.</span></span>

<span data-ttu-id="78f80-132">Это обычно имеют разные параметры конфигурации для разных сред, например, разработки, тестирования и производства.</span><span class="sxs-lookup"><span data-stu-id="78f80-132">It's typical to have different configuration settings for different environments, for example, development, test, and production.</span></span> <span data-ttu-id="78f80-133">`CreateDefaultBuilder` Метод расширения в приложении ASP.NET Core 2.x (или с помощью `AddJsonFile` и `AddEnvironmentVariables` непосредственно в приложении ASP.NET Core 1.x) добавляет поставщиков конфигурации для чтения файлы JSON и системные настройки источников:</span><span class="sxs-lookup"><span data-stu-id="78f80-133">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="78f80-134">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="78f80-134">*appsettings.json*</span></span>
* <span data-ttu-id="78f80-135">*appSettings. \<EnvironmentName > .json*</span><span class="sxs-lookup"><span data-stu-id="78f80-135">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="78f80-136">переменные среды</span><span class="sxs-lookup"><span data-stu-id="78f80-136">environment variables</span></span>

<span data-ttu-id="78f80-137">В разделе [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) объяснение параметров.</span><span class="sxs-lookup"><span data-stu-id="78f80-137">See [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="78f80-138">`reloadOnChange`поддерживается только в ASP.NET Core 1.1 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="78f80-138">`reloadOnChange` is only supported in ASP.NET Core 1.1 and higher.</span></span> 

<span data-ttu-id="78f80-139">Источники конфигурации считываются в порядке, в котором они указаны.</span><span class="sxs-lookup"><span data-stu-id="78f80-139">Configuration sources are read in the order they are specified.</span></span> <span data-ttu-id="78f80-140">В приведенном выше коде считываются последнего переменные среды.</span><span class="sxs-lookup"><span data-stu-id="78f80-140">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="78f80-141">Заменить все значения конфигурации задать с помощью среды были заданы в предыдущих двух поставщиков.</span><span class="sxs-lookup"><span data-stu-id="78f80-141">Any configuration values set through the environment would replace those set in the two previous providers.</span></span>

<span data-ttu-id="78f80-142">Среды обычно имеет одно из `Development`, `Staging`, или `Production`.</span><span class="sxs-lookup"><span data-stu-id="78f80-142">The environment is typically set to one of `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="78f80-143">В разделе [работа с несколькими средами](environments.md) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="78f80-143">See [Working with Multiple Environments](environments.md) for more information.</span></span>

<span data-ttu-id="78f80-144">Рекомендации по настройке:</span><span class="sxs-lookup"><span data-stu-id="78f80-144">Configuration considerations:</span></span>

* <span data-ttu-id="78f80-145">`IOptionsSnapshot`можно перезагрузить данные конфигурации, при его изменении.</span><span class="sxs-lookup"><span data-stu-id="78f80-145">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="78f80-146">Используйте `IOptionsSnapshot` Если необходимо перезагрузить данные конфигурации.</span><span class="sxs-lookup"><span data-stu-id="78f80-146">Use `IOptionsSnapshot` if you need to reload configuration data.</span></span>  <span data-ttu-id="78f80-147">В разделе [IOptionsSnapshot](#ioptionssnapshot) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="78f80-147">See [IOptionsSnapshot](#ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="78f80-148">Ключи конфигурации учитывается регистр.</span><span class="sxs-lookup"><span data-stu-id="78f80-148">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="78f80-149">Рекомендуется указывать последним, переменные среды, чтобы локальной среды можно переопределить действий в файлах конфигурации, развернутые.</span><span class="sxs-lookup"><span data-stu-id="78f80-149">A best practice is to specify environment variables last, so that the local environment can override anything set in deployed configuration files.</span></span>
* <span data-ttu-id="78f80-150">**Никогда не** хранить пароли и другие конфиденциальные данные в коде поставщика конфигурации или в файлах конфигурации в виде обычного текста.</span><span class="sxs-lookup"><span data-stu-id="78f80-150">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="78f80-151">Не используйте секреты производства в разработке и тестовую среду.</span><span class="sxs-lookup"><span data-stu-id="78f80-151">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="78f80-152">Вместо этого укажите секретные данные за пределами дереве проекта, чтобы их может быть сохранена в репозиторий случайно.</span><span class="sxs-lookup"><span data-stu-id="78f80-152">Instead, specify secrets outside the project tree, so they cannot be accidentally committed into your repository.</span></span> <span data-ttu-id="78f80-153">Дополнительные сведения о [работа с несколькими средами](environments.md) и управление ими [безопасного хранения секрета приложения во время разработки](../security/app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="78f80-153">Learn more about [Working with Multiple Environments](environments.md) and managing [safe storage of app secrets during development](../security/app-secrets.md).</span></span>
* <span data-ttu-id="78f80-154">Если `:` не может быть использование переменных среды в системе, замените `:` с `__` (двойной символ подчеркивания).</span><span class="sxs-lookup"><span data-stu-id="78f80-154">If `:` cannot be used in environment variables in your system,  replace `:`  with `__` (double underscore).</span></span>

<a name=options-config-objects></a>

## <a name="using-options-and-configuration-objects"></a><span data-ttu-id="78f80-155">С помощью параметров и объектов конфигурации</span><span class="sxs-lookup"><span data-stu-id="78f80-155">Using Options and configuration objects</span></span>

<span data-ttu-id="78f80-156">Параметры шаблона используются классы пользовательские параметры для представления группу связанных параметров.</span><span class="sxs-lookup"><span data-stu-id="78f80-156">The options pattern uses custom options classes to represent a group of related settings.</span></span> <span data-ttu-id="78f80-157">Рекомендуется создать несвязанной классы для каждого компонента, в приложении.</span><span class="sxs-lookup"><span data-stu-id="78f80-157">We recommended that you create decoupled classes for each feature within your app.</span></span> <span data-ttu-id="78f80-158">Выполните несвязанной классы.</span><span class="sxs-lookup"><span data-stu-id="78f80-158">Decoupled classes follow:</span></span>

* <span data-ttu-id="78f80-159">[Принцип разделения интерфейс (ISP)](http://deviq.com/interface-segregation-principle/) : классы зависеть только от параметров конфигурации, они используют.</span><span class="sxs-lookup"><span data-stu-id="78f80-159">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/) : Classes depend only on the configuration settings they use.</span></span>
* <span data-ttu-id="78f80-160">[Разделение областей ответственности](http://deviq.com/separation-of-concerns/) : параметры для разных частей приложения, не зависящие от них или связанные друг с другом.</span><span class="sxs-lookup"><span data-stu-id="78f80-160">[Separation of Concerns](http://deviq.com/separation-of-concerns/) : Settings for different parts of your app are not dependent or coupled with one another.</span></span>

<span data-ttu-id="78f80-161">Класс параметров должен быть абстрактным открытый конструктор без параметров.</span><span class="sxs-lookup"><span data-stu-id="78f80-161">The options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="78f80-162">Пример:</span><span class="sxs-lookup"><span data-stu-id="78f80-162">For example:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]

<a name=options-example></a>

<span data-ttu-id="78f80-163">В следующем коде включен поставщик конфигурации JSON.</span><span class="sxs-lookup"><span data-stu-id="78f80-163">In the following code, the JSON configuration provider is enabled.</span></span> <span data-ttu-id="78f80-164">`MyOptions` Класса добавляется в контейнер службы и привязан к конфигурации.</span><span class="sxs-lookup"><span data-stu-id="78f80-164">The `MyOptions` class is added to the service container and bound to configuration.</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]

<span data-ttu-id="78f80-165">Следующие [контроллера](../mvc/controllers/index.md) использует [конструктор внедрения зависимостей](xref:fundamentals/dependency-injection#what-is-dependency-injection) на [ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) для доступа к параметрам:</span><span class="sxs-lookup"><span data-stu-id="78f80-165">The following [controller](../mvc/controllers/index.md)  uses [constructor Dependency Injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) on [`IOptions<TOptions>`](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) to access settings:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="78f80-166">Со следующими *appsettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="78f80-166">With the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]

<span data-ttu-id="78f80-167">`HomeController.Index` Возвращает `option1 = value1_from_json, option2 = 2`.</span><span class="sxs-lookup"><span data-stu-id="78f80-167">The `HomeController.Index` method returns `option1 = value1_from_json, option2 = 2`.</span></span>

<span data-ttu-id="78f80-168">Типичные приложения не привязки к файлу параметры единого вся конфигурация.</span><span class="sxs-lookup"><span data-stu-id="78f80-168">Typical apps won't bind the entire configuration to a single options file.</span></span> <span data-ttu-id="78f80-169">Позднее будет показано как использовать `GetSection` для привязки к разделу.</span><span class="sxs-lookup"><span data-stu-id="78f80-169">Later on I'll show how to use `GetSection` to bind to a section.</span></span>

<span data-ttu-id="78f80-170">В следующем коде секунды `IConfigureOptions<TOptions>` служба добавляется в контейнер службы.</span><span class="sxs-lookup"><span data-stu-id="78f80-170">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="78f80-171">Она использует делегат для настройки привязки с `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="78f80-171">It uses a delegate to configure the binding with `MyOptions`.</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]

<span data-ttu-id="78f80-172">Можно добавить несколько поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="78f80-172">You can add multiple configuration providers.</span></span> <span data-ttu-id="78f80-173">Поставщики конфигурации, доступные в пакетах NuGet.</span><span class="sxs-lookup"><span data-stu-id="78f80-173">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="78f80-174">Они применяются в порядке, в котором они зарегистрированы.</span><span class="sxs-lookup"><span data-stu-id="78f80-174">They are applied in order they are registered.</span></span>

<span data-ttu-id="78f80-175">Каждый вызов `Configure<TOptions>` добавляет `IConfigureOptions<TOptions>` службу в контейнер службы.</span><span class="sxs-lookup"><span data-stu-id="78f80-175">Each call to `Configure<TOptions>` adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="78f80-176">В предыдущем примере значения `Option1` и `Option2` указываются в *appsettings.json* --, но значение `Option1` переопределяется настроенных делегата.</span><span class="sxs-lookup"><span data-stu-id="78f80-176">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json* -- but the value of `Option1` is overridden by the configured delegate.</span></span> 

<span data-ttu-id="78f80-177">При включении более одной конфигурации службы последнего источник конфигурации указано «побеждает» (задает значение конфигурации).</span><span class="sxs-lookup"><span data-stu-id="78f80-177">When more than one configuration service is enabled, the last configuration source specified "wins" (sets the configuration value).</span></span> <span data-ttu-id="78f80-178">В приведенном выше коде `HomeController.Index` возвращает `option1 = value1_from_action, option2 = 2`.</span><span class="sxs-lookup"><span data-stu-id="78f80-178">In the preceding code, the `HomeController.Index` method returns `option1 = value1_from_action, option2 = 2`.</span></span>

<span data-ttu-id="78f80-179">При привязке параметров конфигурации, каждое свойство в типе параметры привязан ключу конфигурации формы `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="78f80-179">When you bind options to configuration, each property in your options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="78f80-180">Например `MyOptions.Option1` свойства, связанного с ключом `Option1`, который считывается из `option1` свойство в *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="78f80-180">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span> <span data-ttu-id="78f80-181">Далее в этой статье показан пример вложенного свойства.</span><span class="sxs-lookup"><span data-stu-id="78f80-181">A sub-property sample is shown later in this article.</span></span>

<span data-ttu-id="78f80-182">В следующем коде третий `IConfigureOptions<TOptions>` служба добавляется в контейнер службы.</span><span class="sxs-lookup"><span data-stu-id="78f80-182">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="78f80-183">Привязывает `MySubOptions` к разделу `subsection` из *appsettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="78f80-183">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]

<span data-ttu-id="78f80-184">Примечание: Этот метод расширения требуется `Microsoft.Extensions.Options.ConfigurationExtensions` пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="78f80-184">Note: This extension method requires the `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet package.</span></span>

<span data-ttu-id="78f80-185">С помощью следующих *appsettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="78f80-185">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]

<span data-ttu-id="78f80-186">`MySubOptions` Класса:</span><span class="sxs-lookup"><span data-stu-id="78f80-186">The `MySubOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="78f80-187">Со следующими `Controller`:</span><span class="sxs-lookup"><span data-stu-id="78f80-187">With the following `Controller`:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]

<span data-ttu-id="78f80-188">`subOption1 = subvalue1_from_json, subOption2 = 200`возвращается.</span><span class="sxs-lookup"><span data-stu-id="78f80-188">`subOption1 = subvalue1_from_json, subOption2 = 200` is returned.</span></span>

<span data-ttu-id="78f80-189">Можно также предлагаемых в модель представления или вставить `IOptions<TOptions>` непосредственно в представлении:</span><span class="sxs-lookup"><span data-stu-id="78f80-189">You can also supply options in a view model or inject `IOptions<TOptions>` directly into a view:</span></span>

[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]

<a name=in-memory-provider></a>

## <a name="ioptionssnapshot"></a><span data-ttu-id="78f80-190">IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="78f80-190">IOptionsSnapshot</span></span>

<span data-ttu-id="78f80-191">*Требуется ASP.NET Core 1.1 или более поздней версии.*</span><span class="sxs-lookup"><span data-stu-id="78f80-191">*Requires ASP.NET Core 1.1 or higher.*</span></span>

<span data-ttu-id="78f80-192">`IOptionsSnapshot`поддерживает повторной загрузки данных конфигурации при изменении файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="78f80-192">`IOptionsSnapshot` supports reloading configuration data when the configuration file has changed.</span></span> <span data-ttu-id="78f80-193">Он имеет минимальные издержки.</span><span class="sxs-lookup"><span data-stu-id="78f80-193">It has minimal overhead.</span></span> <span data-ttu-id="78f80-194">С помощью `IOptionsSnapshot` с `reloadOnChange: true`, параметры привязываются к `IConfiguration` и загружаются в память при изменении.</span><span class="sxs-lookup"><span data-stu-id="78f80-194">Using `IOptionsSnapshot` with `reloadOnChange: true`, the options are bound to `IConfiguration` and reloaded when changed.</span></span>

<span data-ttu-id="78f80-195">В следующем образце показано, как новый `IOptionsSnapshot` создается после *config.json* изменений.</span><span class="sxs-lookup"><span data-stu-id="78f80-195">The following sample demonstrates how a new `IOptionsSnapshot` is created after *config.json* changes.</span></span> <span data-ttu-id="78f80-196">Запросы к серверу будут возвращает такой же время, когда *config.json* имеет **не** изменен.</span><span class="sxs-lookup"><span data-stu-id="78f80-196">Requests to the server will return the same time when *config.json* has **not** changed.</span></span> <span data-ttu-id="78f80-197">После первого запроса *config.json* изменения будут отображаться новое время.</span><span class="sxs-lookup"><span data-stu-id="78f80-197">The first request after *config.json* changes will show a new time.</span></span>

[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]

<span data-ttu-id="78f80-198">На следующем рисунке показана вывод server:</span><span class="sxs-lookup"><span data-stu-id="78f80-198">The following image shows the server output:</span></span>

![Отображение изображений браузера «последнее обновление: 11/22/2016 4:43:00»](configuration/_static/first.png)

<span data-ttu-id="78f80-200">Обновить браузер не изменяет значение сообщения или время, отображаемое (когда *config.json* не изменились).</span><span class="sxs-lookup"><span data-stu-id="78f80-200">Refreshing the browser doesn't change the message value or time displayed (when *config.json* has not changed).</span></span>

<span data-ttu-id="78f80-201">Изменить и сохранить *config.json* , а затем обновить браузер:</span><span class="sxs-lookup"><span data-stu-id="78f80-201">Change and save the  *config.json* and then refresh the browser:</span></span>

![Отображение изображений браузера «последнее обновление для e: 11/22/2016 4:53:00»](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="78f80-203">Поставщик в памяти и привязки для класса POCO</span><span class="sxs-lookup"><span data-stu-id="78f80-203">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="78f80-204">Следующий пример демонстрирует использование поставщика в памяти и связать с классом:</span><span class="sxs-lookup"><span data-stu-id="78f80-204">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]

<span data-ttu-id="78f80-205">Значения конфигурации возвращается в виде строк, но привязка позволяет для создания объектов.</span><span class="sxs-lookup"><span data-stu-id="78f80-205">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="78f80-206">Привязка позволяет получить POCO объекты или графы даже весь объект.</span><span class="sxs-lookup"><span data-stu-id="78f80-206">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span> <span data-ttu-id="78f80-207">Следующий пример показано, как выполнить привязку к `MyWindow` и использовать шаблон, параметры с приложением ASP.NET MVC основных компонентов:</span><span class="sxs-lookup"><span data-stu-id="78f80-207">The following sample shows how to bind to `MyWindow` and use the options pattern with a ASP.NET Core MVC app:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]

[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]

<span data-ttu-id="78f80-208">Привязка пользовательского класса в `ConfigureServices` при создании узла:</span><span class="sxs-lookup"><span data-stu-id="78f80-208">Bind the custom class in `ConfigureServices` when building the host:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="78f80-209">Параметры отображения из `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="78f80-209">Display the settings from the `HomeController`:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]

### <a name="getvalue"></a><span data-ttu-id="78f80-210">GetValue</span><span class="sxs-lookup"><span data-stu-id="78f80-210">GetValue</span></span>

<span data-ttu-id="78f80-211">В следующем образце показано [GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) метода расширения:</span><span class="sxs-lookup"><span data-stu-id="78f80-211">The following sample demonstrates the [GetValue<T>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

<span data-ttu-id="78f80-212">ConfigurationBinder `GetValue<T>` метод позволяет задать значение по умолчанию (80 в образце).</span><span class="sxs-lookup"><span data-stu-id="78f80-212">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="78f80-213">`GetValue<T>`для простых сценариях и не привязывается к весь раздел.</span><span class="sxs-lookup"><span data-stu-id="78f80-213">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="78f80-214">`GetValue<T>`Возвращает скалярные значения из `GetSection(key).Value` преобразован к определенному типу.</span><span class="sxs-lookup"><span data-stu-id="78f80-214">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="binding-to-an-object-graph"></a><span data-ttu-id="78f80-215">Привязка к графа объекта</span><span class="sxs-lookup"><span data-stu-id="78f80-215">Binding to an object graph</span></span>

<span data-ttu-id="78f80-216">Можно выполнить привязку рекурсивно для каждого объекта в классе.</span><span class="sxs-lookup"><span data-stu-id="78f80-216">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="78f80-217">Рассмотрим следующий `AppOptions` класса:</span><span class="sxs-lookup"><span data-stu-id="78f80-217">Consider the following `AppOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]

<span data-ttu-id="78f80-218">Следующий пример привязывает к `AppOptions` класса:</span><span class="sxs-lookup"><span data-stu-id="78f80-218">The following sample binds to the `AppOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="78f80-219">**ASP.NET Core 1.1** и более поздней версии можно использовать `Get<T>`, которая работает с весь раздел.</span><span class="sxs-lookup"><span data-stu-id="78f80-219">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="78f80-220">`Get<T>`может быть более convienent, чем при использовании `Bind`.</span><span class="sxs-lookup"><span data-stu-id="78f80-220">`Get<T>` can be more convienent than using `Bind`.</span></span> <span data-ttu-id="78f80-221">Следующий код показывает, как использовать `Get<T>` с примером выше:</span><span class="sxs-lookup"><span data-stu-id="78f80-221">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

<span data-ttu-id="78f80-222">С помощью следующих *appsettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="78f80-222">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="78f80-223">Программа выводит `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="78f80-223">The program displays `Height 11`.</span></span>

<span data-ttu-id="78f80-224">Следующий код может использоваться для модульного тестирования конфигурации:</span><span class="sxs-lookup"><span data-stu-id="78f80-224">The following code can be used to unit test the configuration:</span></span>

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

## <a name="basic-sample-of-entity-framework-custom-provider"></a><span data-ttu-id="78f80-225">Базовый образец пользовательского поставщика Entity Framework</span><span class="sxs-lookup"><span data-stu-id="78f80-225">Basic sample of Entity Framework custom provider</span></span>

<span data-ttu-id="78f80-226">В этом разделе создается основная конфигурация поставщика, который считывает пары "имя значение" из базы данных с помощью EF.</span><span class="sxs-lookup"><span data-stu-id="78f80-226">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="78f80-227">Определение `ConfigurationValue` сущности для хранения значения конфигурации базы данных:</span><span class="sxs-lookup"><span data-stu-id="78f80-227">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="78f80-228">Добавить `ConfigurationContext` хранение и доступ к настроенные значения:</span><span class="sxs-lookup"><span data-stu-id="78f80-228">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="78f80-229">Создайте класс, реализующий [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span><span class="sxs-lookup"><span data-stu-id="78f80-229">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="78f80-230">Создание настраиваемого поставщика конфигурации путем наследования от [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span><span class="sxs-lookup"><span data-stu-id="78f80-230">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="78f80-231">Поставщик конфигурации инициализирует базы данных при пустом:</span><span class="sxs-lookup"><span data-stu-id="78f80-231">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="78f80-232">При запуске образца отображаются выделенные значения из базы данных («value_from_ef_1» и «value_from_ef_2»).</span><span class="sxs-lookup"><span data-stu-id="78f80-232">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="78f80-233">Можно добавить `EFConfigSource` метод расширения для добавления источник конфигурации:</span><span class="sxs-lookup"><span data-stu-id="78f80-233">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="78f80-234">Ниже показано, как использовать собственную `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="78f80-234">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="78f80-235">Обратите внимание, в этом примере добавляется пользовательский `EFConfigProvider` после поставщика JSON, поэтому все параметры из базы данных переопределяют параметры из *appsettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="78f80-235">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="78f80-236">С помощью следующих *appsettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="78f80-236">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="78f80-237">Отображаются следующие сведения:</span><span class="sxs-lookup"><span data-stu-id="78f80-237">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="78f80-238">Поставщик конфигурации Командная строка</span><span class="sxs-lookup"><span data-stu-id="78f80-238">CommandLine configuration provider</span></span>

<span data-ttu-id="78f80-239">Следующий пример включает поставщик конфигурации CommandLine последнего:</span><span class="sxs-lookup"><span data-stu-id="78f80-239">The following sample enables the CommandLine configuration provider last:</span></span>

[!code-csharp[Main](configuration/sample/CommandLine/Program.cs)]

<span data-ttu-id="78f80-240">Используйте следующие параметры для передачи в параметрах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="78f80-240">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=Bob /App:MainWindow:Left=1234
```

<span data-ttu-id="78f80-241">Что отображается:</span><span class="sxs-lookup"><span data-stu-id="78f80-241">Which displays:</span></span>

```console
Hello Bob
Left 1234
```

<span data-ttu-id="78f80-242">`GetSwitchMappings` Метод позволяет использовать `-` вместо `/` и удаляются начальные подраздела префиксы.</span><span class="sxs-lookup"><span data-stu-id="78f80-242">The `GetSwitchMappings` method allows you to use `-` rather than `/` and it strips the leading subkey prefixes.</span></span> <span data-ttu-id="78f80-243">Пример:</span><span class="sxs-lookup"><span data-stu-id="78f80-243">For example:</span></span>

```console
dotnet run -MachineName=Bob -Left=7734
```

<span data-ttu-id="78f80-244">Отображает:</span><span class="sxs-lookup"><span data-stu-id="78f80-244">Displays:</span></span>

```console
Hello Bob
Left 7734
```

<span data-ttu-id="78f80-245">Аргументы командной строки необходимо включить (он может иметь значение null) значение.</span><span class="sxs-lookup"><span data-stu-id="78f80-245">Command-line arguments must include a value (it can be null).</span></span> <span data-ttu-id="78f80-246">Пример:</span><span class="sxs-lookup"><span data-stu-id="78f80-246">For example:</span></span>

```console
dotnet run /Profile:MachineName=
```

<span data-ttu-id="78f80-247">Нормально, но</span><span class="sxs-lookup"><span data-stu-id="78f80-247">Is OK, but</span></span>

```console
dotnet run /Profile:MachineName
```

<span data-ttu-id="78f80-248">приводит к исключению.</span><span class="sxs-lookup"><span data-stu-id="78f80-248">results in an exception.</span></span> <span data-ttu-id="78f80-249">Если указать параметр командной строки префикса - или--для которого отсутствует соответствующий сопоставление коммутатора будет создано исключение.</span><span class="sxs-lookup"><span data-stu-id="78f80-249">An exception will be thrown if you specify a command-line switch prefix of - or -- for which there's no corresponding switch mapping.</span></span>

## <a name="the-webconfig-file"></a><span data-ttu-id="78f80-250">В файле web.config</span><span class="sxs-lookup"><span data-stu-id="78f80-250">The web.config file</span></span>

<span data-ttu-id="78f80-251">Объект *web.config* файл будет необходим для размещения приложения в IIS или IIS Express.</span><span class="sxs-lookup"><span data-stu-id="78f80-251">A *web.config* file is required when you host the app in IIS or IIS-Express.</span></span> <span data-ttu-id="78f80-252">*Web.config* включает AspNetCoreModule в службах IIS для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="78f80-252">*web.config* turns on the AspNetCoreModule in IIS to launch your app.</span></span> <span data-ttu-id="78f80-253">Параметры в *web.config* включить AspNetCoreModule в службах IIS для запуска приложения и настройки других параметров IIS и модулей.</span><span class="sxs-lookup"><span data-stu-id="78f80-253">Settings in *web.config* enable the AspNetCoreModule in IIS to launch your app and configure other IIS settings and modules.</span></span> <span data-ttu-id="78f80-254">Если вы используете Visual Studio и удалить *web.config*, Visual Studio создаст новое.</span><span class="sxs-lookup"><span data-stu-id="78f80-254">If you are using Visual Studio and delete *web.config*, Visual Studio will create a new one.</span></span>

### <a name="additional-notes"></a><span data-ttu-id="78f80-255">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="78f80-255">Additional notes</span></span>

* <span data-ttu-id="78f80-256">После внедрения зависимости (DI) не задано до `ConfigureServices` вызывается.</span><span class="sxs-lookup"><span data-stu-id="78f80-256">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="78f80-257">Система конфигурации не учитывать DI.</span><span class="sxs-lookup"><span data-stu-id="78f80-257">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="78f80-258">`IConfiguration`имеет две специализации.</span><span class="sxs-lookup"><span data-stu-id="78f80-258">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="78f80-259">`IConfigurationRoot`Используется для корневого узла.</span><span class="sxs-lookup"><span data-stu-id="78f80-259">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="78f80-260">Можно инициировать перезагрузку.</span><span class="sxs-lookup"><span data-stu-id="78f80-260">Can trigger a reload.</span></span>
  * <span data-ttu-id="78f80-261">`IConfigurationSection`Представляет раздел конфигурации значения.</span><span class="sxs-lookup"><span data-stu-id="78f80-261">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="78f80-262">`GetSection` И `GetChildren` методы возвращают `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="78f80-262">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="78f80-263">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="78f80-263">Additional resources</span></span>

* [<span data-ttu-id="78f80-264">Работа с несколькими средами</span><span class="sxs-lookup"><span data-stu-id="78f80-264">Working with Multiple Environments</span></span>](environments.md)
* [<span data-ttu-id="78f80-265">Безопасное хранение секретов приложений во время разработки</span><span class="sxs-lookup"><span data-stu-id="78f80-265">Safe storage of app secrets during development</span></span>](../security/app-secrets.md)
* [<span data-ttu-id="78f80-266">Введение зависимостей</span><span class="sxs-lookup"><span data-stu-id="78f80-266">Dependency Injection</span></span>](dependency-injection.md)
* [<span data-ttu-id="78f80-267">Поставщик конфигурации Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="78f80-267">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
