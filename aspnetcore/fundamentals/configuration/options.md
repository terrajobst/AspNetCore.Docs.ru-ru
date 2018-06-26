---
title: Шаблон параметров в ASP.NET Core
author: guardrex
description: Узнайте, как использовать шаблон параметров для представления групп связанных параметров в приложениях ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
uid: fundamentals/configuration/options
ms.openlocfilehash: 1fe05fbc5035ffa2d01bc6be55436146f1434d17
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278548"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="d9b6b-103">Шаблон параметров в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d9b6b-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="d9b6b-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="d9b6b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d9b6b-105">Шаблон параметров использует классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="d9b6b-106">Когда параметры конфигурации изолируются по функции в отдельных классах, в приложениях соблюдаются два важных принципа программной инженерии.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-106">When configuration settings are isolated by feature into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="d9b6b-107">[Принцип изоляции интерфейсов](http://deviq.com/interface-segregation-principle/). Функции (классы), которые зависят от параметров конфигурации, зависят только от используемых ими параметров конфигурации.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="d9b6b-108">[Разделение ответственности](http://deviq.com/separation-of-concerns/). Параметры для разных частей приложения не зависят друг от друга и не связаны друг с другом.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="d9b6b-109">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample)). Информацию в этой статье будет проще воспринимать, если пользоваться примером приложения.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="d9b6b-110">Конфигурация основных параметров</span><span class="sxs-lookup"><span data-stu-id="d9b6b-110">Basic options configuration</span></span>

