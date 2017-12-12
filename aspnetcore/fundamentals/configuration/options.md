---
title: "Параметры шаблона в ASP.NET Core"
author: guardrex
description: "Узнайте, как использовать параметры шаблона для представления групп связанные параметры в приложениях ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 11/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/options
ms.openlocfilehash: 7d89416626433bf737b63eda4b17e65b089ae142
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/29/2017
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="19079-103">Параметры шаблона в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="19079-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="19079-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="19079-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="19079-105">Параметры шаблона использует параметры классы для представления групп связанные параметры.</span><span class="sxs-lookup"><span data-stu-id="19079-105">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="19079-106">Когда параметры конфигурации, изолируются компонент в классы отдельных параметров, приложение устанавливает двух принципов важные программного обеспечения:</span><span class="sxs-lookup"><span data-stu-id="19079-106">When configuration settings are isolated by feature into separate options classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="19079-107">[Принцип разделения интерфейс (ISP)](http://deviq.com/interface-segregation-principle/): функции (классы), зависящих от параметров конфигурации зависят от только параметры конфигурации, которые они используют.</span><span class="sxs-lookup"><span data-stu-id="19079-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="19079-108">[Разделение областей ответственности](http://deviq.com/separation-of-concerns/): параметры для разных частей приложения, не зависящие от них или связанные друг с другом.</span><span class="sxs-lookup"><span data-stu-id="19079-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="19079-109">[Просмотреть или загрузить образец кода](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([загрузке](xref:tutorials/index#how-to-download-a-sample)) проще выполнить с помощью образца приложения в этой статье.</span><span class="sxs-lookup"><span data-stu-id="19079-109">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="19079-110">Основные параметры конфигурации</span><span class="sxs-lookup"><span data-stu-id="19079-110">Basic options configuration</span></span>

<span data-ttu-id="19079-111">Основные параметры конфигурации, представленной в качестве примера &num;1 в [пример приложения](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="19079-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="19079-112">Класс параметров должен быть абстрактным открытый конструктор без параметров.</span><span class="sxs-lookup"><span data-stu-id="19079-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="19079-113">Следующий класс `MyOptions`, имеет два свойства `Option1` и `Option2`.</span><span class="sxs-lookup"><span data-stu-id="19079-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="19079-114">Установка значений по умолчанию является необязательным, но конструктор класса, в следующем примере задает значение по умолчанию `Option1`.</span><span class="sxs-lookup"><span data-stu-id="19079-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="19079-115">`Option2`значение по умолчанию, задать путем инициализации свойства непосредственно (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="19079-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="19079-116">`MyOptions` Класс добавляется в контейнер службы с [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) и привязан к конфигурации:</span><span class="sxs-lookup"><span data-stu-id="19079-116">The `MyOptions` class is added to the service container with [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) and bound to configuration:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="19079-117">Следующие страницы использует модель [внедрения зависимостей конструктор](xref:fundamentals/dependency-injection#what-is-dependency-injection) с [IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) для доступа к параметрам (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="19079-117">The following page model uses [constructor dependency injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="19079-118">Образец *appsettings.json* файл задает значения для `option1` и `option2`:</span><span class="sxs-lookup"><span data-stu-id="19079-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[Main](options/sample/appsettings.json)]

<span data-ttu-id="19079-119">При запуске приложения модель страницы `OnGet` метод возвращает строку, содержащую класс значения параметров:</span><span class="sxs-lookup"><span data-stu-id="19079-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="19079-120">Настройте параметры простой делегат</span><span class="sxs-lookup"><span data-stu-id="19079-120">Configure simple options with a delegate</span></span>

<span data-ttu-id="19079-121">Демонстрируется настройка простыми параметрами с помощью делегата в качестве примера &num;2 в [пример приложения](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="19079-121">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="19079-122">Использование делегата для задания значений параметров.</span><span class="sxs-lookup"><span data-stu-id="19079-122">Use a delegate to set options values.</span></span> <span data-ttu-id="19079-123">В образце приложения используется `MyOptionsWithDelegateConfig` класса (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="19079-123">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="19079-124">В следующем коде секунды `IConfigureOptions<TOptions>` служба добавляется в контейнер службы.</span><span class="sxs-lookup"><span data-stu-id="19079-124">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="19079-125">Она использует делегат для настройки привязки с `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="19079-125">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="19079-126">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="19079-126">*Index.cshtml.cs*:</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="19079-127">Можно добавить несколько поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="19079-127">You can add multiple configuration providers.</span></span> <span data-ttu-id="19079-128">Поставщики конфигурации, доступные в пакетах NuGet.</span><span class="sxs-lookup"><span data-stu-id="19079-128">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="19079-129">Время, они применены, они регистрируются.</span><span class="sxs-lookup"><span data-stu-id="19079-129">They're applied in order that they're registered.</span></span>

<span data-ttu-id="19079-130">Каждый вызов [Настройка&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) добавляет `IConfigureOptions<TOptions>` службу в контейнер службы.</span><span class="sxs-lookup"><span data-stu-id="19079-130">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="19079-131">В предыдущем примере значения `Option1` и `Option2` указываются в *appsettings.json*, но значения `Option1` и `Option2` переопределяются настроенных делегата.</span><span class="sxs-lookup"><span data-stu-id="19079-131">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="19079-132">При включении более одной конфигурации службы указано последнее источник конфигурации *побеждает* и задает значение конфигурации.</span><span class="sxs-lookup"><span data-stu-id="19079-132">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="19079-133">При запуске приложения модель страницы `OnGet` метод возвращает строку, содержащую класс значения параметров:</span><span class="sxs-lookup"><span data-stu-id="19079-133">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="19079-134">Изменение конфигурации</span><span class="sxs-lookup"><span data-stu-id="19079-134">Suboptions configuration</span></span>

<span data-ttu-id="19079-135">Демонстрируется изменение конфигурации в качестве примера &num;3 в [пример приложения](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="19079-135">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="19079-136">Приложения следует создать классы параметров, относящихся к конкретному компоненту группы (классы) в приложении.</span><span class="sxs-lookup"><span data-stu-id="19079-136">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="19079-137">Частей приложения, которые требуют значения конфигурации только должны иметь доступ к значения конфигурации, которые в них используются.</span><span class="sxs-lookup"><span data-stu-id="19079-137">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="19079-138">При привязке параметров конфигурации, каждое свойство в тип параметров привязывается к раздел конфигурации в формате `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="19079-138">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="19079-139">Например `MyOptions.Option1` свойства, связанного с ключом `Option1`, который считывается из `option1` свойство в *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="19079-139">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="19079-140">В следующем коде третий `IConfigureOptions<TOptions>` служба добавляется в контейнер службы.</span><span class="sxs-lookup"><span data-stu-id="19079-140">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="19079-141">Привязывает `MySubOptions` к разделу `subsection` из *appsettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="19079-141">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="19079-142">`GetSection` Требует метод расширения [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="19079-142">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="19079-143">Если приложение использует [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, пакет автоматически включается.</span><span class="sxs-lookup"><span data-stu-id="19079-143">If the app uses the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, the package is automatically included.</span></span>

<span data-ttu-id="19079-144">Образец *appsettings.json* файл определяет `subsection` элемент с разделов для `suboption1` и `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="19079-144">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[Main](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="19079-145">`MySubOptions` Класс определяет свойства, `SubOption1` и `SubOption2`, для хранения значения суб-параметр (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="19079-145">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the sub-option values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="19079-146">Модель страницы `OnGet` метод возвращает строку со значениями параметра вложенных (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="19079-146">The page model's `OnGet` method returns a string with the sub-option values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="19079-147">При запуске приложения `OnGet` метод возвращает строку, содержащую параметр вложенный класс значения:</span><span class="sxs-lookup"><span data-stu-id="19079-147">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="19079-148">Параметры, представленные с модели представления или представления прямого внедрения</span><span class="sxs-lookup"><span data-stu-id="19079-148">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="19079-149">Параметры, предоставляемый модели представления или с прямого представления путем внедрения кода демонстрируется пример &num;4 в [пример приложения](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="19079-149">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="19079-150">Параметры могут передаваться в модель представления или путем вставки `IOptions<TOptions>` непосредственно в представление (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="19079-150">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="19079-151">Прямые вставки внедрить `IOptions<MyOptions>` с `@inject` директиву:</span><span class="sxs-lookup"><span data-stu-id="19079-151">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[Main](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="19079-152">При запуске приложения в отображаемой странице показаны значения параметров:</span><span class="sxs-lookup"><span data-stu-id="19079-152">When the app is run, the option values are shown in the rendered page:</span></span>

![Вариант 1 значения параметров: value1_from_json и вариант 2: -1 загружаются из модели и внедрение в представлении.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="19079-154">Перезагрузить данные конфигурации с IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="19079-154">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="19079-155">Повторная загрузка данных конфигурации с `IOptionsSnapshot` показано в следующем примере &num;5 в [пример приложения](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="19079-155">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="19079-156">*Требуется ASP.NET Core 1.1 или более поздней версии.*</span><span class="sxs-lookup"><span data-stu-id="19079-156">*Requires ASP.NET Core 1.1 or later.*</span></span>

<span data-ttu-id="19079-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) поддерживает повторная загрузка параметров с минимальными лишнюю работу.</span><span class="sxs-lookup"><span data-stu-id="19079-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span> <span data-ttu-id="19079-158">В ASP.NET Core 1.1 `IOptionsSnapshot` представляет собой моментальный снимок [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) и обновления автоматически каждый раз, когда монитор запускает изменения, основанные на изменение источника данных.</span><span class="sxs-lookup"><span data-stu-id="19079-158">In ASP.NET Core 1.1, `IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span> <span data-ttu-id="19079-159">В ASP.NET Core 2.0 и более поздних версиях параметры вычисляются один раз на каждый запрос, когда доступ к и кэшируется на время существования запроса.</span><span class="sxs-lookup"><span data-stu-id="19079-159">In ASP.NET Core 2.0 and later, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="19079-160">В следующем примере показано, как новый `IOptionsSnapshot` создается после *appsettings.json* изменений (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="19079-160">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="19079-161">Несколько запросов к серверу возвращать значения констант *appsettings.json* файл, пока не был изменен и перезагрузок конфигурации.</span><span class="sxs-lookup"><span data-stu-id="19079-161">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="19079-162">На следующем рисунке показана первоначального `option1` и `option2` значения загружены из *appsettings.json* файла:</span><span class="sxs-lookup"><span data-stu-id="19079-162">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="19079-163">Измените значения в *appsettings.json* файл `value1_from_json UPDATED` и `200`.</span><span class="sxs-lookup"><span data-stu-id="19079-163">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="19079-164">Сохранить *appsettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="19079-164">Save the *appsettings.json* file.</span></span> <span data-ttu-id="19079-165">Обновите браузер, чтобы увидеть, что значения параметров будут обновлены:</span><span class="sxs-lookup"><span data-stu-id="19079-165">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="19079-166">Поддержка параметров с IConfigureNamedOptions именованного</span><span class="sxs-lookup"><span data-stu-id="19079-166">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="19079-167">Поддержка параметров с именем [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) демонстрируется пример &num;6 в [пример приложения](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="19079-167">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="19079-168">*Требуется ASP.NET Core 2.0 или более поздней версии.*</span><span class="sxs-lookup"><span data-stu-id="19079-168">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="19079-169">*Именованные параметры* поддержки позволяет приложению различать именованные параметры конфигурации.</span><span class="sxs-lookup"><span data-stu-id="19079-169">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="19079-170">В примере приложения именованные параметры должны быть объявлены с [ConfigureNamedOptions&lt;TOptions&gt;. Настройка](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) метод:</span><span class="sxs-lookup"><span data-stu-id="19079-170">In the sample app, named options are declared with the [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="19079-171">Пример приложения, обращающийся к именованные параметры с [IOptionsSnapshot&lt;TOptions&gt;. Получить](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="19079-171">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="19079-172">Выполнение образца приложения, возвращаются именованные параметры.</span><span class="sxs-lookup"><span data-stu-id="19079-172">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="19079-173">`named_options_1`Предоставляет значения из конфигурации, из которого загружаются *appsettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="19079-173">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="19079-174">`named_options_2`Предоставляет значения по:</span><span class="sxs-lookup"><span data-stu-id="19079-174">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="19079-175">`named_options_2` Делегата в `ConfigureServices` для `Option1`.</span><span class="sxs-lookup"><span data-stu-id="19079-175">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="19079-176">Значение по умолчанию для `Option2` , предоставляемые `MyOptions` класса.</span><span class="sxs-lookup"><span data-stu-id="19079-176">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="19079-177">Настройте все экземпляры именованных параметров с [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) метод.</span><span class="sxs-lookup"><span data-stu-id="19079-177">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="19079-178">В следующем коде настраивается `Option1` для всех именованных экземпляров конфигурации с общим значением.</span><span class="sxs-lookup"><span data-stu-id="19079-178">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="19079-179">Добавьте следующий код вручную на `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="19079-179">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="19079-180">Выполнение образца приложения после добавления кода выдает следующий результат:</span><span class="sxs-lookup"><span data-stu-id="19079-180">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="19079-181">В ASP.NET Core 2.0 и более поздних версиях все параметры являются именованными экземплярами.</span><span class="sxs-lookup"><span data-stu-id="19079-181">In ASP.NET Core 2.0 and later, all options are named instances.</span></span> <span data-ttu-id="19079-182">Существующие `IConfigureOption` экземпляры рассматриваются как предназначенные для `Options.DefaultName` экземпляра, то есть `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="19079-182">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="19079-183">`IConfigureNamedOptions`также реализует `IConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="19079-183">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="19079-184">Реализация по умолчанию [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([ссылки на источник](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) содержит логику для использования каждого соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="19079-184">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) has logic to use each appropriately.</span></span> <span data-ttu-id="19079-185">`null` Именованный параметр используется для всех экземпляров с именами вместо указанный именованный экземпляр ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) и [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) использующих данное соглашение о).</span><span class="sxs-lookup"><span data-stu-id="19079-185">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

## <a name="ipostconfigureoptions"></a><span data-ttu-id="19079-186">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="19079-186">IPostConfigureOptions</span></span>

<span data-ttu-id="19079-187">*Требуется ASP.NET Core 2.0 или более поздней версии.*</span><span class="sxs-lookup"><span data-stu-id="19079-187">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="19079-188">Задать postconfiguration с [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span><span class="sxs-lookup"><span data-stu-id="19079-188">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="19079-189">Запускается после окончания postconfiguration [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) происходит конфигурации:</span><span class="sxs-lookup"><span data-stu-id="19079-189">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="19079-190">[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) для последующей настройки именованные параметры:</span><span class="sxs-lookup"><span data-stu-id="19079-190">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="19079-191">Используйте [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) для последующей настройки всех именованных экземпляров конфигурации:</span><span class="sxs-lookup"><span data-stu-id="19079-191">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="19079-192">Параметры фабрики, мониторинга и кэша</span><span class="sxs-lookup"><span data-stu-id="19079-192">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="19079-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) используется для получения уведомлений при `TOptions` экземпляров изменений.</span><span class="sxs-lookup"><span data-stu-id="19079-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="19079-194">`IOptionsMonitor`поддерживает reloadable "Параметры", уведомления об изменениях, и `IPostConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="19079-194">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

<span data-ttu-id="19079-195">[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) создание новых параметрах экземпляров отвечает (ядро ASP.NET 2.0 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="19079-195">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 or later) is responsible for creating new options instances.</span></span> <span data-ttu-id="19079-196">Он имеет один [создать](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) метод.</span><span class="sxs-lookup"><span data-stu-id="19079-196">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="19079-197">Реализация по умолчанию принимает все зарегистрированные `IConfigureOptions` и `IPostConfigureOptions` и запускает все сначала настраивается, за которым следует после настраивает.</span><span class="sxs-lookup"><span data-stu-id="19079-197">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="19079-198">Позволяют разделить `IConfigureNamedOptions` и `IConfigureOptions` и вызывает только соответствующий интерфейс.</span><span class="sxs-lookup"><span data-stu-id="19079-198">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="19079-199">[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ядро ASP.NET 2.0 или более поздней версии) используется `IOptionsMonitor` кэш `TOptions` экземпляров.</span><span class="sxs-lookup"><span data-stu-id="19079-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="19079-200">`IOptionsMonitorCache` Делает недействительными параметры экземпляров в мониторе, чтобы значение будет пересчитано ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span><span class="sxs-lookup"><span data-stu-id="19079-200">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="19079-201">Значения могут быть вручную обеспечены также [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span><span class="sxs-lookup"><span data-stu-id="19079-201">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="19079-202">[Снимите](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) метод используется, когда все именованные экземпляры может быть повторно создан по требованию.</span><span class="sxs-lookup"><span data-stu-id="19079-202">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

## <a name="see-also"></a><span data-ttu-id="19079-203">См. также</span><span class="sxs-lookup"><span data-stu-id="19079-203">See also</span></span>

* [<span data-ttu-id="19079-204">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="19079-204">Configuration</span></span>](xref:fundamentals/configuration/index)