<span data-ttu-id="d9b6b-111">Конфигурация основных параметров демонстрируется в примере № 1 в [образце приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="d9b6b-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="d9b6b-112">Класс параметров должен быть неабстрактным с открытым конструктором без параметров.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="d9b6b-113">Приведенный ниже класс (`MyOptions`) имеет два свойства: `Option1` и `Option2`.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="d9b6b-114">Задавать значения по умолчанию необязательно, однако конструктор класса в этом примере задает значение по умолчанию для `Option1`.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="d9b6b-115">Значение по умолчанию для `Option2` устанавливается путем прямой инициализации (*Models/MyOptions.cs*).</span><span class="sxs-lookup"><span data-stu-id="d9b6b-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="d9b6b-116">Класс `MyOptions` добавляется в контейнер службы с помощью [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) и привязывается к конфигурации:</span><span class="sxs-lookup"><span data-stu-id="d9b6b-116">The `MyOptions` class is added to the service container with [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) and bound to configuration:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="d9b6b-117">Следующая страничная модель использует [внедрение зависимостей конструктора](xref:fundamentals/dependency-injection#what-is-dependency-injection) с помощью [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) для доступа к параметрам (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="d9b6b-117">The following page model uses [constructor dependency injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="d9b6b-118">В файле *appsettings.json* образца задаются значения для свойств `option1` и `option2`:</span><span class="sxs-lookup"><span data-stu-id="d9b6b-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=2-3)]

<span data-ttu-id="d9b6b-119">Когда приложение выполняется, метод `OnGet` страничной модели возвращает строку со значениями класса параметров:</span><span class="sxs-lookup"><span data-stu-id="d9b6b-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="d9b6b-120">Настройка простых параметров с помощью делегата</span><span class="sxs-lookup"><span data-stu-id="d9b6b-120">Configure simple options with a delegate</span></span>

<span data-ttu-id="d9b6b-121">Настройка простых параметров с помощью делегата демонстрируется в примере № 2 в [образце приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="d9b6b-121">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="d9b6b-122">Используйте делегат для задания значений параметров.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-122">Use a delegate to set options values.</span></span> <span data-ttu-id="d9b6b-123">В образце приложения используется класс `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="d9b6b-123">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="d9b6b-124">В приведенном ниже коде в контейнер службы добавляется еще одна служба `IConfigureOptions<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-124">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="d9b6b-125">Она использует делегат для настройки привязки к `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="d9b6b-125">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="d9b6b-126">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="d9b6b-126">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="d9b6b-127">Можно добавить несколько поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-127">You can add multiple configuration providers.</span></span> <span data-ttu-id="d9b6b-128">Поставщики конфигурации доступны в пакетах NuGet.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-128">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="d9b6b-129">Они применяются в порядке регистрации.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-129">They're applied in order that they're registered.</span></span>

<span data-ttu-id="d9b6b-130">Каждый вызов [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) добавляет службу `IConfigureOptions<TOptions>` в контейнер службы.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-130">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="d9b6b-131">В предыдущем примере значения `Option1` и `Option2` задаются в файле *appsettings.json*, однако значения `Option1` и `Option2` переопределяются настроенным делегатом.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-131">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="d9b6b-132">Если включено более одной службы конфигурации, последний источник конфигурации *имеет приоритет* и определяет значение конфигурации.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-132">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="d9b6b-133">Когда приложение выполняется, метод `OnGet` страничной модели возвращает строку со значениями класса параметров:</span><span class="sxs-lookup"><span data-stu-id="d9b6b-133">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="d9b6b-134">Конфигурация подпараметров</span><span class="sxs-lookup"><span data-stu-id="d9b6b-134">Suboptions configuration</span></span>

<span data-ttu-id="d9b6b-135">Конфигурация подпараметров демонстрируется в примере № 3 в [образце приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="d9b6b-135">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="d9b6b-136">В приложениях должны создаваться классы приложений, относящиеся к определенным группам функциональных возможностей (классам).</span><span class="sxs-lookup"><span data-stu-id="d9b6b-136">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="d9b6b-137">Части приложения, которым требуются значения конфигурации, должны иметь доступ только к используемым ими значениям конфигурации.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-137">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="d9b6b-138">При привязке параметров к конфигурации каждое свойство, относящееся к типу параметров, привязывается к ключу конфигурации в формате `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-138">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="d9b6b-139">Например, свойство `MyOptions.Option1` привязывается к ключу `Option1`, который считывается из свойства `option1` в файле *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-139">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="d9b6b-140">В приведенном ниже коде в контейнер службы добавляется третья служба `IConfigureOptions<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-140">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="d9b6b-141">Она привязывает `MySubOptions` к разделу `subsection` файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-141">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="d9b6b-142">Методу расширения `GetSection` требуется пакет NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="d9b6b-142">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="d9b6b-143">Если приложение использует [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 или более поздней версии), пакет включается автоматически.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-143">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="d9b6b-144">В файле *appsettings.json* образца определяется член `subsection` с ключами для `suboption1` и `suboption2`.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-144">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="d9b6b-145">Класс `MySubOptions` определяет свойства `SubOption1` и `SubOption2` для хранения значений подпараметров (*Models/MySubOptions.cs*).</span><span class="sxs-lookup"><span data-stu-id="d9b6b-145">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the sub-option values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="d9b6b-146">Метод `OnGet` страничной модели возвращает строку со значениями параметров (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="d9b6b-146">The page model's `OnGet` method returns a string with the sub-option values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="d9b6b-147">Когда приложение выполняется, метод `OnGet` возвращает строку со значениями класса подпараметров:</span><span class="sxs-lookup"><span data-stu-id="d9b6b-147">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="d9b6b-148">Параметры, предоставляемые моделью представления или посредством прямого внедрения представления</span><span class="sxs-lookup"><span data-stu-id="d9b6b-148">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="d9b6b-149">Параметры, предоставляемые моделью представления или посредством прямого внедрения представления, демонстрируются в примере № 4 в [образце приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="d9b6b-149">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="d9b6b-150">Параметры могут передаваться в модель представления или путем внедрения `IOptions<TOptions>` непосредственно в представление (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="d9b6b-150">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="d9b6b-151">Чтобы выполнить прямое внедрение, внедрите `IOptions<MyOptions>` с помощью директивы `@inject`:</span><span class="sxs-lookup"><span data-stu-id="d9b6b-151">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="d9b6b-152">При запуске приложения значения параметров отображаются на отрисовываемой странице:</span><span class="sxs-lookup"><span data-stu-id="d9b6b-152">When the app is run, the option values are shown in the rendered page:</span></span>

![Значения параметров Option1: value1_from_json и Option2: –1 загружаются из модели и путем внедрения в представление.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="d9b6b-154">Повторная загрузка данных конфигурации с помощью IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="d9b6b-154">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="d9b6b-155">Повторная загрузка данных конфигурации с помощью `IOptionsSnapshot` демонстрируется в примере № 5 в [образце приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="d9b6b-155">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="d9b6b-156">*Требуется ASP.NET Core 1.1 или более поздней версии.*</span><span class="sxs-lookup"><span data-stu-id="d9b6b-156">*Requires ASP.NET Core 1.1 or later.*</span></span>

<span data-ttu-id="d9b6b-157">Интерфейс [IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) поддерживает повторную загрузку параметров с минимальными затратами на обработку.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span> <span data-ttu-id="d9b6b-158">В ASP.NET Core 1.1 интерфейс `IOptionsSnapshot` является моментальным снимком [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) и обновляется автоматически каждый раз, когда монитор инициирует изменения при изменении источника данных.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-158">In ASP.NET Core 1.1, `IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span> <span data-ttu-id="d9b6b-159">В ASP.NET Core 2.0 и более поздних версиях параметры вычисляются один раз на каждый запрос при обращении к ним и кэшируются на все время существования запроса.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-159">In ASP.NET Core 2.0 and later, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="d9b6b-160">В приведенном ниже примере демонстрируется создание экземпляра `IOptionsSnapshot` после изменения файла *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="d9b6b-160">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="d9b6b-161">Несколько запросов к серверу возвращают константные значения, предоставляемые файлом *appsettings.json*, пока файл не изменится и конфигурация не загрузится повторно.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-161">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="d9b6b-162">Ниже показаны начальные значения `option1` и `option2`, загруженные из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-162">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="d9b6b-163">Измените значения в файле *appsettings.json* на `value1_from_json UPDATED` и `200`.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-163">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="d9b6b-164">Сохраните файл *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-164">Save the *appsettings.json* file.</span></span> <span data-ttu-id="d9b6b-165">Обновите окно браузера, чтобы увидеть обновленные значения параметров:</span><span class="sxs-lookup"><span data-stu-id="d9b6b-165">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="d9b6b-166">Поддержка именованных параметров в IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="d9b6b-166">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="d9b6b-167">Поддержка именованных параметров в [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) демонстрируется в примере № 6 в [образце приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="d9b6b-167">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="d9b6b-168">*Требуется ASP.NET Core 2.0 или более поздней версии.*</span><span class="sxs-lookup"><span data-stu-id="d9b6b-168">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="d9b6b-169">Поддержка *именованных параметров* позволяет приложению различать конфигурации именованных параметров.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-169">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="d9b6b-170">В примере приложения именованные параметры должны быть объявлены с помощью [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure), что, в свою очередь, вызывает метод расширения [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure):</span><span class="sxs-lookup"><span data-stu-id="d9b6b-170">In the sample app, named options are declared with the [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) which in turn calls the extension method [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="d9b6b-171">Образец приложения обращается к именованным параметрам с помощью метода [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="d9b6b-171">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="d9b6b-172">При запуске образца приложения возвращаются именованные параметры:</span><span class="sxs-lookup"><span data-stu-id="d9b6b-172">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="d9b6b-173">Значения `named_options_1` предоставляются из конфигурации, которая загружается из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-173">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="d9b6b-174">Значения `named_options_2` предоставляются следующим образом:</span><span class="sxs-lookup"><span data-stu-id="d9b6b-174">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="d9b6b-175">Делегатом `named_options_2` в `ConfigureServices` для `Option1`.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-175">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="d9b6b-176">Значение по умолчанию для `Option2` предоставляется классом `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-176">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="d9b6b-177">Настройте все экземпляры именованных параметров с помощью метода [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall).</span><span class="sxs-lookup"><span data-stu-id="d9b6b-177">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="d9b6b-178">Приведенный ниже код настраивает `Option1` для всех именованных экземпляров конфигурации с общим значением.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-178">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="d9b6b-179">Добавьте следующий код в метод `Configure` вручную:</span><span class="sxs-lookup"><span data-stu-id="d9b6b-179">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="d9b6b-180">Если запустить образец приложения после добавления, результат будет следующим:</span><span class="sxs-lookup"><span data-stu-id="d9b6b-180">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="d9b6b-181">В ASP.NET Core 2.0 и более поздних версиях все параметры являются именованными экземплярами.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-181">In ASP.NET Core 2.0 and later, all options are named instances.</span></span> <span data-ttu-id="d9b6b-182">Существующие экземпляры `IConfigureOption` считаются нацеленными на экземпляр `Options.DefaultName`, который имеет значение `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-182">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="d9b6b-183">Интерфейс `IConfigureNamedOptions` также реализует интерфейс `IConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-183">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="d9b6b-184">Реализация [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) по умолчанию ([источник ссылки](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs)) содержит логику для надлежащего использования каждого экземпляра.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-184">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) has logic to use each appropriately.</span></span> <span data-ttu-id="d9b6b-185">Именованный параметр `null` предназначен для всех именованных экземпляров, а не для какого-то определенного ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) и [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) следуют этому соглашению).</span><span class="sxs-lookup"><span data-stu-id="d9b6b-185">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

## <a name="ipostconfigureoptions"></a><span data-ttu-id="d9b6b-186">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="d9b6b-186">IPostConfigureOptions</span></span>

<span data-ttu-id="d9b6b-187">*Требуется ASP.NET Core 2.0 или более поздней версии.*</span><span class="sxs-lookup"><span data-stu-id="d9b6b-187">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="d9b6b-188">Задайте пост-конфигурацию с помощью [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span><span class="sxs-lookup"><span data-stu-id="d9b6b-188">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="d9b6b-189">Пост-конфигурация применяется после выполнения всех действий настройки с помощью [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1):</span><span class="sxs-lookup"><span data-stu-id="d9b6b-189">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="d9b6b-190">Для последующей настройки именованных параметров доступен метод [PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure):</span><span class="sxs-lookup"><span data-stu-id="d9b6b-190">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="d9b6b-191">Для последующей настройки всех именованных экземпляров конфигурации служит метод [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall):</span><span class="sxs-lookup"><span data-stu-id="d9b6b-191">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="d9b6b-192">Фабрика, мониторинг и кэш параметров</span><span class="sxs-lookup"><span data-stu-id="d9b6b-192">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="d9b6b-193">Интерфейс [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) служит для создания уведомлений об изменении экземпляров `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="d9b6b-194">`IOptionsMonitor` поддерживает перезагружаемые параметры, уведомления об изменениях и `IPostConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-194">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

<span data-ttu-id="d9b6b-195">Интерфейс [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 или более поздней версии) отвечает за создание экземпляров параметров.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-195">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 or later) is responsible for creating new options instances.</span></span> <span data-ttu-id="d9b6b-196">Он имеет единственный метод [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create).</span><span class="sxs-lookup"><span data-stu-id="d9b6b-196">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="d9b6b-197">Реализация по умолчанию принимает все зарегистрированные интерфейсы `IConfigureOptions` и `IPostConfigureOptions` и выполняет сначала все конфигурации, а затем постконфигурации.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-197">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="d9b6b-198">Она различает интерфейсы `IConfigureNamedOptions` и `IConfigureOptions` и вызывает только соответствующий интерфейс.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-198">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="d9b6b-199">Интерфейс [IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 или более поздней версии) используется интерфейсом `IOptionsMonitor` для кэширования экземпляров `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="d9b6b-200">`IOptionsMonitorCache` делает экземпляры параметров в мониторе недействительными, что приводит к повторному вычислению значений ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span><span class="sxs-lookup"><span data-stu-id="d9b6b-200">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="d9b6b-201">Значения можно также вводить вручную с помощью [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span><span class="sxs-lookup"><span data-stu-id="d9b6b-201">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="d9b6b-202">Метод [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) используется, если необходимо повторно создать все именованные экземпляры по требованию.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-202">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

## <a name="accessing-options-during-startup"></a><span data-ttu-id="d9b6b-203">Доступ к параметрам во время запуска</span><span class="sxs-lookup"><span data-stu-id="d9b6b-203">Accessing options during startup</span></span>

<span data-ttu-id="d9b6b-204">`IOptions` можно использовать в методе `Configure`, так как службы создаются до выполнения метода `Configure`.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-204">`IOptions` can be used in `Configure`, since services are built before the `Configure` method executes.</span></span> <span data-ttu-id="d9b6b-205">Если в `ConfigureServices` создается поставщик службы для доступа к параметрам, он не будет содержать конфигурации параметров, предоставленные после его создания.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-205">If a service provider is built in `ConfigureServices` to access options, it wouldn't contain any options configurations provided after the service provider is built.</span></span> <span data-ttu-id="d9b6b-206">Поэтому из-за очередности регистрации служб состояние параметров может быть несогласованным.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-206">Therefore, an inconsistent options state may exist due to the ordering of service registrations.</span></span>

<span data-ttu-id="d9b6b-207">Так как параметры, как правило, загружаются из конфигурации, конфигурация может использоваться при запуске как в `Configure`, так и в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d9b6b-207">Since options are typically loaded from configuration, configuration can be used in startup in both `Configure` and `ConfigureServices`.</span></span> <span data-ttu-id="d9b6b-208">Примеры использования конфигурации во время запуска см. в статье [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="d9b6b-208">For examples of using configuration during startup, see the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="see-also"></a><span data-ttu-id="d9b6b-209">См. также</span><span class="sxs-lookup"><span data-stu-id="d9b6b-209">See also</span></span>

* [<span data-ttu-id="d9b6b-210">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="d9b6b-210">Configuration</span></span>](xref:fundamentals/configuration/index)
